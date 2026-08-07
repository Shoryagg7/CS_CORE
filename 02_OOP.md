# OOP — Topic Map

> **Block C.** Runs immediately after C++ Block B so it rides free on the object
> model already taught — vtables, virtual destructors, and slicing are the same
> machinery seen from the design side. Two items here are **[ASKED]**: the
> diamond problem and virtual classes / dynamic allocation. Expect both again.
> Legend and fill-in rule: [00_PLAN.md](00_PLAN.md).

---

# Tier 1 — Foundations

## 3.1.1 The four pillars, as mechanics not definitions

> No textbook recitation. Each pillar explained in terms of what the compiler
> and the runtime actually do.

- [ ] **Encapsulation** — bundling data with the methods that act on it; access specifiers
- [ ] **Abstraction** — exposing an interface while hiding implementation
- [ ] **Inheritance** — is-a relationships; public / protected / private inheritance
- [ ] **Polymorphism** — compile-time (overloading, templates) vs runtime (virtual dispatch)

## 3.1.2 Classes and objects

- [ ] Class vs object vs instance
- [ ] Constructors: default, parameterized, copy, move
- [ ] Destructors and destruction order
- [ ] Initializer lists and why they beat assignment in the constructor body
- [ ] The `this` pointer
- [ ] **[HOT]** `struct` vs `class` in C++ — default access only
- [ ] Friend functions and friend classes

## 3.1.3 Access specifiers

- [ ] public, protected, private
- [ ] How public / protected / private *inheritance* changes access in the derived class

---

# Tier 2 — Core interview material

## 3.2.1 Polymorphism in depth — **[HOT]**

- [ ] **[HOT]** Overloading vs overriding vs hiding
- [ ] Virtual functions, vtable dispatch cost
- [ ] Abstract classes and interfaces
- [ ] Runtime type identification: `dynamic_cast`, `typeid`
- [ ] The four casts: `static_cast`, `dynamic_cast`, `const_cast`, `reinterpret_cast`

## 3.2.2 The diamond problem — **[ASKED]**, expect it again

> Exact setup asked: class A is the base; B and C each inherit from A;
> D inherits from both B and C. All five bullets below must be answerable.

- [ ] **The problem** — D holds two separate copies of A's members, one via B and one via C: ambiguity on `d.someMemberOfA`, plus duplicated state that can diverge
- [ ] **Why it happens** — each inheritance path constructs its own A subobject
- [ ] **The fix** — virtual inheritance: `class B : virtual public A`, `class C : virtual public A` → D has exactly one shared A subobject
- [ ] **The consequences of the fix** — the most-derived class (D) becomes responsible for constructing the virtual base A, bypassing B's and C's constructor calls to it; layout changes, virtual base pointers added, `sizeof` grows, member access becomes indirect, real runtime cost
- [ ] **Alternative answer worth giving** — avoid the diamond entirely: composition, or make A a pure interface with no data members (no state → no duplicated state)

## 3.2.3 Virtual classes and dynamic allocation — **[ASKED]**

- [ ] "Virtual class" in interview usage means either a class with virtual functions **or** a virtually-inherited base — clarify which is meant, be ready for both
- [ ] Abstract classes cannot be instantiated; they exist to be inherited
- [ ] **[HOT]** Why polymorphism requires pointers or references, never objects by value (object slicing)
- [ ] Dynamic allocation of polymorphic objects: `Base* obj = new Derived();`
- [ ] **[HOT]** Why `delete obj` through a base pointer needs a virtual destructor — without it only `~Base()` runs and derived resources leak
- [ ] Modern form: `std::unique_ptr<Base> obj = std::make_unique<Derived>();`

## 3.2.4 SOLID — all five, one concrete example each

- [ ] **S**ingle Responsibility
- [ ] **O**pen/Closed
- [ ] **L**iskov Substitution — **[HOT]**, most asked
- [ ] **I**nterface Segregation
- [ ] **D**ependency Inversion — **[HOT]**, second most asked

## 3.2.5 Composition over inheritance — **[HOT]**

- [ ] is-a vs has-a
- [ ] When inheritance is the wrong tool
- [ ] Why deep hierarchies are fragile — the fragile base class problem

---

# Tier 3 — Advanced

## 3.3.1 Design patterns

- [ ] **Strategy** — swap algorithms behind one interface
- [ ] **Factory** and Abstract Factory
- [ ] **Singleton** — and **[HOT]** why it is widely considered an anti-pattern (global state, untestable, thread-safety hazards)
- [ ] **Observer** — publish/subscribe; maps directly to Kafka fan-out in DeliverIQ
- [ ] Decorator, Adapter, Builder

## 3.3.2 Advanced OOP concepts

- [ ] Multiple inheritance beyond the diamond
- [ ] Virtual function tables in multiple-inheritance layouts
- [ ] CRTP (Curiously Recurring Template Pattern) — static polymorphism
- [ ] Pimpl idiom and compile-firewall benefits
- [ ] Coupling and cohesion

---

## Block G drill list — every [HOT] / [ASKED] in this file

- [ ] **[ASKED]** Diamond problem — all five bullets, unprompted
- [ ] **[ASKED]** Virtual classes and dynamic allocation
- [ ] `struct` vs `class`
- [ ] Overloading vs overriding vs hiding
- [ ] Why polymorphism needs pointers or references — object slicing
- [ ] Why `delete` through a base pointer needs a virtual destructor
- [ ] Liskov Substitution — with an example
- [ ] Dependency Inversion — with an example
- [ ] Composition over inheritance
- [ ] Why Singleton is an anti-pattern
