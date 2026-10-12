# Git Practice

A hands-on exercise file. Put it in an empty folder, turn that folder into a repo, and work through each level by **editing this file** and running the commands. Every exercise ends with a check so you know it worked.

---

## Setup

```bash
mkdir ~/git-practice
mv ~/Downloads/git-practice.md ~/git-practice/   # adjust if it saved elsewhere
cd ~/git-practice
git init
git status
```

**Check:** `git status` lists `git-practice.md` under "Untracked files".

---

## Level 1 — Your first commit

Git has three places a change can live:

1. **Working directory**: the files on disk you edit
2. **Staging area**: changes you've marked to go into the next commit (`git add`)
3. **History**: saved snapshots (`git commit`)

```bash
git add git-practice.md
git status                         # now "Changes to be committed"
git commit -m "Add practice file"
git log --oneline
```

**Check:** `git log --oneline` shows one commit.

---

## Level 2 — Edit, diff, commit

Change the line below to describe your current project, then save:

> My current project: CHANGE ME now

```bash
git status            # file shows as "modified"
git diff              # red = removed, green = added
git add git-practice.md
git diff --staged     # what's about to be committed
git commit -m "Describe current project"
```

**Check:** `git log --oneline` shows two commits.

**Shortcut:** `git commit -am "message"` stages and commits all *already tracked* files in one step. It won't pick up brand-new files.

---

## Level 3 — Undo things

**Discard an unsaved mistake.** Delete this whole sentence, save, then run:

```bash
git restore git-practice.md
```

**Check:** the sentence is back.

**Unstage something.** Add a word anywhere, then:

```bash
git add git-practice.md
git restore --staged git-practice.md   # back to unstaged; your edit is kept
git restore git-practice.md            # now discard it too
```

**Fix the last commit message:**

```bash
git commit --amend -m "Better message"
```

Only amend commits you haven't pushed yet.

---

## Level 4 — Ignore files

```bash
touch notes.tmp
mkdir data && touch data/big_image.tif
git status      # both show as untracked
```

Create `.gitignore`:

```bash
printf "*.tmp\ndata/\n.DS_Store\n" > .gitignore
git status      # only .gitignore shows now
git add .gitignore
git commit -m "Add gitignore"
```

**Check:** `notes.tmp` and `data/` never appear in `git status`.

---

## Level 5 — Branches

A branch is a separate line of work. `main` stays safe while you experiment.

```bash
git switch -c experiment       # create and move to a new branch
```

Add this line at the bottom of the file, then commit it:

```bash
echo "Line added on the experiment branch" >> git-practice.md
git commit -am "Experiment change"
```

```bash
git switch main
tail -3 git-practice.md        # experiment line is NOT here
git merge experiment
tail -3 git-practice.md        # now it is
git branch -d experiment       # delete the merged branch
```

**Check:** `git log --oneline --graph` shows the history.

---

## Level 6 — Merge conflict (the important one)

Conflicts happen when two branches change the same line. Practice on this line:

> Favorite organism: CHANGE ME

```bash
git switch -c branch-a
```
Change the line to `Favorite organism: E. coli`, then:
```bash
git commit -am "A: E. coli"
git switch main
```
Change the same line to `Favorite organism: zebrafish`, then:
```bash
git commit -am "Main: zebrafish"
git merge branch-a        # CONFLICT
```

Open the file. You'll see:

```
<<<<<<< HEAD
> Favorite organism: zebrafish
=======
> Favorite organism: E. coli
>>>>>>> branch-a
```

Delete the markers and keep the version you want (or write a combination), then:

```bash
git add git-practice.md
git commit -m "Resolve organism conflict"
git branch -d branch-a
```

**Check:** no `<<<<<<<` markers remain, and `git status` is clean.

To back out of a merge mid-conflict instead: `git merge --abort`.

---

## Level 7 — Look back in time

```bash
git log --oneline                      # list commits
git show HEAD~1                        # what the previous commit changed
git diff HEAD~2 HEAD                   # compare two commits
git log -p -- git-practice.md          # full change history of one file
git restore --source=HEAD~2 git-practice.md   # pull an old version into your working copy
git restore git-practice.md            # undo that
```

---

## Level 8 — Push to GitHub

1. Create an **empty** repo on github.com named `git-practice` (no README).
2. Then:

```bash
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/git-practice.git
git push -u origin main
```

After that, the cycle is: edit → `git add` → `git commit` → `git push`.
To get changes made elsewhere (e.g. edited on github.com): `git pull`.

**Check:** refresh the GitHub page and your commits are there.

---

## Cheat sheet

| Goal | Command |
|---|---|
| What's going on? | `git status` |
| See unstaged changes | `git diff` |
| Stage a file | `git add FILE` |
| Commit | `git commit -m "msg"` |
| History | `git log --oneline --graph` |
| Discard edits to a file | `git restore FILE` |
| Unstage a file | `git restore --staged FILE` |
| New branch | `git switch -c NAME` |
| Change branch | `git switch NAME` |
| Merge into current branch | `git merge NAME` |
| Upload | `git push` |
| Download | `git pull` |

**Habits worth keeping:** run `git status` constantly, commit small and often, write messages that say *why*, and never run `git init` in your home folder.
