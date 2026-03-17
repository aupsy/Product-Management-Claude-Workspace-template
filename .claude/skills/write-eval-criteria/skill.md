---
name: write-eval-criteria
description: Generate a rigorous eval criteria document for a product feature, grounded in domain expert research and your team's benchmarking methodology
user-invocable: true
argument-hint: <feature-name> e.g. lead-profiling or outreach-personalization
allowed-tools: WebSearch, WebFetch, Read, Glob, Write
---

# Write Eval Criteria

Generates `Knowledge/product/evals/[feature-slug]-eval-criteria-YYYYMMDD.md` using a task-based, LLM-as-judge scoring framework. Before proposing any metrics, the skill researches how expert practitioners in the domain evaluate this problem — ensuring rubrics reflect real-world best practices, not just product spec language.

**User's request:** $ARGUMENTS

---

## Step 1: Understand the Feature

Read the following in parallel:

1. **Feature PRD or initiative overview** — look for `Projects/[feature-folder]/PRD-*.md`, `PROJECT.md`, or `INITIATIVE-OVERVIEW*.md`
2. **Reference eval** — check `Knowledge/product/evals/` for any existing benchmark or eval methodology doc to align on format and scoring approach
3. **Existing evals** — glob `Knowledge/product/evals/*.md` to check if a related eval already exists that can inform or be extended

Extract as `$FEATURE_CONTEXT`:
- What the feature does (inputs to outputs)
- Who it is for (buyer persona: seller, marketer, admin, analyst, etc.)
- What success looks like (from PRD success metrics)
- Any quality targets or reliability NFRs stated in the PRD
- The MVP/phased roadmap stages if present

---

## Step 2: Research Domain Expert Guidance

**This step is mandatory.** Before proposing any metrics, research how human experts and top practitioners in the relevant domain approach this problem. The goal is to understand what excellence looks like from a practitioner perspective — not just what the product spec says.

### 2a. Identify the domain

From `$FEATURE_CONTEXT`, determine the real-world human workflow this feature is automating or augmenting. Examples:
- Lead profiling: B2B prospecting and buying committee mapping
- Outreach personalization: enterprise cold outreach and account-based marketing
- Lead qualification: BANT/MEDDIC methodology
- Research briefs: competitive intelligence and account research
- Content generation: editorial quality and audience targeting
- Data enrichment: data quality and coverage assessment

### 2b. Web research

Run 2-3 targeted web searches to find practitioner guidance:
- "How do top [domain] professionals evaluate [task]?"
- "[Domain task] best practices evaluation criteria"
- "AI agent evaluation [domain task] framework"

Use `WebFetch` to read the most relevant 1-2 results in full.

Store key insights as `$EXPERT_GUIDANCE`:
- What dimensions do human experts use to judge quality in this domain?
- What are the hard cases that separate good from great?
- What failure modes do practitioners warn against?
- Any named frameworks or methodologies worth encoding into rubrics?

**If the user has already provided expert, customer, or domain leader guidance** in the conversation, use that directly as `$EXPERT_GUIDANCE` and skip or supplement web research.

---

## Step 3: Design the Eval Framework

Combine `$FEATURE_CONTEXT` + `$EXPERT_GUIDANCE` to design the framework.

### 3a. Identify evaluable stages

Determine how many distinct stages the feature has. Each stage should represent a meaningfully different output type with its own quality dimensions. Examples:
- Two-stage: Discovery/Research (Stage 1) + Generation/Ranking (Stage 2)
- Single-stage: one evaluation block for simpler features

### 3b. Design 5-7 metrics per stage

Design principle: **Start from what an expert practitioner would notice.** If a top professional in this domain reviewed this output, what would they praise or criticize? Then express that as a measurable criterion.

Each metric must:
- Have a crisp 1-sentence definition (no ambiguity)
- Be scorable by an LLM judge with a rubric (avoid metrics requiring real-world validation to score)
- Be distinct from other metrics in the same stage (no overlap)
- Reflect at least one insight from `$EXPERT_GUIDANCE` that would not come from the PRD alone

### 3c. Design rubrics (4-band, 0-10)

For each metric, write bands where:
- **8-10**: What an expert practitioner would call excellent
- **5-7**: Adequate but not impressive; a human professional would improve it
- **2-4**: Below the bar; creates rework or slows the end user
- **0-1**: Harmful, hallucinated, or completely wrong

### 3d. Test corpus design
- 3 synthetic user/company profiles (C1) representing distinct segments or personas
- 100-200 test cases (C2) with appropriate mix of easy/hard cases
- Explicit coverage of hard cases: edge cases, ambiguous data, missing fields, adversarial inputs
- Ground truth: LLM generates reference answers, PM spot-checks 10%+ sample

### 3e. Identify baselines
- Human expert baseline (quality ceiling)
- Naive/simple automation baseline (minimum bar the agent must beat)

---

## Step 4: Write the Eval Document

Save to `Knowledge/product/evals/[feature-slug]-eval-criteria-YYYYMMDD.md` using this structure:

```
# [Feature Name] - Eval Criteria

> Product: [Product name from CLAUDE.md]
> Feature: [Feature description]
> Date: [TODAY]
> Reference: [PRD link]

## Overview
2-3 sentences: what the feature does, why it needs its own eval, what the
core quality question is.

## Stage N: [Stage Name]

*Core question: [The single evaluative question this stage answers]*

Summary table:
| Metric | Definition |
|---|---|
| Metric name | 1-sentence definition |

Scoring notes: prerequisites, edge cases, phase-specific variations.

### [Metric Name]
1-2 sentence expanded definition including what the hard case is and why it matters.

| Score | Criteria |
|---|---|
| 8-10 | Excellent — what a practitioner would call great |
| 5-7 | Adequate — a human professional would improve it |
| 2-4 | Below bar — creates rework or friction for the user |
| 0-1 | Harmful — hallucinated, wrong, or missing entirely |

[Repeat for each metric and stage]

## Scoring Methodology
| Parameter | Value |
|---|---|
| Scale | 0-10 per metric |
| Judge | LLM-as-judge with per-metric rubric |
| Calibration | PM spot-checks 10%+ sample to validate judge consistency |
| Variance reduction | Each sample evaluated 5 times; mean score recorded as final |
| Aggregate | Mean of metrics per stage; overall = mean of all stage scores |

## Test Corpus Design

### User/Company Profiles (C1)
Table: Profile | Segment | Product type | Key characteristic

### Test Case Corpus (C2)
Table: Dimension | Spec — size, mix, hard cases, ground truth method

### Ground Truth Method
LLM generates reference answers independently. PM spot-checks 10%+ sample.
End-user acceptance data from production provides retrospective validation.

## Baseline Comparisons
Table: Baseline | Description | What it isolates

## Production Outcome Metrics (Complement to Offline Eval)
Table: Metric | Definition | Target (requires real usage data to measure)

## Eval Phases
Table aligned to MVP roadmap: Phase | Eval focus | Corpus size | Human review

## Next Steps
1. LLM judge setup and calibration
2. Corpus construction
3. Engineering prerequisites (e.g., logging, output schema)
4. Offline validation plan
5. Share results with team
```

---

## Step 5: Confirm to User

After writing the file:
- Share the file path as a clickable markdown link
- State how many stages and metrics the framework covers
- Call out the 1-2 most important insights from domain expert research that shaped the rubrics (that would not have come from the PRD alone)
- Flag any metrics that require engineering prerequisites to score
- Invite the user to share additional expert, customer, or practitioner guidance to refine rubrics further

---

## Notes

- **Domain research is not optional.** Rubrics written purely from the PRD measure what the feature is supposed to do, not what practitioners actually care about. The web research step is what makes rubrics field-credible.
- **Avoid ML jargon.** Terms like "gold label", "precision/recall", "F1 score" must be replaced with plain language — these docs are read by PMs, end users, and engineering leads, not ML researchers.
- **Hard cases determine the score distribution.** Design rubrics so that 8-10 requires handling the case that separates a good agent from a naive lookup. Easy inputs should score 5-7 at best.
- **LLM-as-judge is the default.** No dedicated domain experts are required. A PM spot-checking 10%+ of outputs is sufficient for calibration. User acceptance data from production is the retrospective validation layer.
- **If a PRD does not exist**, ask the user to describe the feature inputs, outputs, and success criteria before proceeding — do not infer a full eval framework from a feature name alone.
- **Adapt the reference eval step** to whatever benchmarking or eval methodology your team uses. If none exists yet, skip Step 1.2 and rely on the domain research to anchor the framework.
