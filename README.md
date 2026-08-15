# Conventional Conversations

A specification for tagging intent in everyday text — the same idea as
[Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/),
applied to how people talk to each other instead of how they describe
code changes.

Text strips out tone. A flat sentence reads calm from one reader and
hostile from another, because the reader supplies whatever tone is
missing — usually out of their own mood, not the sender's. Conventional
Conversations adds one small, explicit tag to the front of a message so
intent doesn't have to be guessed:

```
force(scope): message
```

## Structure

```
.
├── site/    the specification website
└── skill/   an installable Claude Code skill
```

### `site/`

The spec itself, as a single self-contained static page
(`site/index.html`) — grammar, the four forces, the scope list, a
force × scope reference matrix, and a short FAQ.

To preview locally, just open `site/index.html` in a browser. To deploy,
point a static host at the `site/` directory as its publish root.

### `skill/`

`skill/SKILL.md` packages the spec as a Claude Code skill, so Claude can
tag a message on request ("tag this for clarity", "put this in
conventional conversations format") instead of the spec being reference
material only.

To install it:

```
cp skill/SKILL.md ~/.claude/skills/conventional-conversations/SKILL.md
```

## The grammar

- **force** — `ask`, `tell`, `require`, or `offer`. Required, and closed —
  these four cover the space; the set isn't meant to grow.
- **scope** — `advice`, `decision`, `feedback`, `info`, `clarification`,
  `plan`, or `feeling`. Optional, and open — extend it for your own team
  or context.

See `site/index.html` for the full specification, examples, and the
reference matrix.

## License

TBD.
