# td-intern-harshboghani4425

TECHNODICT internship practical assignments.

This repository is my working space for the TECHNODICT internship, starting with the Chapter 03 professional Git workflow assignment. It contains documentation only.

## Contents

| File | What it is |
|---|---|
| [`profile.md`](profile.md) | Intern profile and Git learning objectives |
| [`notes/chapter-03-git.md`](notes/chapter-03-git.md) | Chapter 03 notes on the professional Git workflow |
| [`prompts/study_buddy.md`](prompts/study_buddy.md) | Reusable prompt for studying Git with an AI assistant |
| [`.github/pull_request_template.md`](.github/pull_request_template.md) | TECHNODICT pull request template |
| [`.gitignore`](.gitignore) | Keeps `.env`, keys and local files out of Git |
| [`.env.example`](.env.example) | Environment variable names with placeholder values only |

## How changes are made

Every change follows the same workflow, and nothing is pushed directly to `main`:

```text
Repository → Issue → Branch → Changes → Commit → Push → Pull Request → Review → Merge
```

`main` is protected: a pull request is required, review conversations must be resolved before merging, and force pushes and deletion are blocked.
