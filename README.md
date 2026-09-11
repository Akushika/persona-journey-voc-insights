# Persona, Journey Map & VOC Insights Skill

A single, portable prompt skill for UX designers and researchers: turn your customer research and product signals into evidence-based persona updates, a journey map, a behavior-change analysis, and a business-value read — connected wherever the evidence supports it, and flagged as a gap where it doesn't.

Works with any AI model (Claude, ChatGPT, Gemini), any industry, any research format. Outputs a self-contained HTML dashboard you can open in a browser. Every insight in the output links back to something you provided.

The full skill lives in [`skill/persona-journey-voc-universal.md`](skill/persona-journey-voc-universal.md) — that's the only file you need.

---

## Quick start

**Claude (Claude Code):**
```bash
cp skill/persona-journey-voc-universal.md ~/.claude/commands/persona-journey-voc-universal.md
```
Then run `/persona-journey-voc-universal`.

**ChatGPT / GPT-4 / Gemini:**
Paste the full contents of `persona-journey-voc-universal.md` as your first message or system prompt, then share your persona and research materials in the next message.

**Sharing with your team:**
Send them this repo (or just the one `.md` file). Each person runs it independently against their own research folder; outputs can be compared side by side.

---

## What you need before you start

- At least one usable research/evidence source (interviews, support tickets, VOC feedback, analytics, surveys, market research) — pasted, or as a folder/file path
- A clear answer to "what question should this analysis answer?"

Everything else — existing personas, a journey-stage model, business metrics, a prior run to compare against, an existing dashboard schema — is optional. The skill asks for it up front (Step 0 in the file) and uses sensible defaults when you say you don't have it.

If you say something exists but it turns out to be missing or unreadable, the skill stops and tells you exactly what's wrong rather than guessing or silently falling back to a default — see the "Explicit failure rule" in the skill file.

---

## What you get

- Per-persona coverage: goals, pain points, blockers, change status, confidence, and unresolved questions — each tied to a quote from your materials
- A cross-persona comparison: shared pain points, divergences, and systemic gaps
- A journey map with friction, opportunity, and persona differences per stage
- A behavior-change read (or a labeled baseline snapshot, if this is your first run)
- Business-value linkages, labeled by evidence strength — never presented as fact when it isn't
- A self-contained HTML dashboard, plus optional markdown reports or slide content

---

## Traceability

Every insight is labeled as one of:
- `[From: filename — "direct quote"]`
- `[From: research — "source name, date"]`
- `[Framework principle — not from your materials]`
- `[inference]`

Nothing appears without a label. No statistic is used without a source you provided.

---

*This repo intentionally ships one skill file, not a multi-step pipeline — see `persona-journey-voc-universal.md` for the full method.*
