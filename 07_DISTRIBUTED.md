# Distributed Systems — Topic Map

> **Block F** (with [08_DELIVERIQ.md](08_DELIVERIQ.md)). This block doubles as
> project-defence prep: every answer here should be anchored in DeliverIQ, which
> is what makes it concrete rather than recited. Several topics below are
> *exactly* the ones DeliverIQ gets wrong — the outbox, delivery semantics,
> replication factor — and the honest version of those is worth more in the room
> than a clean-sounding one.
> Legend and fill-in rule: [00_PLAN.md](00_PLAN.md).

---

# Tier 1 — Foundations

- [ ] Why distribute at all: scale, availability, fault tolerance, locality
- [ ] Horizontal vs vertical scaling
- [ ] **[HOT]** Stateless vs stateful services, and why statelessness enables horizontal scaling
- [ ] The eight fallacies of distributed computing
- [ ] Latency vs availability vs consistency as competing goals

---

# Tier 2 — Core

## Consistency

- [ ] **[HOT]** CAP theorem — and the sharp follow-up about single-node databases
- [ ] PACELC as the refinement of CAP
- [ ] **[HOT]** Consistency models: strong, eventual, causal, read-your-writes, monotonic reads
- [ ] ACID vs BASE

## Replication and partitioning

- [ ] **[HOT]** Replication: leader-follower, multi-leader, leaderless; synchronous vs asynchronous
- [ ] **[HOT]** Quorum — why R + W > N guarantees overlap
- [ ] Partitioning and sharding
- [ ] **[HOT]** Consistent hashing, and why modulo hashing breaks on resize
- [ ] Consensus: Paxos and Raft at concept level — leader election, log replication, split brain

## Messaging

- [ ] **[HOT]** Delivery semantics: at-most-once, at-least-once, exactly-once; idempotent consumers
- [ ] **[HOT]** The dual-write problem and the **transactional outbox** pattern; CDC and Debezium — *DeliverIQ's largest honest gap*
- [ ] Message queues vs event logs — RabbitMQ vs Kafka
- [ ] Kafka internals: topics, partitions, offsets, consumer groups, rebalancing, per-partition ordering guarantees

---

# Tier 3 — Advanced / Nutanix-relevant

## Failure and repair

- [ ] Failure detection: heartbeats, timeouts, phi-accrual
- [ ] **[HOT-NX]** Merkle trees for replica divergence detection — used in Dynamo, Cassandra, and Nutanix-style storage
- [ ] Anti-entropy and read repair
- [ ] Vector clocks and Lamport timestamps

## Coordination

- [ ] Distributed locking and its hazards: lock expiry, fencing tokens, why Redlock is debated
- [ ] Distributed transactions: two-phase commit, three-phase commit, saga pattern

## Resilience

- [ ] Backpressure and load shedding
- [ ] Circuit breakers, bulkheads, retries with exponential backoff and jitter
- [ ] Caching strategies: cache-aside, write-through, write-behind; cache invalidation
- [ ] Rate limiting algorithms: token bucket, leaky bucket, fixed window, sliding window log, sliding window counter

## Storage

- [ ] **[HOT-NX]** Storage tiering, deduplication, erasure coding vs replication

---

## Resources

- *Designing Data-Intensive Applications*, Martin Kleppmann — the single best book in the whole plan.
- **MIT 6.824** lectures (YouTube) — distributed systems course.
- Kafka official docs — for the parts DeliverIQ actually uses.

## Block G drill list — every [HOT] in this file

- [ ] Stateless vs stateful, and why it enables horizontal scaling
- [ ] CAP + the single-node follow-up
- [ ] The five consistency models
- [ ] Replication topologies; sync vs async
- [ ] Quorum: why R + W > N
- [ ] Consistent hashing vs modulo hashing on resize
- [ ] At-most / at-least / exactly-once, and how idempotent consumers get you there
- [ ] Dual-write problem → transactional outbox — **volunteered, not extracted**
- [ ] **[HOT-NX]** Merkle trees for replica divergence
- [ ] **[HOT-NX]** Erasure coding vs replication
