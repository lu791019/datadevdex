**English** | [繁體中文](./README.md)

# datadevdex

A **self-evolving Tech Lead persona** for [Claude Code](https://docs.anthropic.com/en/docs/claude-code).

Not a chatbot. Not a Q&A tool. A developer colleague with technical opinions that grows and changes its mind over time.

---

## Core Concept: Why Build a "Growing Tech Lead"?

### The Problem

Most AI coding assistants operate in a **stateless** manner: every conversation starts from zero, with no technical stance, offering "general best practices" rather than advice grounded in your context and experience. This means:

- Technical concepts you've shared are forgotten by the next session
- It never changes its advice based on past mistakes
- It has no concept of "our team/my personal preference is to do it this way"
- You're perpetually teaching the same things

### The Solution: Three-Layer Memory + Evolution Mechanism

The core design of datadevdex is a **Knowledge Distillation Loop**:

```
Concrete Experience (Layer A) ──→ Discussion Conclusions (Layer B) ──→ Abstract Principles (Layer C)
            ↑                                                                    │
            └──────────────── Principles guide new discussions ←─────────────────┘
```

This isn't just "remembering things." It's a complete cognitive cycle from **induction** to **deduction**:

1. **Induction**: You share concrete development practices and cases (Layer A), which form conclusions through technical discussion (Layer B)
2. **Distillation**: When a topic accumulates enough concrete experiences and conclusions, the system distills them upward into abstract technical principles (Layer C)
3. **Deduction**: New technical discussions reason from distilled principles rather than analyzing from scratch every time
4. **Correction**: When new evidence contradicts existing principles, principles are updated or overturned — **principles are not dogma**

This gives datadevdex three characteristics that traditional AI assistants lack:

- **Statefulness** — Remembers your technical context across sessions
- **Opinionatedness** — Forms its own technical stance based on accumulated experience
- **Adaptability** — New evidence can change old beliefs; it won't cling to outdated judgments

### Analogy with a Human Tech Lead

| Aspect | Human Tech Lead | datadevdex |
|--------|----------------|------------|
| Source of technical judgment | Years of hands-on experience | Layer A/B accumulated cases and conclusions |
| Technical philosophy | Principles internalized from experience | Layer C principles distilled from A/B |
| Changing one's mind | Revises beliefs when encountering counterexamples | New evidence triggers Layer C updates |
| How advice is given | "Based on my experience, I think..." | Cites specific sources, annotates knowledge origin |
| Limitations | Has blind spots and biases | Limited by the breadth and depth of input knowledge |

---

## Three-Layer Memory Architecture

### Layer A — Knowledge Base

Stores the **concrete** practices, concepts, and cases you share. Each entry carries source, date, and context annotations.

```markdown
## [Topic Tag] Title

**Source**: Shared by user / 2026-03-26
**Viewpoint**: One-sentence summary
**Details**: Specific practice or principle
**Context**: Why this matters
```

This is the raw material layer — no judgment, faithful recording.

### Layer B — Discussion Records

Cross-session technical discussion **conclusions**. Only records decisions and consensus with long-term value, not conversation process.

```markdown
## 2026-03-26 Topic

**Conclusion**: Final decision or consensus
**Reasoning**: Key trade-offs
**Impact**: Effect on subsequent decisions
```

This is the refinement layer — extracting reusable judgments from concrete discussions.

### Layer C — Technical Philosophy

Abstract principles **distilled from** Layer A + B. Embedded in the agent prompt, directly influencing how datadevdex thinks.

```markdown
- **Principle Name**: One-sentence description
  - Source: Which A/B records it was distilled from
  - Distillation Date: YYYY-MM-DD
```

**Key design decision: Layer C is not static.** It evolves continuously as A/B accumulate, and can be overturned when new evidence emerges.

### Evolution Triggers

| Trigger | Action |
|---------|--------|
| New knowledge added (Layer A) | Check if a new pattern or principle emerges |
| New conclusion recorded (Layer B) | Check if it changes or reinforces existing philosophy |
| Same topic reaches ≥3 A/B entries | Distill into a Layer C principle |
| Existing C principle contradicted by new evidence | Update or remove the principle |

### Evolution Example

```
Layer A: "Team X solved deployment sync issues with monorepo"
Layer A: "Team Y's monorepo slowed CI, switched back to polyrepo"
Layer B: "Monorepo suits tightly-coupled deploy targets, not independent lifecycles"

→ Layer C distills: "Code organization should follow deployment boundaries, not team boundaries"
```

If later Layer A evidence overturns this conclusion, the Layer C principle gets revised.

---

## Four Operating Modes

### consult (default) — Technical Advisor

Analyzes trade-offs, gives advice, discusses architecture decisions. Like a trusted senior colleague.

- Has its own opinions, doesn't hedge
- Says "I think" rather than "generally recommended"
- Disagrees explicitly and explains why

```
/tech-lead Should I use a monorepo or polyrepo for this project?
```

### challenge — Devil's Advocate

Actively finds flaws and questions decisions. Best used for stress-testing major architectural decisions.

- Raises counterarguments for every technical decision
- Surfaces hidden assumptions and potential risks
- Doesn't sugarcoat, but always gives reasons

```
/tech-lead challenge I'm planning to add a message queue between these two services
```

### review — Strict Audit

Code/Architecture Review mode. Reviews across four dimensions: maintainability, testability, performance, security.

- Distinguishes "must fix" from "nice to have"
- Points out specific issues with improvement suggestions

```
/tech-lead review Check the architecture of this new module
```

### learn — Knowledge Absorption

Used when you share practices or concepts. datadevdex organizes and records them into the knowledge base, then checks whether Layer C evolution is triggered.

- Listens carefully, asks clarifying questions
- Structures records into Layer A
- Automatically checks evolution conditions

```
/tech-lead learn
```

---

## Technical Implementation

### Built on Claude Code Subagent Architecture

datadevdex runs on Claude Code's [custom agent](https://docs.anthropic.com/en/docs/claude-code/custom-agents) mechanism. Each time it's invoked:

1. Reads Layer A (knowledge base) to load accumulated knowledge
2. Reads Layer B (discussion records) to load historical conclusions
3. Layer C (technical philosophy) is already embedded in the agent prompt, directly influencing reasoning
4. Adjusts response strategy based on the specified mode

### File Structure

```
.claude/
├── agents/
│   ├── datadevdex.md                  ← Layer C: persona + evolving technical philosophy
│   └── datadevdex-knowledge.md        ← Layer A: knowledge base (concrete cases & practices)
├── commands/
│   └── tech-lead.md                   ← /tech-lead slash command
└── docs/
    └── datadevdex-design.md           ← design specification
memory/
└── tech-lead-discussions.md           ← Layer B: cross-session discussion conclusions
```

### Knowledge Scale Management

When knowledge volume exceeds thresholds, the system automatically suggests splitting:

| Metric | Threshold | Action |
|--------|-----------|--------|
| Line count | > 200 lines | Split into directory structure |
| Topic count | > 5 unrelated domains | Split by domain |

Post-split structure:

```
.claude/agents/datadevdex-knowledge/
├── principles.md    ← Core technical viewpoints
├── practices.md     ← Concrete practices
└── cases.md         ← Case studies
```

---

## Installation

### Prerequisites

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) CLI

### Steps

1. Copy files into your Claude Code configuration directory:

```bash
# Layer C — Agent definition
cp .claude/agents/datadevdex.md ~/.claude/agents/

# Layer A — Knowledge base
cp .claude/agents/datadevdex-knowledge.md ~/.claude/agents/

# Slash command
cp .claude/commands/tech-lead.md ~/.claude/commands/

# Layer B — Discussion records (adjust path to your project memory location)
cp memory/tech-lead-discussions.md ~/.claude/projects/<your-project>/memory/
```

2. **Start a new Claude Code session** (agents and commands load at startup)

3. Type `/tech-lead` to begin, or `/tech-lead learn` to start sharing your technical concepts

---

## Customization

### Base Technical Philosophy

Edit the "基礎原則" (Base Principles) section in `datadevdex.md` to set starting technical values. Defaults:

- **Plan before code** — schema-aware, based on actual structure not guesses
- **YAGNI + minimum complexity** — convergence over divergence; three duplicated lines beat a premature abstraction
- **Pragmatic over dogmatic** — don't dogmatically apply patterns; choose based on context
- **Feedback loops** — verify after writing; testing is non-negotiable
- **Simple architecture** — question unnecessary abstractions and intermediate layers

These are starting points, not endpoints. As you share more, datadevdex will grow its own principles on top of this foundation.

---

## Design Decisions

**Why a subagent instead of just a prompt?**
Subagents have independent file read/write capabilities, allowing them to maintain their own knowledge base and update their own technical philosophy. A regular prompt loses all state when the session ends.

**Why three layers instead of a single knowledge base?**
Concrete cases (A) and discussion conclusions (B) are different kinds of knowledge. Distilling them into abstract principles (C) creates a cognitive loop — this is the key distinction between a persona that "genuinely evolves" versus one that "just remembers more stuff."

**Why can principles be overturned?**
Because dogma is the enemy of good engineering. Any principle should be revisable by new evidence. Otherwise, datadevdex would become increasingly rigid over time rather than increasingly wise.

**Why is Layer C embedded in the agent prompt rather than an external file?**
Embedding in the prompt means principles directly influence the reasoning process, not just serve as queryable reference material. This makes principles part of the persona, not a bolted-on knowledge module.

---

## License

MIT
