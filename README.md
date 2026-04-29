# psycho-plugin

A Claude Code plugin for psychologists and therapists. Helps with case conceptualization, goal-setting, session strategy, and supervision — using the Lazarus BASIC ID model, with support for CBT and SFBT/ORCT approaches.

> **Language note:** The skills interact in Russian (as the author's working language), but the framework is universal. Contributions adapting the interface to other languages are welcome.

---

## What it does

Four skills that support your therapy workflow:

| Skill | When to use | What it does |
|-|-|-|
| `/psy:conceptualize` | After a session | Analyzes a transcript → updates client conceptualization using BASIC ID |
| `/psy:goals` | After goals discussion | Refines therapy contract and goals using 6-criteria framework |
| `/psy:strategy` | Before a session | Builds session strategy from conceptualization + goals |
| `/psy:supervise` | For self-supervision | Scores your work on 4 axes (conceptualization, goals, strategy, therapist performance) |

**The workflow:**

```
Session → transcript → /psy:conceptualize → /psy:goals
                                                   ↓
                          /psy:strategy ← next session
                                   ↓
                          /psy:supervise (anytime)
```

Each client has a single markdown file (`clients/name.md`) that accumulates across sessions: conceptualization, goals, strategy, and session history.

---

## Requirements

- [Claude Code](https://claude.ai/code) (CLI or desktop app)

---

## Installation

```bash
claude plugin install https://github.com/LeoDiKadyrov/psycho-plugin
```

Or from a local clone:

```bash
git clone https://github.com/LeoDiKadyrov/psycho-plugin
claude plugin install ./psycho-plugin
```

---

## Setup

### 1. Fill the knowledge base

The `references/` folder contains structured guides that the skills use during analysis. They come pre-filled with a general framework, but you can enrich them with your own training materials, supervision notes, or case examples.

| File | Contents |
|-|-|
| `references/lazarus-basic-id.md` | Lazarus multimodal model (BASIC ID), complaint→problem→request schema |
| `references/cbt.md` | CBT conceptualization, 6 goal criteria, session structure, intervention types |
| `references/orct.md` | SFBT principles, miracle question, EARS structure, solution-focused techniques |

The more you put in, the better the skills perform.

### 2. Create a client file

Copy the template and fill in the basics:

```bash
cp templates/client-template.md clients/anna.md
```

Then edit the frontmatter:

```yaml
---
modality: cbt   # or: orct
name: anna
created: 2026-05-01
---
```

---

## Usage

### After a session

```
/psy:conceptualize anna transcript.txt
```

Analyzes the transcript through the BASIC ID lens. Shows proposed updates to the conceptualization. You confirm before anything is written.

If you don't have a file, paste the transcript directly:

```
/psy:conceptualize anna
> Вставь текст транскрипта сессии:
[paste here]
```

### Working with goals

```
/psy:goals anna transcript.txt
```

Evaluates the goal against 6 criteria (positive framing, no third parties, achievable by therapy means, specific, realistic, clear). Shows three goal states (vague → medium → clear) and checks ecological validity.

### Before next session

```
/psy:strategy anna
```

Builds a session plan: problem frame → targets (what worsens / what helps) → priority → interventions → session structure → homework. Saves to `clients/anna.md`.

### Supervision

```
/psy:supervise anna transcript.txt
```

Scores 4 axes (0–10 each, total /40):
- Conceptualization quality
- Contract & goals
- Strategy
- Therapist performance

With specific examples from the transcript. Highlights strengths alongside growth areas.

### Override modality per-call

```
/psy:strategy anna orct
```

Overrides the modality set in `anna.md` for this call only.

---

## Client file structure

```markdown
---
modality: cbt
name: anna
created: 2026-05-01
---

## Концептуализация (BASIC ID)

### B — Поведение (Behavior)
### A — Аффект (Affect)
### S — Ощущения (Sensation)
### I — Образы (Imagery)
### C — Когниции (Cognition)
### I — Межличностные отношения (Interpersonal)
### D — Биология (Drugs/Biology)

---

## Контракт и цели

### Запрос клиента
### Согласованные цели терапии
### Критерии достижения

---

## Стратегия

---

## История сессий
```

Client files are in `.gitignore` — they stay on your machine.

---

## Repository structure

```
psycho-plugin/
├── .claude-plugin/
│   └── plugin.json          # plugin metadata
├── skills/
│   ├── conceptualize/
│   │   └── SKILL.md         # psy:conceptualize
│   ├── goals/
│   │   └── SKILL.md         # psy:goals
│   ├── strategy/
│   │   └── SKILL.md         # psy:strategy
│   └── supervise/
│       └── SKILL.md         # psy:supervise
├── references/
│   ├── lazarus-basic-id.md  # BASIC ID knowledge base
│   ├── cbt.md               # CBT knowledge base
│   └── orct.md              # SFBT/ORCT knowledge base
├── templates/
│   └── client-template.md   # starting point for new clients
└── clients/                 # your client files (gitignored)
```

---

## Contributing

PRs welcome. The most useful contributions:

- **Enrich the knowledge base** — add depth to `references/cbt.md` or `references/orct.md` based on evidence-based protocols
- **Add a new modality** — create `references/gestalt.md` or `references/act.md` and update the skills to support it
- **Improve a skill** — better output format, smarter analysis logic, clearer instructions in `SKILL.md`
- **Translate the interface** — adapt skills to work in English, German, Spanish, etc.
- **Add a new skill** — ideas: `/psy:homework` (design between-session tasks), `/psy:progress` (track goal progress over time), `/psy:intake` (structure initial assessment)

### How to contribute

1. Fork the repo
2. Create a branch: `git checkout -b feat/your-feature`
3. Make your changes
4. Open a PR with a clear description of what changed and why

Skills are just markdown files — no code required to contribute.

---

## Approach

This plugin is built around the **Lazarus BASIC ID multimodal model** as a universal conceptualization framework, combined with approach-specific knowledge (CBT, SFBT/ORCT). The framework comes from [Psychodemia](https://psychodemia.ru) — a Russian-language psychology training school.

The skills follow the school's clinical framework:
- Complaint → Problem → Request → Conceptualization
- 6 goal criteria (positive, realistic, no third parties, achievable by therapy, specific, clear)
- 4-step strategy planning (problem frame → targets → priority → interventions)
- 7-element session structure

---

## License

MIT — see [LICENSE](LICENSE)
