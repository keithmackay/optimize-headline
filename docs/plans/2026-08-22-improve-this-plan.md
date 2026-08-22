# Implementation Plan: optimize-headline improve-this findings

**Source:** `docs/reviews/2026-08-21-improve-this.md`
**Status of already-fixed findings:** #1 (help.md orphaned) was fixed in the 2026-08-22 `--help` wiring pass, and a target-document resolution step was added to the Workflow. Excluded below.

Phased so that the pattern-library expansion (the largest content change) happens before the output-format fix that depends on knowing the final slot count, and the honesty guardrail (highest safety impact) ships early and independently.

---

## Phase 1: Add an honesty/overclaim guardrail (Finding #3, High/High)

Ship this first and independently — it's a one-rule addition with no dependency on the pattern-library work below, and it's the highest-consequence gap for a general-purpose tool.

### Task 1.1 — Add a redline rule to Headline Redline Rules

Add: "**Earn the claim.** A headline built on manufactured alarm or a curiosity gap (Number, Alert, Threat Frame patterns) must be a promise the content actually keeps. Before recommending it, confirm the body (or stated topic/argument) genuinely supports the stakes framed in the headline. If the body isn't written yet and the claim can't be verified, flag this in the output rather than recommending the option silently."

### Task 1.2 — Add a named-entity caution

Add: "If a headline names a real company, product, or person as the threat or subject of criticism, flag this explicitly in the output as needing the writer's own judgment call — do not silently recommend it as the top option."

### Task 1.3 — Update the Output Format

Add an optional line: `CAUTION: [if any recommended option manufactures stakes not yet supported by the content, or names a real entity as a threat, state it here]`

**Verification:** Run the skill against a topic with no body yet and confirm it surfaces the unverifiable-claim caution rather than confidently recommending a Threat Frame headline.

---

## Phase 2: Expand the pattern library beyond writeli's shape (Finding #2, High/High)

The largest completeness gap. Do this before Phase 3 (output format), since the output format's slot design should be built around the final pattern count, not patched twice.

### Task 2.1 — Add six new patterns to Headline Patterns

Append, following the existing pattern/example format:
7. **The How-To** -- `How to [Outcome] Without [Common Pain]` -- backbone of blog/YouTube content
8. **The Listicle** -- `[N] [Things] That [Payoff]` -- newsletters, roundups
9. **The Comparison** -- `[X] vs [Y]: Which Actually [Criterion]` -- high search intent
10. **The Outcome Promise** -- `Get [Result] in [Timeframe]` -- landing pages, product content
11. **The Definitive Guide** -- `The Complete Guide to [Topic]` -- pillar SEO content
12. **The Soft Curiosity** -- `What Nobody Tells You About [Topic]` -- lower-drama register than Threat Frame or Alert

### Task 2.2 — Reframe "ranked by engagement pull" (Finding #7, Medium/Medium)

Change the section heading from "Headline Patterns (ranked by engagement pull)" to "Headline Patterns (by register)" or similar, and group the twelve patterns under two or three register bands (e.g. "High-drama / opinion-led" containing the original six, "Utility / informational" containing the six new ones, "Neutral / SEO-oriented" if a further split helps). Add one line: "Pick from the band that matches this content's medium and register — a Threat Frame in developer documentation reads as manipulative; a How-To in an opinion column reads as flat."

### Task 2.3 — Diversify worked examples (Finding #10, Low/Medium)

Rewrite 2-3 of the existing example headlines to draw from non-LinkedIn/finance contexts (a how-to, a technical newsletter, a video title), so at least one example exists per register band from Task 2.2.

**Verification:** Confirm all twelve patterns have a template, an example, and a register-band assignment; confirm at least 3 of the ~12 examples are non-business-finance content.

---

## Phase 3: Add length/platform guidance (Finding #4, High/Medium)

### Task 3.1 — Add a "Length and Truncation" subsection after Headline Redline Rules

State the known thresholds explicitly, framed as heuristics that will drift: "Email subject lines: ~40 characters before mobile truncation. SEO title tags: ~60 characters. YouTube titles: ~60 characters before ellipsis. LinkedIn post text: ~140-210 characters before 'see more.' These numbers shift with platform UI changes — treat them as a floor for caution, not a hard spec, and note in the output if a recommended option is near or over a relevant limit."

### Task 3.2 — Add a workflow step to ask about medium when unstated

Insert into Workflow, after resolving the target content: "If the target platform/medium is not already clear from context (email subject line, SEO title, video title, social post, etc.), ask the user before generating alternatives, since the Length and Truncation guidance and the register bands (Phase 2) both depend on it."

**Verification:** Manually check that the Thesis Reversal pattern's ~45-character scaffolding is flagged as a poor fit when the target medium is an email subject line, per the new guidance.

---

## Phase 4: Fix the output-format contradiction (Finding #5, Medium/High)

Depends on Phase 2 (final pattern count and bands) and Phase 1 (the caution line).

### Task 4.1 — Rework the Output Format template

Replace the hardcoded three slots (`[Number/specificity variation]`, `[Urgency/threat variation]`, `[Audience-named variation]`) with a generic structure that can surface any three patterns: `OPTION A: [Pattern name] variation -- [why it works]`, `OPTION B: ...`, `OPTION C: ...`, so all twelve patterns (post-Phase 2) are reachable in the output, not just the original three.

### Task 4.2 — Add a "keep current" path (Finding #8, Medium/Medium)

Add an explicit output branch: if the current headline already satisfies the Headline Redline Rules and no pattern in the relevant register band would clearly outperform it, output `RECOMMENDED: KEEP CURRENT -- [one-sentence rationale]` instead of forcing three alternatives and a pick.

### Task 4.3 — Handle the bare-title-no-content case (Finding #8)

Add to Workflow: "If given only a bare title with no body, topic description, or argument, ask the user for the one-sentence version of what the content will argue or cover before generating alternatives — 'understand the actual stakes' requires that minimum input."

**Verification:** Re-run the Output Format mentally against a case where the current headline is already strong, and confirm "KEEP CURRENT" is a reachable, well-defined output rather than a forced substitution.

---

## Phase 5: De-duplicate and correct minor issues (Findings #6, #9)

### Task 5.1 — Single source of truth for the pattern list (Finding #6, Medium/High)

Replace the full pattern list in `README.md`'s Highlights and `help.md`'s USAGE with a one-line pointer: "twelve ranked patterns across three registers — see `SKILL.md` for templates and examples." Keep the full list only in `SKILL.md`.

### Task 5.2 — Fix the install-directory ambiguity (Navigability note)

In `README.md`'s Installation section, clarify: "run these commands from the parent directory you cloned into, before `cd`-ing into `optimize-headline`" or adjust the `ln -s` command to use an absolute path instead of `$(pwd)`.

**Verification:** Confirm `README.md` and `help.md` no longer enumerate pattern names, and that a fresh reader following the install steps literally would end up with a working symlink.

---

## Next Steps (beyond this plan)

- Consider whether the register-band grouping from Phase 2 should eventually become a `references/patterns.md` split if the library grows further, per the review's forward-looking note — not needed yet at ~12 patterns. [Claude's idea]
- Consider adding a lightweight "confidence" tag per recommended option once real usage data exists on which patterns actually perform well outside LinkedIn. [Keith's idea]
