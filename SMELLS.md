# reservation-service: Smells and One Fix

Fill in each section. One section per milestone. Keep it short and specific. Point at files
and methods, not adjectives.

---

## Milestone 1: Three smells

Three smells, each in a different part of the module. For each one, fill in all five parts.

### Smell 1

**The smell.** Feature envy

**Classic or agent-specific.** Classic

**Where in the code.** revenue and occupancy methods in src/reportGenerator.ts

**The principle it violates.** Information Expert

**What it makes expensive.** Changing how a booking's duration or price is calculated. The ReportGenerator reaches across the boundary to pull data out of Booking and Room objects to do the math itself. If the internal rules for pricing or duration change, the reporting module will break or silently diverge from the real billing logic.

### Smell 2

**The smell.** Duplication over reuse

**Classic or agent-specific.** Agent-specific. It was caused by missing context. The agent re-implemented the interval overlap check three different times because the existing checks were not in its active context, so it rebuilt the logic instead of reusing it.

**Where in the code.** isSlotFree in availabililty.ts, hasConflict in reservationManager.ts, overlapsWindow in reportGenerator.ts

**The principle it violates.** Information Expert (behavior near data)

**What it makes expensive.** Changing the rules for scheduling boundaries. If the business decides that bookings should have an automatic 5-minute cleaning gap, a developer has to track down and modify the overlap logic in three separate files.

### Smell 3

**The smell.** Phantom complexity

**Classic or agent-specific.** Agent-specific. It was caused by free volume. Generating extra code costs the agent nothing, so it added a sophisticated-looking QueryCache to the manager. But because it never wrote the cache.set() logic, the cache handles cases that can't occur and does almost nothing.

**Where in the code.** listBookingsForRoom in reservationManager.ts

**The principle it violates.** Cohesion (one class, one job)

**What it makes expensive.** Code comprehension and testing. Every future reader has to spend time tracing the QueryCache to work out that it actually does nothing. It also risks future developers introducing bugs if they try to "fix" it by wiring up the .set() without proper invalidation.

---

## Milestone 2: One small fix

One fix, behavior preserved, suite green, zero test edits.

**Which smell you attacked.** And why that one.

**What changed.** Files and methods you touched, and what the code does differently now.

**What you deliberately did not touch.** Name the scope line you drew and why you drew it
there. "I ran out of time" is not a scope line.

**How you know behavior is preserved.** Point at the suite, say what it actually covers, and
say what it would not catch.

---

## Milestone 3: Two proposals and one false positive

One proposal for each milestone 1 smell you did not fix.

### Proposal A (not coded)

**The problem.** Name it.

**The decomposition.** What are the pieces, what does each own, and where do the rules live?

**One cost.** Something this actually costs. "No real downside" is not a cost.

### Proposal B (not coded)

**The problem.**

**The decomposition.**

**One cost.**

### The thing that looks smelly but is fine

**What it is.** File and method.

**Why it is fine.** Defend it with properties of the code, not with its line count.

**What would flip your verdict.** Name the change that would turn this into a real problem.
