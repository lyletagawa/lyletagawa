---
title: "Materialized Views Aren't Snapshots"
date: 2026-07-12
publishdate: 2026-07-12
lastmod: 2026-07-12
summary: "Postgres, BigQuery, and ClickHouse all offer something called a materialized view. Only one behaves like the stored snapshot most engineers expect, and assuming otherwise can leave a table quietly, permanently empty."
tags: ["databases", "systems", "performance"]
image: /images/materialized-views-arent-snapshots.jpg
draft: true
---

![Postgres, BigQuery, and ClickHouse all offer something called a materialized view. Only one behaves like the stored snapshot most engineers expect, and assuming otherwise can leave a table quietly, permanently empty.](/images/materialized-views-arent-snapshots.jpg)
*A vintage Kodak Brownie Hawkeye camera. Oracle called this database feature a "snapshot" before renaming it "materialized view" in the late 1990s. Photo: [Joe Haupt (2017)](https://commons.wikimedia.org/wiki/File:Vintage_Kodak_Brownie_Hawkeye_Camera_Flash_Outfit,_No._177K,_Made_In_USA,_Circa_1950s_(35681386515).jpg). CC BY-SA 2.0.*

## Materialized Views Aren't Snapshots

Create a materialized view over a ClickHouse table that already holds a year of data, then query it. The result set comes back empty, not stale, not partial. Just empty. The view has been running since the moment you created it, and it has faithfully ignored every row that existed before that moment{{< cite 1 "Incremental materialized view. ClickHouse Docs." >}}. Multiply that gap by every dashboard built on top of the view, and one wrong assumption about what "materialized" means quietly breaks a quarter's worth of reporting.

That's not a bug. ClickHouse's materialized view isn't a stored copy of a query result at all. It's an insert trigger. Every time a block of rows lands in the source table, the view's query runs against just that block, and the result gets appended to a separate target table{{< cite 1 "Incremental materialized view. ClickHouse Docs." >}}. Rows that existed before the view was created never pass through the trigger, because the trigger didn't exist yet either.

## What Is a Materialized View?

Oracle shipped the underlying idea first, in the early 1990s, under a different name. Oracle7 introduced "snapshots," tables that stored the result of a query against a remote database and refreshed on a schedule.

The general problem got a formal name a few years later. Ashish Gupta and Inderpal Singh Mumick's 1995 survey defined a materialized view as any stored, precomputed query result that a system keeps synchronized with the data it was built from, and cataloged the tradeoffs involved in keeping that synchronization efficient{{< cite 2 "Gupta, Ashish, and Inderpal Singh Mumick (1995). Maintenance of Materialized Views: Problems, Techniques, and Applications. IEEE Data Engineering Bulletin, 18(2), 3-18." >}}. That definition is deliberately silent on mechanism. It says nothing about whether the sync happens continuously, on a schedule, or by hand, which turns out to be exactly where the disagreement lives.

Gupta and Mumick split the maintenance problem into two broad strategies: incremental maintenance, which updates a view using only the changes to the underlying data, and full recomputation, which reruns the whole query from scratch{{< cite 2 "Gupta, Ashish, and Inderpal Singh Mumick (1995). Maintenance of Materialized Views: Problems, Techniques, and Applications. IEEE Data Engineering Bulletin, 18(2), 3-18." >}}. Three decades later, every system in this post is still just picking a point on that same spectrum.

Oracle's own terminology caught up a few years after that. By Oracle8i, in the late 1990s, Oracle's documentation was using "snapshot" and "materialized view" interchangeably, and the newer term stuck{{< cite 3 "Snapshot Concepts and Architecture. Oracle8i Replication Release 2 (8.1.6)." >}}.

## Where This Shows Up Today

Postgres sticks closest to the original snapshot model. A materialized view is a real, stored table, and it stays exactly as stale as it was the moment you created it until you explicitly run REFRESH MATERIALIZED VIEW{{< cite 4 "REFRESH MATERIALIZED VIEW. PostgreSQL Documentation." >}}. Nothing refreshes it on its own. No scheduler ships with Postgres to do that for you.

BigQuery automates the same model instead of replacing it. It refreshes materialized views incrementally in the background, reprocessing only the rows that changed, and can rewrite a plain query against the base table to use the materialized view automatically when the results match{{< cite 5 "Introduction to materialized views. BigQuery Documentation." >}}. That rewriting happens transparently. A team can adopt a materialized view without touching a line of application code, and only notice it exists by checking the query plan. The mental model is still a stored snapshot of a query. BigQuery just keeps the snapshot current for you and hides the wiring.

## Common Mistakes

**Expecting a ClickHouse view to backfill on creation.** The view only sees rows inserted after it exists. If the source table already had data, that data needs a manual backfill, typically a one-time INSERT INTO ... SELECT run by hand{{< cite 1 "Incremental materialized view. ClickHouse Docs." >}}.

**Assuming any materialized view refreshes itself.** Postgres won't touch a materialized view again until something explicitly tells it to. Teams that skip building that scheduler discover their dashboard is showing old numbers{{< cite 4 "REFRESH MATERIALIZED VIEW. PostgreSQL Documentation." >}}.

**Confusing ClickHouse's two materialized view types.** Incremental views are insert triggers, cheap and continuous. Refreshable views recompute the entire query on a schedule instead, closer to what Postgres does{{< cite 6 "Refreshable materialized view. ClickHouse Docs." >}}. Refreshable views exist because some queries, complex joins especially, don't decompose cleanly into per-block updates. Reaching for the wrong type isn't a style choice. It changes both your data freshness and your infrastructure bill.

**Refreshing without planning for the lock.** A plain REFRESH MATERIALIZED VIEW blocks reads while it runs. The CONCURRENTLY option avoids that, but it requires a UNIQUE index on the view, a requirement most teams discover only after the first production refresh stalls a dashboard{{< cite 4 "REFRESH MATERIALIZED VIEW. PostgreSQL Documentation." >}}.

## Put It Into Practice

Before you build anything on a materialized view, ask what actually keeps it in sync. Not what the vendor calls it. What mechanism moves data into it, and on what trigger.

If you're moving between systems, don't assume the word travels with the behavior. A materialized view that works one way in Postgres can work a completely different way in the next database you touch, same name, different contract.

The feature earns its keep in every one of these systems. It just isn't the same feature twice.

## Go Further

**The original survey.** Gupta and Mumick's 1995 paper is dense but readable, and lays out the maintenance problem in general terms before any specific database had picked a specific answer{{< cite 2 "Gupta, Ashish, and Inderpal Singh Mumick (1995). Maintenance of Materialized Views: Problems, Techniques, and Applications. IEEE Data Engineering Bulletin, 18(2), 3-18." >}}.

**Materialize.** A streaming database built specifically to keep materialized views continuously correct as data changes, using differential dataflow instead of either insert triggers or scheduled recomputation{{< cite 7 "Materialize Documentation." >}}.

---

## References

<ol class="references">
  <li id="ref-1">"Incremental materialized view." <em>ClickHouse Docs</em>. <a href="https://clickhouse.com/docs/materialized-view/incremental-materialized-view">https://clickhouse.com/docs/materialized-view/incremental-materialized-view</a></li>
  <li id="ref-2">Gupta, Ashish, and Inderpal Singh Mumick (1995). "Maintenance of Materialized Views: Problems, Techniques, and Applications." <em>IEEE Data Engineering Bulletin</em>, 18(2): 3-18. <a href="http://sites.computer.org/debull/95JUN-CD.pdf">http://sites.computer.org/debull/95JUN-CD.pdf</a></li>
  <li id="ref-3">"Snapshot Concepts and Architecture." <em>Oracle8i Replication Release 2 (8.1.6)</em>. <a href="https://docs.oracle.com/cd/A81042_01/DOC/server.816/a76959/mview.htm">https://docs.oracle.com/cd/A81042_01/DOC/server.816/a76959/mview.htm</a></li>
  <li id="ref-4">"REFRESH MATERIALIZED VIEW." <em>PostgreSQL Documentation</em>. <a href="https://www.postgresql.org/docs/current/sql-refreshmaterializedview.html">https://www.postgresql.org/docs/current/sql-refreshmaterializedview.html</a></li>
  <li id="ref-5">"Introduction to materialized views." <em>BigQuery Documentation</em>. <a href="https://docs.cloud.google.com/bigquery/docs/materialized-views-intro">https://docs.cloud.google.com/bigquery/docs/materialized-views-intro</a></li>
  <li id="ref-6">"Refreshable materialized view." <em>ClickHouse Docs</em>. <a href="https://clickhouse.com/docs/materialized-view/refreshable-materialized-view">https://clickhouse.com/docs/materialized-view/refreshable-materialized-view</a></li>
  <li id="ref-7">"Materialize Documentation." <a href="https://materialize.com/docs/">https://materialize.com/docs/</a></li>
</ol>

---

## Outtakes

**"Materialized view" is a contradiction in terms, according to the man who wrote the relational database dictionary.** C.J. Date argues views are virtual by definition, so a "materialized view" is really just a snapshot wearing a nicer name ([Date, 2008](https://archive.org/details/relationaldataba0000date_a8w3)).

**ClickHouse's own docs talk you out of using its backfill shortcut.** The POPULATE keyword can backfill a view at creation time, but any row inserted into the source table while it's running gets silently dropped, so ClickHouse recommends a manual backfill instead ([ClickHouse Docs](https://clickhouse.com/docs/sql-reference/statements/create/view)).

**Postgres didn't get materialized views until 2013.** CREATE MATERIALIZED VIEW arrived in PostgreSQL 9.3, decades after Oracle shipped the equivalent feature as "snapshots" in the early 1990s ([PostgreSQL 9.3 Release Notes](https://www.postgresql.org/docs/9.3/release-9-3.html)).

---

## Changelog

**2026-07-12** Initial release.
