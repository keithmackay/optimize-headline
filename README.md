# optimize-headline

A Claude Code skill that generates and evaluates headline, title, and subject-line alternatives for any published content - articles, blog posts, newsletters, videos. Applies a ranked pattern library and a set of redline rules as a dedicated pass, separate from drafting the body.

## Highlights

- **Six ranked headline patterns** - Number-led, Alert, Threat Frame, Thesis Reversal, Uncomfortable Question, Scenario Setup
- **Redline rules** - specificity over generality, named audience, cut the dead-weight opening words, avoid the overused "-ing" opening
- **Structured output** - current title plus 3 ranked alternatives with a recommendation and rationale

## Getting Started

### Installation

```bash
git clone https://github.com/keithmackay/optimize-headline.git

# Global install (available in all projects)
ln -s "$(pwd)/optimize-headline" ~/.claude/skills/optimize-headline

# Project-local install (available only in one project)
ln -s "$(pwd)/optimize-headline" /path/to/your/project/.claude/skills/optimize-headline
```

## Usage

```
/optimize-headline           # generate 3 ranked headline alternatives
/optimize-headline --help    # reads and displays help.md verbatim, takes no other action
```

See `SKILL.md` for the full pattern library and redline rules.

## Contributing

Pull requests are welcome - fork the repo, make your change, and open a PR describing what it does and why.

## License

[MIT](LICENSE)
