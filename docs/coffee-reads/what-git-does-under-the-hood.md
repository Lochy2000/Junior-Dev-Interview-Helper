# What Git Is Actually Doing Under the Hood ☕

> **4 min read** · Git & Version Control

Most developers think of a Git commit as *"saving changes to a file"*.

That's not what happens. Commits don't store changes at all. They store **complete snapshots** of your entire project — and understanding this changes how you think about everything Git does.

---

## The Common Mental Model (That's Wrong)

People imagine Git works like Microsoft Word's track changes:

```
Commit 1: Added file.txt
Commit 2: Changed line 3 of file.txt (+1 line, -0 lines)
Commit 3: Deleted section in file.txt (-5 lines)
```

Git storing a list of diffs — the differences between each version.

It doesn't. (Well, it does under certain optimisations, but conceptually it doesn't.)

---

## What Git Actually Stores

Every commit is a **complete snapshot** of every tracked file at that moment. If you commit 10 files, Git stores all 10. Unchanged files aren't re-stored — Git points to the existing copy — but the commit *references* every file.

Each commit is a plain text object that looks like this:

```
tree 92ec2   ← points to a snapshot of your file structure
parent a4f3b  ← the previous commit
author Alice <alice@example.com>
date   Mon Jan 15 10:23:00 2024

Fix navbar responsive layout
```

That `tree` reference points to another object that maps filenames to file contents. It's objects pointing to objects.

---

## Everything Is a Hash

Git identifies every object (commits, files, folders) by its **SHA-1 hash** — a 40-character fingerprint of the content.

```bash
git log --oneline
# a4f3b2c Fix navbar responsive layout
# 9e12d4f Add user authentication
# 3b8c0a1 Initial commit
```

Those 7-character codes are the first 7 characters of a full 40-character hash. The hash is calculated from the *content itself*, not a sequential ID.

This means:
- **Identical content always has the same hash** — Git never stores the same file twice
- **Any change produces a completely different hash** — even changing one character
- **You can verify integrity** — if a hash matches, the content is definitely unchanged

---

## Branches Are Just Sticky Notes

Here's the bit that surprises most people.

A **branch** is not a copy of your code. It's not a parallel track. It's just a text file containing a single hash — pointing to the latest commit on that branch.

```bash
cat .git/refs/heads/main
# 0a5d6db2f3a8e1b9c4d7e6f2a1b0c3d4e5f6a7b8
```

That's it. A branch is 40 characters pointing to a commit.

When you make a new commit, the branch file gets updated to point at the new commit. When you create a new branch, Git creates a new text file pointing at the current commit. That's all branching is.

```
main    → commit C
feature → commit C   (just created, same point)

After a commit on feature:

main    → commit C
feature → commit D  (D's parent is C)
```

---

## HEAD Is Just "Where You Are"

`HEAD` is another file — it points to whichever branch (or commit) you currently have checked out:

```bash
cat .git/HEAD
# ref: refs/heads/main
```

When you `git checkout feature`, Git rewrites `HEAD` to point at `feature`. That's the whole checkout operation, conceptually.

---

## So Why Does This Matter?

Understanding this makes Git commands less mysterious:

- **`git merge`** — find the common ancestor, combine changes, make a new snapshot
- **`git rebase`** — replay your commits on top of another branch (create new snapshots with same changes)
- **`git reset --hard`** — move the branch pointer to a different commit (previous snapshot)
- **`git stash`** — commit your changes to a temporary reference, clean the working tree
- **Detached HEAD** — you've moved HEAD to point directly at a commit, not a branch

None of these are magic. They're all just moving pointers and creating snapshots.

---

## The Takeaway

> Git is a content-addressable store — a database where the key is a hash of the content. Commits are snapshots, not diffs. Branches are just pointers. Once you see this, Git stops feeling unpredictable.

---

## Want to Go Deeper?

- 📄 [Git Guide](../devops/git.md) — workflows, branching, merging
- 📄 [Git Commands Cheat Sheet](../cheat-sheets/git-commands.md) — quick reference
- 🌐 [Git Internals (Pro Git Book)](https://git-scm.com/book/en/v2/Git-Internals-Git-Objects) — the full technical picture

---

*← [Back to Coffee Reads](./README.md)*
