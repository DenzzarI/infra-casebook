# 01 — CI for this repository

**Stack:** GitHub Actions, yamllint, gitleaks
**Time spent:** ~3 hours

## Problem

A repository without automated checks can accept broken changes.
Two risks matter most: invalid YAML and leaked secrets. A leaked
secret is much worse: bots scan public repositories within minutes,
so rewriting history does not help — the secret must be rotated.
The goal is to stop both problems before they reach `main`.

## Context and constraints

- One maintainer, public repository
- Free GitHub Actions runners
- Work is done from a Linux VM (`devbox`) on Proxmox

## Solution

The workflow runs on every push to `main` and on every pull request,
so changes are checked before they are merged.

It has two independent jobs that run in parallel. If one fails,
it is clear which check failed:

- **yaml** — `yamllint` checks the syntax and style of all YAML files.
  The `truthy` rule is disabled for keys, because `on:` is a valid
  key in GitHub Actions.
- **secrets** — `gitleaks` searches for passwords, tokens and keys.
  It uses `fetch-depth: 0` to scan the full Git history, not only
  the latest files: a secret deleted in a later commit still exists
  in history.

## Verification

I opened a pull request with an intentionally broken YAML file
(a duplicate key and wrong indentation):

- `yaml` failed and reported the exact line and column
- `secrets` passed, because the file contained no secrets

After I removed the file, the checks re-ran and the pull request
turned green. The checks catch bad changes and pass good ones.

## Lessons learned

- **Each Linux user has its own Git config.** My first commit failed
  with `Author identity unknown`: I had configured Git as `root`,
  but worked as a normal user. I also learned not to work as `root`.
- **Read the result of each command.** `git push` said
  "Everything up-to-date" because the commit had failed before it.
- **`git branch -d` vs `-D`.** `-d` deletes only merged branches and
  protects unmerged work; `-D` forces deletion.
