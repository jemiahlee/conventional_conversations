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
- `info` vs `feeling` — the test isn't whether it's phrased as a question,
  it's whether the content can be produced on demand. A number, a time, a
  status can always be pulled up and handed over — that's `info`. A
  feeling can't be manufactured to order; it either already exists or it
  doesn't. "I need to know how you feel about this" is still `feeling`,
  not `info`, no matter how factually it's phrased.
- `info` covers status/progress updates too — don't add a separate
  `status` scope, it's redundant ("I'm doing X" = `tell(info)`).
- Future intent to act is also just `tell(plan)` — no separate `intend`
  force or scope needed.
- `decision` vs `info` — if the message reports completion of something
  the *other party* already decided, that's `info` ("done, here's the
  status"). If it announces a choice the *sender* made themselves, even a
  small implementation one, that's `decision`. When it's genuinely
  borderline, lean `info` — it's the lower-stakes read.
- The message should add something the tag didn't already say. A scope
  names a category, not a script — `require(plan): send me a plan by
  Monday` is circular; `require(plan): I need the approach by Monday`
  adds the part the tag couldn't.

## When to use this

**Tagging a message the user is writing:**

- They explicitly ask to tag, convert, or draft a message in this format.
- They're drafting something async and high-stakes-to-misread (a Slack
  message, email, PR comment, handoff note) and ask for help making the
  intent unambiguous — offer the tag, don't force it into every reply.

**Tagging Claude's own replies, unprompted:**

- Auto-prefix Claude's own message with its force(scope) tag — no need to
  be asked each time — when the reply is important enough that a misread
  would cost something: delivering a decision, requiring something from
  the user by a point in time, flagging a real risk, or asking for a
  call on something consequential.
- Skip it for routine replies — a status update, an acknowledgment, a
  small clarifying answer. Tagging everything defeats the point; it
  should mark the messages that actually carry stakes.
- When in doubt, the FAQ's own test applies: would you normally soften
  this with an exclamation point or three, or reread it before sending?
  If yes, tag it.

Either way: don't retrofit casual, face-to-face-equivalent, or low-stakes
chat with a tag. Not every message needs one.

## Receiving a tagged message from the user

The force sets the expectation for what Claude does next — the same
definitions from the table above, applied to incoming messages:

- **`tell(...)`** is information only. Update understanding of what was
  said; don't infer a follow-up action from it (an edit, a commit, a
  reset) unless one is separately, explicitly requested. A tell reports
  state — it doesn't delegate work.
- **`ask(...)`** expects a response — answer it.
- **`require(...)`** expects the named content produced or delivered —
  do that.
- **`offer(...)`** expects nothing — acknowledge if relevant, take the
  offer up only if it's wanted.

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

**What if a message needs more than one tag?** It doesn't get one — pick
the dominant force and let the rest live in the words. If a tagged
message keeps growing long, that's usually the tell: it's covering two
forces under one tag, not that it needs more explanation. Split it into
two tagged messages instead, the same way a commit that wants "and" in
its subject line is usually two commits.

None of this is meant to catch every subtlety of a message — it's a
signal for general intent, not a full parse. Treat the tag as a heading,
not a summary.

**Where does this taxonomy come from?** Loosely, John Searle's speech act
theory — his taxonomy of illocutionary acts, building on J.L. Austin's
earlier work in *How to Do Things with Words*. Searle split utterances
into assertives, directives, commissives, expressives, and declaratives.
`tell` maps to assertive; `ask` and `require` both sit under directive,
weak and strong ends of the same category; `offer` maps to commissive.
`feeling` covers what Searle called expressive, as a scope rather than
its own force. Declaratives — utterances that change reality just by
being said, like "I now pronounce you..." — didn't have a place in
everyday text, so there's no fifth force.
