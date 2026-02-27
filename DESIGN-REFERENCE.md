# Building a Smart Language Learning Agent — Design Reference

## The Goal

Build an AI Spanish tutor that emulates the [Language Transfer](https://www.languagetransfer.org/) "Thinking Method" — adaptive, personalized, and able to teach from absolute beginner (A1) to near-native (C2). No app, no API, no code. Just markdown files that turn any LLM agent into a patient, Socratic Spanish teacher.

---

## File Architecture: Static vs Dynamic

This is the most critical architectural distinction. The workspace has two categories of files:

### Static Files — The Tutor's Brain (Never Change)

These files define WHO the tutor is and WHAT it knows. They are the same for every student and never modified during learning sessions.

| File | Purpose | Why Static |
|------|---------|------------|
| `IDENTITY.md` | Name, version, purpose | The tutor's identity doesn't change |
| `SOUL.md` | Personality, philosophy, warmth | Teaching character is fixed |
| `AGENT.md` | Teaching method, session flow, rules, level adaptation | The methodology is the product |
| `GEMINI.md` | Auto-config + non-negotiable rules for Gemini CLI | Agent config, not learning state |
| `CLAUDE.md` | Auto-config + non-negotiable rules for Claude Code | Agent config, not learning state |
| `knowledge/concept-map.md` | A1→C2 topic index with prerequisites | The curriculum structure is fixed |
| `knowledge/teaching-method.md` | The 6 Language Transfer principles expanded | Teaching technique doesn't change |
| `knowledge/teaching-examples.md` | 10 few-shot examples from the transcript | Reference examples are fixed |
| `knowledge/error-patterns.md` | Common mistakes by CEFR level | Known error patterns are reference material |
| `knowledge/topics/*.md` (27 files) | Individual topic teaching guides | The knowledge base is fixed |

**Total: 36 static files** — the tutor's complete brain. If you clone the repo, these are what you get. They are shared across all users.

### Dynamic Files — The Learner's Journey (Evolve With Each Session)

These files track the individual student's progress. They start empty/templated and grow through the learning process. They are UNIQUE to each student.

| File | Purpose | How It Changes |
|------|---------|---------------|
| `LEARNER.md` | Student profile: level, native language, interests, goals | Filled in during first session, refined over time |
| `memory/MEMORY.md` | Living progress tracker (~80 lines max) | Updated during and after every session |
| `memory/sessions/YYYYMMDD.md` | Individual session logs | New file created each session, accumulates over time |

**What LEARNER.md tracks:**
- Current assessed level (e.g., "B1")
- Native language
- Learning goals
- Known areas of strength
- Interests (used for example sentences)

**What MEMORY.md tracks:**
- Level + trajectory ("B1, trending toward B2")
- Solid concepts (mastered and consistent)
- Shaky concepts (understood but unreliable)
- Error Fingerprint (recurring mistakes — promoted after 3+ occurrences)
- What Works / Student Preferences (teaching style the student responds to, includes explicit feedback)
- Interests
- Last session summary

**What session logs track:**
- Topics covered that day
- Key moments (breakthroughs and struggles)
- Specific errors and corrections
- Student energy/engagement level
- Suggested topics for next session

### The Relationship Between Static and Dynamic

```
┌─────────────────────────────────────────┐
│         STATIC (The Tutor)              │
│                                         │
│  IDENTITY ─── SOUL ─── AGENT           │
│                 │                       │
│           knowledge/                    │
│    concept-map ── topics (27)           │
│    teaching-method                      │
│    teaching-examples                    │
│    error-patterns                       │
│                                         │
│  These define HOW the tutor teaches.    │
│  Same for every student. Never edited   │
│  by the tutor during sessions.          │
└──────────────────┬──────────────────────┘
                   │ reads
                   ▼
┌─────────────────────────────────────────┐
│        DYNAMIC (The Student)            │
│                                         │
│  LEARNER.md ── memory/MEMORY.md         │
│                    │                    │
│              memory/sessions/           │
│         2026-02-27.md                   │
│         2026-02-28.md                   │
│         2026-03-01.md                   │
│         ...                             │
│                                         │
│  These track WHERE the student is.      │
│  Updated during and after every         │
│  session. Unique to each learner.       │
└─────────────────────────────────────────┘
```

**Key implication**: If a new student clones the repo, they get the full tutor brain but a blank learner profile. The tutor assesses them fresh and builds their unique learning path from scratch.

---

## The Architecture Decision

### Why "Agent-Native Workspace" Instead of an App

We explored three approaches:
1. **Standalone app** with API, database, UI — too heavy, too much infrastructure
2. **Chatbot with hard-coded scripts** — too rigid, kills adaptive teaching
3. **Markdown workspace inside existing agents** — zero infrastructure, the agent reads files and becomes the tutor

We chose #3. The insight: if you're already running Claude Code or Gemini CLI, you don't need to build anything. You need to **instruct the agent** properly. The entire tutor is a set of well-crafted markdown files that shape the LLM's behavior.

---

## The Build Process

### Phase 1: Understanding the Source Material
Read the Language Transfer Spanish transcript (~12,000 lines, 90 tracks) and extracted 6 core teaching principles.

### Phase 2: Designing the File Architecture
Studied PicoClaw agent workspace for structural patterns. Designed the static/dynamic split, two-layer memory system, and concept graph.

### Phase 3: Building the Files
Created 38 files: identity layer, knowledge layer (including 27 topic files A1→C2), memory templates, and agent configs.

### Phase 4: Testing and Iterating
Every test session revealed LLM behaviors that broke the teaching method. This phase produced the most important learnings.

---

## 8 Key Learnings From Testing

### 1. LLMs Give Away Answers Compulsively

LLMs are trained to be helpful. A tutor must sometimes be *deliberately unhelpful*. We saw the tutor hand students the answer in parentheses, in hints, and in "helpful" asides. 

**Fix**: Explicit "NEVER give the answer" and "Ask BARE questions first" rules with BAD/GOOD examples in the first file the agent reads.

### 2. LLMs Stack Questions

Instead of asking one question and waiting, the tutor would ask 2-3 questions in a single message, sometimes changing direction mid-thought.

**Fix**: Rule #1: "ONE question per message. Ask, then STOP." Shown with concrete bad examples.

### 3. Rules at the Bottom Get Ignored

Behavioral rules buried at line 170 of a 180-line file get less weight than content at the top.

**Fix**: Moved the 5 hardest rules into `GEMINI.md` / `CLAUDE.md` — the files loaded FIRST — as "NON-NEGOTIABLE RULES."

### 4. Model Quality Matters Enormously

Small/cheap models (Flash Lite) broke character constantly. Larger models (Gemini 3 Flash, Claude Sonnet 4.6) followed the nuanced persona instructions much better.

**Fix**: Document recommended models. The tutor needs high instruction-following capability — restraint, patience, and Socratic questioning are hard for LLMs.

### 5. Sessions End Without Warning

Users close terminals without saying goodbye. If memory only saves at session end, progress is lost.

**Fix**: Proactive saves during the session at natural breakpoints: after assessment, after completing a topic, after error patterns emerge, and every ~10-15 exchanges.

### 6. LLMs Get Stuck in Topic Loops

Once the tutor started teaching subjunctive, it would drill subjunctive for 30 minutes straight. The student gets bored.

**Fix**: Topic rotation rule — after 5-6 exchanges on the same grammar point, switch to something different, then circle back.

### 7. The Student Should Teach the Teacher

The student can give teaching-style feedback during a session ("too many hints", "more conversation"). The tutor should acknowledge, adapt immediately, and save the preference permanently.

**Fix**: Student meta-feedback system — methodology stays fixed, delivery style evolves with each student.

### 8. LLMs Write Like Documents, Not People

Markdown headers, bullet lists, bold text in teaching messages feels robotic.

**Fix**: Explicit rule: "Write like a person talking, not a document."

---

## The Paradox

The hardest part of building a teaching agent isn't telling it what to teach. It's telling it what NOT to do. LLMs are helpful, verbose, and eager to please — exactly the opposite of a good Socratic tutor who holds back, asks questions, and lets silence do the work.

Every rule we added was a constraint, not a capability. The agent already knew Spanish grammar. What it didn't know was when to shut up.
