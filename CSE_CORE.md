# Full Interview Prep Syllabus — C++, OOP, OS, Linux, DBMS, CN, Distributed Systems

> Paste this whole file into the new project as context. Everything needed is here.
> Marker key: **[HOT]** = near-certain interview question, do not skip.
> Marker key: **[ASKED]** = confirmed asked in a real interview of mine.

---

## §0. Context

**Candidate:** Final-year CS Engineering student, Thapar Institute. On-campus placement route, no prior internship. CGPA 8.8/10.

**Competitive programming:** Codeforces Expert (1687), CodeChef 3-Star (1732), LeetCode Knight (peak 2084), 2000+ problems solved. C++ is the contest language.

**Resume stack:** C++, Python, SQL. FastAPI, REST APIs, SQLAlchemy, Pydantic, async programming. PostgreSQL, Redis. Kafka, event-driven architecture, pub/sub. System design, rate limiting, caching, idempotency, fault tolerance, concurrency control. Docker, Docker Compose, Git, GitHub Actions, CI/CD, Prometheus, Grafana, **Linux**.

**Target:** Nutanix Intern / Member of Technical Staff, 11 Aug 2026.

**Job description — primary skills:** Python, C++, exposure to RESTful APIs, distributed systems. Good to know: JS/HTML/CSS/React. Exposure to Django/Rails/NodeJS/SQL.

**Role scope:** core data path, platform deployment, data protection and replication, Linux kernel development, application programming, user interfaces.

**What Nutanix does:** Hyperconverged infrastructure — collapses compute, storage, and virtualization into one software layer on commodity servers. Distributed storage fabric plus their own hypervisor (AHV). Domain is OS, virtualization, storage internals, distributed systems.

### Prior interview result — the failure to learn from

Cleared the Eternal (Zomato) OA, failed the interview. **Cause: Computer Networks.** Could not answer basics like TCP vs UDP because the topic was never revised. This was a *revision* gap, not a comprehension gap.

**Confirmed questions asked in that interview:**
- **[ASKED]** Diamond problem: class A is the base, B and C inherit from A, D inherits from both B and C. Identify the problem, explain why it happens, and how to solve it.
- **[ASKED]** Virtual classes and dynamic allocation in OOP.
- **[ASKED]** TCP vs UDP (failed).

These reveal the depth level: not definitions, but *why does this break and how do you fix it*. Assume Nutanix goes deeper still.

### Existing project — DeliverIQ

Distributed order dispatch API. Python/FastAPI/PostgreSQL/Redis/Kafka/Docker. 3 API replicas, two-phase claiming with `SELECT FOR UPDATE SKIP LOCKED`, geohash rider matching with fairness banding, Kafka fan-out to 3 consumer groups, Redis token-bucket rate limiting, idempotency keys, JWT auth, Prometheus/Grafana. Complete, no longer being developed.

**Honest gaps — must be volunteered, never hidden:**
- Resume says the scheduler is O(log n); it is actually **O(n log n) per dispatch** — reloads all pending orders and rebuilds the heap on every call
- Resume cites ~123 RPS / p99 220 ms from Locust; that test only hits unauthenticated `POST /orders`, so it measures a plain INSERT — not matching, locking, or Kafka
- JWT secret is the hardcoded dev default, so admin endpoints are forgeable in the deployed instance
- **No transactional outbox** — a crash between `db.commit()` and `publish_event()` loses the event permanently
- Single Kafka broker, RF=1, so `acks=all` provides no real durability
- Rate limiter bypassable by rotating a client-supplied `X-API-Key` header
- Kafka is mocked in tests; the concurrency test is manual, not in CI

---

## §1. How to use this document

**Teaching contract for the assistant:**

For every topic, in this order:
1. **What it is** — plain words, assume zero knowledge, explain jargon on first use
2. **Why it exists** — what problem it solves, what breaks without it
3. **How it works** — the mechanism, not just the label
4. **Alternatives and tradeoffs** — what else could be used, and why not
5. **[HOT] interview Q&A** — the exact question, and the answer phrased the way it should be said out loud
6. **One small exercise** — something to actually run or write before moving on

**Rules:**
- I know competitive-programming C++ but have never written systems C++. No classes, no RAII, no smart pointers, no move semantics. Teach those from zero.
- Use full readable variable names in examples, not short CP-style names.
- One section at a time. Wait for "next" before continuing.
- Correct me directly if I state something wrong. Do not be agreeable about errors.
- Recommend a specific resource when one fits.

**Order of study:** C++ → OOP → OS → Linux → DBMS → CN → Distributed Systems. Each domain runs Tier 1 → Tier 2 → Tier 3.

**Already covered — do not repeat:** C++ §2.1.1 memory regions (stack vs heap, stack frames, stack overflow, heap allocation cost, dangling pointers, memory leaks, fragmentation, thread-per-stack vs shared-heap).

---

## §2. C++

### Tier 1 — Foundations

**2.1.1 Memory model** *(covered)*
Stack vs heap, stack frames, stack overflow, allocation cost, dangling pointers, leaks, fragmentation, one stack per thread vs one shared heap.

**2.1.2 Pointers and references**
- Pointer syntax, dereferencing, pointer arithmetic
- References — what they are, why they can't be null or reseated
- **[HOT]** Pointers vs references: when to use each
- `nullptr` vs `NULL` vs `0`
- Pointer to pointer; arrays decaying to pointers
- Function pointers

**2.1.3 const correctness**
- `const` variables, `const` parameters, `const` member functions
- **[HOT]** `const int*` vs `int* const` vs `const int* const` — read right to left
- `mutable`
- Why const-correctness matters in APIs

**2.1.4 Compilation model**
- Preprocessor → compiler → assembler → linker
- Header files vs source files; declaration vs definition
- Header guards and `#pragma once`
- **[HOT]** Why `#include <bits/stdc++.h>` is unacceptable in production
- The One Definition Rule
- Static vs dynamic linking

**2.1.5 The three meanings of `static`**
- File scope (internal linkage)
- Class member (shared across all instances)
- Local variable (persists across calls)

### Tier 2 — Core interview material

**2.2.1 RAII** — **[HOT]**, the single most important C++ idea
- Resource Acquisition Is Initialization: bind resource lifetime to object lifetime
- Constructor acquires, destructor releases
- Why it's exception-safe — stack unwinding calls destructors
- Applies to memory, files, locks, sockets, database connections
- **[HOT]** "What happens if an exception is thrown between `new` and `delete`?"

**2.2.2 Smart pointers** — **[HOT]**, and **[ASKED]** territory (dynamic allocation)
- `unique_ptr` — sole ownership, move-only, zero runtime overhead
- `shared_ptr` — reference counting, the control block, atomic refcount cost
- `weak_ptr` — non-owning observer; breaks reference cycles
- `make_unique` / `make_shared` and why they're preferred
- **[HOT]** `unique_ptr` vs `shared_ptr` — when each
- **[HOT]** What problem `weak_ptr` solves (cyclic `shared_ptr` never reaches refcount 0 → leak)
- Custom deleters
- Why `auto_ptr` was removed

**2.2.3 Move semantics**
- lvalue vs rvalue; rvalue references (`&&`)
- Move constructor and move assignment
- **[HOT]** `std::move` does not move — it is a cast to rvalue
- Copy elision and RVO
- **[HOT]** Rule of 0 / Rule of 3 / Rule of 5
- `noexcept` on move operations and why containers care

**2.2.4 Object model** — **[HOT]**, highest value section
- Virtual functions and dynamic dispatch
- **[HOT]** vtable and vptr — how a virtual call is resolved at runtime
- **[HOT]** Virtual destructors — deleting a derived object through a base pointer without one is undefined behaviour
- Pure virtual functions and abstract classes
- Static vs dynamic binding
- **[HOT]** Object slicing
- `override` and `final`
- **[HOT]** Why a constructor cannot be virtual
- **[HOT]** What happens when a virtual function is called inside a constructor
- Object memory layout; size of an object with and without virtuals

**2.2.5 STL containers and internals**
- `vector` — contiguous, amortized O(1) `push_back`, doubling growth, **[HOT]** iterator invalidation on reallocation
- **[HOT]** `map` (red-black tree, O(log n), ordered) vs `unordered_map` (hash table, O(1) average, O(n) worst)
- `set`, `multiset`, `deque`, `list`, `forward_list`, `array`
- `priority_queue`, `stack`, `queue` as adapters
- **[HOT]** `emplace_back` vs `push_back`
- Iterator categories and invalidation rules per container
- **[HOT]** Why `vector` doubles instead of growing by a fixed amount

**2.2.6 Copy semantics**
- Shallow vs deep copy
- Copy constructor and copy assignment operator
- Self-assignment safety
- **[HOT]** What happens when a class holding a raw pointer is copied by default

### Tier 3 — Advanced / differentiator

**2.3.1 Templates**
- Function and class templates; template instantiation
- Template specialization and partial specialization
- Variadic templates
- `auto`, `decltype`, type deduction
- Why template errors are notoriously bad

**2.3.2 Exceptions and error handling**
- `try` / `catch` / `throw`; stack unwinding
- Exception safety guarantees: basic, strong, no-throw
- Why destructors must not throw
- `noexcept`
- Exceptions vs error codes — the tradeoff

**2.3.3 Concurrency in C++**
- `std::thread`, `join`, `detach`
- `std::mutex`, `lock_guard`, `unique_lock` — RAII applied to locks
- `condition_variable`
- `std::atomic`; data races vs race conditions
- Memory ordering at concept level

**2.3.4 Modern C++ features**
- Lambdas and capture modes (by value, by reference, the dangers of each)
- `constexpr`
- Structured bindings
- Range-based for loops
- Smart use of `auto`

**2.3.5 Undefined behaviour**
- What UB means and why the compiler may do anything
- Common causes: signed overflow, out-of-bounds access, use-after-free, uninitialized reads, strict aliasing violations

---

## §3. OOP

### Tier 1 — Foundations

**3.1.1 The four pillars, as mechanics not definitions**
Do not recite textbook lines. Explain each in terms of what the compiler and runtime actually do.
- **Encapsulation** — bundling data with the methods that act on it; access specifiers
- **Abstraction** — exposing an interface while hiding implementation
- **Inheritance** — is-a relationships; public/protected/private inheritance
- **Polymorphism** — compile-time (overloading, templates) vs runtime (virtual dispatch)

**3.1.2 Classes and objects**
- Class vs object vs instance
- Constructors: default, parameterized, copy, move
- Destructors and destruction order
- Initializer lists and why they beat assignment in the constructor body
- `this` pointer
- **[HOT]** `struct` vs `class` in C++ (default access only)
- Friend functions and classes

**3.1.3 Access specifiers**
- public, protected, private
- How public / protected / private *inheritance* changes access in the derived class

### Tier 2 — Core interview material

**3.2.1 Polymorphism in depth** — **[HOT]**
- **[HOT]** Overloading vs overriding vs hiding
- Virtual functions, vtable dispatch cost
- Abstract classes and interfaces
- Runtime type identification: `dynamic_cast`, `typeid`
- The four casts: `static_cast`, `dynamic_cast`, `const_cast`, `reinterpret_cast`

**3.2.2 The diamond problem** — **[ASKED]**, expect it again

The exact setup asked: class A is the base; B and C each inherit from A; D inherits from both B and C.

Must be able to explain all four of these:
- **The problem:** D contains **two separate copies** of A's members — one via B, one via C. Ambiguity on `d.someMemberOfA` (the compiler cannot tell which copy), and duplicated state that can diverge.
- **Why it happens:** each inheritance path constructs its own A subobject. B has an A, C has an A, and D inherits both paths, so D has two A subobjects.
- **The fix:** **virtual inheritance** — `class B : virtual public A` and `class C : virtual public A`. Now D has exactly one shared A subobject.
- **The consequences of the fix:** the *most derived* class (D) becomes responsible for constructing the virtual base A, bypassing B and C's constructor calls to it. Object layout changes — virtual base pointers are added, so `sizeof` grows and member access becomes indirect. There is a real runtime cost.
- Alternative answer worth giving: **avoid the diamond entirely** by using composition, or by making A a pure interface with no data members (no state means no duplicated state).

**3.2.3 Virtual classes and dynamic allocation** — **[ASKED]**
- "Virtual class" in interview usage usually means either a class with virtual functions, or a virtually-inherited base — clarify which is meant, and be ready for both
- Abstract classes cannot be instantiated; they exist to be inherited
- **[HOT]** Why polymorphism requires pointers or references, never objects by value (object slicing)
- Dynamic allocation of polymorphic objects: `Base* obj = new Derived();`
- **[HOT]** Why `delete obj` through a base pointer needs a virtual destructor — without it only `~Base()` runs, and derived resources leak
- Modern form: `std::unique_ptr<Base> obj = std::make_unique<Derived>();`

**3.2.4 SOLID** — know all five, with one concrete example each
- **S**ingle Responsibility
- **O**pen/Closed
- **L**iskov Substitution — **[HOT]**, most asked
- **I**nterface Segregation
- **D**ependency Inversion — **[HOT]**, second most asked

**3.2.5 Composition over inheritance** — **[HOT]**
- is-a vs has-a
- When inheritance is the wrong tool
- Why deep hierarchies are fragile (the fragile base class problem)

### Tier 3 — Advanced

**3.3.1 Design patterns**
- **Strategy** — swap algorithms behind one interface
- **Factory** and Abstract Factory
- **Singleton** — and **[HOT]** why it's widely considered an anti-pattern (global state, untestable, thread-safety hazards)
- **Observer** — publish/subscribe; maps directly to Kafka fan-out in DeliverIQ
- Decorator, Adapter, Builder

**3.3.2 Advanced OOP concepts**
- Multiple inheritance beyond the diamond
- Virtual function tables in multiple-inheritance layouts
- CRTP (Curiously Recurring Template Pattern) — static polymorphism
- Pimpl idiom and compile-firewall benefits
- Coupling and cohesion

---

## §4. Operating Systems

### Tier 1 — Foundations
- What an OS does; kernel vs user space; system calls
- **[HOT]** Process vs thread; the PCB; process states
- Context switching and its cost
- Multiprogramming, multitasking, multiprocessing
- Monolithic vs microkernel

### Tier 2 — Core
- **[HOT]** `fork()`, `exec()`, `wait()`; zombie and orphan processes; predict-the-output `fork()` puzzles
- Inter-process communication: pipes, shared memory, message queues, sockets
- CPU scheduling: FCFS, SJF, SRTF, round robin, priority, multilevel feedback queue
- Preemptive vs non-preemptive; starvation and aging
- **[HOT]** Virtual memory: why it exists, address translation, page tables, MMU
- **[HOT]** Paging, page faults, TLB, demand paging, thrashing
- Page replacement: FIFO, LRU, optimal, clock/second-chance, Belady's anomaly
- Segmentation vs paging
- Internal vs external fragmentation
- **[HOT]** Synchronization: mutex vs semaphore vs spinlock vs condition variable
- Critical section problem; atomic operations
- **[HOT]** Deadlock: the four necessary conditions; prevention vs avoidance vs detection; Banker's algorithm
- Classic problems: producer-consumer, readers-writers, dining philosophers
- Race conditions and how they differ from data races

### Tier 3 — Advanced / Nutanix-relevant
- File systems: inodes, directory structure, hard vs soft links, journaling
- Disk scheduling: FCFS, SSTF, SCAN, C-SCAN
- I/O: buffering, caching, spooling, DMA, interrupts vs polling
- **[HOT for Nutanix]** Virtualization: type 1 vs type 2 hypervisors; full virtualization vs paravirtualization; hardware-assisted virtualization
- **[HOT for Nutanix]** VMs vs containers — isolation boundary, boot time, overhead, kernel sharing
- Memory overcommit, ballooning, page sharing
- Copy-on-write; `fork()` implemented via COW
- Kernel modules and drivers at concept level

---

## §5. Linux (on the resume — must be defensible)

### Tier 1 — Command line fluency
- Filesystem hierarchy: `/etc`, `/var`, `/proc`, `/dev`, `/tmp`, `/usr`
- File ops: `ls`, `cd`, `cp`, `mv`, `rm`, `find`, `locate`
- Text processing: `cat`, `less`, `head`, `tail`, `grep`, `sed`, `awk`, `cut`, `sort`, `uniq`, `wc`
- Pipes, redirection, `stdin`/`stdout`/`stderr`, `tee`, `xargs`
- Permissions: `chmod`, `chown`, the rwx octal notation, `umask`
- **[HOT]** What `chmod 755` means and why it's the common default

### Tier 2 — Process and system management
- `ps`, `top`, `htop`, `kill`, `killall`, signals (SIGTERM vs SIGKILL vs SIGHUP)
- **[HOT]** SIGTERM vs SIGKILL — why you send TERM first
- Background/foreground jobs, `&`, `nohup`, `jobs`, `fg`, `bg`
- `systemctl` and services; `journalctl`
- Disk and memory: `df`, `du`, `free`, `lsblk`, `mount`
- Networking tools: `ping`, `curl`, `wget`, `netstat`/`ss`, `traceroute`, `dig`, `nslookup`
- Package managers: `apt`, `yum`/`dnf`
- SSH, key-based auth, `scp`, `rsync`
- Environment variables, `.bashrc` vs `.bash_profile`, `PATH`

### Tier 3 — Systems depth
- **[HOT]** The `/proc` filesystem — what it exposes and why it's not a real filesystem
- File descriptors; everything-is-a-file
- **[HOT]** `strace` and `ltrace` — observing system calls
- `lsof`
- Shell scripting: variables, conditionals, loops, functions, exit codes
- `cron` and scheduled jobs
- Namespaces and cgroups — **the mechanism containers are built on**, directly relevant to Nutanix
- Kernel vs user space transitions; system call cost
- Static vs dynamic libraries, `ldd`, `LD_LIBRARY_PATH`

---

## §6. DBMS

### Tier 1 — Foundations
- Relational model: tables, rows, columns, domains
- Keys: primary, foreign, candidate, composite, super
- **[HOT]** ACID — all four letters with a concrete example each
- DDL vs DML vs DCL vs TCL
- ER modelling; cardinality

### Tier 2 — Core
- **[HOT]** Normalization: 1NF, 2NF, 3NF, BCNF; what anomaly each removes
- **[HOT]** When to deliberately denormalize
- SQL: `SELECT`, `WHERE`, `GROUP BY`, `HAVING`, `ORDER BY`, `LIMIT`
- **[HOT]** All join types — inner, left, right, full, cross, self
- Subqueries vs joins; correlated subqueries
- Aggregate functions; window functions
- **[HOT]** Indexing: B-tree and B+ tree structure, why B+ trees for databases, clustered vs non-clustered, composite indexes, covering indexes
- **[HOT]** When an index *hurts* (write amplification, low-cardinality columns)
- Query execution plans; `EXPLAIN`
- **[HOT]** Transactions and isolation levels: read uncommitted, read committed, repeatable read, serializable
- **[HOT]** The anomalies each level permits: dirty read, non-repeatable read, phantom read
- Locking: shared vs exclusive, row vs table, two-phase locking
- Optimistic vs pessimistic concurrency control
- **[HOT]** `SELECT FOR UPDATE` and `SKIP LOCKED` — used directly in DeliverIQ

### Tier 3 — Advanced
- **[HOT]** SQL vs NoSQL — when each, and why
- Document, key-value, column-family, and graph stores
- **[HOT]** CAP theorem, and the trap follow-up "does CAP apply to a single-node SQL database?" (no — partitions are the premise)
- Sharding and partitioning strategies
- Replication: leader-follower, multi-leader, leaderless
- Write-ahead log and crash recovery
- MVCC — how Postgres avoids read locks
- Connection pooling
- Deadlock detection in databases

---

## §7. Computer Networks (the topic that cost the last offer)

### Tier 1 — Foundations, breadth first
- **[HOT]** OSI 7 layers and TCP/IP 4 layers — name them and what lives at each
- Encapsulation and headers as data moves down the stack
- **[HOT] [ASKED]** **TCP vs UDP** — connection-oriented vs connectionless, reliability, ordering, flow control, congestion control, header size, use cases. **This is the question that was failed. Know it cold.**
- Client-server vs peer-to-peer
- Bandwidth vs latency vs throughput
- LAN, WAN, MAN

### Tier 2 — Core
- **[HOT]** TCP three-way handshake (SYN, SYN-ACK, ACK) and four-way teardown (FIN, ACK, FIN, ACK)
- TCP sequence and acknowledgement numbers
- **[HOT]** Flow control (sliding window) vs congestion control (slow start, congestion avoidance, AIMD, fast retransmit)
- TIME_WAIT and why it exists
- **[HOT]** "What happens when you type a URL into a browser" — DNS → TCP handshake → TLS → HTTP request → response → render. **Be able to go three levels deep on any step.**
- DNS: recursive vs iterative resolution, record types (A, AAAA, CNAME, MX, NS), caching and TTL
- HTTP methods, status code classes, headers, cookies
- **[HOT]** HTTP/1.1 vs HTTP/2 vs HTTP/3 — pipelining, multiplexing, head-of-line blocking, QUIC over UDP
- **[HOT]** HTTPS and the TLS handshake; symmetric vs asymmetric encryption; certificates and chain of trust
- IP addressing, IPv4 vs IPv6, subnetting, CIDR
- Public vs private addresses, NAT
- ARP, DHCP, ICMP
- Ports, sockets, the 5-tuple connection identifier

### Tier 3 — Applied
- Router vs switch vs hub vs bridge
- Routing: static vs dynamic, distance vector vs link state, BGP at concept level
- **[HOT]** REST semantics: statelessness, idempotency, safe methods, which HTTP verbs are idempotent
- WebSockets vs long polling vs server-sent events
- Load balancers: L4 vs L7, algorithms, health checks
- Reverse proxy vs forward proxy
- CDNs
- Common attacks: DDoS, man-in-the-middle, DNS spoofing

---

## §8. Distributed Systems

Anchor every answer in DeliverIQ — that is what makes these concrete rather than recited.

### Tier 1 — Foundations
- Why distribute at all: scale, availability, fault tolerance, locality
- Horizontal vs vertical scaling
- **[HOT]** Stateless vs stateful services, and why statelessness enables horizontal scaling
- The eight fallacies of distributed computing
- Latency vs availability vs consistency as competing goals

### Tier 2 — Core
- **[HOT]** CAP theorem — and the sharp follow-up about single-node databases
- PACELC as the refinement of CAP
- **[HOT]** Consistency models: strong, eventual, causal, read-your-writes, monotonic reads
- ACID vs BASE
- **[HOT]** Replication: leader-follower, multi-leader, leaderless; synchronous vs asynchronous
- **[HOT]** Quorum: why R + W > N guarantees overlap
- Partitioning and sharding; **[HOT]** consistent hashing and why modulo hashing breaks on resize
- Consensus: Paxos and Raft at concept level — leader election, log replication, split brain
- **[HOT]** Delivery semantics: at-most-once, at-least-once, exactly-once; idempotent consumers
- **[HOT]** The dual-write problem and the **transactional outbox** pattern; CDC and Debezium
- Message queues vs event logs — RabbitMQ vs Kafka
- Kafka internals: topics, partitions, offsets, consumer groups, rebalancing, ordering guarantees per partition

### Tier 3 — Advanced / Nutanix-relevant
- Failure detection: heartbeats, timeouts, phi-accrual
- **[HOT for Nutanix]** Merkle trees for replica divergence detection — used in Dynamo, Cassandra, and Nutanix-style storage
- Anti-entropy and read repair
- Vector clocks and Lamport timestamps
- Distributed locking and its hazards: lock expiry, fencing tokens, why Redlock is debated
- Distributed transactions: two-phase commit, three-phase commit, saga pattern
- Backpressure and load shedding
- Circuit breakers, bulkheads, retries with exponential backoff and jitter
- Caching strategies: cache-aside, write-through, write-behind; cache invalidation
- Rate limiting algorithms: token bucket, leaky bucket, fixed window, sliding window log, sliding window counter
- **[HOT for Nutanix]** Storage tiering, deduplication, erasure coding vs replication

---

## §9. DeliverIQ re-pitch for this audience

Reframe from food delivery to distributed systems. Opener:

> "I built a distributed system where three stateless replicas contend for shared state. The interesting parts were concurrency correctness — a double-dispatch race I fixed with row-level locking using `SELECT FOR UPDATE SKIP LOCKED` — and the consistency gap between my database and my event log, which is the dual-write problem."

**Talking points that map to their domain:**
- Two-phase claim: acquire all locks before mutating anything, so a failed rider claim never loses the order
- `SKIP LOCKED`: trades strict ordering for throughput — losers skip to the next row instead of blocking
- Redis is advisory, Postgres is authoritative — a stale index entry costs a wasted attempt, never a wrong outcome
- Kafka fan-out to independent consumer groups with per-group offsets; at-least-once plus idempotent consumers
- Fail-open on Redis outage: availability over protection, deliberately chosen
- Verified concurrency: 15 concurrent dispatches across 3 replicas, 10 succeeded, 5 correctly rejected, zero duplicate rider or order IDs

**Volunteer the §0 gaps before being asked.** The O(log n) correction and the missing outbox are the two that most improve credibility when self-reported.

---

## §10. Questions to ask at the end

**Conceptual, domain-grounded:**
> "Since the storage fabric replicates data across nodes, how do you detect and repair replica divergence — is it Merkle-tree comparison like Dynamo-style systems, or something driven by the write path?"

**If the conversation was OS-flavoured:**
> "Where does the line sit between AHV and the storage layer — how much of the data path runs in the hypervisor versus in a user-space service?"

**Role-oriented:**
> "For an intern joining an aggregate posting like this, how does team allocation actually work — matched to interest, or driven by where the need is?"

---

## §11. Resources

*Verify these before relying on them — titles and channels may have changed.*

**C++**
- **learncpp.com** — free, structured, the best C++ resource available. Chapters on RAII, smart pointers, move semantics are directly on target.
- **cppreference.com** — the reference; use for exact behaviour, not learning
- **The Cherno** (YouTube) — C++ series on pointers, references, smart pointers, move semantics
- **CppCon "Back to Basics"** track (YouTube) — aimed exactly at this gap
- *Effective Modern C++*, Scott Meyers — for depth after the basics
- **C++ Core Guidelines** (isocpp.github.io/CppCoreGuidelines) — what good C++ looks like

**OS**
- *Operating System Concepts* (Silberschatz) — the standard text
- **Gate Smashers** (YouTube) — fast breadth in Indian-interview framing
- **Neso Academy** (YouTube) — clearer on synchronization and deadlock
- OSTEP (Operating Systems: Three Easy Pieces) — free online, excellent on virtual memory

**Linux**
- *The Linux Command Line*, William Shotts — free PDF
- `man` pages — actually read them
- **Linux Journey** (linuxjourney.com) — structured beginner path

**DBMS**
- *Database System Concepts* (Korth)
- **Use The Index, Luke** (use-the-index-luke.com) — the best free indexing resource
- HackerRank SQL track — for query-writing practice

**CN**
- *Computer Networking: A Top-Down Approach* (Kurose & Ross) — the standard
- **Gate Smashers / Neso Academy** — for the breadth sweep
- **High Performance Browser Networking** (free online, Ilya Grigorik) — excellent on TCP, TLS, HTTP/2

**Distributed Systems**
- *Designing Data-Intensive Applications*, Martin Kleppmann — the single best book in this list
- **MIT 6.824** lectures (YouTube) — distributed systems course
- Kafka official docs — for the parts DeliverIQ uses

---
