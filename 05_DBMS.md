# DBMS — Topic Map

> **Block E** (with Linux). Sweep-heavy — DeliverIQ already uses transactions,
> row locking, and Postgres indexing in anger, so much of this is naming and
> sharpening what is already half-known.
>
> **Overlap note:** [SQL.md](SQL.md) already covers query writing in depth
> (SELECT → aggregation → joins → subqueries → window functions). Do **not**
> re-teach syntax here. This file is the *theory* round — "explain X" — plus the
> internals SQL.md does not reach: index structures, isolation levels, MVCC,
> locking, CAP.
> Legend and fill-in rule: [00_PLAN.md](00_PLAN.md).

---

# Tier 1 — Foundations

- [ ] Relational model: tables, rows, columns, domains
- [ ] Keys: primary, foreign, candidate, composite, super
- [ ] **[HOT]** ACID — all four letters with a concrete example each
- [ ] DDL vs DML vs DCL vs TCL
- [ ] ER modelling; cardinality

---

# Tier 2 — Core

## Design

- [ ] **[HOT]** Normalization: 1NF, 2NF, 3NF, BCNF — what anomaly each removes
- [ ] **[HOT]** When to deliberately denormalize

## Querying *(syntax lives in [SQL.md](SQL.md) — theory only here)*

- [ ] `SELECT`, `WHERE`, `GROUP BY`, `HAVING`, `ORDER BY`, `LIMIT`
- [ ] **[HOT]** All join types — inner, left, right, full, cross, self
- [ ] Subqueries vs joins; correlated subqueries
- [ ] Aggregate functions; window functions

## Indexing

- [ ] **[HOT]** B-tree vs B+ tree structure, and why B+ trees for databases
- [ ] Clustered vs non-clustered indexes
- [ ] Composite indexes; covering indexes
- [ ] **[HOT]** When an index *hurts* — write amplification, low-cardinality columns
- [ ] Query execution plans; `EXPLAIN`

## Transactions and concurrency

- [ ] **[HOT]** Isolation levels: read uncommitted, read committed, repeatable read, serializable
- [ ] **[HOT]** The anomalies each level permits: dirty read, non-repeatable read, phantom read
- [ ] Locking: shared vs exclusive, row vs table; two-phase locking
- [ ] Optimistic vs pessimistic concurrency control
- [ ] **[HOT]** `SELECT FOR UPDATE` and `SKIP LOCKED` — used directly in DeliverIQ

---

# Tier 3 — Advanced

- [ ] **[HOT]** SQL vs NoSQL — when each, and why
- [ ] Document, key-value, column-family, and graph stores
- [ ] **[HOT]** CAP theorem, and the trap follow-up: *"does CAP apply to a single-node SQL database?"* (no — partitions are the premise)
- [ ] Sharding and partitioning strategies
- [ ] Replication: leader-follower, multi-leader, leaderless
- [ ] Write-ahead log and crash recovery
- [ ] MVCC — how Postgres avoids read locks
- [ ] Connection pooling
- [ ] Deadlock detection in databases

---

## Resources

- *Database System Concepts* (Korth).
- **Use The Index, Luke** (use-the-index-luke.com) — the best free indexing resource.
- HackerRank SQL track / LeetCode Top SQL 50 — query-writing practice (see [SQL.md](SQL.md)).

## Block G drill list — every [HOT] in this file

- [ ] ACID, one concrete example per letter
- [ ] Normalization 1NF → BCNF, and the anomaly each kills
- [ ] When to denormalize on purpose
- [ ] Every join type, from memory
- [ ] Why B+ trees and not B-trees
- [ ] When an index hurts
- [ ] Four isolation levels ↔ three anomalies, mapped both directions
- [ ] `SELECT FOR UPDATE SKIP LOCKED` — what it does and what it trades away
- [ ] SQL vs NoSQL
- [ ] CAP, plus the single-node follow-up
