# Framework

This section abstracts the failure patterns documented in the case studies into reusable analytical principles. It is not a methodology document, a tutorial, or a set of best practices derived from general research literature. Every concept here traces back to a specific failure that occurred in this system.

---

## Why This Exists

A case study documents an incident. A framework documents a pattern. The distinction matters because incidents are specific — they describe a failure in a particular context with a particular mechanism — while patterns are structural. They appear across different incidents, different systems, and different domains, because they reflect a property of how complex systems fail rather than a property of any individual failure.

The case studies in this repository describe seven distinct failures. Taken individually, each is instructive. Taken together, they reveal patterns that a post-mortem format cannot efficiently express: that certain failure types are inherently harder to detect than others; that some failures are fixable and others are structural; that the sequence in which research is conducted systematically determines which failures are caught early and which compound into expensive dead ends.

This framework is the attempt to make those patterns explicit.

---

## What This Is Not

This framework does not describe how to build a system. It does not describe how to avoid all failure — some failures are only identifiable after they occur. It does not generalize from these failures to claims about research methodology in other domains. The scope is deliberately narrow: the failure patterns documented here, abstracted to the level where they are recognizable across contexts without losing the precision that makes them useful.

---

## Components

**[failure-taxonomy.md](failure-taxonomy.md)**
A classification of the failure types observed, with definitions, detection difficulty, and the distinction between fixable and structural failures. Provides vocabulary for the other documents.

**[feasibility-checklist.md](feasibility-checklist.md)**
A structured set of tests to perform before building a model or committing to a research direction. Each test targets a specific failure mode that, historically, was not caught until after significant work had been invested.

**[kill-criteria.md](kill-criteria.md)**
Conditions under which continued work on a research direction should stop. Includes signals that distinguish productive iteration from structural impasse, and hard decision rules derived from failure patterns.

**[constraint-first-research.md](constraint-first-research.md)**
The sequencing principle that emerges from all of the above: why beginning with constraints rather than models changes which failures are caught, when they are caught, and what the cost of catching them is.

---

## How to Use This

Read the case studies before reading this section. The framework will be abstract without the concrete incidents behind it. Read the taxonomy first — it establishes the vocabulary used in the other documents. The feasibility checklist and kill criteria are operational; they are written to be applied, not just read.

The framework is not a guarantee. It is a structured record of how these specific failures could have been caught earlier, expressed at a level of generality that makes them applicable to future research directions.

---

*All failures referenced here are documented in [/case-studies](../case-studies/).*
