# DeliverIQ — Project Defence

> **Block F** (with [07_DISTRIBUTED.md](07_DISTRIBUTED.md)). Not a subject — a
> deliverable. The project is complete and no longer being developed, so nothing
> here is a to-do list for code. It is a to-do list for *what can be said about
> the code, accurately, under pressure*.
>
> Legend and fill-in rule: [00_PLAN.md](00_PLAN.md).

---

## The source of depth — one file, and it does not live here

**[`deliveriq/docs/INTERVIEW_PREP.md`](../deliveriq/docs/INTERVIEW_PREP.md)**

That file is the single consolidated prep doc: what was built, why, how it works
and where it breaks, ordered **basics → depth** exactly as an interview walks it
(§1 foundations → §2 persistence → §3 Redis → §4 algorithms → §5 concurrency →
§6 events → §7 production → §8 system design → §9 honest gaps → §10 drills).
It replaced three older docs (`Interview_prep.md`, `PLAN.md`,
`Backend_Interview_Zero_to_Master.md`), which are gone.

**It lives in the project repo, not in CS_CORE, and is not duplicated here.**
This file is the *route* — the checklist of what must be reproducible cold. That
file is the *map*. Section references below (`IP §n`) point into it.

---

## The re-pitch — reframe from food delivery to distributed systems

- [ ] The opener, delivered in one breath (`IP §0.1`):

  > "I built a distributed system where three stateless replicas contend for
  > shared state. The interesting parts were concurrency correctness — a
  > double-dispatch race I fixed with row-level locking using
  > `SELECT FOR UPDATE SKIP LOCKED` — and the consistency gap between my
  > database and my event log, which is the dual-write problem."

- [ ] Why this opener works — it names a race and a known named problem in the
      first fifteen seconds, and hands the interviewer two threads to pull

---

## Talking points that map to Nutanix's domain

- [ ] **Two-phase claim** — acquire all locks before mutating anything, so a failed rider claim never loses the order (`IP §5.4`)
- [ ] **`SKIP LOCKED`** — trades strict ordering for throughput; losers skip to the next row instead of blocking (`IP §5.3`)
- [ ] **Redis is advisory, Postgres is authoritative** — a stale index entry costs a wasted attempt, never a wrong outcome (`IP §3.4`, `§5.4`)
- [ ] **Kafka fan-out** to independent consumer groups with per-group offsets; at-least-once plus idempotent consumers (`IP §6.5`, `§6.7`)
- [ ] **Fail-open on Redis outage** — availability over protection, deliberately chosen and decided *per dependency* (`IP §7.5`)
- [ ] **Verified concurrency** — 15 concurrent dispatches across 3 replicas: 10 succeeded, 5 correctly rejected, zero duplicate rider or order IDs (`IP §5.5`)
- [ ] **Liveness vs readiness** — a liveness probe that depends on Redis turns a cache blip into a fleet restart (`IP §7.5`)

---

## The honest gaps — each needs a *stated fix*, not just an admission

**Governing rule:** these are **volunteered, never hidden**. A self-reported flaw
reads as engineering judgement; the same flaw extracted by an interviewer reads
as either not knowing or not saying. The O(log n) correction and the missing
outbox are the two that most improve credibility when self-reported.

Full statements and fixes: **`IP §9`**. Box is `[x]` only when both the flaw and
the fix can be stated cleanly, out loud.

- [ ] **Scheduler complexity** — resume says O(log n); it is **O(n log n) per dispatch** (`IP §9.1`)
- [ ] **Benchmark measures the wrong thing** — ~123 RPS / p99 220 ms is a plain INSERT on unauthenticated `POST /orders` (`IP §9.2`)
- [ ] **Forgeable admin auth** — JWT secret is the hardcoded dev default in the deployed instance (`IP §9.3`)
- [ ] **No transactional outbox** — the dual-write problem, by name (`IP §9.4`)
- [ ] **Durability theatre** — single broker, RF=1, so `acks=all` means nothing (`IP §9.5`)
- [ ] **Rate limiter bypassable** — by rotating a client-supplied `X-API-Key` (`IP §9.6`)
- [ ] **Test gaps** — Kafka patched at call sites; the concurrency proof is a manual script, not CI (`IP §9.7`)

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

Full drill set, stories and master checklist: **`IP §10`**.

- [ ] Deliver the opener cold, unprompted, in under twenty seconds
- [ ] Volunteer the O(n log n) correction without being asked
- [ ] Volunteer the missing outbox without being asked
- [ ] Defend `SKIP LOCKED` — including what it costs
- [ ] Answer "how would you make this production-ready?" using the gaps list as the answer
- [ ] Tell the thundering-herd story with its numbers: 5/15 → 10/15, zero doubles (`IP §5.5`)
- [ ] Sketch the architecture from memory, naming a trade-off at every arrow (`IP §8.7`)
