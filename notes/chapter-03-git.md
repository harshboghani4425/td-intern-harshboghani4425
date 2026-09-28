# Chapter 03 — Professional Git Workflow

These notes cover the workflow used in this assignment: taking one change from an issue to a squash-merged pull request, without ever working directly on `main`.

```text
Repository → Issue → Branch → Changes → Commit → Push → Pull Request → Review → Merge
```

## 1. Git vs GitHub

| Git | GitHub |
|---|---|
| A version control tool that runs on your own computer | A hosting platform for Git repositories |
| Tracks every change to your files as commits | Adds collaboration: issues, pull requests, reviews, branch protection |
| Works offline | Needs an internet connection and an account |

Git is the engine; GitHub is where the team meets. You can use Git without GitHub, but not the other way round.

## 2. Core concepts

| Concept | Meaning | Example |
|---|---|---|
| **Repository** | A project folder whose full history Git tracks | `td-intern-<username>` |
| **Working tree** | The files as they are on your disk right now, including unsaved-to-Git edits | editing `profile.md` in VS Code |
| **Staging area** | The set of changes chosen for the next commit | `git add profile.md` |
| **Commit** | A saved snapshot with a message, an author and a unique hash | `docs: add intern profile` |
| **Branch** | An independent line of work that starts from another branch | `issue-1-chapter-03-git-workflow` |
| **Remote** | The copy of the repository on GitHub, usually called `origin` | `git push origin <branch>` |

`git status` is the most useful command: it shows which branch you are on and which files are changed, staged or untracked.

## 3. The workflow step by step

| Step | Why | Typical command or action |
|---|---|---|
| **Repository** | One place for the project and its history | create on GitHub, then `git clone` |
| **Issue** | Describes the work and how to know it is done (acceptance criteria) | GitHub → Issues → New issue |
| **Branch** | Keeps unfinished work away from `main` | `git switch -c issue-1-chapter-03-git-workflow` |
| **Changes** | Do the actual work | edit files |
| **Commit** | Save logical, small steps with clear messages | `git add <file>` then `git commit -m "docs: add intern profile"` |
| **Push** | Publish the branch to GitHub | `git push -u origin issue-1-chapter-03-git-workflow` |
| **Pull request** | Proposes merging the branch into `main` and starts the review | GitHub → Compare & pull request |
| **Review** | A second look at quality, correctness and security | comments on the Files changed tab |
| **Merge** | Brings the approved change into `main` | Squash and merge |

Naming the branch after the issue (`issue-<number>-<short-description>`) means anyone can trace the branch back to the reason it exists.

## 4. Conventional Commits

Format:

```text
<type>: <short description in the imperative mood>
```

| Type | Use it for |
|---|---|
| `feat` | a new feature |
| `fix` | a bug fix |
| `docs` | documentation only |
| `chore` | maintenance, configuration, tooling |
| `refactor` | restructuring code without changing behaviour |
| `test` | adding or fixing tests |

Good: `docs: add chapter 03 git notes` — says what changed, in one line.
Weak: `update`, `final changes`, `fixed stuff` — say nothing about the change.

Each commit should be one logical piece of work. Three meaningful commits are better than ten tiny ones or one huge one.

## 5. Pull requests and code review

A pull request (PR) is a request to merge one branch into another. A good PR description explains what changed and why, links the issue, and states how the change was checked.

Writing `Closes #1` in the PR description links issue #1. When the PR is merged into the default branch, GitHub closes the issue automatically. The keywords `close`, `fix` and `resolve` (and their other forms) work the same way.

What a reviewer checks:

- Does the change meet the issue's acceptance criteria?
- Is the content correct, clear and complete?
- Are commit messages and branch naming consistent?
- Is anything sensitive being committed?

How to respond to review comments: fix the issue in a new commit on the same branch, push it, reply to the comment explaining the fix, then resolve the conversation. On GitHub, a PR author cannot approve their own PR, so a solo review is recorded as comments rather than an approval.

## 6. Merging and squash merge

| Strategy | Result on `main` | When it fits |
|---|---|---|
| Merge commit | All branch commits plus an extra merge commit | when the full branch history matters |
| **Squash merge** | One new commit containing all the branch's changes | small, focused PRs like this one |
| Rebase merge | Branch commits replayed one by one, no merge commit | teams that want a linear, detailed history |

Squash merging keeps `main` readable: one PR becomes one commit with a clear message, while the individual commits remain visible inside the merged PR. After merging, the branch has done its job and should be deleted.

## 7. Branch protection

Branch protection rules stop risky changes to important branches. For `main` in this repository:

- a pull request is required before merging, so nobody can push directly to `main`
- conversations must be resolved before merging, so review comments cannot be ignored
- force pushes and branch deletion are blocked, so history cannot be rewritten or lost
- the rules also apply to administrators, so the owner follows the same process

Required status checks (for example automated tests) are added once a repository has CI. This repository has no CI yet, so there are no checks to require.

## 8. `.gitignore`, `.env` and `.env.example`

- **`.gitignore`** lists files Git must never track: environment files, keys, dependencies, build output and logs.
- **`.env`** holds real configuration values, such as API keys, for one machine. It stays local and is ignored by Git.
- **`.env.example`** is committed. It lists the same variable names with placeholder values, so someone setting up the project knows what to configure.

### Why secrets must never be committed

A pushed secret must be treated as leaked. Public repositories are scanned by automated bots, and deleting the file later does not remove it from Git history or from anyone's clone. If a secret is ever committed:

1. Revoke or rotate the key immediately at its source. This is the step that actually protects you.
2. Remove it from the code and move it into `.env`.
3. Then clean the history if needed. Cleaning history alone is never enough.

A quick check before every commit: run `git status` and `git diff --staged`, and make sure no `.env` file or key appears.

## 9. Quick command reference

```bash
git clone <url>                     # copy a repository to your computer
git status                          # what changed, which branch am I on
git switch -c <branch>              # create and move to a new branch
git add <file>                      # stage a file for the next commit
git diff --staged                   # review exactly what will be committed
git commit -m "docs: <message>"     # save a snapshot
git push -u origin <branch>         # publish the branch and track it
git switch main && git pull         # update main after the PR is merged
git branch -d <branch>              # delete a merged local branch
```

## 10. Common mistakes and safe fixes

| Mistake | Safe fix |
|---|---|
| Started working on `main` by accident (not committed yet) | `git switch -c <new-branch>` — your uncommitted changes move with you |
| Staged the wrong file | `git restore --staged <file>` |
| Typo in the last commit message, **not pushed yet** | `git commit --amend -m "<corrected message>"` |
| Typo in a commit that is already pushed | leave it, or fix it with a new commit; avoid rewriting shared history |
| A `.env` file shows up in `git status` | add `.env` to `.gitignore` before committing anything |

Commands such as `git reset --hard`, `git clean -fd` and `git push --force` can permanently delete work. Check `git status` first, and use them only when you understand exactly what they will remove.
