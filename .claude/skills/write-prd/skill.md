---
name: write-prd
description: Write a structured Product Requirements Document using an interactive interview, based on the chatprd.ai template
user-invocable: true
argument-hint: [initiative name or brain-dump] (optional)
allowed-tools: Read, Write, Glob
---

# Write PRD

Generates a structured PRD saved to `Projects/[initiative]/PRD-[name]-YYYYMMDD.md`.

**User's request:** $ARGUMENTS

Uses the 6-section structure from [chatprd.ai](https://www.chatprd.ai/templates/prd-product-requirement-document-template):
Background → Objectives → Features → User Experience → Milestones → Technical Requirements

---

## Step 0: Determine Mode

**If `$ARGUMENTS` is a brain-dump** (>3 sentences of freeform text):
- Extract key points from the brain-dump
- Skip the interview for sections where the brain-dump provides enough signal
- Ask only the remaining critical questions

**If `$ARGUMENTS` is an initiative name** (short, like "mobile onboarding revamp"):
- Run the full interview

**If no arguments**:
- Ask: "What initiative are we writing a PRD for?"

---

## Step 1: Load Context

Read in parallel:
- `CLAUDE.md` — product name, OKRs, priority framework
- `Customers/` — recent engagement notes for relevant customer context
- `Projects/[initiative]/` — if a folder already exists, read any existing docs

---

## Step 2: Interview (one section at a time)

Work through each section. Ask 2–3 questions, then draft that section before moving on. Do not ask all questions upfront.

### Section 1: Background

Ask:
1. What problem does this initiative solve? Who experiences this problem, and how often?
2. What is the market opportunity or strategic context? (trends, competitive pressure, customer demand)
3. What is your vision for the outcome — what does success look like in 12 months?

Draft the Background section, then ask: "Does this capture it? Anything to add or change?"

### Section 2: Objectives

Ask:
1. What are the 2–3 most important measurable outcomes? (complete with: metric, baseline, target, timeline)
2. Are there qualitative goals — things you want users to feel or say?
3. What are the top 2 risks that could prevent success?

Draft Objectives, then confirm.

### Section 3: Features

Ask:
1. What are the core capabilities users need? Walk me through the top 3–5.
2. For each: is this Must Have, Should Have, Could Have, or Won't Have (MoSCoW)?
3. Are there technical integrations or dependencies we need to account for?

Draft Features with MoSCoW table, then confirm.

### Section 4: User Experience

Ask:
1. Walk me through the user's journey — from how they discover / enter the feature to achieving their goal.
2. What are the key UX principles or constraints? (e.g., mobile-first, accessibility, consistent with design system)
3. How will you validate the UX before full release? (usability testing, A/B, beta group?)

Draft User Experience section, then confirm.

### Section 5: Milestones

Ask:
1. What are the major phases of delivery? (e.g., design, alpha, beta, GA)
2. Are there hard dates or dependencies driving the timeline?
3. What does post-launch evaluation look like — how will you know it's working?

Draft Milestones, then confirm.

### Section 6: Technical Requirements

Ask:
1. What tech stack or platform does this build on?
2. Are there security, compliance, or data residency requirements?
3. What performance targets matter? (latency, uptime, scale)
4. What third-party integrations are needed?

Draft Technical Requirements, then confirm.

---

## Step 3: Assemble the Full PRD

Combine all confirmed sections into the final document.

```markdown
# PRD: [Initiative Name]

**Author:** [YOUR-NAME]
**Product:** [YOUR-PRODUCT-NAME]
**Status:** Draft
**Date:** [YYYY-MM-DD]
**Version:** 1.0

---

## 1. Background

### Problem Statement
[Clearly articulate the core problem this initiative solves]

### Market Opportunity
[Relevant trends, market size, competitive context]

### User Personas
[Who are the target users? What are their key characteristics and goals?]

### Vision Statement
[A clear, inspiring vision that encapsulates the outcome]

### Product Origin
[How did this initiative emerge? Customer request, data signal, strategic bet?]

---

## 2. Objectives

### SMART Goals

| Goal | Metric | Baseline | Target | Timeline |
|------|--------|----------|--------|----------|
| [Goal] | [Metric] | [Baseline] | [Target] | [Date] |

### Key Performance Indicators
[How will you measure success on an ongoing basis?]

### Qualitative Objectives
[What do you want users to feel or say when this ships?]

### Strategic Alignment
[How does this connect to team OKRs and company strategy?]

### Risk Mitigation

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|-----------|
| [Risk] | H/M/L | H/M/L | [Plan] |

---

## 3. Features

### Core Features

| Feature | Description | Priority (MoSCoW) | User Benefit |
|---------|-------------|-------------------|-------------|
| [Feature] | [What it does] | Must Have | [Benefit] |
| [Feature] | [What it does] | Should Have | [Benefit] |
| [Feature] | [What it does] | Could Have | [Benefit] |
| [Feature] | [What it does] | Won't Have (this version) | [Benefit] |

### Technical Specifications
[Key technical constraints, integrations, and implementation notes]

### Future Enhancements
[Capabilities deferred to a future version]

---

## 4. User Experience

### User Journey
[Step-by-step: how does a user move from entry point to goal achievement?]

1. [Step 1]
2. [Step 2]
...

### UI Design Principles
[Key design constraints: mobile-first? accessibility level? design system?]

### Usability Testing Plan
[How will you validate UX before GA? Beta users, A/B test, usability sessions?]

### Accessibility
[Compliance level, e.g., WCAG 2.1 AA]

### Feedback Loops
[How will you gather user feedback post-launch?]

---

## 5. Milestones

### Development Phases

| Phase | Scope | Target Date | Owner |
|-------|-------|------------|-------|
| Design | [Deliverables] | [Date] | [Owner] |
| Alpha | [Deliverables] | [Date] | [Owner] |
| Beta | [Deliverables] | [Date] | [Owner] |
| GA | [Deliverables] | [Date] | [Owner] |

### Critical Path
[What are the sequentially dependent tasks that determine the timeline?]

### Review Points
[Key checkpoints: design review, eng kickoff, pre-beta review, GA readiness]

### Launch Plan
[GTM, comms, docs, support readiness]

### Post-Launch Evaluation
[How and when will you evaluate success? Who reviews? What triggers iteration?]

---

## 6. Technical Requirements

### Tech Stack
[Core technologies, platforms, frameworks]

### System Architecture
[High-level component overview; link to architecture diagram if available]

### Security & Compliance
[Data classification, access control, audit logging, compliance requirements]

### Performance Targets

| Metric | Target |
|--------|--------|
| [e.g., API response time] | [e.g., p95 < 200ms] |
| [e.g., Uptime] | [e.g., 99.9%] |

### Integration Requirements
[Third-party services, internal APIs, data sources]

---

## Appendix

### Open Questions
- [ ] [Question that needs resolution]

### Related Documents
- [Link to related PRDs, research, or design files]
```

---

## Step 4: Save the PRD

Determine the initiative name (from arguments or interview). Save to:

```
Projects/[initiative-slug]/PRD-[initiative-slug]-[YYYYMMDD].md
```

If the `Projects/[initiative-slug]/` folder doesn't exist, also create:
```
Projects/[initiative-slug]/INITIATIVE-OVERVIEW.md
```
(using the `_TEMPLATE-INITIATIVE/INITIATIVE-OVERVIEW.md` template, pre-filled with context from the PRD).

---

## Step 5: Confirm to User

Show:
- File path (clickable link)
- 3-bullet TL;DR of the PRD
- Suggested next steps: share for review, schedule eng kickoff, add to roadmap
