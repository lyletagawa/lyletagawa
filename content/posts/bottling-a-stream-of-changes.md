---
title: "Bottling a Stream of Changes"
date: 2026-07-12
publishdate: 2026-07-12
lastmod: 2026-07-15
summary: "Debezium traces back to a tool called Bottled Water, built to bottle a database's write-ahead log into Kafka messages. A scheduled query against a timestamp column gets called 'CDC' too, but it isn't the same thing."
tags: ["kafka", "databases", "streaming"]
image: /images/bottling-a-stream-of-changes.jpg
draft: true
---

![Debezium traces back to a tool called Bottled Water, built to bottle a database's write-ahead log into Kafka messages. A scheduled query against a timestamp column gets called 'CDC' too, but it isn't the same thing.](/images/bottling-a-stream-of-changes.jpg)
*A cross section of a California redwood, more than 800 years of growth recorded as an ordered, append-only sequence of rings. Photo: [Elwood P. Dowd (2023)](https://commons.wikimedia.org/wiki/File:Cross_section_of_a_Sequoia_sempervirens_showing_its_tree_rings.jpg). CC BY-SA 4.0.*

## Bottling a Stream of Changes

In 2015, Martin Kleppmann released Bottled Water, an open-source tool that read PostgreSQL's write-ahead log and turned every committed row change into a Kafka message{{< cite 1 "Kleppmann, Martin (2015). Bottled Water: Real-time Integration of PostgreSQL and Kafka." >}}. A year later, Randall Hauch, a software engineer at Red Hat, started a similar project on top of Kafka Connect, a still-new piece of Kafka infrastructure{{< cite 2 "Morling, Gunnar. Change Data Capture and Debezium. InfoQ Podcast." >}}. He called it Debezium.

Both read the log instead of querying for changes. A scheduled query against a timestamp or version column often gets called "CDC" too, but it isn't the same thing. Real change data capture taps the log the database already writes for its own crash recovery and replication{{< cite 3 "Five Advantages of Log-Based Change Data Capture. Debezium Blog." >}}.

## What Is Change Data Capture?

The difference shows up at the edges. A polling query that runs every 30 seconds only sees the state a row was in at that instant. If a value was set and changed back within that window, the poll sees nothing. If a row was inserted and deleted between two polls, the poll never learns it existed. And a plain query has no way to notice a delete at all, since a deleted row doesn't show up in a "what changed" result, it just isn't there anymore{{< cite 3 "Five Advantages of Log-Based Change Data Capture. Debezium Blog." >}}.

Log-based CDC doesn't have this problem, because it isn't inferring changes from state, it's replaying a record of events that already happened. Every insert, update, and delete gets written to the database's transaction log before the transaction commits. Postgres calls this the write-ahead log and exposes it to external tools through logical decoding{{< cite 4 "Logical Decoding. PostgreSQL Documentation." >}}. Reading that log costs the source database nothing extra, since it's already writing the log anyway, and it captures the complete, ordered history, including every delete.

## Where This Shows Up Today

Debezium's MySQL connector reads MySQL's binary log, the binlog, and turns every row-level insert, update, and delete into a Kafka event{{< cite 5 "Debezium connector for MySQL. Debezium Documentation." >}}. The connector doesn't run a single query against your tables during normal operation. It just tails a file the database was writing anyway.

Slack put this to work at real scale. Their old pipeline read nightly database backups into an S3 data lake, and data could be up to a day old by the time anyone queried it. Slack helped build a Debezium connector to read changes directly from the binlog, cutting that latency from 24 hours to under 10 minutes across hundreds of tables and thousands of database shards{{< cite 6 "Change Data Capture and Kafka: How Slack Transitioned to CDC with Debezium and Kafka Connect. Confluent Current." >}}.

## Common Mistakes

**Calling a polling job "CDC."** A scheduled query against a LAST_UPDATED column misses deletes entirely and can miss any change that happened and got overwritten between two polls. It's a reasonable pattern for some use cases. It just isn't change data capture{{< cite 3 "Five Advantages of Log-Based Change Data Capture. Debezium Blog." >}}.

**Assuming Debezium delivers every event exactly once.** Debezium guarantees at-least-once delivery. When a connector crashes and restarts, it resumes from its last recorded position, which can mean replaying events a consumer already saw{{< cite 7 "Frequently Asked Questions. Debezium Documentation." >}}. Consumers need to be idempotent, or you need to deduplicate downstream.

**Assuming Debezium skips existing data.** Unlike a ClickHouse materialized view, Debezium's default snapshot mode takes a full, locked read of every captured table before it starts streaming{{< cite 5 "Debezium connector for MySQL. Debezium Documentation." >}}. On a large table that snapshot can hold a lock and run for hours, the opposite surprise from expecting a tool to ignore history.

**Treating Debezium as a fix for the dual-write problem.** Writing to your database and separately publishing to Kafka is never atomic on its own. One can succeed while the other fails. The outbox pattern solves this by writing the event to a table in the same transaction as the data change, then letting Debezium read the outbox table instead of guessing{{< cite 8 "Morling, Gunnar (2019). Reliable Microservices Data Exchange With the Outbox Pattern. Debezium Blog." >}}.

## Put It Into Practice

Before you call something CDC, ask what it's actually reading. A query is polling, a log is CDC, and the two produce similar-looking output most of the time. That's exactly why the gap only shows up when it matters, a missed delete, a lost intermediate state, a replica silently drifting from its source.

If you're already running Debezium, check your snapshot mode and your consumer's idempotency before you need them. Both are easy to configure correctly ahead of time and expensive to discover wrong in production.

The log was always there. Reading it instead of guessing at it is the entire idea.

## Go Further

**The talk that started it.** Kleppmann's "Turning the Database Inside Out" argues that a log of events, not a snapshot of current state, should be the source of truth systems get built around{{< cite 9 "Kleppmann, Martin (2015). Turning the Database Inside Out. Strange Loop." >}}.

**Real exactly-once delivery.** Kafka Connect added native exactly-once support for source connectors in version 3.3, for teams that want the guarantee built in instead of deduplicating it themselves{{< cite 10 "KIP-618: Exactly-Once Support for Source Connectors. Apache Kafka." >}}.

**Snapshots without the lock.** Debezium's incremental snapshots, added in version 1.6, let a connector capture existing data in chunks alongside live streaming instead of one long blocking pass{{< cite 11 "Pechanec, Jiri (2021). Incremental Snapshots in Debezium. Debezium Blog." >}}.

---

## References

<ol class="references">
  <li id="ref-1">Kleppmann, Martin (2015). "Bottled Water: Real-time Integration of PostgreSQL and Kafka." <a href="https://martin.kleppmann.com/2015/04/23/bottled-water-real-time-postgresql-kafka.html">https://martin.kleppmann.com/2015/04/23/bottled-water-real-time-postgresql-kafka.html</a></li>
  <li id="ref-2">Morling, Gunnar. "Change Data Capture and Debezium." <em>InfoQ Podcast</em>. <a href="https://www.infoq.com/podcasts/change-data-capture-debezium/">https://www.infoq.com/podcasts/change-data-capture-debezium/</a></li>
  <li id="ref-3">Morling, Gunnar (2018). "Five Advantages of Log-Based Change Data Capture." <em>Debezium Blog</em>. <a href="https://debezium.io/blog/2018/07/19/advantages-of-log-based-change-data-capture/">https://debezium.io/blog/2018/07/19/advantages-of-log-based-change-data-capture/</a></li>
  <li id="ref-4">"Logical Decoding." <em>PostgreSQL Documentation</em>. <a href="https://www.postgresql.org/docs/current/logicaldecoding.html">https://www.postgresql.org/docs/current/logicaldecoding.html</a></li>
  <li id="ref-5">"Debezium connector for MySQL." <em>Debezium Documentation</em>. <a href="https://debezium.io/documentation/reference/stable/connectors/mysql.html">https://debezium.io/documentation/reference/stable/connectors/mysql.html</a></li>
  <li id="ref-6">"Change Data Capture and Kafka: How Slack Transitioned to CDC with Debezium and Kafka Connect." <em>Confluent Current</em>. <a href="https://current.confluent.io/2024-sessions/change-data-capture-kafka-how-slack-transitioned-to-cdc-with-debezium-kafka-connect">https://current.confluent.io/2024-sessions/change-data-capture-kafka-how-slack-transitioned-to-cdc-with-debezium-kafka-connect</a></li>
  <li id="ref-7">"Frequently Asked Questions." <em>Debezium Documentation</em>. <a href="https://debezium.io/documentation/faq/">https://debezium.io/documentation/faq/</a></li>
  <li id="ref-8">Morling, Gunnar (2019). "Reliable Microservices Data Exchange With the Outbox Pattern." <em>Debezium Blog</em>. <a href="https://debezium.io/blog/2019/02/19/reliable-microservices-data-exchange-with-the-outbox-pattern/">https://debezium.io/blog/2019/02/19/reliable-microservices-data-exchange-with-the-outbox-pattern/</a></li>
  <li id="ref-9">Kleppmann, Martin (2015). "Turning the Database Inside Out." <a href="https://martin.kleppmann.com/2015/03/04/turning-the-database-inside-out.html">https://martin.kleppmann.com/2015/03/04/turning-the-database-inside-out.html</a></li>
  <li id="ref-10">"KIP-618: Exactly-Once Support for Source Connectors." <em>Apache Kafka</em>. <a href="https://cwiki.apache.org/confluence/display/KAFKA/KIP-618:+Exactly-Once+Support+for+Source+Connectors">https://cwiki.apache.org/confluence/display/KAFKA/KIP-618:+Exactly-Once+Support+for+Source+Connectors</a></li>
  <li id="ref-11">Pechanec, Jiri (2021). "Incremental Snapshots in Debezium." <em>Debezium Blog</em>. <a href="https://debezium.io/blog/2021/10/07/incremental-snapshots/">https://debezium.io/blog/2021/10/07/incremental-snapshots/</a></li>
</ol>

---

## Outtakes

**Debezium's name is a chemistry pun.** It combines "DBs," the abbreviation for databases, with the "-ium" suffix used for elements on the periodic table. Say it fast: "DBs-ium" ([Debezium FAQ](https://debezium.io/documentation/faq/)).

**Debezium had a predecessor.** A year before Hauch started Debezium, Kleppmann released Bottled Water, a Confluent-sponsored tool that streamed PostgreSQL's write-ahead log into Kafka using the same logical-decoding approach ([Kleppmann, 2015](https://martin.kleppmann.com/2015/04/23/bottled-water-real-time-postgresql-kafka.html)).

**Every database calls its transaction log something different.** Postgres has the WAL, MySQL has the binlog, MongoDB has the oplog, the same underlying idea under three different names ([Debezium Blog](https://debezium.io/blog/2018/07/19/advantages-of-log-based-change-data-capture/)).

---

## Changelog

**2026-07-12** Initial release.
