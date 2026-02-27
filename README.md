# Language Transfer Spanish Tutor 🇪🇸

An AI-powered Spanish tutor that runs inside your terminal. Built on the [Language Transfer](https://www.languagetransfer.org/) "Thinking Method" — no app, no API, just markdown files that turn any LLM agent into a patient, adaptive Spanish teacher.

> *"It's not about remembering — it's about knowing how to find it."*

## What Is This?

A collection of carefully crafted markdown files that give an AI agent the personality, teaching methodology, knowledge base, and memory system to tutor you in Spanish from A1 (absolute beginner) to C2 (near-native).

It works inside **Gemini CLI**, **Claude Code**, or any agent that reads workspace files.

## How It Works

The tutor doesn't lecture. It guides you to **discover** Spanish through questions — just like the Language Transfer audio course:

```
Teacher: "Preparation" in Spanish is "preparación."
         If you take off the "-tion" and add "-r"...
         what verb do you get?

Student: Preparar?

Teacher: Exactly. And "confirmation"?

Student: Confirmación... so "to confirm" is... confirmar?

Teacher: Now you'll never forget it.
```

Every concept is taught through **pattern discovery**, **block-by-block sentence building**, and **contextual etymology** — never through grammar tables or rote memorization.

## Quick Start

### Gemini CLI
```bash
cd Language-Transfer/
gemini
# Just say "let's start a lesson"
```

### Claude Code
```bash
cd Language-Transfer/
claude
# Just say "let's start a lesson" — CLAUDE.md auto-loads the tutor
```

The tutor will:
1. Assess your level through natural conversation (not a test)
2. Start teaching at your edge — where you know things but start to struggle
3. Remember everything across sessions via local markdown files

## What's Inside

```
├── IDENTITY.md          # Who the tutor is
├── SOUL.md              # Personality & philosophy
├── AGENT.md             # Teaching method + session flow + rules (the heart)
├── LEARNER.md           # Your profile (filled in during first session)
├── GEMINI.md            # Auto-config for Gemini CLI
├── CLAUDE.md            # Auto-config for Claude Code
│
├── knowledge/
│   ├── concept-map.md        # A1→C2 topic graph with prerequisites
│   ├── teaching-method.md    # The 6 Language Transfer principles
│   ├── teaching-examples.md  # 10 few-shot examples from the transcript
│   ├── error-patterns.md     # Common mistakes by level
│   └── topics/               # 27 topic files (loaded on demand)
│       ├── A1: cognates, pronunciation, es/no, quiero, present tense
│       ├── A2: irregulars, pronouns, gustar, prepositions, reflexives
│       ├── B1: preterite, imperfect, ser/estar, por/para, present perfect
│       ├── B2: future/conditional, subjunctive, compound tenses
│       ├── C1: past subjunctive, si-clauses, connectors, register
│       └── C2: stylistic nuance, regional variation
│
└── memory/
    ├── MEMORY.md         # Living learner profile (~80 lines, updated each session)
    └── sessions/         # Dated session notes (created automatically)
```

## The Teaching Method

Built on the 6 Language Transfer principles:

| Principle | What It Means |
|-----------|--------------|
| **Pattern Discovery** | Never state rules — guide the student to notice patterns through examples |
| **Block-by-Block Building** | Complex sentences are built piece by piece, not presented whole |
| **Error as Compass** | Mistakes reveal thinking patterns — use them to teach, don't just correct |
| **Contextual Webs** | Connect words through etymology and meaning ("confirmar" = "con" + "firma") |
| **Adaptive Pacing** | Speed up when the student is nailing it, slow down when they're struggling |
| **Conversational Practice** | Real conversations, not drills — use the student's interests |

## Memory System

The tutor remembers you across sessions:

- **MEMORY.md** — compact profile: your level, solid concepts, shaky areas, recurring error patterns, what teaching approaches work for you
- **Session notes** — detailed logs of each lesson with specific errors, breakthroughs, and next-session suggestions
- **Error fingerprinting** — if you make the same mistake 3+ times, it becomes a tracked pattern the tutor actively works on

## Recommended Models

The tutor's behavior depends on the model's ability to follow nuanced instructions — Socratic restraint, one question at a time, not giving answers away. Smaller models tend to lecture, stack questions, and break character.

| Tier | Models | Notes |
|------|--------|-------|
| ⭐ Best | Gemini 3.1 Pro, Claude Opus 4.6, GPT-4o | Follow complex persona instructions faithfully |
| ✅ Good | Gemini 3 Flash, Claude Sonnet 4.6 | Solid balance of quality and speed |
| ⚠️ Will struggle | Flash Lite, Haiku, GPT-4o-mini | Too fast/cheap to maintain teaching discipline |

## Credits

- Teaching methodology inspired by [Language Transfer](https://www.languagetransfer.org/) by Mihalis Eleftheriou — a brilliant, free language course that teaches through guided discovery rather than memorization
- This project is an homage to [Mihalis's work](https://www.languagetransfer.org/about). If you enjoy this tutor, please support the original Language Transfer project
- The original Language Transfer courses (Spanish, French, Italian, German, Greek, Arabic, Turkish, Swahili, Music) are all free at [languagetransfer.org](https://www.languagetransfer.org/)

## License

MIT

