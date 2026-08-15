---
name: conventional-conversations
description: >
  Tags a message's intent explicitly using the Conventional Conversations
  grammar — force(scope): message — so tone isn't left for the reader to
  guess. Trigger: "tag this message", "put this in conventional
  conversations format", "write this as force(scope)", "add a conventional
  conversations tag", or when the user asks for help wording a message
  clearly and unambiguously (Slack message, email, PR comment, etc).
  By Jeremiah Lee: https://github.com/jemiahlee
---

# Conventional Conversations

A message-tagging grammar, same idea as Conventional Commits but for
human-to-human text: prefix a message with a small, explicit tag naming
what kind of message it is, so the reader parses intent instead of
guessing it from their own mood.

## Grammar

```
force(scope): message
```

- `force` — **MUST** be present. One of the four below.
- `scope` — **MAY** be present, in parentheses, naming the topic. Omit the
  parenthetical for a bare force (`ask: got a minute?`) when nothing fits.

## Forces (closed set — don't invent new ones)

| Force | Meaning |
|---|---|
| `ask` | Soliciting a response. No expectation beyond that. |
| `tell` | Stating something as settled from the sender's side. No response required. |
| `require` | Needs the named content produced or delivered. A call for content, never for agreement with it — `require(advice)` means "give me your recommendation," not "obey it." |
| `offer` | Making yourself available. No obligation on the recipient to accept. |

## Scopes (open set — extend per team/context)

`advice`, `decision`, `feedback`, `info`, `clarification`, `plan`, `feeling`

- `feeling` names the category, not the specific emotion — the emotion
  goes in the message. Write `tell(feeling): this is the third deploy
  failure this month, and it's genuinely frustrating`, not
  `tell(frustrated): ...`.
- `require(feeling)` is the one soft cell in the grid — a feeling can't be
  compelled the way a decision or a number can. Phrase it as an invitation
  to self-report ("give me an honest read on how this landed") rather than
  a demand.
- `info` covers status/progress updates too — don't add a separate
  `status` scope, it's redundant ("I'm doing X" = `tell(info)`).
- Future intent to act is also just `tell(plan)` — no separate `intend`
  force or scope needed.

## When to use this

- The user explicitly asks to tag, convert, or draft a message in this
  format.
- The user is drafting something async and high-stakes-to-misread (a
  Slack message, email, PR comment, handoff note) and asks for help
  making the intent unambiguous — offer the tag, don't force it into
  every reply.
- Don't retrofit casual, face-to-face-equivalent, or low-stakes chat with
  a tag. Not every message needs one (see FAQ below).

## Worked examples

```
ask(advice): should we ship Friday, or wait for QA?
→ Looking for your input. No pressure either way.

tell(decision): shipping Friday. Rolled back the migration change.
→ Decision's made — letting you know, not asking.

require(feedback): review this by EOD — need sign-off before I merge.
→ Not optional. I need your eyes on this today.

offer(clarification): happy to walk through the diagram again if the
arrows were confusing.
→ No judgment if this was unclear — I'll re-explain, just say the word.

tell(feeling): this is the third time the deploy process has broken
this month, and it's genuinely frustrating.
→ Naming it so it's not left for you to read between the lines.
```

## FAQ

**Isn't tagging every message awkward?** Not every message needs one.
Reach for it when the cost of misread tone is highest: async threads,
cross-timezone handoffs, anything you'd normally soften with three
exclamation points.

**What if none of the seven scopes fit?** Use the bare force with no
scope, or extend the list for your context — forces stay closed and
small; scopes grow with the team using them.

**Isn't "require" harsh?** It's a defined tag, not a tone of voice — a
required field on a form doesn't imply judgment about the person filling
it in. Once a team knows require means "I need this content," not "obey
me," the word does its job without the edge.
