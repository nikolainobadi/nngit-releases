# nngit

Simpler Git commands with safety checks, AI commit messages, and repo activity

nngit is a macOS command-line tool for everyday Git work: creating and switching branches, working with worktrees, publishing branches, opening pull requests, and cleaning up finished work. Interactive prompts help you choose targets and review actions.

This repository holds release builds only. Source code is not hosted here.

## Install

Requires macOS 15 or later, Homebrew, and Git.

```sh
brew install nikolainobadi/nn/nngit
```

GitHub workflows also require the GitHub CLI (`gh`), authenticated to your account. AI commit messages require a configured message generator; the default is Codex CLI (`codex exec`). You can supply your own commit message without an AI generator.

## Get started

From an existing Git repository, view your activity over the last seven calendar days:

```sh
nngit activity --days 7 --verbose
```

This shows a day-by-day breakdown of your commits and change activity, including days with no commits. Add `--everyone` to include all authors. Activity scores measure the size of changes, not their difficulty or quality.

Explore the available commands and their options:

```sh
nngit --help
nngit new-branch --help
```

## Commands

Run repository commands from the repository you want to work in.

| Command | What it does |
| --- | --- |
| `nngit new-git` | Initialize the current folder with a `.gitignore` and an initial commit, with optional GitHub repository creation. |
| `nngit new-branch` | Create a branch from the current one after checking whether it is safe to branch from. |
| `nngit switch-branch` | Choose a local or remote branch, or find the worktree where it is already checked out. |
| `nngit new-worktree` | Create a worktree for a new or existing branch under `.worktrees/`. |
| `nngit new-commit` | Stage every change and generate a commit message for review. |
| `nngit new-push` | Publish the current branch to a remote for the first time and set up tracking. |
| `nngit new-pr` | Open a pull request, publishing or pushing the branch first when needed. |
| `nngit new-remote` | Create a GitHub repository, add it as `origin`, and push `main`. |
| `nngit delete-branch` | Delete finished local branches, with confirmation for removing associated worktrees. |
| `nngit stop-tracking` | Stop tracking files matched by `.gitignore`, keeping them on disk. |
| `nngit discard` | Discard uncommitted changes, with options to choose files and staged or unstaged changes. |
| `nngit undo soft` | Undo commits while keeping their changes staged. |
| `nngit undo hard` | Undo commits and throw away their changes. |
| `nngit activity` | Summarize repository activity for today or a chosen number of days. |

`discard` and `undo hard` remove changes. Review each command's help before using them. Undo commands block pushed commits and commits authored by others unless you explicitly pass `--force`; `undo hard` also blocks uncommitted tracked changes and file collisions.

## Commit messages

`new-commit` stages **every change**, including untracked files that Git does not ignore. Review your working tree before running it.

With the default generator installed and ready to use:

```sh
nngit new-commit
```

nngit passes the staged diff and your commit guidelines to the configured generator, then shows the proposed message for confirmation. You can accept it, request another message, or cancel. Cancellation or a failure after staging restores the previous staging state.

The first run creates `~/.nngit/config.json` and starter guidelines at `~/.nngit/guidelines/commit-message.md`. Edit the guidelines to control message style, or change `commitMessage.generator` in the config to use another executable and argument list.

To skip generation and provide your own message instead:

```sh
nngit new-commit --message "Fix branch selection"
```

This also stages every change before committing.

## Releases

Builds and release notes for every version are on the [releases page](https://github.com/nikolainobadi/nngit-releases/releases).
