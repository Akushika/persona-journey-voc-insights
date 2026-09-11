# persona-journey-voc-universal

## What this skill does

Turns your customer research and product signals (interviews, support tickets, VOC feedback, analytics, surveys, market research) into evidence-based persona updates, a journey map, and a change-over-time analysis — connected to business value where the evidence supports it.

Works with any AI model, any industry, any research format. Outputs a self-contained HTML dashboard you can view in a browser. Every insight links back to something you provided — nothing is made up.

This is a single-file, portable version of a multi-prompt internal skill (source inventory → signal extraction → persona synthesis → journey mapping → behavior-change analysis → business-value linkage → validation → dashboard publishing). All eight stages are folded into the steps below so it can run standalone, without a project-specific folder structure or schema files.

---

## One rule that applies throughout

> Every insight, quote, metric, or recommendation must trace back to:
> - Something in your research or persona materials (quoted directly)
> - A metric you supplied, with its source and date
> - A principle from this framework (labelled as such)
>
> If it can't be traced, it won't appear.
> Labels used: `[From: filename — "quote"]`, `[From: research — "source, date"]`, `[Framework principle — not from your materials]`

Never invent evidence, quotes, metrics, personas, journey stages, or business impact. Never merge a weak signal into a confirmed finding. Preserve contradictions instead of resolving them into a false consensus.

**Two kinds of "missing" get treated differently:**
- **Declined** — the user was asked and explicitly said "I don't have this" (e.g. no journey model, no existing dashboard). This is not a failure. Use the built-in default, label it as a default, and continue.
- **Claimed but unusable** — the user said the input exists (a persona file, a journey model, an existing dashboard schema, a prior run) but it can't be found, read, or parsed. This **blocks**. Do not substitute a default for something that was supposed to exist. Stop and output the `FAILURE:` block below instead of guessing.

### Explicit failure rule

When a blocking check fails, stop immediately and output exactly:

```
FAILURE:
- step: <step name, e.g. "Step 1 — source validation">
- reason: <specific check that failed>
- missing_or_conflicting_input: <exact file, path, or claim that couldn't be resolved>
- required_human_action: <what the user needs to provide or fix before this can continue>
```

Do not continue past a `FAILURE:`. Do not guess at a fix. Do not publish a partial dashboard or report as if it were the final output.

---

## Step 0 — Setup questions

Answer these before analysis starts. Required questions are marked. Optional ones add depth but aren't blocking.

### A — Your team and the goal

**A1 — Required**
What team are you on, and what do you do?
*e.g. "UX Research for a B2B commerce platform" / "CX insights, retail"*

**A2 — Required**
What product or service is this research about, and where are you in the roadmap right now?

**A3 — Required**
What's the main question you need this analysis to answer?
*e.g. "Are our personas still accurate?" / "Where in the journey are we losing people?" / "What changed since last quarter?"*

**A4 — Optional**
Who will read this output? `UX / Design` `Product` `Research` `Leadership` `Support / CS`

### B — Your existing personas

**B1 — Required**
Share your existing persona documents, if any:
- **Folder or file path** (`.md`, `.txt`, `.pdf`)
- **Paste** the content directly — include at minimum: name/role, goals, pain points
- **None yet** — the skill will draft baseline personas from the evidence in Step B and flag them as evidence-derived rather than team-authored

**B2 — Optional**
If you have more than 5 personas, which ones matter most for this analysis?

### C — Your research and evidence

**C1 — Required**
Share your source material. Any mix is fine:
- Customer interviews / usability studies
- Support tickets
- VOC (voice of customer) feedback
- Product analytics exports
- Survey responses
- Market research
- Existing journey-mapping notes

Share as a folder path, pasted text, or file paths. For each source, note its type and date if known.

**C2 — Optional**
Do you have a defined journey-stage model already (e.g. Awareness → Evaluation → Onboarding → Use → Support)? If not, the default 7-stage model in Step 5 will be used.

**C3 — Optional**
Business metrics you want insights connected to (adoption, retention, conversion, satisfaction, support burden, revenue). Share numbers with their source and date — unsourced numbers will be excluded.

**C4 — Optional**
Is this a first analysis, or a repeat? If repeat, share the prior output (dashboard JSON, report, or summary) so this run can detect what changed rather than starting cold. If you say "repeat" but the file can't be located or read, this blocks — either supply it or say "first run" instead.

**C5 — Optional**
Do you already have a dashboard app, JSON contract, or fixed schema this output must plug into? If yes, share it — the output must match it exactly, or the run fails rather than inventing a different shape. If no, the built-in schema in Step 8 is used.

### D — Output preferences

**D1 — Required**
What do you want at the end?
- `Dashboard only` — HTML file, opens in browser
- `Dashboard + written reports` — HTML plus markdown reports per section
- `Dashboard + slide content` — HTML plus bullet points for a presentation

**D2 — Optional**
Anything to focus on, or leave at full scope (personas + journey + behavior change + business value)?

---

## Step 1 — Confirm what was received, then run the blocking gate

Before analysis, list back to the user:
1. Every source file/folder received, grouped by type (interview, support, VOC, analytics, survey, market research, journey notes, other) — and for each, whether it's usable, partially usable, or unusable, with a reason if not usable
2. Every persona document received, or confirmation that none exist yet
3. Which of the approved source types have no coverage at all (a gap to flag, not to fill)
4. Any stats or claims with no stated source — these will be excluded from the output
5. Whether this is a first run (baseline) or a comparison run (prior output supplied)
6. Whether an existing dashboard schema was supplied (C5), and whether it's readable

Ask the user to confirm this list before moving on.

**Blocking gate — all of these must pass before Step 2 begins:**

1. At least one source is usable (not just partially usable, not zero).
2. Every persona file the user said they'd provide is actually readable — if one was claimed but can't be found or parsed, this blocks (do not silently drop it or draft a replacement).
3. If C4 said "repeat run," the prior output is readable. If it isn't, block — do not fall back to first-run mode without telling the user why.
4. If C5 supplied a schema, it's readable and well-formed. If it's broken or partially specified, block rather than half-following it.

If any of these fail, stop and output the `FAILURE:` block from above with `step: "Step 1 — blocking gate"`. Do not proceed to Step 2 on a failed gate, and do not produce a partial dashboard to compensate.

---

## Step 2 — Extract signals from the evidence

Read every usable source and pull out only what's directly supported by it. For each finding, capture:

- source type, file/name, and date (if known)
- persona or segment it relates to (if identifiable)
- journey stage it relates to (if identifiable)
- the signal type: `pain_point`, `unmet_need`, `motivation`, `blocker`, `friction`, `behavior_signal`, `sentiment`, `opportunity`, `business_signal`
- the evidence excerpt (a real quote or observation — never paraphrased into something stronger than what was said)
- confidence: `high` / `medium` / `low` / `weak_signal` — based on repetition, source quality, and directness

Keep observed evidence, interpreted insight, and recommendation clearly separate — never blend them into a single unsupported statement.

---

## Step 3 — Update personas

For each persona (existing or evidence-derived), determine, using only what the evidence supports:

- who they are, their goals, motivations, pain points, blockers, behaviors, decision drivers, unmet needs
- **change status**: `confirmed` (multiple independent sources agree it changed), `emerging` (a pattern starting to appear), `unchanged`, `weak_signal`, or `contradicted` (evidence on both sides — show the conflict, don't resolve it)
- business relevance, if evidence supports it
- a confidence level and any unresolved questions

Do not invent a new persona unless the user explicitly said none exist yet (B1) — in that case, label the persona as `[Draft — derived from evidence, not yet validated by your team]`. If the user said personas exist but none passed the Step 1 gate, this already blocked — don't reach Step 3.

**Cross-persona comparison** (required whenever more than one persona is in scope):
- Shared pain points, unmet needs, and blockers affecting more than one persona
- Divergences — where similar personas need meaningfully different things
- Systemic signals appearing across unrelated personas
- Personas with unusually weak evidence coverage

Only surface a cross-persona pattern when at least two independent personas' evidence sets support it.

---

## Step 4 — Map the journey

Use the user's journey-stage model if supplied (C2). If they said one exists but it can't be located or read, block here and output `FAILURE:` — do not quietly substitute the default. If they explicitly said they don't have one, use this default and adapt stage names to the user's domain:

| Stage | Customer goal | Actions | Friction | Support needs | Opportunity |
|-------|---------------|---------|----------|----------------|-------------|
| Awareness / Discovery | | | | | |
| Evaluation / Research | | | | | |
| Decision / Onboarding | | | | | |
| Daily Use | | | | | |
| Support / Recovery | | | | | |
| Renewal / Expansion | | | | | |

For each stage, fill in only what the evidence supports: customer goals, actions, touchpoints, emotional state, friction, blockers, support burden, opportunities, and which personas experience it differently. Mark weakly supported stages clearly rather than filling gaps with assumption. Every opportunity needs a paired risk or friction it addresses — don't list one without the other.

Surface across all stages:
1. Biggest friction concentration
2. Highest-value opportunity moment
3. Notable persona-to-persona differences at the same stage

---

## Step 5 — Behavior change over time

**If this is a first run:** don't attempt a before/after comparison. Instead, produce a baseline snapshot — label every finding `baseline`, not `confirmed change` or `emerging`, and note that change detection becomes available once this baseline exists to compare against next time.

**If prior output was supplied:** compare current evidence against it by persona, journey stage, theme, and sentiment. For each change, label it `confirmed` (evidence at 2+ time points or independent sources), `emerging`, `weak_signal`, or `ambiguous`. Only name a likely driver when the evidence supports it — otherwise mark it `[inference]`. Never invent a trend direction.

Output: change summary by persona and by journey stage, strongest confirmed shifts, emerging signals, likely drivers, and unresolved questions.

---

## Step 6 — Connect to business value

Only where evidence supports it, map findings to: adoption, activation, engagement, conversion, retention, churn risk, satisfaction, support burden, or revenue-related outcomes.

Label every linkage as one of: **direct evidence** (a metric was supplied), **strong inference**, **weak inference**, or **unsupported**. Unsupported linkages do not get published as validated outcomes — state plainly that no business linkage could be established, rather than forcing one.

Output: top high-confidence risks, top high-confidence opportunities, and the areas that matter but currently have no measurement — flagged as research gaps, not failures.

---

## Step 7 — Validate before publishing

Before generating the dashboard, check the full output set:

- Does every major claim trace to a source-backed record from Step 2?
- Do the persona, journey, and behavior-change sections contradict each other anywhere without explanation?
- Is any weak signal being presented as confirmed?
- Is any business-value statement missing its evidence label?
- Are unresolved questions and evidence gaps still visible, or did they get quietly dropped?

If a check fails, don't publish — say specifically what's missing or unsupported and what would resolve it. If everything passes, proceed to the dashboard.

---

## Step 8 — Dashboard and local server

**If C5 supplied an existing dashboard schema or contract:** the output must conform to it exactly — same field names, same structure, same file path if one was specified. Do not invent a different shape "to make it better." If any required field in that schema has no validated finding to fill it, write an explicit null/empty value with a note, rather than fabricating content to complete the schema. If the schema and the evidence genuinely conflict (e.g. it demands a metric type nothing in the sources supports), block and output `FAILURE:` rather than forcing a fit.

**Otherwise**, generate one self-contained HTML file using the built-in structure below.

**File name:** `[team-name]_persona_journey_insights_[YYYY-MM-DD].html`

**What to include:**

**Overview tab**
- Team, product, date, run mode (baseline / comparison)
- Table: all personas with change status and confidence
- Top 3 findings and top 3 recommended next actions

**One tab per persona**
- Summary, goals, pain points, blockers, change status, confidence
- Most critical finding with its direct quote
- Unresolved questions

**Journey tab**
- The stage table from Step 4, with friction and opportunity highlighted
- Biggest friction concentration, highest-value opportunity, persona differences

**Cross-persona & business value tab**
- Shared pain points and unmet needs across personas
- Business-value linkages with their evidence label
- Behavior-change summary (or baseline notice on a first run)

**Validation tab**
- Evidence gaps, contradictions, stale or weak areas, recommended next research

**Styling:**
- Dark header with team/product name
- Green (#1E8B4E) / Orange (#D46B08) / Red (#C0392B) for confidence or risk
- No external libraries — all CSS/JS inline, tab switching via a `switchTab()` function
- Every card includes a traceability footer with source and quote

**Local server:**
```bash
cd /path/to/output/folder
python3 -m http.server 8765
# Open: http://localhost:8765/[filename].html
```
If running inside Claude Code (CLI with Bash access): save the file to a folder the user confirms, launch the server automatically, and print the URL. Stop with `Ctrl+C`.

If the user asked for reports or slide content instead of (or in addition to) the dashboard, produce those as plain markdown covering the same sections.

---

## Output rules

1. **No invented statistics.** Missing benchmark → write `[No benchmark provided — validate with your own research]`.
2. **No inferring from job titles or assumptions.** Only use what's written in the source material.
3. **No vague language.** Don't write "users may feel" — write what the source actually said.
4. **Label everything.** `[From: file.md — "quote"]` for evidence, `[Framework principle]` for framework logic, `[inference]` for reasoned-but-unconfirmed conclusions.
5. **Flag incomplete personas and thin evidence up front**, and note which sections are limited as a result.
6. **Confidence is conservative.** A vague mention gets `weak_signal` or `partial`, never `confirmed`.
7. **Contradictions stay visible.** Never collapse conflicting evidence into a single tidy answer.

---

## How to use this with different AI tools

**Claude (Claude Code):** Save to `.claude/commands/persona-journey-voc-universal.md`. Run with `/persona-journey-voc-universal`.

**ChatGPT / GPT-4:** Paste this file as your first message or system prompt, then share your materials in the next message.

**Gemini or other models:** Same — paste as system prompt, then share materials.

**Sharing with your team:** Send the `.md` file. Each person runs it independently against their own research folder; outputs can be compared side by side.

**Adapting for a new domain:** Replace the default journey stages in Step 4 and the business-value categories in Step 6 with ones relevant to your industry. Everything else works as-is.

---

## If your inputs are incomplete

The skill distinguishes "you don't have this" (fine — use the default) from "you said you had it but it's not usable" (blocks — see the Explicit failure rule above).

| Input | If explicitly declined | If claimed but unusable |
|-------|------------------------|--------------------------|
| At least one usable evidence source | Not allowed — this is required, run does not start | Blocks — ask for a readable source |
| Persona documents | Baseline personas are drafted from evidence and marked as drafts | **Blocks** — do not draft a replacement silently |
| Journey-stage model | Default 7-stage model is used | **Blocks** — do not fall back to default silently |
| Business metrics with source | Business-value section states no linkage could be established | Unsourced numbers are excluded, not blocking, but noted |
| Prior run output | Treated as a first run; baseline snapshot is written | **Blocks** — do not silently downgrade to first-run mode |
| Existing dashboard schema | Built-in schema in Step 8 is used | **Blocks** — do not invent a different shape |

---

*persona-journey-voc-universal — version 1.1 — September 2026*
*Consolidates source inventory, signal extraction, persona synthesis, journey mapping, behavior-change analysis, business-value linkage, validation, and dashboard publishing into one portable file.*
*v1.1 adds a blocking validation gate and explicit FAILURE reporting — declined inputs get a default, claimed-but-unusable inputs stop the run.*
*Works with any LLM. Any industry. Any research format. Every insight cites its source.*
