# optimize-headline

A Claude Code skill that generates and evaluates headline, title, and subject-line alternatives for any published content - articles, blog posts, newsletters, videos. Applies a pattern library grouped by register and a set of redline rules as a dedicated pass, separate from drafting the body.

## Highlights

- **Twelve headline patterns across three registers** - see `SKILL.md` for templates and examples
- **Redline rules** - specificity over generality, named audience, cut the dead-weight opening words, avoid the overused "-ing" opening, and earn any claim the headline makes
- **Length and truncation guidance** - per-platform character thresholds for subject lines, title tags, and video titles
- **Structured output** - current title plus 3 ranked alternatives with a recommendation and rationale, or a "keep current" verdict when the baseline already wins

## Getting Started

### Installation

Run these commands from the parent directory you cloned into - do not `cd` into `optimize-headline` first, or `$(pwd)` will resolve to the wrong path.

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
