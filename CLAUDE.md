# psyskills — Development Guidelines

## Quick Start

```bash
# Install locally
claude plugin install /path/to/psyskills

# Install from GitHub
claude plugin install https://github.com/LeoDiKadyrov/psycho-plugin
```

## Skills & Usage

| Skill | Command | Purpose |
|-|-|-|
| psy:conceptualize | `/psy:conceptualize <client> [transcript] [modality]` | Session transcript → BASIC ID update |
| psy:goals | `/psy:goals <client>` | Goal-setting via 6 criteria |
| psy:strategy | `/psy:strategy <client>` | Session strategy (4-step) |
| psy:supervise | `/psy:supervise <client>` | Supervision on 4 axes /40 |

## Client Files

Each client lives in `clients/<name>.md`. Create from template:

```bash
cp templates/client-template.md clients/имя.md
```

Required frontmatter:

```yaml
---
modality: cbt   # or orct
name: имя_клиента
created: YYYY-MM-DD
---
```

## Clinical Reference

`references/` contains authoritative clinical source material:
- `lazarus-basic-id.md` — BASIC ID model (Lazarus)
- `cbt.md` — CBT framework, goal criteria, session structure
- `orct.md` — SFBT/ORCT principles

`Psychodemia.md` is the upstream source (gitignored — obtain separately from the school).



## Ethical Hard Constraints (AIMHC Framework)

These constraints are non-negotiable. They apply to all features, prompts, and skill logic.

### Never do
- AI must not diagnose or treat. psyskills assists the clinician — it does not replace clinical judgment.
- Do not pass client PHI or session content to any public API or third-party service without explicit HIPAA-compliant safeguards and a BAA.
- Do not build features that create emotional dependency on AI (companion-style engagement, flattery loops, retention mechanics).

### Always do
- Keep the clinician in control. Every AI output is a suggestion; the clinician decides.
- Support autonomy — outputs should help the clinician think, not think for them.
- Assume the clinician will disclose AI use to clients. Don't design features that make disclosure awkward.

## Chatbot Evaluation Checklist (C.H.A.T.B.O.T)

When integrating or recommending any AI tool in skills:

| Criterion | Question |
|-|-|
| Consent | Does the user know what they're agreeing to? |
| Human Oversight | Is a clinician reviewing outputs? |
| Autonomy | Does it support independence or create dependency? |
| Transparent | Clear data/privacy policy? Can user delete data? |
| Built on Evidence | Clinical expertise behind it? Crisis protocol? |
| Objective | Goal-focused? Culturally inclusive? |
| Task Fit | Is AI the right tool here, or is a human better? |

## Skill Design Principles

- Skills are clinical tools, not companions. Tone: professional, structured, brief.
- Every skill output must be reviewable and editable by the clinician before use.
- Skills operate on `clients/*.md` files — never read or write outside that scope without user intent.
- `Psychodemia.md` is the authoritative clinical source. Don't contradict it.

## Project Structure

```
skills/          # Claude Code skills (one dir per skill)
references/      # Clinical reference material (lazarus, cbt, orct)
templates/       # Client file templates
clients/         # Per-client markdown files (gitignored except .gitkeep)
```

## Source

Ethical framework: [AI Mental Health Collective](https://www.aimentalhealthcollective.com), Dr. Rachel Wood, 2026.
