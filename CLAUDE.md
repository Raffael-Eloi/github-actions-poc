# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.
## Rules

Always read and follow `.claude/rules/behavior.md` before responding to any request.

## What this repository is

A learning / proof-of-concept repository for **GitHub Actions**. It has no application code, build system, package manager, or test suite. It contains two things:

- `.github/workflows/workflow.yml` — a scratch workflow used to experiment with Actions features (jobs, `needs`, job outputs, triggers).
- `GitHubActions.md` — extensive personal study notes covering Actions concepts end to end (workflows, triggers, contexts, artifacts, caching, environments, custom actions, GitHub Packages, GitHub Script, Enterprise models, etc.). Many `!image.png` / `!...` markers are placeholders for images that were not carried over.

The work items driving changes are the TODO list at the top of `GitHubActions.md` (reusable workflows, caching an action, artifact upload/download by name, automated PR reviews, status badge, publishing a GitHub package).

## Working with the workflow

There is no local runner configured. The workflow is validated by pushing (it triggers `on: push`) or via the **Run workflow** button (`on: workflow_dispatch`), then inspecting results in the repo's **Actions** tab. To exercise it locally without pushing, [`act`](https://github.com/nektos/act) is the tool the notes reference (`act -j <job-id>`), but it is not set up in this repo.

Current `workflow.yml` state: the `second` job's `run` step (`jobs.initial.outputs`) is placeholder/pseudocode, not valid shell or a real output reference — expect it to be filled in as the job-output experiment progresses. Passing data between jobs requires the producing job to declare `outputs:` (sourced from a step's `$GITHUB_OUTPUT`) and the consuming job to both declare `needs:` and read `needs.<job>.outputs.<name>`.

## Conventions

- Pin third-party actions to a specific version (tag or full commit SHA), never a branch — this is called out repeatedly in `GitHubActions.md` and should be followed in any workflow edits here.
