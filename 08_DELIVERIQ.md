# DeliverIQ — Project Defence

> **Block F** (with [07_DISTRIBUTED.md](07_DISTRIBUTED.md)). Not a subject — a
> deliverable. The project is complete and no longer being developed, so nothing
> here is a to-do list for code. It is a to-do list for *what can be said about
> the code, accurately, under pressure*.
>
> **Governing rule:** the honest gaps in §0 of [CSE_CORE.md](CSE_CORE.md) are
> **volunteered, never hidden**. A self-reported flaw reads as engineering
> judgement; the same flaw extracted by an interviewer reads as either not
> knowing or not saying. The O(log n) correction and the missing outbox are the
> two that most improve credibility when self-reported.
>
> Legend and fill-in rule: [00_PLAN.md](00_PLAN.md).

---

## The re-pitch — reframe from food delivery to distributed systems

- [ ] The opener, delivered in one breath:

  > "I built a distributed system where three stateless replicas contend for
  > shared state. The interesting parts were concurrency correctness — a
  > double-dispatch race I fixed with row-level locking using
  > `SELECT FOR UPDATE SKIP LOCKED` — and the consistency gap between my
  > database and my event log, which is the dual-write problem."

- [ ] Why this opener works — it names a race and a known named problem in the
      first fifteen seconds, and hands the interviewer two threads to pull

---

## Talking points that map to Nutanix's domain

- [ ] **Two-phase claim** — acquire all locks before mutating anything, so a failed rider claim never loses the order
- [ ] **`SKIP LOCKED`** — trades strict ordering for throughput; losers skip to the next row instead of blocking
- [ ] **Redis is advisory, Postgres is authoritative** — a stale index entry costs a wasted attempt, never a wrong outcome
- [ ] **Kafka fan-out** to independent consumer groups with per-group offsets; at-least-once plus idempotent consumers
- [ ] **Fail-open on Redis outage** — availability over protection, deliberately chosen
- [ ] **Verified concurrency** — 15 concurrent dispatches across 3 replicas: 10 succeeded, 5 correctly rejected, zero duplicate rider or order IDs

---

## The honest gaps — each needs a *stated fix*, not just an admission

Admitting a flaw without knowing its remedy is half an answer. Each box below is
`[x]` only when both the flaw and the fix can be stated cleanly.

- [ ] **Scheduler complexity** — resume says O(log n); it is actually **O(n log n) per dispatch**, because every call reloads all pending orders and rebuilds the heap. *(Fix: persistent heap / indexed priority queue, incremental updates.)*
- [ ] **Benchmark does not measure what it claims** — ~123 RPS / p99 220 ms from Locust, but that test only hits unauthenticated `POST /orders`, so it measures a plain INSERT — not matching, not locking, not Kafka.
- [ ] **Forgeable admin auth** — JWT secret is the hardcoded dev default in the deployed instance.
- [ ] **No transactional outbox** — a crash between `db.commit()` and `publish_event()` loses the event permanently. *This is the dual-write problem, by name.*
- [ ] **Durability theatre** — single Kafka broker, RF=1, so `acks=all` provides no real durability.
- [ ] **Rate limiter bypassable** — by rotating a client-supplied `X-API-Key` header.
- [ ] **Test gaps** — Kafka is mocked; the concurrency test is manual, not in CI.

---

## Questions to ask at the end

- [ ] **Conceptual, domain-grounded:**

  > "Since the storage fabric replicates data across nodes, how do you detect and
  > repair replica divergence — is it Merkle-tree comparison like Dynamo-style
  > systems, or something driven by the write path?"

- [ ] **If the conversation was OS-flavoured:**

  > "Where does the line sit between AHV and the storage layer — how much of the
  > data path runs in the hypervisor versus in a user-space service?"

- [ ] **Role-oriented:**

  > "For an intern joining an aggregate posting like this, how does team
  > allocation actually work — matched to interest, or driven by where the need is?"

---

## Block G drill list

- [ ] Deliver the opener cold, unprompted, in under twenty seconds
- [ ] Volunteer the O(log n) correction without being asked
- [ ] Volunteer the missing outbox without being asked
- [ ] Defend `SKIP LOCKED` — including what it costs
- [ ] Answer "how would you make this production-ready?" using the gaps list as the answer
