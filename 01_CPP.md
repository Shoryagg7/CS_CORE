# C++ — Topic Map

> **Block B.** Largest genuine gap: contest C++ is fluent, systems C++ is zero —
> no classes, no RAII, no smart pointers, no move semantics. Taught from scratch,
> full readable variable names, jargon explained on first use.
> Legend and fill-in rule: [00_PLAN.md](00_PLAN.md).

---

# Tier 1 — Foundations

## 2.1.1 Memory model — ✅ COVERED, do not re-teach

- [x] Stack vs heap
- [x] Stack frames, stack overflow
- [x] Heap allocation cost
- [x] Dangling pointers, memory leaks, fragmentation
- [x] One stack per thread vs one shared heap

## 2.1.2 Pointers and references

- [ ] Pointer syntax, dereferencing, pointer arithmetic
- [ ] References — what they are, why they cannot be null and cannot be reseated
- [ ] **[HOT]** Pointers vs references — when to use each
- [ ] `nullptr` vs `NULL` vs `0`
- [ ] Pointer to pointer
- [ ] Arrays decaying to pointers
- [ ] Function pointers

## 2.1.3 const correctness

- [ ] `const` variables, `const` parameters, `const` member functions
- [ ] **[HOT]** `const int*` vs `int* const` vs `const int* const` — read right to left
- [ ] `mutable`
- [ ] Why const-correctness matters in APIs

## 2.1.4 Compilation model

- [ ] Preprocessor → compiler → assembler → linker
- [ ] Header files vs source files; declaration vs definition
- [ ] Header guards and `#pragma once`
- [ ] **[HOT]** Why `#include <bits/stdc++.h>` is unacceptable in production
- [ ] The One Definition Rule (ODR)
- [ ] Static vs dynamic linking

## 2.1.5 The three meanings of `static`

- [ ] File scope — internal linkage
- [ ] Class member — shared across all instances
- [ ] Local variable — persists across calls

---

# Tier 2 — Core interview material

## 2.2.1 RAII — **[HOT]**, the single most important C++ idea

- [ ] Resource Acquisition Is Initialization — bind resource lifetime to object lifetime
- [ ] Constructor acquires, destructor releases
- [ ] Why it is exception-safe — stack unwinding calls destructors
- [ ] Applies to memory, files, locks, sockets, DB connections
- [ ] **[HOT]** "What happens if an exception is thrown between `new` and `delete`?"

## 2.2.2 Smart pointers — **[HOT]**, and **[ASKED]** territory (dynamic allocation)

- [ ] `unique_ptr` — sole ownership, move-only, zero runtime overhead
- [ ] `shared_ptr` — reference counting, the control block, atomic refcount cost
- [ ] `weak_ptr` — non-owning observer, breaks reference cycles
- [ ] `make_unique` / `make_shared` and why they are preferred
- [ ] **[HOT]** `unique_ptr` vs `shared_ptr` — when each
- [ ] **[HOT]** What problem `weak_ptr` solves — cyclic `shared_ptr` never reaches refcount 0 → leak
- [ ] Custom deleters
- [ ] Why `auto_ptr` was removed

## 2.2.3 Move semantics

- [ ] lvalue vs rvalue; rvalue references (`&&`)
- [ ] Move constructor and move assignment
- [ ] **[HOT]** `std::move` does not move — it is a cast to rvalue
- [ ] Copy elision and RVO
- [ ] **[HOT]** Rule of 0 / Rule of 3 / Rule of 5
- [ ] `noexcept` on move operations and why containers care

## 2.2.4 Object model — **[HOT]**, highest-value section

- [ ] Virtual functions and dynamic dispatch
- [ ] **[HOT]** vtable and vptr — how a virtual call is resolved at runtime
- [ ] **[HOT]** Virtual destructors — deleting a derived object through a base pointer without one is undefined behaviour
- [ ] Pure virtual functions and abstract classes
- [ ] Static vs dynamic binding
- [ ] **[HOT]** Object slicing
- [ ] `override` and `final`
- [ ] **[HOT]** Why a constructor cannot be virtual
- [ ] **[HOT]** What happens when a virtual function is called inside a constructor
- [ ] Object memory layout; `sizeof` an object with and without virtuals

## 2.2.5 STL containers and internals

- [ ] `vector` — contiguous, amortized O(1) `push_back`, doubling growth
- [ ] **[HOT]** Iterator invalidation on `vector` reallocation
- [ ] **[HOT]** `map` (red-black tree, O(log n), ordered) vs `unordered_map` (hash table, O(1) average, O(n) worst)
- [ ] `set`, `multiset`, `deque`, `list`, `forward_list`, `array`
- [ ] `priority_queue`, `stack`, `queue` as adapters
- [ ] **[HOT]** `emplace_back` vs `push_back`
- [ ] Iterator categories and invalidation rules per container
- [ ] **[HOT]** Why `vector` doubles instead of growing by a fixed amount

## 2.2.6 Copy semantics

- [ ] Shallow vs deep copy
- [ ] Copy constructor and copy assignment operator
- [ ] Self-assignment safety
- [ ] **[HOT]** What happens when a class holding a raw pointer is copied by default

---

# Tier 3 — Advanced / differentiator

## 2.3.1 Templates

- [ ] Function and class templates; template instantiation
- [ ] Template specialization and partial specialization
- [ ] Variadic templates
- [ ] `auto`, `decltype`, type deduction
- [ ] Why template errors are notoriously bad

## 2.3.2 Exceptions and error handling

- [ ] `try` / `catch` / `throw`; stack unwinding
- [ ] Exception safety guarantees: basic, strong, no-throw
- [ ] Why destructors must not throw
- [ ] `noexcept`
- [ ] Exceptions vs error codes — the tradeoff

## 2.3.3 Concurrency in C++

- [ ] `std::thread`, `join`, `detach`
- [ ] `std::mutex`, `lock_guard`, `unique_lock` — RAII applied to locks
- [ ] `condition_variable`
- [ ] `std::atomic`; data races vs race conditions
- [ ] Memory ordering at concept level

## 2.3.4 Modern C++ features

- [ ] Lambdas and capture modes — by value, by reference, the dangers of each
- [ ] `constexpr`
- [ ] Structured bindings
- [ ] Range-based for loops
- [ ] Smart use of `auto`

## 2.3.5 Undefined behaviour

- [ ] What UB means and why the compiler may do anything
- [ ] Common causes: signed overflow, out-of-bounds access, use-after-free, uninitialized reads, strict aliasing violations

---

## Resources

- **learncpp.com** — free, structured, best available. Chapters on RAII, smart pointers, move semantics are directly on target.
- **cppreference.com** — the reference; for exact behaviour, not for learning.
- **The Cherno** (YouTube) — pointers, references, smart pointers, move semantics.
- **CppCon "Back to Basics"** track (YouTube) — aimed exactly at this gap.
- *Effective Modern C++*, Scott Meyers — depth after the basics.
- **C++ Core Guidelines** (isocpp.github.io/CppCoreGuidelines) — what good C++ looks like.

## Block G drill list — every [HOT] in this file

- [ ] Pointers vs references — when to use each
- [ ] `const int*` vs `int* const` vs `const int* const`
- [ ] Why `#include <bits/stdc++.h>` is unacceptable in production
- [ ] Exception thrown between `new` and `delete`
- [ ] `unique_ptr` vs `shared_ptr`
- [ ] What problem `weak_ptr` solves
- [ ] `std::move` does not move
- [ ] Rule of 0 / 3 / 5
- [ ] vtable and vptr — resolving a virtual call
- [ ] Virtual destructors
- [ ] Object slicing
- [ ] Why a constructor cannot be virtual
- [ ] Virtual function called inside a constructor
- [ ] Iterator invalidation on `vector` reallocation
- [ ] `map` vs `unordered_map`
- [ ] `emplace_back` vs `push_back`
- [ ] Why `vector` doubles
- [ ] Default copy of a class holding a raw pointer
