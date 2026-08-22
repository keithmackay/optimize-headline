# improve-this review — optimize-headline

**Date:** 2026-08-21
**Scope:** Full project (`/Users/Keith.MacKay/Projects/optimize-headline`)
**Project type:** Prompt-only Claude Code skill. Four content files — `SKILL.md` (65 lines, the always-loaded prompt), `README.md` (40 lines), `help.md` (12 lines), `LICENSE`, `.gitignore`. No code, no tests, no build.
**Context:** Recently extracted from the personal LinkedIn-writing skill `writeli` into a standalone, general-purpose headline skill. The generalization goal is treated as a first-class evaluation lens throughout.

**Categories evaluated:** Clarity & Simplification · Completeness · Accuracy & Consistency · Navigability & Structure · Redundancy · Edge Case Coverage · Token Efficiency & Progressive Disclosure

---

## Part 1 — Priority List

```
#1  [Impact: High   | Confidence: High]   Accuracy & Consistency — `--help` is specified but has no wiring to help.md
#2  [Impact: High   | Confidence: High]   Completeness — Pattern library is still writeli-shaped; no utility/SEO/how-to patterns
#3  [Impact: High   | Confidence: High]   Edge Case Coverage — No honesty/overclaim guardrail on fear-and-curiosity patterns
#4  [Impact: High   | Confidence: Medium] Completeness — No length or platform-truncation guidance despite claiming subject lines and video titles
#5  [Impact: Medium | Confidence: High]   Accuracy & Consistency — Output format contradicts the workflow's own instruction
#6  [Impact: Medium | Confidence: High]   Redundancy — Pattern list is duplicated across three files with no source of truth
#7  [Impact: Medium | Confidence: Medium] Completeness — "Ranked by engagement pull" is an unsourced, context-free ranking
#8  [Impact: Medium | Confidence: Medium] Edge Case Coverage — No path for "the current headline is already good" or for no-content-yet
#9  [Impact: Low    | Confidence: High]   Token Efficiency — Arguments section and hardcoded "3" are minor always-loaded overhead
#10 [Impact: Low    | Confidence: Medium] Clarity — Residual LinkedIn/business-finance flavor in every worked example
```

---

## Part 2 — Categorized Breakdown

### Accuracy & Consistency

**#1 — `--help` is specified but has no wiring to `help.md`** — Impact: High, Confidence: High

`SKILL.md:13` promises `/optimize-headline --help` "Prints this skill's summary and argument list, without generating anything." A `help.md` file exists at the project root and contains exactly that text. But `SKILL.md` never mentions `help.md` — not by filename, not by path, not in the workflow. Nothing tells the model where the help text lives or that it should be read verbatim rather than improvised.

The practical result is that `--help` produces a model-generated paraphrase of the Arguments section while `help.md` sits orphaned and unread. Output is non-deterministic across invocations, and any future edit to `help.md` has no effect on anything. This is the single clearest correctness defect in the project: a documented feature whose implementation file is unreachable from the entry point.

**#5 — Output format contradicts the workflow's own instruction** — Impact: Medium, Confidence: High

Workflow step 4 (`SKILL.md:20`) says to generate three alternatives "each drawing from a different pattern in the Headline Patterns list" — a list of six. But the Output Format block (`SKILL.md:60-62`) hardcodes the three slots as `[Number/specificity variation]`, `[Urgency/threat variation]`, and `[Audience-named variation]`. Those map to patterns 1, 3, and 2 respectively. Patterns 4 (Thesis Reversal), 5 (Uncomfortable Question), and 6 (Scenario Setup) have no slot they can ever occupy.

Two-thirds of the pattern library is documented but structurally unreachable in the output. When the template and the instruction disagree, the concrete template usually wins, so the effective library is three patterns, not six. This also compounds finding #2: the three surviving slots are precisely the three most LinkedIn-flavored patterns.

### Completeness

**#2 — Pattern library is still writeli-shaped** — Impact: High, Confidence: High

All six patterns are high-drama, opinion-led, business-thought-leadership forms: curiosity-gap numbers, alerts, threats, thesis reversals, provocations, scenario drops. That is a coherent and well-observed set — for LinkedIn posts by an individual with a point of view. It is a narrow slice of the space the skill now claims.

The README and the `description` frontmatter both promise coverage of blog posts, newsletters, and videos. The dominant, empirically highest-performing headline forms in exactly those channels are absent:

- **How-to / instructional** — "How to X Without Y" (the backbone of blog and YouTube)
- **Listicle / enumerated** — "7 X That Y" (newsletters, listicles, roundups)
- **Comparison / versus** — "X vs Y: Which Actually Z" (heavy search intent)
- **Outcome / benefit-led** — "Get X in Y Time" (landing pages, product content)
- **Guide / definitive** — "The Complete Guide to X" (pillar SEO content)
- **Curiosity-without-threat** — "What Nobody Tells You About X" (softer register)

A writer bringing a tutorial, a product changelog, a recipe post, or a documentation title gets six variations of urgency-and-stakes framing, all of which will read as tonally wrong. The generalization is currently asserted in the prose and the frontmatter but not delivered in the library. This is the largest gap between the project's stated goal and its actual content.

**#4 — No length or platform-truncation guidance** — Impact: High, Confidence: Medium

The skill explicitly names newsletters and videos, and the `description` names subject lines. Each of those formats has a hard, well-known truncation point that is arguably the single most format-dependent constraint on a headline: email subject lines truncate around 40 characters on mobile, SEO title tags around 60, YouTube titles around 60 before ellipsis, LinkedIn post text around 140-210 before "see more." None of this appears anywhere in the skill.

Consequence: the skill will happily recommend a 90-character headline for an email subject line, where the payload clause gets cut off and the pattern's entire mechanism is destroyed. Several of the offered patterns are structurally long — the Thesis Reversal template (`I Said [Claim]. [Time] Later, I Need to Update My Thesis.`) is roughly 45 characters of scaffolding before any content, making it near-unusable as a subject line. The skill does not know what medium it is writing for and never asks.

Confidence is Medium rather than High only because exact thresholds drift with platform UI changes; the need for *some* length awareness is not in doubt.

**#7 — "Ranked by engagement pull" is unsourced and context-free** — Impact: Medium, Confidence: Medium

The heading at `SKILL.md:23` asserts a ranking. No basis is given, and no context is attached. Engagement pull relative to which audience, which platform, which content type? The ordering is plausible for a LinkedIn business feed and implausible for, say, developer documentation or a technical newsletter, where the Threat Frame ("Your Database Is a Time Bomb") reads as manipulative rather than compelling.

Because the ranking is presented as fact rather than as a heuristic keyed to a context, it will bias selection toward the top three patterns in every situation — including the ones where they are actively wrong for the register. Reframing the ordering as conditional ("for opinion-led social and newsletter content, roughly in this order") would cost one line and remove a false universal.

### Edge Case Coverage

**#3 — No honesty or overclaim guardrail** — Impact: High, Confidence: High

Patterns 1, 2, and 3 — the three that own all the output slots — work by manufacturing alarm or a curiosity gap. Nothing in the redline rules requires the headline to be supported by the body. There is no instruction along the lines of "the headline must make a promise the content actually keeps" or "do not manufacture stakes the piece does not establish."

This is the highest-consequence gap for a general-purpose tool. The skill's own workflow allows operating before the body exists ("or the topic/argument, if the body isn't written yet", `SKILL.md:17`), meaning it can generate a threat-framed headline with nothing behind it and the writer then has to backfill. In the original `writeli` context this mattered less — one author, one voice, personal accountability for the claim. Shipped as a general tool for arbitrary writers and arbitrary content, a pattern library with no honesty check is a clickbait generator with good taste. A single redline rule fixes it.

Additionally missing: any guidance on named-entity or accusatory headlines (framing a real company or person as the threat carries defamation and reputational exposure that a general-audience tool should at least flag).

**#8 — No "current headline is already good" path, and no input-availability branch** — Impact: Medium, Confidence: Medium

The output format mandates three alternatives and a recommendation on every run. There is no defined behavior for the case where the existing headline is already strong — the structure forces the model to produce three alternatives and pick one, which biases toward recommending change for its own sake. "KEEP CURRENT" is not an expressible outcome.

Related gaps in input handling: the workflow assumes the content is available to read but gives no instruction for the case where the user supplies only a bare title with no body and no topic (what does "understand the actual stakes, audience, and specificity available" mean then?), and no instruction to *ask* about the target platform, audience, or length budget. Given finding #4, a single clarifying question about medium would be the highest-value addition to the workflow.

### Redundancy

**#6 — Pattern list duplicated across three files** — Impact: Medium, Confidence: High

The six pattern names appear in `SKILL.md:25-44` (full, with templates and examples), `README.md` Highlights (name list), and `help.md` USAGE (name list). Three copies, no designated source of truth. Adding a seventh pattern — which finding #2 argues is needed, several times over — requires three coordinated edits, and the two summary copies will silently drift when someone forgets.

The README and help.md copies also add little: both are bare name lists that convey almost nothing without the templates. Replacing them with a count and a pointer ("six ranked patterns — see `SKILL.md`") would preserve the information value and eliminate the drift surface. Note that `README.md` and `help.md` also duplicate each other's usage block nearly verbatim.

### Navigability & Structure

No significant findings. For a four-file project, the structure is appropriate and correctly minimal: a skill file, a README for humans, a help file for the `--help` path, a license, and a `.gitignore` that sensibly covers `.DS_Store`, swap files, `.claude/settings.local.json`, and `.sessionstats/`. `SKILL.md`'s internal section order — purpose, arguments, workflow, patterns, rules, output — is logical and reads top-to-bottom without forward references.

One minor note: the README install snippet runs `git clone` and then `ln -s "$(pwd)/optimize-headline" ...`. That is correct only if the reader has not `cd`'d into the cloned directory, which is not stated. A one-word clarification ("from the parent directory") would remove the ambiguity.

### Clarity & Simplification

**#10 — Residual writeli flavor in every worked example** — Impact: Low, Confidence: Medium

Every concrete example is business/finance/LinkedIn: "800 Volts DC. The Number That Should Wake You Up.", "Three PE Bets That Won't Survive 2026", "The Changing PE Landscape". A reader arriving at this as a general headline tool sees a consistent signal that it is built for one register and one industry, which will suppress adoption for the blog, newsletter, and video use cases the README advertises.

The examples themselves are good — they are vivid and they demonstrate the patterns clearly. The issue is uniformity, not quality. Diversifying two or three of them across content types (a how-to, a technical newsletter, a video title) would carry the generalization claim far more convincingly than the prose currently does.

Otherwise the prose is tight and well-edited. The redline rules in particular are excellent: concrete, each with a stated reason, several with before/after contrast. "Cut the first 3 words" and "Avoid -ing openings" are exactly the kind of specific, checkable guidance that survives contact with a model. No verbosity worth cutting; the file is not padded.

### Token Efficiency & Progressive Disclosure

**#9 — Minor always-loaded overhead** — Impact: Low, Confidence: High

At 65 lines / ~3.5 KB, `SKILL.md` is comfortably within a sensible always-loaded budget and does **not** need to be split. Setup and installation are already correctly out in `README.md` rather than in the skill file — the progressive-disclosure fundamentals are right.

Two small observations:

- The `## Arguments` section (`SKILL.md:10-13`) is loaded on every invocation but is only relevant when the user passes `--help`. It is four lines, so the cost is trivial and the fix carries its own risk (see finding #1 — the argument surface is currently under-specified, not over-specified). Not worth acting on in isolation.
- **Forward-looking:** if finding #2 is addressed and the library grows to ten-plus patterns with templates and examples, `SKILL.md` will approach the size where a `references/patterns.md` split becomes worthwhile — keeping the workflow, redline rules, and a pattern *index* in `SKILL.md`, with the full templates and examples pulled in on demand. That is a consequence of the expansion, not a problem with the file today.

A Clarity & Simplification pass on the always-loaded content is worth doing alongside any pattern-library expansion, to keep the additions as tight as the existing rules.

---

## Summary

The skill is well-written and the redline rules are its strongest asset — concrete, reasoned, and genuinely useful. The core issue is that the extraction from `writeli` generalized the *framing* (description, README, prose) without generalizing the *content* (patterns, examples, output slots). Closing that gap — plus wiring up `--help` and adding an honesty guardrail — would make the standalone positioning real rather than aspirational.
