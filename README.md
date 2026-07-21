# Second Brain

*A personal knowledge system where the human only captures, and the AI does the organizing, maintaining, and synthesizing.*

This repo documents the **design** of a second brain I'm building — the thinking, not the contents. It's a living systems-design project, not a library to `pip install`. I'm publishing the architecture because the architecture isn't the moat; the discipline of running it is.

---

## The problem

Most "second brain" systems die the same way: **the cost of maintaining them grows faster than the value they return.** You capture more, you organize more, you tag more — and eventually the upkeep isn't worth it, so the system rots.

The usual response is better folders, better tags, better apps. That's treating the symptom. The real bottleneck isn't storage or search — it's **getting the right context to the right place at the right moment.**

## The thesis

Invert the maintenance model.

> **The human only captures. The AI organizes, maintains, and synthesizes.**

Capture has to cost under ten seconds and zero decisions — you drop a thought, you don't file it. Everything downstream (distilling, linking, pruning, surfacing) is the engine's job. The system's north star is a single sentence that doubles as a design filter:

> *A context compiler that helps you — and your AIs — think, decide, and operate better.*

The differentiator isn't "think better" (every note app claims that). It's **context on hand for you and your AIs** — portable context for a human+AI workflow. Everything in the system is measured against one question: *did this help me think, decide, or operate?* If it didn't, it doesn't belong.

## AI-agnostic by design

This is the core bet, so it's worth stating plainly: **the system depends on no single AI.** The brain is just plain-text Markdown on your disk — permanent, portable context that any model can read, today's or next year's. The AI engine is a swappable part pointed at the vault, not a dependency baked into it. Switch models, use two at once, or hand a slice of context to a third — the knowledge stays put and keeps compounding. Most tools lock your thinking inside one vendor's product; here the context is the asset and the model is the commodity.

An easy, concrete combination to run it: **Obsidian** (the vault — local Markdown, no lock-in) plus **Claude** (the engine, pointed at that vault). But that's a convenience, not a requirement — any Markdown editor and any capable model will do.

## Design principles

1. **Frictionless capture, zero organizing.** You deposit; you never file. If capture costs more than ten seconds, the system dies.
2. **The AI writes the knowledge; the human writes the sources.** Strict separation.
3. **Plain text is the source of truth.** Markdown — portable, no lock-in, readable by any AI engine present or future. Backup and sync are optional and mechanism-free; the system runs 100% local.
4. **The schema is the product.** The folder structure is trivial. The value lives in the constitution and the modules.
5. **Compounding over retrieval.** This isn't a chatbot over your notes. It's a knowledge base that gets *better* with every ingestion, not just bigger.
6. **No output is one-directional.** Everything the system produces feeds back into it.
7. **Minimal core, modules by tier.** The base brain works on day one. A fixed kernel with a living interior: deliberate evolution instead of entropy.

## Architecture at a glance

Four layers over a system layer that governs them all:

```
  CAPA 4 · OUTPUTS        what the system produces (drafts, decisions, projects)
  CAPA 3 · INTELLIGENCE   the AI engine, pointed at the vault
  CAPA 2 · BRAIN          distilled knowledge, held in three memories
  CAPA 1 · INPUTS         raw captured evidence — immutable
─────────────────────────────────────────────────────────────────
  ⚙ CAPA 0 · SYSTEM       the constitution + rules that govern everything
```

Two invariants hold the whole thing together:

- **Inputs are immutable.** Nobody edits them — not even the AI. They're the evidence that lets the brain be recompiled from scratch. Their value is *quality*, not volume. (Hoarding is failure mode #1.)
- **Synthesis never lives outside the vault.** When context goes out to another tool, whatever comes back returns to the inputs. The vault stays the single source of truth.

### The three memories

The engine doesn't scan thousands of notes. It starts from a small working memory and navigates links into the depths — the same way you don't re-read your whole life to answer a question.

| Memory | What it holds | When it loads |
|---|---|---|
| **Working** | Who you are, what you're on, what's open, where you left off | Every session, automatically |
| **Area** | A living summary per area: state, key entities, live decisions, preferences | On demand — one file per area |
| **Deep** | The detail, one page per thing | Only when something specific is queried |

Session startup loads *only* the constitution plus working memory. Everything else is loaded just-in-time. That's what keeps it fast.

### Flows: three types

Every flow in the system is one of three — the same three any LLM-managed knowledge base needs (h/t Karpathy's LLM-wiki framing of *ingest · query · lint*):

- **Ingest** — distill new material into the brain, one source at a time, *in dialogue*. The conversation is the value, so this never runs unattended.
- **Query / produce** — use the brain: draft an output in your voice, or brief a decision with past decisions and trade-offs.
- **Maintain** — keep the system healthy: report, prune, and once a month propose changes to the schema itself.

## A few hard-won lessons

The parts I'd actually put on a whiteboard:

- **Guaranteed behavior goes inside a flow that runs; suggested behavior goes in a description.** An "always-on" rule written in prose loses the attention race against whatever the current task is — I watched a critical review step silently fail to fire, twice in one day, until it became an explicit step in a flow that actually executes. This one reshaped the whole system.
- **Anti-entropy: the constitution doesn't grow.** Every new rule has to justify why it can't live in a workflow, and name which rule it replaces. The default is *don't add*.
- **Emergence over predefinition.** Categories, contacts, and expert lenses aren't defined up front — they emerge from real use, at concrete thresholds, and nothing enters without approval. You can't design the right taxonomy before you have the data.
- **The metric is outputs, not inputs.** If captured material grows for a month and nothing gets produced, the system is failing — no matter how full it looks. This kills the collector's fallacy by construction.
- **Real friction beats design on paper.** Nearly every major revision came from days of actual use, not theory. Elegant-but-unused is just wrong.
- **The dialogue is the value.** Ingestion is a conversation, not an automation. Automating the thinking would kill the product.

## Status

Kernel built and running on my own instance; adoption is deliberately staged — install the capture-and-ingest habit first, let structure emerge, add reflection and decision tooling as the habit holds. Building in public, iterating from real use. Expect this document to change as the system teaches me things.

## What's deliberately not here

The design is open; the instance is not. This repo intentionally leaves out my personal data and the specifics of how the system gets tailored to a given person or context — those are the parts that take judgment to get right, and they're not interesting as text anyway. If you want to talk architecture, that's what this page is for.

---

<sub>Personal project by Jonas López. Notes and framing are my own; the engine is Claude.</sub>
