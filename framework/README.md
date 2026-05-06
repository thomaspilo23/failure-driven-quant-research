# Framework

This section abstracts the failure patterns documented in the case studies into reusable analytical principles. It is not a methodology document, a tutorial, or a set of best practices derived from general research literature. Every concept here traces back to a specific failure that occurred in this system.

---

## Why This Exists

A case study documents an incident. A framework documents a pattern. The distinction matters because incidents are specific and bounded — they describe a failure in a particular context with a particular mechanism — while patterns are structural. They appear across different incidents, different systems, and different domains, because they reflect a property of how complex systems fail, not a property of any individual failure.

The case studies in this repository describe seven distinct failures. Taken individually, each is instructive. Taken together, they reveal patterns that a post-mortem format cannot efficiently express: that certain failure types are inherently harder to detect than others; that some failures are fixable and others are structural; that the sequence in which research is conducted determines which failures surface early and which compound into expensive dead ends.

A repeated failure is not repeated evidence of bad execution. It is repeated evidence that the underlying assumption may be wrong.

This framework makes those patterns explicit — precisely enough to be recognizable, but general enough to apply beyond the incidents that generated them.

---

## What This Is Not

This framework does not describe how to build a system. It does not claim to prevent all failure — some failures are only identifiable after they occur. It does not generalize from these specific failures to universal claims about research methodology. The scope is narrow by design: the failure patterns observed here, abstracted to the level where the mechanism is visible without the specifics that make it system-dependent.

---

## Components

**[failure-taxonomy.md](failure-taxonomy.md)**
Five failure types with strict definitions, detection difficulty, typical misdiagnosis, and the fixable/structural classification. Establishes vocabulary used throughout the other documents.

**[feasibility-checklist.md](feasibility-checklist.md)**
Structured tests to perform before committing to a research direction. Each targets a specific failure mode that was not caught until after significant work had been invested. Includes the Failure of Overlap Rule — the mathematical condition under which a direction is infeasible regardless of model quality.

**[kill-criteria.md](kill-criteria.md)**
Conditions under which continued work must stop. Distinguishes productive iteration from structural impasse. Includes the Escalation Rule — how to correctly update beliefs when multiple independent approaches fail identically.

**[constraint-first-research.md](constraint-first-research.md)**
The sequencing principle that emerges from the above: why addressing constraints before models changes which failures are caught, when they are caught, and what catching them costs.

---

## How to Use This

Read the case studies before this section. The framework will be abstract without the concrete incidents behind it. Read the taxonomy first — it establishes vocabulary used in the other documents. The feasibility checklist and kill criteria are operational and written to be applied.

---

*All failures referenced here are documented in [/case-studies](../case-studies/).*
