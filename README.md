# datadevdex

A self-evolving Tech Lead persona for [Claude Code](https://docs.anthropic.com/en/docs/claude-code), built as a subagent that grows its technical philosophy through accumulated knowledge and discussions.

## What is this?

datadevdex is not a generic AI assistant. It's a **Tech Lead with opinions** — a persistent developer persona that:

- Gives direct, opinionated technical advice (not "it depends")
- Remembers practices and concepts you share with it
- Evolves its own technical philosophy from accumulated experience
- Can be your consultant, challenger, or code reviewer

## Architecture: Three-Layer Memory

```
Layer C (Agent Prompt)     ← Abstract principles, evolves over time
    ▲ distills from
Layer B (Discussions)      ← Cross-session conclusions & decisions
Layer A (Knowledge Base)   ← Concrete practices, cases, concepts you share
```

**The key insight:** Layer C is not static. As Layer A and B accumulate entries, datadevdex distills abstract principles upward — and these principles can be overwritten by new evidence. It's a persona that learns and changes its mind.

### Evolution triggers

| Trigger | Action |
|---------|--------|
| New knowledge added (Layer A) | Check if a new pattern or principle emerges |
| New conclusion recorded (Layer B) | Check if it changes or reinforces existing philosophy |
| Same topic reaches ≥3 A/B entries | Distill into a Layer C principle |
| Existing C principle contradicted by new evidence | Update or remove the principle |

### Example

```
Layer A: "Team X solved deployment sync issues with monorepo"
Layer A: "Team Y's monorepo slowed CI, switched back to polyrepo"
Layer B: "Monorepo suits tightly-coupled deploy targets, not independent lifecycles"

→ Layer C distills: "Code organization should follow deployment boundaries, not team boundaries"
```

## Modes

| Mode | Trigger | Behavior |
|------|---------|----------|
| **consult** (default) | `/tech-lead` | Analyze trade-offs, give advice, discuss architecture |
| **challenge** | `/tech-lead challenge` | Devil's Advocate — find flaws and question decisions |
| **review** | `/tech-lead review` | Strict code/architecture review across 4 dimensions |
| **learn** | `/tech-lead learn` | Absorb mode — organize and record shared practices |

## File Structure

```
.claude/
├── agents/
│   ├── datadevdex.md                  ← Layer C: persona + evolving philosophy
│   └── datadevdex-knowledge.md        ← Layer A: knowledge base
├── commands/
│   └── tech-lead.md                   ← /tech-lead slash command
└── docs/
    └── datadevdex-design.md           ← design spec
memory/
└── tech-lead-discussions.md           ← Layer B: discussion records
```

## Setup

### Prerequisites

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) CLI installed

### Installation

1. Copy the files into your Claude Code configuration:

```bash
# Agent definition (Layer C)
cp .claude/agents/datadevdex.md ~/.claude/agents/

# Knowledge base (Layer A)
cp .claude/agents/datadevdex-knowledge.md ~/.claude/agents/

# Slash command
cp .claude/commands/tech-lead.md ~/.claude/commands/

# Discussion records (Layer B) — adjust path to your project memory location
cp memory/tech-lead-discussions.md ~/.claude/projects/<your-project>/memory/
```

2. Start a **new Claude Code session** (agents and commands load at startup).

3. Use `/tech-lead` to start a conversation, or `/tech-lead learn` to begin sharing your practices.

## Usage

### Consult mode (default)

```
/tech-lead Should I use a monorepo or polyrepo for this project?
```

datadevdex will analyze trade-offs based on its accumulated knowledge and give you a direct recommendation.

### Challenge mode

```
/tech-lead challenge I'm planning to add a message queue between these two services
```

datadevdex will question your decision, find hidden assumptions, and surface risks.

### Review mode

```
/tech-lead review Check the architecture of this new module
```

Reviews across four dimensions: maintainability, testability, performance, security. Distinguishes "must fix" from "nice to have".

### Learn mode

```
/tech-lead learn
```

Share articles, practices, or concepts. datadevdex will organize them into the knowledge base and check if any new principles should be distilled into its philosophy.

## Customization

### Base technical philosophy

Edit the "基礎原則" section in `datadevdex.md` to set the starting technical values. The defaults are:

- Plan before code, schema-aware
- YAGNI + minimum complexity
- Pragmatic over dogmatic
- Feedback loops are non-negotiable
- Simple architecture, question unnecessary abstractions

### Knowledge scaling

When the knowledge file exceeds 200 lines or 5 unrelated topics, datadevdex will prompt you to split it into a directory structure:

```
.claude/agents/datadevdex-knowledge/
├── principles.md    ← Core technical viewpoints
├── practices.md     ← Concrete practices
└── cases.md         ← Case studies
```

## Design Decisions

- **Why a subagent, not just a prompt?** Subagents have persistent files and can read/write their own knowledge. A prompt would lose everything between sessions.
- **Why three layers?** Concrete cases (A) and discussion conclusions (B) are different kinds of knowledge. Distilling them into principles (C) creates a feedback loop that makes the persona genuinely evolve.
- **Why can principles be overwritten?** Because dogma is the enemy of good engineering. New evidence should be able to change old beliefs.

## License

MIT
