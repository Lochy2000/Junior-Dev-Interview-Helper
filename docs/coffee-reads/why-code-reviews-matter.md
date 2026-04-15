# Why Developers Obsess Over Code Reviews ☕

> **3 min read** · Developer Culture

New developers often assume code reviews exist to catch bugs.

That's part of it. But if bug-catching were the main point, you'd just run more tests.

The real reasons are more interesting.

---

## What a Code Review Actually Is

A code review is when one or more developers read through changes before they're merged into the main codebase. They leave comments, ask questions, suggest changes.

In most professional teams, **no code reaches production without being reviewed**.

---

## Reason 1: Knowledge Sharing

When only one person understands a piece of code, you have a problem — what happens when they're on holiday? Leave the company? Get hit by a bus?

Code reviews spread understanding. The reviewer learns how the system works. The author learns how their code reads to someone else. Both people leave knowing more than before.

> "The best documentation is often the conversation in a pull request."

---

## Reason 2: Catching Things Tests Don't

Tests verify that code does what you *intended*. They don't verify that you intended the right thing.

A reviewer might notice:
- "This only handles the happy path — what if the user's session has expired?"
- "This query will be slow once we have more than 10,000 users"
- "We already have a utility function that does this"
- "This breaks the convention we use everywhere else"

None of these are unit-test failures. They're judgment calls that require human context.

---

## Reason 3: Maintaining Consistency

Codebases that grow over years without review tend to become inconsistent. Different conventions, different patterns, different names for the same concept.

Code reviews are the mechanism for enforcing shared standards — not through rules, but through conversation.

---

## Reason 4: Making Junior Devs Better, Faster

For someone early in their career, a good code review is like getting a personalised tutorial on the specific code they just wrote.

This is faster than learning from books because:
- The feedback is immediate
- It's on *your* code
- It comes from someone who knows the codebase

Senior developers often say their fastest growth came from having their code reviewed.

---

## What Makes a Good Review?

Reviewing is a skill in itself:

```
❌ Bad comment: "This is wrong."
✅ Good comment: "This could cause issues if `user` is null here — 
   can we add a null check? See line 47 for an example of how we 
   handle this elsewhere."

❌ Bad comment: "Why did you do it this way?"
✅ Good comment: "I usually see this pattern done with a Map for O(1) 
   lookup — would that work here? Happy to discuss either way."
```

Good reviews:
- **Explain why**, not just what
- **Ask questions** rather than issuing demands
- **Distinguish** between "this must change" and "this is just a suggestion"
- **Acknowledge** things done well

---

## What to Expect as a Junior Dev

When you join a team:
- **Expect feedback** — it's not personal, it's professional
- **Ask questions** in your own PR — "I wasn't sure about this approach, open to suggestions"
- **Review others' code too** — even as a junior, your perspective is valid
- **Don't get defensive** — treat every comment as a learning opportunity

The developers who grow fastest treat code review as the most valuable feedback loop available to them.

---

## The Takeaway

> Code reviews exist primarily for knowledge sharing, consistency, and catching the things automated tests can't. The bug-finding is a bonus. They're one of the most powerful learning mechanisms available to developers at any level — especially juniors.

---

## Want to Go Deeper?

- 📄 [Git Guide](../devops/git.md) — pull requests, branching, collaboration
- 📄 [Contributing Guidelines](../../CONTRIBUTING.md) — how to give good feedback in this repo

---

*← [Back to Coffee Reads](./README.md)*
