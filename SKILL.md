---
name: optimize-headline
description: Use when drafting or improving a headline, title, or subject line for any published content (article, blog post, newsletter, video) - applies a ranked pattern library and redline rules to generate and evaluate alternatives.
---

# Optimize Headline

Generates and evaluates headline alternatives for any piece of published content. A headline is the highest-leverage set of words in most content formats - this skill treats it as its own redline pass, separate from drafting the body.

## Arguments

- `/optimize-headline` (no args) - Read the target content (or the draft title provided), apply the redline rules, and produce 3 ranked alternatives
- `/optimize-headline --help` - Prints this skill's summary and argument list, without generating anything

## Workflow

1. **Read the content** (or the topic/argument, if the body isn't written yet) to understand the actual stakes, audience, and specificity available.
2. **Draft or take the current title** as the baseline.
3. **Apply the Headline Redline Rules** below.
4. **Generate 3 alternatives**, each drawing from a different pattern in the Headline Patterns list.
5. **Output** in the format below, with a recommendation and one-sentence rationale.

## Headline Patterns (ranked by engagement pull)

1. **The Number That Doesn't Belong** -- Lead with a specific number that creates a curiosity gap
   - Pattern: `[Unexpected number]. [The stakes framing].`
   - Example: "800 Volts DC. The Number That Should Wake You Up."

2. **The Alert Headline** -- Event + stakes for a named audience
   - Pattern: `[Event]: The [X] That Should Have Every [Audience] on Alert`
   - Works especially well for reactions to news, conference takeaways, or market moves

3. **The Threat Frame** -- Asset + vulnerability
   - Pattern: `Your [Thing] Is a [Threat]`
   - Short, punchy, impossible to scroll past

4. **The Thesis Reversal** -- Public self-correction builds credibility
   - Pattern: `I Said [Claim]. [Time] Later, I Need to Update My Thesis.`

5. **The Uncomfortable Question** -- Flip the frame on something assumed safe
   - Pattern: `Why Is [Industry] Still [Doing the Wrong Thing]?`

6. **The Scenario Setup** -- Drop reader into a moment
   - Pattern: `You're [in situation]. [Thing] just changed everything.`

## Headline Redline Rules

- **Specificity beats generality.** "Three PE Bets That Won't Survive 2026" beats "The Changing PE Landscape"
- **Name the audience.** If the piece is for a specific role or community, say so in the headline - it qualifies readers and drives shares within that community
- **Cut the first 3 words.** Most draft titles start with dead weight ("A Look at...", "Thoughts on...", "Why I Think..."). Start at the point of tension.
- **Test against the scroll.** Would this make someone stop mid-scroll or mid-inbox-triage? If you're not sure, it's not there yet.
- **Avoid -ing openings.** "Rethinking X" and "Navigating Y" are among the most overused headline patterns across platforms. They signal analysis, not urgency.
- **Primary keyword placement.** If SEO/discoverability matters for this content, prefer including the primary topic keyword in the headline - but never at the expense of the engagement pull above; the redline rules take precedence.

## Output Format

```
CURRENT: [original title]

OPTION A: [Number/specificity variation] -- [why it works]
OPTION B: [Urgency/threat variation] -- [why it works]
OPTION C: [Audience-named variation] -- [why it works]

RECOMMENDED: [Option X] -- [one-sentence rationale]
```
