# Iron Marty

**A self-improvement project in conjunction with AI.**

Iron Marty is a GitHub-tracked personal health management system powered by **Dr. Paul AI**, an AI health advisor persona built in [Cursor](https://cursor.sh). Named after [Dr. Paul Saladino](https://carnivoremd.com), whose animal-based/carnivore dietary framework informs the approach.

## What This Is

This repository is a living, version-controlled record of one man's pursuit of better health — with AI as an analytical partner. It combines:

- **Medical records and lab data** — structured for AI analysis
- **An AI health advisor (Dr. Paul AI)** — persistent across sessions, tracking commitments, analyzing trends, and recommending protocol adjustments
- **Weekly check-in system** — structured Sunday reviews with data from MyFitnessPal (food/activity) and Oura Ring (sleep/recovery)
- **Version-controlled history** — every change tracked via Git and `CHANGELOG.md`

## File Inventory

| File | Purpose |
|------|---------|
| `marty_health_context.md` | Source of truth — conditions, medications, supplements, diet, exercise, labs |
| `dr_paul_current_health_plan.md` | Living health plan — goals, protocol summary, active concerns, action items |
| `dr_paul_ai_analysis.md` | Baseline deep-dive analysis of the full health profile |
| `dr_paul_persona.md` | AI persona definition, operating instructions, check-in protocol |
| `dr_paul_weekly_plan.md` | Concise printable weekly plan (kitchen-friendly) |
| `dr_paul_med_timing.md` | Optimized daily medication and supplement timing schedule |
| `dr_paul_check_in_history.md` | Running log of all weekly check-in entries (SOAP format) |
| `CHANGELOG.md` | All project changes by date |
| `.cursor/rules/dr_paul_ai.mdc` | Cursor rule — auto-loads the Dr. Paul AI persona every session |

## How It Works

### Weekly Check-in (Sundays)

1. Marty shares data: current weight, Oura Ring sleep data, MyFitnessPal food/activity logs, subjective report
2. Dr. Paul AI asks structured questions covering sleep, nutrition, exercise, supplements, caffeine, and action item progress
3. All documents are updated based on the check-in
4. A new printable weekly plan is generated
5. Changes are committed to Git

### Document Flow

```
.cursor/rules/dr_paul_ai.mdc (auto-loads persona)
        |
        v
dr_paul_persona.md (instructions) --> dr_paul_current_health_plan.md (active plan)
        |                                       |
        v                                       v
marty_health_context.md (data)          dr_paul_weekly_plan.md (printable)
        |                               dr_paul_med_timing.md (timing)
        v                               dr_paul_check_in_history.md (log)
dr_paul_ai_analysis.md (baseline)
```

## Philosophy

- **Carnivore/animal-based** — dietary approach informed by Dr. Paul Saladino
- **Evidence-informed, not dogmatic** — research is contextualized, conventional guidelines questioned when warranted
- **Lipid interpretation** — Saladino framework: metabolic health markers (fasting insulin, TG/HDL ratio, Apo B) prioritized over LDL-C alone
- **Supplement minimalism** — fill carnivore diet gaps, optimize sleep and cortisol, minimize total count
- **Accountability-driven** — commitments are tracked, followed up on, and not allowed to disappear
- **Physician-collaborative** — flags items for physician review, recommends questions to ask, does not replace clinical care

## Disclaimer

Dr. Paul AI is an AI-powered health tracking and analysis tool. All recommendations are informational and do not constitute medical advice. Medication and supplement changes should always be discussed with prescribing physicians. Lab interpretation should be reviewed by a qualified clinician in the context of the full medical history.
