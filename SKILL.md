---
name: optimize-headline
description: Use when drafting or improving a headline, title, or subject line for any published content (article, blog post, newsletter, video) - applies a pattern library grouped by register and a set of redline rules to generate and evaluate alternatives.
---

# Optimize Headline

Generates and evaluates headline alternatives for any piece of published content. A headline is the highest-leverage set of words in most content formats - this skill treats it as its own redline pass, separate from drafting the body.

## Arguments

- `/optimize-headline` (no args) - Read the target content (or the draft title provided), apply the redline rules, and produce 3 ranked alternatives
- `/optimize-headline --help` - Do not run any other part of this skill. Read and display the contents of `help.md` (in this skill's folder) verbatim, then stop.

## Workflow

1. **Resolve the target content.** If the user's invocation or the current conversation already names a specific file, draft title, or pasted content, confirm it before proceeding: "Generating headlines for `<path or description>` - confirm?" If no target was specified, ask the user which content (file, draft title, or topic/argument) to work from. Never guess.
2. **Establish the medium.** If the target platform/medium is not already clear from context (email subject line, SEO title tag, video title, social post, blog headline, etc.), ask the user before generating alternatives - both the Length and Truncation guidance and the register bands below depend on it.
3. **Get the minimum input.** If given only a bare title with no body, topic description, or argument, ask the user for the one-sentence version of what the content will argue or cover before generating alternatives - "understand the actual stakes" requires that minimum input.
4. **Read the content** (or the topic/argument, if the body isn't written yet) to understand the actual stakes, audience, and specificity available.
5. **Draft or take the current title** as the baseline.
6. **Apply the Headline Redline Rules** below.
7. **Generate 3 alternatives**, each drawing from a different pattern in the Headline Patterns list, chosen from the register band that matches the medium.
8. **Output** in the format below, with a recommendation and one-sentence rationale - or a "keep current" verdict if the baseline already wins.

## Headline Patterns (by register)

Pick from the band that matches this content's medium and register - a Threat Frame in developer documentation reads as manipulative; a How-To in an opinion column reads as flat.

### Band 1: High-drama / opinion-led

Best for opinion columns, commentary, social posts, and reaction pieces where a point of view is the product.

1. **The Number That Doesn't Belong** -- Lead with a specific number that creates a curiosity gap
   - Pattern: `[Unexpected number]. [The stakes framing].`
   - Example: "800 Volts DC. The Number That Should Wake You Up."

2. **The Alert Headline** -- Event + stakes for a named audience
   - Pattern: `[Event]: The [X] That Should Have Every [Audience] on Alert`
   - Example: "The npm Registry Outage: The Dependency Risk That Should Have Every Platform Team on Alert"
   - Works especially well for reactions to news, conference takeaways, or market moves

3. **The Threat Frame** -- Asset + vulnerability
   - Pattern: `Your [Thing] Is a [Threat]`
   - Example: "Your Staging Environment Is a Security Hole"
   - Short, punchy, impossible to scroll past

4. **The Thesis Reversal** -- Public self-correction builds credibility
   - Pattern: `I Said [Claim]. [Time] Later, I Need to Update My Thesis.`
   - Example: "I Said Monorepos Were Overkill. Two Years Later, I Need to Update My Thesis."

5. **The Uncomfortable Question** -- Flip the frame on something assumed safe
   - Pattern: `Why Is [Industry] Still [Doing the Wrong Thing]?`
   - Example: "Why Is Enterprise Software Still Shipping Passwords?"

6. **The Scenario Setup** -- Drop reader into a moment
   - Pattern: `You're [in situation]. [Thing] just changed everything.`
   - Example: "You're Three Hours Into an Outage. The Runbook Was Written for a System That No Longer Exists."

### Band 2: Utility / instructional

Best for blog posts, newsletters, documentation, video tutorials, and product content where the reader arrives wanting a job done.

7. **The How-To** -- Outcome plus the pain it avoids
   - Pattern: `How to [Outcome] Without [Common Pain]`
   - Example: "How to Migrate a Postgres Database Without Downtime"
   - The backbone of blog and YouTube content

8. **The Listicle** -- Countable, scannable payoff
   - Pattern: `[N] [Things] That [Payoff]`
   - Example: "Seven Git Aliases That Cut My Rebase Time in Half"
   - Newsletters, roundups

9. **The Outcome Promise** -- Result plus timeframe
   - Pattern: `Get [Result] in [Timeframe]`
   - Example: "Get a Working CI Pipeline in an Afternoon"
   - Landing pages, product content

10. **The Soft Curiosity** -- A gap opened without manufactured alarm
    - Pattern: `What Nobody Tells You About [Topic]`
    - Example: "What Nobody Tells You About Running a Newsletter Past 10,000 Subscribers"
    - Lower-drama register than Threat Frame or Alert - use when the audience distrusts hype

### Band 3: Neutral / SEO-oriented

Best for search-driven pages, pillar content, and reference material where discoverability matters more than voice.

11. **The Comparison** -- Two options plus the deciding criterion
    - Pattern: `[X] vs [Y]: Which Actually [Criterion]`
    - Example: "Rust vs Go: Which Actually Ships Faster on a Small Team"
    - High search intent

12. **The Definitive Guide** -- Comprehensive coverage of one topic
    - Pattern: `The Complete Guide to [Topic]`
    - Example: "The Complete Guide to Structured Logging"
    - Pillar SEO content

## Headline Redline Rules

- **Specificity beats generality.** "Three PE Bets That Won't Survive 2026" beats "The Changing PE Landscape"
- **Name the audience.** If the piece is for a specific role or community, say so in the headline - it qualifies readers and drives shares within that community
- **Cut the first 3 words.** Most draft titles start with dead weight ("A Look at...", "Thoughts on...", "Why I Think..."). Start at the point of tension.
- **Test against the scroll.** Would this make someone stop mid-scroll or mid-inbox-triage? If you're not sure, it's not there yet.
- **Avoid -ing openings.** "Rethinking X" and "Navigating Y" are among the most overused headline patterns across platforms. They signal analysis, not urgency.
- **Primary keyword placement.** If SEO/discoverability matters for this content, prefer including the primary topic keyword in the headline - but never at the expense of the engagement pull above; the redline rules take precedence.
- **Earn the claim.** A headline built on manufactured alarm or a curiosity gap (Number, Alert, Threat Frame patterns) must be a promise the content actually keeps. Before recommending it, confirm the body (or stated topic/argument) genuinely supports the stakes framed in the headline. If the body isn't written yet and the claim can't be verified, flag this in the output rather than recommending the option silently.
- **Named entities need a human call.** If a headline names a real company, product, or person as the threat or subject of criticism, flag this explicitly in the output as needing the writer's own judgment call - do not silently recommend it as the top option.

## Length and Truncation

Known thresholds, as of writing:

- **Email subject lines:** ~40 characters before mobile truncation
- **SEO title tags:** ~60 characters
- **YouTube titles:** ~60 characters before ellipsis
- **LinkedIn post text:** ~140-210 characters before "see more"

These numbers shift with platform UI changes - treat them as a floor for caution, not a hard spec, and note in the output if a recommended option is near or over a relevant limit. Some patterns carry structural overhead that makes them a poor fit for tight media: the Thesis Reversal's two-sentence scaffolding runs ~45 characters before any content, which effectively rules it out for an email subject line.

## Output Format

Any of the twelve patterns can fill any slot - name the pattern used rather than assuming a fixed set.

```
CURRENT: [original title]

OPTION A: [Pattern name] variation -- [the headline] -- [why it works]
OPTION B: [Pattern name] variation -- [the headline] -- [why it works]
OPTION C: [Pattern name] variation -- [the headline] -- [why it works]

RECOMMENDED: [Option X] -- [one-sentence rationale]
CAUTION: [optional - if any recommended option manufactures stakes not yet supported by the content, names a real entity as a threat, or runs near/over the relevant length limit, state it here]
```

If the current headline already satisfies the Headline Redline Rules and no pattern in the relevant register band would clearly outperform it, do not force three alternatives and a pick. Output instead:

```
CURRENT: [original title]

RECOMMENDED: KEEP CURRENT -- [one-sentence rationale]
```
