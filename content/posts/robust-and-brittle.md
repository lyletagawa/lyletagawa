---
title: "Robust and Brittle... The Southwest Failure"
date: 2023-01-08T15:07:19-05:00
summary: "Southwest Airlines has grown while desperately trying to be scrappy, creative and humorous. The challenge is that scrappy doesn’t scale well."
tags: ["learning", "resilience"]
draft: false
---

![]()

##

[Todd Conklin](https://www.hophub.org/conklin-bio) discusses [The Southwest Failure](https://preaccidentpodcast.podbean.com/e/papod-426-robust-and-brittlethe-southwest-failure/) that occurred between December 22-29, 2022.

Southwest Airlines has grown while desperately trying to be scrappy, creative and humorous. The challenge is that scrappy doesn't scale well.

## The First Domino

Southwest Airlines is a point-to-point airline, not a hub and spoke. For the most part, many people can do a nonstop budget airline flight on Southwest from where they are to where they need to go, and it works beautifully.

The challenge is, in a point-to-point, is that a delay has the linear ability to domino that failure throughout the entire day. So it's a complex system, but operationally it's done in a linear fashion.

If the first turnover is 40 minutes late, they're going to spend the rest of the day either 40 minutes late or working desperately to try to make up those 40 minutes, and oftentimes it doesn't work.
There's clearly a structural weakness in their system. Or as [Adrian Cockroft](https://mastodon.social/@adrianco) and [Courtney Nash](https://twitter.com/courtneynash) would say, part of their system is always a little broken someplace.

## Balance between Robust and Resilient

What's interesting is this balance between robust and resilient. Southwest is a very effective company. Normally, they move a lot of people in a linear fashion, effectively, and on time.

It's a good system that works well until it doesn't. The challenge is that robust is always important. Robust matters. Robust is what gives the shareholder value its value.

But resilience is what saves you. And a very robust system with very little resilience is a system that when it fails, has the potential to fail catastrophically.

And that's what we saw with Southwest, a stable but brittle system that when tested and when a multitude of conditions coexisted, lacked the ability to recover.

## Operational Feedback Loop

There was a strong push when they were scrappy to focus on operations, to listen to the people, because the people knew. The early leaders of Southwest Airlines were operational people who moved into leadership. But over time, and with retirements, and as the priorities of the organization changed, funding and finances drove a new set of requirements into the equation.

And then what you have is a scrappy airline that got used to solving problems based upon operational expertise in real time, and could handle weak systems quite well, because weak systems fail a lot.

What you had was a system that lost its operational expertise. They lost that operational feedback loop.

With the absence of the operational feedback loop, what they counted on was the financial feedback loop. They assumed that the financial feedback loop was giving them operational information.

Over time, this drift was natural and slow, and probably hard to notice, and probably felt like the right thing to do. There was a significant lack in investment on the operational side of Southwest Airlines. And they didn't replace, for instance, their cruise scheduling software, which is a very complex process on a good day. It's difficult for a small airline. It is incredibly difficult when the system is stressed, and it's only made more complex by the size of the airline. They didn't replace that software system since its inception in 1990. That's a system that's running to failure, and ran to failure.

## The Lesson

The lesson for us is probably written in what they had to do in order to stop the bleeding.

Because they lacked resilience, they didn't have recoverability.

What they ended up doing was canceling thousands of flights every day for an entire series of days, which included the entire holiday break. They just canceled all the flights. And the reason they canceled the flights is because they had to draw a line operationally and start again.

They stopped flying so they could figure out where all the equipment was and where all the people were, because they didn't know either of those things.

They had to start again in their linear point-to-point system and move equipment and people to where that equipment and people needed to be to start again from scratch.

That is a lesson for us, because that is an organization that so significantly lacked resilience in the system, that their only reasonable response was to stop operations completely.

The most important part of this lesson is to come out and say:
- Here's what we learned
- Here's what failed
- Here's what we learned about what failed
- Here's how we chose to improve
- Here's how we put resilience in the system so that when it fails again, and it will, here's how we're going to recover.

You get the failure you have, or you have the opportunity to get a failure you want to have.

## Transcription

```
$ wget -q https://mcdn.podbean.com/mf/web/7im5kb/PAPod_426_-_Robust_and_Brittle9c8bd.mp3
$ whisper PAPod_426_-_Robust_and_Brittle9c8bd.mp3 --model medium --language English
[00:00.000 --> 00:04.560]  Well, the year's starting again, are you ready for it?
[00:04.560 --> 00:05.560]  I think you are.
[00:05.560 --> 00:07.560]  I mean, I feel like you are.
[00:07.560 --> 00:11.120]  Of course my schedule has gotten totally blown out of the water, but that's just how stuff
[00:11.120 --> 00:12.120]  happens for me.
[00:12.120 --> 00:17.680]  It's not really a problem, it's just the way we all learn to adapt in a rapidly changing
[00:17.680 --> 00:31.880]  and complex world.
[00:31.880 --> 00:35.120]  Hey everybody, Todd Conklin, Pre-Accident Investigation Safety Moment.
[00:35.120 --> 00:36.800]  Hello my friends.
[00:36.800 --> 00:38.240]  It is a new year.
[00:38.240 --> 00:41.940]  In fact, it's 2023.
[00:41.940 --> 00:42.940]  And we know it would happen.
[00:42.940 --> 00:43.940]  I mean, it's inevitable.
[00:43.940 --> 00:45.200]  It's going to happen.
[00:45.200 --> 00:49.440]  I hope you had a great holiday season, no matter what you celebrate or how much you
[00:49.440 --> 00:51.160]  celebrated or how you celebrated.
[00:51.160 --> 00:52.760]  I'm fine with that.
[00:52.760 --> 00:57.960]  But I also hope that charging into the new year was exciting as well.
[00:57.960 --> 00:59.160]  I have mixed emotions.
[00:59.160 --> 01:04.040]  I mean, I always kind of have mixed emotions, so I'm filled with mixed emotions now because,
[01:04.040 --> 01:11.840]  you know, I always set a pretty good intention, a resolution, if you will.
[01:11.840 --> 01:14.580]  But intention is probably a better word than resolution.
[01:14.580 --> 01:15.580]  And I've done it for years.
[01:15.580 --> 01:16.840]  I've talked about it with you guys for a year.
[01:16.840 --> 01:18.320]  I mean, we've been on this journey together.
[01:18.320 --> 01:22.280]  Remember the year I said yes to everything and the year I bought somebody lunch every
[01:22.280 --> 01:23.280]  day?
[01:23.280 --> 01:24.280]  You remember all this.
[01:24.280 --> 01:25.280]  It's all part of it.
[01:25.280 --> 01:26.280]  It's weird.
[01:26.280 --> 01:29.840]  This year, I don't have any specific intention.
[01:29.840 --> 01:36.440]  And I don't know if I've gotten lazy, overwhelmed by events, which could be, or if I'm going
[01:36.440 --> 01:39.080]  to sort of let the universe happen.
[01:39.080 --> 01:43.120]  Because I know, I mean, I learned totally this year that I have very little control
[01:43.120 --> 01:44.120]  over anything.
[01:44.120 --> 01:45.440]  But I kind of knew that before.
[01:45.440 --> 01:47.760]  I mean, that's not new knowledge.
[01:47.760 --> 01:49.000]  You probably figured that out as well.
[01:49.000 --> 01:53.800]  I mean, that's an important thing to learn at some point on the journey, that you're
[01:53.800 --> 01:55.880]  not really driving this thing.
[01:55.880 --> 01:58.200]  You're sort of along for the ride.
[01:58.200 --> 02:02.920]  And that's why, you know, adaptive behavior, which we talk about in creating safety and
[02:02.920 --> 02:08.560]  reliability, it's so vital because of the very same things you live and work under every
[02:08.560 --> 02:09.560]  single day.
[02:09.560 --> 02:12.160]  I mean, it's just how work happens.
[02:12.160 --> 02:14.720]  It's also how life happens.
[02:14.720 --> 02:18.800]  At least it's reminded us that over the last several years.
[02:18.800 --> 02:25.280]  It's weird how, if you think about it, how much the world changed and how much that's
[02:25.280 --> 02:30.000]  impacted us, how different we are now than when we were.
[02:30.000 --> 02:34.120]  And not to get too deep in it, because I'm pretty sure you could go pretty far down the
[02:34.120 --> 02:37.560]  tube on this, but it's just kind of different.
[02:37.560 --> 02:42.920]  And it shows up, at least for me, in the lack of a resolution.
[02:42.920 --> 02:46.760]  It's not that I'm going to do this year unintentional.
[02:46.760 --> 02:48.120]  I'm very intentional.
[02:48.120 --> 02:50.620]  I'll think of something, I'm sure.
[02:50.620 --> 02:56.040]  But I don't have the same spirit of challenge and adventure that I once would add to my
[02:56.040 --> 02:58.440]  life to make it exciting.
[02:58.440 --> 03:02.440]  Now it seems like I'm getting that excitement, whether I want it or not.
[03:02.440 --> 03:05.160]  I mean, it's just kind of a part of the journey.
[03:05.160 --> 03:07.680]  And you guys know exactly what I'm talking about.
[03:07.680 --> 03:11.840]  That's the cool part of what I'm talking about, is it's no mystery for you.
[03:11.840 --> 03:13.080]  You're on this journey.
[03:13.080 --> 03:15.560]  But it's an interesting one.
[03:15.560 --> 03:17.640]  It's pretty cold here.
[03:17.640 --> 03:18.640]  And cold's fine.
[03:18.640 --> 03:22.800]  I mean, and as Steve de Albuquerque told me once, you're never cold.
[03:22.800 --> 03:26.120]  You just don't have on the right equipment.
[03:26.120 --> 03:27.920]  So okay, that's a good way to look at it.
[03:27.920 --> 03:29.680]  Glass half full, glass half empty.
[03:29.680 --> 03:32.080]  He's an engineer, so glass half broken.
[03:32.080 --> 03:33.820]  Those all fit together.
[03:33.820 --> 03:40.600]  But in fact, I miss the long bike rides, because it's really cold.
[03:40.600 --> 03:45.480]  I mean, I could dress warmer and wear, you know, my Michelin Man suit and go out there
[03:45.480 --> 03:47.880]  and take on the world.
[03:47.880 --> 03:52.120]  But it's awfully warm to stay in and do work in.
[03:52.120 --> 03:53.120]  And there's tons of work.
[03:53.120 --> 03:56.760]  I don't know how you guys are feeling about that, but work's flying around like crazy
[03:56.760 --> 03:57.760]  as well.
[03:57.760 --> 04:01.680]  So I'm not getting the long periods of thoughtful time that I usually enjoy, because that's
[04:01.680 --> 04:04.640]  what I do, is I'll ride my bike and think about stuff.
[04:04.640 --> 04:09.240]  And weirdly, and I do mean this weirdly, I think a lot about what are we going to talk
[04:09.240 --> 04:11.480]  about on the podcast?
[04:11.480 --> 04:12.920]  What's interesting to me?
[04:12.920 --> 04:15.040]  And it's a kind of a good place to think about those things.
[04:15.040 --> 04:20.000]  I still think about them, but now it's not on a bicycle.
[04:20.000 --> 04:22.240]  Maybe I just have sort of cabin fever.
[04:22.240 --> 04:23.980]  That's also probably the case.
[04:23.980 --> 04:27.440]  And I'm waiting for spring because spring is, you know, as you know, if you've been
[04:27.440 --> 04:33.360]  on the journey, the time of year that I like the most, the whole rejuvenation, that all
[04:33.360 --> 04:35.240]  is a cool part.
[04:35.240 --> 04:37.760]  But until then, we still got more winter.
[04:37.760 --> 04:39.680]  It'll eventually spring or get here.
[04:39.680 --> 04:41.880]  I'm not worried that it's not going to make it.
[04:41.880 --> 04:44.760]  I just got a couple more months of not spring.
[04:44.760 --> 04:49.500]  For those of you listening in the other hemisphere, the good hemisphere at this time of year,
[04:49.500 --> 04:53.760]  and I'll say that I said that right out loud, I'm not embarrassed, right?
[04:53.760 --> 04:57.960]  You're enjoying the very weather I wish I were enjoying.
[04:57.960 --> 05:02.400]  So when that happens, take a little deep breath and enjoy it doubly for me.
[05:02.400 --> 05:06.320]  That'll be perfect because I can't wait to get on my bicycle and I can't wait to spend
[05:06.320 --> 05:08.840]  some time just riding around and looking at places.
[05:08.840 --> 05:10.120]  And so that's exciting.
[05:10.120 --> 05:12.960]  Everything's been replaced from the giant theft.
[05:12.960 --> 05:19.840]  So now I have more bicycles than I have since, which is, you know, pretty normal, I guess.
[05:19.840 --> 05:24.320]  I wouldn't say I have too many, because I don't know what too many means, but parking
[05:24.320 --> 05:25.320]  is a problem.
[05:25.320 --> 05:26.320]  Let's just say that.
[05:26.320 --> 05:27.760]  We'll just put that out there.
[05:27.760 --> 05:30.240]  And other than that, everything's going grand.
[05:30.240 --> 05:31.880]  What is your intention for the new year?
[05:31.880 --> 05:33.040]  What are you going to do?
[05:33.040 --> 05:35.880]  There are people out there definitely career change thinking.
[05:35.880 --> 05:36.880]  Good for you.
[05:36.880 --> 05:37.880]  Good on you.
[05:37.880 --> 05:40.120]  It's scary, but you should do it if it fits.
[05:40.120 --> 05:42.640]  There are people that have big plans for this year.
[05:42.640 --> 05:43.800]  I'm all about it.
[05:43.800 --> 05:50.520]  People that are sort of encroaching upon the twilight of their career, the latter part
[05:50.520 --> 05:51.520]  of their career.
[05:51.520 --> 05:53.440]  Good on you too.
[05:53.440 --> 05:59.240]  And then there's life, which just keeps slamming in and giving us uncertainty and creating
[05:59.240 --> 06:05.000]  opportunities for adaptations in a complex system.
[06:05.000 --> 06:06.000]  How'd that sound?
[06:06.000 --> 06:11.520]  Did that sound better than saying I'm frustrated that everything's uncertain and it's driving
[06:11.520 --> 06:12.520]  me bananas?
[06:12.520 --> 06:17.400]  Yeah, I think that does sound better, but it sort of leads us to the topic of the pod
[06:17.400 --> 06:21.400]  and the topic of the pod is pretty interesting.
[06:21.400 --> 06:27.800]  This one, because I've had lots of requests and actually lots of conversations that are
[06:27.800 --> 06:32.740]  mostly by necessity anonymous.
[06:32.740 --> 06:36.560]  So I thought about this and I thought I could record people in disguise, their voice, I
[06:36.560 --> 06:43.480]  could do, there's a million opportunities to handle this kind of need for anonymity.
[06:43.480 --> 06:48.240]  And I understand the need for anonymity more so now than I probably ever have before just
[06:48.240 --> 06:54.820]  because so much of what we talk about, there are whole industries that won't allow their
[06:54.820 --> 06:58.220]  workforce to have these conversations.
[06:58.220 --> 07:01.160]  And that's sort of frightening to me, but it's true.
[07:01.160 --> 07:02.720]  I mean, it's what happens.
[07:02.720 --> 07:09.800]  And I just did an incredibly cool podcast with Courtney Nash and Adrian Cockroft, which
[07:09.800 --> 07:10.920]  you're going to hear soon.
[07:10.920 --> 07:17.400]  You were going to hear it now, but it sort of got bumped up one because of the conversation
[07:17.400 --> 07:20.480]  that I've been asked to have with you today.
[07:20.480 --> 07:26.240]  And what's so crazy when you talk to these DevOps people is how, well, first of all,
[07:26.240 --> 07:29.520]  how incredibly similar what they do is to what we do.
[07:29.520 --> 07:35.920]  So that's amazing, but how much they think about the fact that every system is kind of
[07:35.920 --> 07:37.700]  broken.
[07:37.700 --> 07:39.840]  Every single system is kind of broken.
[07:39.840 --> 07:43.240]  There are little parts of it that are broken all the time.
[07:43.240 --> 07:50.080]  And what's interesting about that is I actually think that's a pretty important entrée, if
[07:50.080 --> 07:56.700]  I may use a little French, into what we need to talk about today because today we got some
[07:56.700 --> 08:10.320]  big stuff to talk about, and so we probably should actually have the conversation.
[08:10.320 --> 08:17.480]  So if you haven't followed it in the rest of the world, it's been really interesting
[08:17.480 --> 08:26.920]  to watch over the last couple weeks in the United States a phenomena that has taken place.
[08:26.920 --> 08:30.360]  I'm hesitant to use taken because that's past tense.
[08:30.360 --> 08:35.560]  I still think it is kind of taking place, but it's definitely the bulk of it seems to
[08:35.560 --> 08:37.400]  have happened.
[08:37.400 --> 08:44.920]  And that is the phenomena of an organization, a company, that I use quite a bit because
[08:44.920 --> 08:53.840]  I live in the American Southwest, and there's an airline, kind of one of the grandfathers
[08:53.840 --> 08:59.920]  of the hip cat cool budget airlines called Southwest.
[08:59.920 --> 09:09.240]  And for the most part, they've enjoyed a long reputation of being friendly and kind of irreverent
[09:09.240 --> 09:19.600]  and creative and humorous and on time and very disciplined to their schedules.
[09:19.600 --> 09:22.800]  And it's interesting because those of us that have flown them a while, and there's going
[09:22.800 --> 09:27.120]  to be a bunch of people on this podcast, I know because you all have called me, who flown
[09:27.120 --> 09:34.840]  them a long time, we've seen it kind of move from a scrappy little airline that did amazing
[09:34.840 --> 09:44.840]  things in a point to point aviation model to now a major carrier, not just in the United
[09:44.840 --> 09:48.880]  States, not just in North America, but in the world.
[09:48.880 --> 09:56.840]  And they've grown while desperately trying to be scrappy, creative and humorous.
[09:56.840 --> 10:00.520]  And it's funny because scrappy is very, very exciting.
[10:00.520 --> 10:04.080]  And it's a great way, I think, to start up a business.
[10:04.080 --> 10:09.640]  The challenge is, is that scrappy doesn't scale well.
[10:09.640 --> 10:15.240]  And that lack of scaling is a part of the Southwest story.
[10:15.240 --> 10:20.360]  Because if you didn't follow this story, and the rest of the world you may not have, there's
[10:20.360 --> 10:26.160]  a big story here, they pretty much collapsed.
[10:26.160 --> 10:33.720]  I wouldn't call it a complex failure, I would call it a chaotic failure.
[10:33.720 --> 10:42.280]  And in fact, what took place is the entire operational scheme that moved people in their
[10:42.280 --> 10:49.680]  planes from point to point throughout North America just stopped working.
[10:49.680 --> 10:58.560]  And it stopped at probably the least appropriate, probably the least convenient, maybe I should
[10:58.560 --> 11:04.080]  say it this way, the worst possible time it could happen right over the holidays when
[11:04.080 --> 11:11.720]  in fact lots of people are traveling and when in fact the travel is more meaningful to the
[11:11.720 --> 11:13.560]  people on the trip.
[11:13.560 --> 11:17.560]  We're going to see family, we're going to see mom and dad, we're going to see grandkids,
[11:17.560 --> 11:20.280]  we're going to go to weddings, we're going to do things.
[11:20.280 --> 11:26.360]  And so it's a really important time to travel, and it's a really busy time to travel.
[11:26.360 --> 11:33.440]  And that December is a month full of weather is kind of a part of this story as well.
[11:33.440 --> 11:39.760]  Because what happened is, is there was a storm that blew up really across kind of the northern
[11:39.760 --> 11:45.880]  Midwest of the United States, and it sort of moved along and it dramatically impacted
[11:45.880 --> 11:51.600]  operations in Denver and it dramatically impacted operations in Chicago.
[11:51.600 --> 11:54.800]  And because they're a point to point airline, and you understand that point to point means
[11:54.800 --> 12:00.700]  they're not a hub and spoke, so they don't have these major hubs that every plane flies
[12:00.700 --> 12:05.640]  in and out of and then redistributes you to every other plane, like many airlines have
[12:05.640 --> 12:10.960]  around the world, they actually fly from point A to point B.
[12:10.960 --> 12:17.000]  And for the most part, many people can do a nonstop budget airline flight on Southwest
[12:17.000 --> 12:21.020]  from where they are to where they need to go, and it works pretty beautifully.
[12:21.020 --> 12:29.160]  The challenge is, in a point to point, is that a delay has the linear ability to domino
[12:29.160 --> 12:34.380]  that failure throughout the entire stinking day.
[12:34.380 --> 12:38.800]  So it's a complex system, but operationally it's done in a linear fashion.
[12:38.800 --> 12:46.700]  So if the first exchange, the first turnover is 40 minutes late, they're going to spend
[12:46.700 --> 12:51.360]  the rest of the day either 40 minutes late or working desperately to try to make up those
[12:51.360 --> 12:54.080]  40 minutes, and oftentimes it doesn't work.
[12:54.080 --> 12:59.960]  So there's clearly a structural weakness in their system.
[12:59.960 --> 13:06.040]  Or as Adrian Cockroft and Courtney Nash would say, part of their system is always a little
[13:06.040 --> 13:07.040]  broken someplace.
[13:07.040 --> 13:13.080]  And I actually think that's a really fair thing to say about Southwest Airlines.
[13:13.080 --> 13:21.360]  What's interesting is this balance between robust and resilient.
[13:21.360 --> 13:25.120]  Because if you think about what happened to Southwest, Southwest is a very, very, very
[13:25.120 --> 13:27.140]  effective company.
[13:27.140 --> 13:34.320]  And normally, they move a lot of people in a linear fashion, effectively on time, within
[13:34.320 --> 13:42.280]  schedule, and people get to their special occasions, mom's birthday, the wedding, Christmas
[13:42.280 --> 13:44.600]  dinner, whatever the case is.
[13:44.600 --> 13:54.140]  And so normally, it's a really good system that works really well until it doesn't.
[13:54.140 --> 13:59.880]  And the challenge is that robust is always important.
[13:59.880 --> 14:01.580]  Robust matters.
[14:01.580 --> 14:03.580]  Robust is how we do business.
[14:03.580 --> 14:10.480]  And as a pilot associate of mine told me, and then I read later in an additional write-up
[14:10.480 --> 14:15.440]  by a very senior pilot in Southwest Airlines, everybody kind of anonymous in this part of
[14:15.440 --> 14:22.400]  the story, if shareholders are making money, the system must be working well.
[14:22.400 --> 14:25.160]  Now, I want you to hold on to that idea.
[14:25.160 --> 14:30.760]  If the shareholders are making money, the system must be working well.
[14:30.760 --> 14:35.880]  Not that that's a bad thing, but it's going to be a really important part of this relationship
[14:35.880 --> 14:40.120]  between robust and resilient.
[14:40.120 --> 14:42.360]  Because here's what I'll tell you.
[14:42.360 --> 14:44.000]  Robust matters.
[14:44.000 --> 14:47.920]  It's what gives the shareholder value its value.
[14:47.920 --> 14:50.640]  But resilience is what saves you.
[14:50.640 --> 14:59.200]  And a very robust system with very little resilience is a system that when it fails,
[14:59.200 --> 15:07.460]  it has the potential to fail catastrophically, or chaotically, or horribly.
[15:07.460 --> 15:14.400]  And that's exactly what you saw with Southwest, is this very, very, very stable but brittle
[15:14.400 --> 15:25.880]  system that when tested and when a multitude of conditions coexisted, lacked the ability
[15:25.880 --> 15:26.880]  to recover.
[15:26.880 --> 15:36.800]  And it has a lot to do with the fact that there was a strong push when they were scrappy
[15:36.800 --> 15:42.500]  to focus on operations, to listen to the people, because the people knew.
[15:42.500 --> 15:49.200]  And the early leaders of Southwest Airlines really were operational people who moved into
[15:49.200 --> 15:52.600]  the leadership suite.
[15:52.600 --> 16:00.520]  But over time, and with retirements, and as the priorities of the organization change,
[16:00.520 --> 16:08.560]  not because the service model changes, but because funding and finances drive an entirely
[16:08.560 --> 16:12.160]  new set of requirements into the equation.
[16:12.160 --> 16:18.600]  What happens is the operations people are often replaced with organizational people,
[16:18.600 --> 16:22.240]  with finance people.
[16:22.240 --> 16:30.280]  And then what you have is a scrappy airline that got used to solving problems based upon
[16:30.280 --> 16:35.940]  operational expertise in real time, and could actually handle weak systems quite well, because
[16:35.940 --> 16:40.120]  weak systems fail a lot, hence the name weak systems.
[16:40.120 --> 16:46.160]  What you had was a system that lost its operational expertise.
[16:46.160 --> 16:51.280]  And I might go farther in saying, although this is my opinion, so balance it for what
[16:51.280 --> 16:56.200]  you will, they also lost that operational feedback loop.
[16:56.200 --> 17:02.800]  And with the absence of the operational feedback loop, what they counted on was the financial
[17:02.800 --> 17:05.240]  feedback loop.
[17:05.240 --> 17:11.760]  And they assumed that the financial feedback loop actually was giving them operational
[17:11.760 --> 17:14.920]  information.
[17:14.920 --> 17:21.040]  If you want to know how work is done, you don't talk to the accountants.
[17:21.040 --> 17:23.480]  You talk to the workers.
[17:23.480 --> 17:29.640]  And yet, over time, and I bet this drift was natural and slow, and probably hard to notice,
[17:29.640 --> 17:32.480]  and probably felt like the right thing to do.
[17:32.480 --> 17:40.200]  But over time, what happened is the accountant voice became the only voice in the room.
[17:40.200 --> 17:43.120]  Now that's unfair.
[17:43.120 --> 17:51.220]  And I'm making a wide stereotypical sweeping comment, but let's say it a little more fairly.
[17:51.220 --> 18:00.980]  The voice of finances outweighed both in volume and credibility and importance, the voice
[18:00.980 --> 18:03.240]  of operations.
[18:03.240 --> 18:12.160]  And so what you saw, believe it or not, was a significant lack in investment on the operational
[18:12.160 --> 18:15.280]  side of Southwest Airlines.
[18:15.280 --> 18:19.840]  And they didn't replace, for instance, their cruise scheduling software, which is a very
[18:19.840 --> 18:22.880]  complex process in a good day.
[18:22.880 --> 18:26.840]  It's very difficult to do with a small airline.
[18:26.840 --> 18:32.680]  It is incredibly complex when the system is stressed, and it's only made more complex
[18:32.680 --> 18:35.280]  by the size of the airline.
[18:35.280 --> 18:43.860]  They didn't really replace that software system since its inception in 1990.
[18:43.860 --> 18:48.900]  So here's what we know, and all of our friends in DevOps would help us with this.
[18:48.900 --> 18:51.680]  That's a system that's running to failure.
[18:51.680 --> 18:53.200]  And guess what?
[18:53.200 --> 18:56.240]  It ran to failure.
[18:56.240 --> 19:05.280]  Now the lesson for us in this, I think is probably written in what they had to do in
[19:05.280 --> 19:09.080]  order to stop the bleeding.
[19:09.080 --> 19:14.080]  Because they lacked resilience, so they really didn't have recoverability.
[19:14.080 --> 19:21.800]  And so what they ended up doing was canceling thousands of flights every day for an entire
[19:21.800 --> 19:26.700]  series of days, which included the entire holiday break.
[19:26.700 --> 19:28.040]  They just canceled all the flights.
[19:28.040 --> 19:33.120]  And the reason they canceled the flights is because they had to sort of draw a line operationally
[19:33.120 --> 19:35.280]  and start again.
[19:35.280 --> 19:39.480]  They stopped flying so they could figure out where all the equipment was and where all
[19:39.480 --> 19:43.440]  the people were, because they didn't know either of those things.
[19:43.440 --> 19:48.440]  And by people, I mean flight attendants and pilots.
[19:48.440 --> 19:55.800]  And they had to start again in their linear point to point system and move equipment and
[19:55.800 --> 20:02.940]  people to where that equipment and people needed to be to start again from scratch.
[20:02.940 --> 20:10.900]  Now that is a lesson for us, because that is an organization that so significantly lacked
[20:10.900 --> 20:20.680]  resilience in the system, that their only reasonable response, and I hesitated that
[20:20.680 --> 20:24.160]  reasonable because I don't even know what word to use, the only response they could
[20:24.160 --> 20:30.760]  figure out that made sense to them to use was to stop operations completely.
[20:30.760 --> 20:33.120]  Think about this manufacturing people.
[20:33.120 --> 20:40.000]  Think about this oil and gas people to stop operations completely so they could reset
[20:40.000 --> 20:49.680]  and start again under the system restrictions that got them in trouble in the first place.
[20:49.680 --> 20:57.000]  Blame is not worth discussing because this isn't one person.
[20:57.000 --> 21:00.860]  This isn't one category.
[21:00.860 --> 21:11.240]  This is an organization that became very convinced that shareholder value was a good monitoring
[21:11.240 --> 21:19.440]  device for operational work and it existed up and down the organization.
[21:19.440 --> 21:25.320]  There's room enough to include everybody in the responsibility and culpability for this
[21:25.320 --> 21:27.280]  failure.
[21:27.280 --> 21:34.560]  What's interesting is the recoverability for this is incredibly expensive and so there's
[21:34.560 --> 21:40.080]  damage to the organization, to Southwest Airlines and its brand, which is really important and
[21:40.080 --> 21:45.480]  I'm pretty willing to bet, I would love for you guys to tell me and you will, I know,
[21:45.480 --> 21:47.320]  I kind of think their brand will make it.
[21:47.320 --> 21:48.880]  I think the brand is recoverable.
[21:48.880 --> 21:52.720]  I don't think people are mad at the brand.
[21:52.720 --> 21:59.600]  The challenge is, is will they choose to do nothing?
[21:59.600 --> 22:04.800]  Because by resetting, it allowed them the, what's the word, luxury, is it luxury the
[22:04.800 --> 22:05.800]  right word?
[22:05.800 --> 22:10.200]  It allowed them the ability to go back to the way they were doing business and restart
[22:10.200 --> 22:13.400]  business using the systems they had.
[22:13.400 --> 22:21.240]  Now I'll grant you, that's what they needed to do, I mean, that's what they had to do.
[22:21.240 --> 22:24.080]  What will they learn from this?
[22:24.080 --> 22:32.680]  And is the ability to learn built in to a system that's lost contact with the operational
[22:32.680 --> 22:33.720]  feedback loop?
[22:33.720 --> 22:39.320]  Because this happened earlier to Southwest, maybe you guys helped me remember, a couple
[22:39.320 --> 22:44.080]  years ago, two, three years ago, maybe four years ago, kind of the same thing happened.
[22:44.080 --> 22:49.640]  Not as significant because it didn't happen at, it didn't happen over Christmas and not
[22:49.640 --> 22:53.080]  as significant because the entire system didn't buckle.
[22:53.080 --> 22:58.720]  But they got several days where they had to cancel a majority of their flights.
[22:58.720 --> 23:04.440]  And it strikes me from the outside looking in that that was not a teachable moment for
[23:04.440 --> 23:05.440]  them.
[23:05.440 --> 23:10.840]  They didn't actually take that failure and learn from it, which is my big fear with blame.
[23:10.840 --> 23:15.360]  I mean, you can blame whoever you want to, blame the CEO, but they're all really a part
[23:15.360 --> 23:17.520]  of this failure.
[23:17.520 --> 23:24.480]  The challenge is, is that if you spend time blaming, then you're not spending time learning.
[23:24.480 --> 23:28.380]  And the learning part of this, which is, I think, the most important part of this lesson.
[23:28.380 --> 23:33.320]  And the part that I think will most likely save their brand is to come out and say, here's
[23:33.320 --> 23:35.840]  what we learned.
[23:35.840 --> 23:37.840]  Here's what failed.
[23:37.840 --> 23:40.880]  Here's what we learned about what failed.
[23:40.880 --> 23:43.520]  Here's how we chose to improve.
[23:43.520 --> 23:48.280]  And here's how we put resilience in the system so that when it fails again, and it will,
[23:48.280 --> 23:53.700]  because Adrian and Courtney have taught me, every system is a little bit broken.
[23:53.700 --> 23:58.480]  When it fails again, here's how we're going to recover.
[23:58.480 --> 24:00.280]  I mean, we talk about this all the time.
[24:00.280 --> 24:05.280]  You get the failure you have, or you have the opportunity to get a failure you want
[24:05.280 --> 24:06.720]  to have.
[24:06.720 --> 24:12.560]  And my guess is everybody in leadership from top to bottom in Southwest Airlines did not
[24:12.560 --> 24:15.440]  want the failure they got.
[24:15.440 --> 24:22.240]  They would much rather have created a much different failure modality that something
[24:22.240 --> 24:26.680]  would have snapped, something would have broken, a flight would have been canceled, but it
[24:26.680 --> 24:34.040]  wouldn't have significant impact to the point of resetting what they do in order to improve.
[24:34.040 --> 24:35.920]  That's our challenge.
[24:35.920 --> 24:38.540]  And that's where we live on this one.
[24:38.540 --> 24:46.840]  And the opportunity to share this with you, to have this conversation, is my pleasure.
[24:46.840 --> 24:50.480]  Only because it gave me an opportunity to talk to a whole lot of people who do this
[24:50.480 --> 24:55.760]  for a living in this industry, and they had so much to share.
[24:55.760 --> 25:01.560]  If you get a chance, talk to your friends about this failure, talk to your coworkers
[25:01.560 --> 25:05.800]  and operations people about this failure, and talk to your leadership team about this
[25:05.800 --> 25:13.880]  failure and ask them this question, what happened and how did it happen?
[25:13.880 --> 25:27.080]  And see where they go with this, because that creates that opportunity to learn.
[25:27.080 --> 25:29.400]  What do you think?
[25:29.400 --> 25:31.560]  That's the pod for the new year, first one.
[25:31.560 --> 25:32.560]  What do you think?
[25:32.560 --> 25:36.480]  Everybody's here, and man, there's so much more to talk about, and I want to thank everybody
[25:36.480 --> 25:40.400]  who contacted me, and so many people did to talk to me about this.
[25:40.400 --> 25:43.440]  And there's a lot of stuff, I mean, I took notes and everything.
[25:43.440 --> 25:46.340]  There's a lot of stuff I left out of this conversation.
[25:46.340 --> 25:51.360]  But the big one to me, if you ask me to think about the two things that really struck me
[25:51.360 --> 25:57.920]  as poignant, at least from my vantage point, talking to people about the Southwest failure,
[25:57.920 --> 26:03.440]  it was this idea that if shareholder value was good, operations must therefore automatically
[26:03.440 --> 26:04.440]  be good.
[26:04.440 --> 26:06.360]  That was a big thing.
[26:06.360 --> 26:09.080]  I mean, that kind of stopped me in my tracks a little bit.
[26:09.080 --> 26:12.920]  I mean, when you say it like we're saying it now, it doesn't sound that interesting,
[26:12.920 --> 26:15.600]  but it's very interesting to hear them say that.
[26:15.600 --> 26:22.060]  And then the other thing was, quite honestly, their lack of recoverability in their system.
[26:22.060 --> 26:26.180]  And I'm not talking about their point to point system, which all absolutely lacks the ability
[26:26.180 --> 26:27.180]  to recover.
[26:27.180 --> 26:28.180]  It really does.
[26:28.180 --> 26:30.920]  I mean, and that's one they should focus on dramatically.
[26:30.920 --> 26:37.440]  But like in their crew scheduling system and their crew communication systems, they just
[26:37.440 --> 26:39.060]  didn't have resilience there.
[26:39.060 --> 26:41.340]  And that's remarkable to me.
[26:41.340 --> 26:44.160]  And quite honestly, those are things I'd look at.
[26:44.160 --> 26:46.160]  The opportunity to learn is great, guys.
[26:46.160 --> 26:47.980]  Don't screw this up, Southwest.
[26:47.980 --> 26:51.280]  If you do, you sort of deserve what happened to you.
[26:51.280 --> 26:52.280]  You need to learn.
[26:52.280 --> 26:53.280]  Learning is everything.
[26:53.280 --> 26:54.640]  Learning is vital.
[26:54.640 --> 27:01.640]  And this failure is either a giant cost to the business or a giant opportunity to improve.
[27:01.640 --> 27:05.720]  And my advice to you is I would try to spend the money towards getting better.
[27:05.720 --> 27:07.240]  That's what I try to do.
[27:07.240 --> 27:09.640]  But until then, my friends, that is the pod for today.
[27:09.640 --> 27:11.800]  I hope you enjoyed it immensely.
[27:11.800 --> 27:14.440]  I enjoyed spending time with you.
[27:14.440 --> 27:16.200]  Learn something new every single day.
[27:16.200 --> 27:18.360]  Have as much fun as you possibly can.
[27:18.360 --> 27:20.640]  Be kind to each other because that's important.
[27:20.640 --> 27:21.720]  Check in on one another.
[27:21.720 --> 27:32.120]  And for goodness sakes, you guys, be safe.
```

## References

- Audio transcribed using [OpenAI Whisper](https://github.com/openai/whisper)
