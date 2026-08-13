---
title: "Idempotency"
date: 2026-08-05
publishdate: 2026-08-05
lastmod: 2026-08-05
summary: "Idempotency lets a client safely retry a request without knowing whether the first attempt succeeded. Stripe's idempotency keys, HTTP's own method semantics, and Kafka show how the pattern works."
tags: ["reliability", "apis", "distributed"]
image: /images/idempotency.jpg
draft: true
---

![Idempotency lets a client safely retry a request without knowing whether the first attempt succeeded. Stripe's idempotency keys, HTTP's own method semantics, and Kafka show how the pattern works.](/images/idempotency.jpg)
*A rotary telephone dial. Photo: [Nenad Stojkovic (2021)](https://commons.wikimedia.org/wiki/File:The_old_telephone_closeup._(51570403149).jpg). CC BY 2.0.*

## Idempotency

In 2017, Stripe engineer Brandur Leach wrote up a problem every payments API eventually hits. A network drops mid-request, the client has no way to tell whether the charge went through, and it's left with two options. Give up, or retry and risk charging the customer twice{{< cite 1 "Leach, Brandur (2017). Designing robust and predictable APIs with idempotency. Stripe Blog, February 22, 2017." >}}.

Stripe's fix was the `Idempotency-Key` header. The client generates a unique key for each attempted operation and sends it with the request. If Stripe has already processed that key, it returns the original result instead of running the charge again. Leach was direct about the stakes. He wrote that "accidentally calling [a charge endpoint] twice would lead to the customer being double-charged, which is very bad{{< cite 1 "Leach, Brandur (2017). Designing robust and predictable APIs with idempotency. Stripe Blog, February 22, 2017." >}}." The key turns a dangerous retry into a safe one.

That's idempotency. An operation that produces the same result no matter how many times you run it. Once an operation has that property, retrying it stops being a risk and becomes the obvious response to a request that might have failed silently.

## What Is Idempotency?

The concept predates modern APIs by decades. The original HTTP specification classified GET, PUT, and DELETE as idempotent, and POST as not, long before "idempotency key" entered common use. The current definition, in RFC 9110, states it plainly. A method is idempotent if the effect of multiple identical requests matches the effect of one{{< cite 2 "Fielding, R., M. Nottingham, and J. Reschke, eds. (2022). RFC 9110: HTTP Semantics, Section 9.2.2. Internet Engineering Task Force." >}}.

GET and DELETE are idempotent because repeating them leaves the server in the same state either way. Deleting an already-deleted resource just does nothing the second time. PUT is idempotent because it sets a field to a specific value, and running it five times leaves the field holding that same value each time. POST has never been idempotent by specification, because a typical POST creates something new. Submit the same order twice and you get two orders, unless the application does something extra to prevent it.

Idempotent doesn't mean nothing happens. It means what happens converges to the same end state. A PUT that overwrites a row still writes to disk every time you call it. The result stays constant while the work still happens every time.

## Built Into The Stack

**Stripe.** The `Idempotency-Key` header covered above is the version most engineers meet first, a client-generated key scoped to one logical operation, cached for 24 hours{{< cite 1 "Leach, Brandur (2017). Designing robust and predictable APIs with idempotency. Stripe Blog, February 22, 2017." >}}.

**Kafka.** Kafka's idempotent producer solves the same problem one layer down, inside the message broker itself. Each producer gets a unique ID, and every message it sends carries a sequence number. If a network blip causes the producer to resend a message the broker already wrote, the broker recognizes the sequence number and drops the duplicate instead of writing it twice{{< cite 3 "Apache Kafka (2026). Documentation: Message Delivery Semantics. kafka.apache.org." >}}.

**Amazon SQS.** FIFO queues take a similar approach at the queue level. Every message carries a deduplication ID, either supplied by the client or hashed from the message body, and SQS silently discards any duplicate sent with the same ID inside a five-minute window{{< cite 4 "Amazon Web Services (2026). Using the MessageDeduplicationId Property. Amazon SQS Developer Guide." >}}.

**HTTP itself.** Every `PUT` and `DELETE` request a browser or client library retries across a flaky connection is leaning on this same guarantee. The retry logic doesn't have to be clever, because the spec already promises the server will behave.

## Where It Breaks Down

Idempotency only protects the one operation it wraps. The moment an idempotent write also needs to trigger a side effect, publishing an event, sending an email, calling a second service, the guarantee doesn't automatically extend to that second action. A database commit and an outgoing message are two separate operations unless something ties them together, and a crash between them turns one idempotent write into a duplicate message anyway.

Two identical requests arriving close enough together create a race the key alone doesn't solve. If a server hasn't finished recording the first request's key when a second, duplicate request arrives, both can look like the first attempt, and both can run. Real implementations close this gap with a lock or a uniqueness constraint on the key itself, rather than a cache lookup that reads and writes as separate steps.

Idempotency doesn't guarantee order either. A safely retried request tells the server nothing about when that retry arrives relative to other, different requests. A payment retried out of order against a balance update can still land in the wrong sequence, even though neither request duplicated anything.

## Common Mistakes

**Confusing idempotent with side-effect-free.** Idempotent means repeating the operation produces the same result. It doesn't mean the second attempt does nothing. A retried charge still has to check for the existing key. A retried "send welcome email" call still executes code on the second attempt, code that has to recognize the duplicate and suppress the send rather than firing again.

**Reusing a key across a different request.** Most implementations, Stripe included, treat the key as bound to one specific request body. Sending the same key with different parameters is undefined behavior at best and a rejected request at worst{{< cite 1 "Leach, Brandur (2017). Designing robust and predictable APIs with idempotency. Stripe Blog, February 22, 2017." >}}.

**Letting the dedup window expire underneath a slow retry.** Stripe's cache holds a key for 24 hours{{< cite 1 "Leach, Brandur (2017). Designing robust and predictable APIs with idempotency. Stripe Blog, February 22, 2017." >}}. A client that queues a retry longer than that, or a customer who reopens an abandoned checkout the next day, sends what the server now sees as a brand new request. The window has to match how long a real retry might realistically wait, rather than how long the immediate failure took to resolve.

## Put It Into Practice

Start with the operations in your system that actually cost something if duplicated, charges, order creation, anything that sends a message a person will see twice. Give each one a key derived from something the client already has, a client-generated UUID, an order ID, a checkout token, rather than inventing a new identifier scheme.

Store the key and the operation's result together, and check it before doing any work rather than after. The check and the write need the same atomicity guarantee as the operation itself, or the race described above reopens. A uniqueness constraint on the key column in the same table as the write is usually enough.

Pick a window and write it down. Stripe's 24 hours works for a checkout flow where a customer might close the tab and come back at lunch. A background job that retries for a week needs a window that also lasts a week. Whichever window you choose, a request that arrives after it expires should look like a fresh one, because as far as your system can prove, it is.

## Go Further

**Exactly-once semantics.** Idempotency is what makes at-least-once delivery behave like exactly-once, without the underlying delivery actually becoming exactly-once end to end. Kafka's idempotent producer is the same mechanism this post describes, applied to message brokers instead of APIs{{< cite 3 "Apache Kafka (2026). Documentation: Message Delivery Semantics. kafka.apache.org." >}}.

**The Transactional Outbox pattern.** The gap described in "Where It Breaks Down," a database write and a side effect that can drift apart, has a standard fix. Write the event to an outbox table in the same transaction as the business write, and let a separate process deliver it{{< cite 5 "Richardson, Chris (2026). Pattern: Transactional outbox. microservices.io." >}}.

**The Two Generals Problem.** Idempotency exists because of a deeper impossibility. Two parties communicating over an unreliable channel can never be fully certain a message arrived, which is why retrying is necessary in the first place. See [an earlier post on this blog](/posts/two-generals-problem/) for the full argument.

---

## References

<ol class="references">
  <li id="ref-1">Leach, Brandur (2017). "Designing robust and predictable APIs with idempotency." <em>Stripe Blog</em>, February 22, 2017. <a href="https://stripe.com/blog/idempotency">https://stripe.com/blog/idempotency</a></li>
  <li id="ref-2">Fielding, R., M. Nottingham, and J. Reschke, eds. (2022). "RFC 9110: HTTP Semantics," Section 9.2.2. <em>Internet Engineering Task Force</em>. <a href="https://www.rfc-editor.org/rfc/rfc9110.html#section-9.2.2">https://www.rfc-editor.org/rfc/rfc9110.html#section-9.2.2</a></li>
  <li id="ref-3">Apache Kafka (2026). "Documentation: Message Delivery Semantics." <a href="https://kafka.apache.org/documentation/#semantics">https://kafka.apache.org/documentation/#semantics</a></li>
  <li id="ref-4">Amazon Web Services (2026). "Using the MessageDeduplicationId Property." <em>Amazon SQS Developer Guide</em>. <a href="https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/using-messagededuplicationid-property.html">https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/using-messagededuplicationid-property.html</a></li>
  <li id="ref-5">Richardson, Chris (2026). "Pattern: Transactional outbox." <em>microservices.io</em>. <a href="https://microservices.io/patterns/data/transactional-outbox.html">https://microservices.io/patterns/data/transactional-outbox.html</a></li>
</ol>

---

## Changelog

**2026-08-05** Initial release.  
