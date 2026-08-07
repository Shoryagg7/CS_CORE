# Master Plan — Nutanix Prep

> Source of truth for context, gaps, and the teaching contract:
> [CSE_CORE.md](CSE_CORE.md). This file is the **route**; the subject files are
> the **map**. Nothing here duplicates CSE_CORE — read that first, once.

---

## Block order (fixed)

| Block | Content | File(s) | Why here |
|---|---|---|---|
| **A** | CN **[HOT]** floor — layers, TCP vs UDP, handshake, URL→browser | [06_CN.md](06_CN.md) §A | Kills the known killer immediately. If everything else collapses, the last failure does not repeat. |
| **B** | C++ Tier 1 + Tier 2, deep | [01_CPP.md](01_CPP.md) | Largest genuine gap. Needs the most consolidation time, so it goes earliest. |
| **C** | OOP incl. diamond **[ASKED]** | [02_OOP.md](02_OOP.md) | Rides free on the C++ object model from Block B. Fast because of ordering. |
| **D** | OS → ending on virtualization, VMs vs containers | [03_OS.md](03_OS.md) | Nutanix's actual domain. |
| **E** | DBMS + Linux | [05_DBMS.md](05_DBMS.md), [04_LINUX.md](04_LINUX.md) | Both partly covered by DeliverIQ and the resume. Sweep-heavy. |
| **F** | Distributed Systems + DeliverIQ re-pitch | [07_DISTRIBUTED.md](07_DISTRIBUTED.md), [08_DELIVERIQ.md](08_DELIVERIQ.md) | Doubles as project-defence prep. |
| **G** | CN Tier 2 + Tier 3 full + retrieval drill on every **[HOT]** | [06_CN.md](06_CN.md) §G | CN bookends the plan: floor first so it can't be skipped, depth last so it's freshest. |

**Bookend rule:** CN is the only subject that appears twice. That is deliberate
and not negotiable — Block A is a floor, not a pass.

---

## Subject files

| File | Subject | Block | Status |
|---|---|---|---|
| [01_CPP.md](01_CPP.md) | C++ | B | not started |
| [02_OOP.md](02_OOP.md) | OOP | C | not started |
| [03_OS.md](03_OS.md) | Operating Systems | D | not started |
| [04_LINUX.md](04_LINUX.md) | Linux | E | not started |
| [05_DBMS.md](05_DBMS.md) | DBMS | E | not started |
| [06_CN.md](06_CN.md) | Computer Networks | A + G | not started |
| [07_DISTRIBUTED.md](07_DISTRIBUTED.md) | Distributed Systems | F | not started |
| [08_DELIVERIQ.md](08_DELIVERIQ.md) | DeliverIQ defence | F | not started |
| [SQL.md](SQL.md) | SQL hands-on (pre-existing) | feeds E | in progress |

---

## Conventions used in every subject file

**Status legend** — update the box as we go:

- `[ ]` not started
- `[~]` taught — understood on the page, not yet reproducible cold
- `[x]` **locked** — can explain it out loud, unprompted, without notes

`[~]` → `[x]` only happens in a retrieval drill, never right after a lesson.
Recognising an explanation is not the same as producing one; the last interview
was lost on a topic that would have felt like `[~]`.

**Markers**

- **[HOT]** — near-certain interview question, do not skip
- **[ASKED]** — confirmed asked in a real interview
- **[HOT-NX]** — hot specifically because of Nutanix's domain

**Fill-in rule** — each topic is a heading with a checklist under it. When a
topic is taught, the lesson is appended *under that heading*, following the
§1 teaching contract:

1. What it is → 2. Why it exists → 3. How it works → 4. Alternatives and
tradeoffs → 5. **[HOT]** Q&A phrased as it would be said aloud → 6. One small
exercise.

---

## Already covered — do not re-teach

- C++ **2.1.1 Memory model** — stack vs heap, stack frames, stack overflow,
  allocation cost, dangling pointers, leaks, fragmentation, per-thread stack vs
  shared heap.

---

## Retrieval drill (Block G)

One pass over every **[HOT]** and **[ASKED]** item across all seven subjects.
Question asked cold, answered out loud, no notes open. Anything that comes out
as a stumble drops from `[x]` back to `[~]`.
