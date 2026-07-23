---
name: capture
description: Turn things noticed during this session into Todoist tasks for this repo.
argument-hint: [thing to capture]
disable-model-invocation: true
allowed-tools: Bash(pwd), Bash(git remote get-url origin), Bash(git rev-parse --short HEAD), Bash(grep:*)
---

# Todoist capture

## Prerequisite

Needs the Todoist MCP server. If no Todoist tools are available, stop and tell
the user to install Doist's connector plugin:

```
/plugin marketplace add doist/todoist-mcp
/plugin install todoist@doist
```

Then `/mcp` to complete the OAuth flow. Do not silently discard the capture —
report what would have been created so the user can retry after installing.

## Repo identity

**Directory:** !`pwd`
**Git remote:** !`git remote get-url origin`
**Commit:** !`git rev-parse --short HEAD`
**composer.json name:** !`grep -m1 '"name"' composer.json`
**package.json name:** !`grep -m1 '"name"' package.json`

Any line above may error if the file or remote doesn't exist — that's expected,
just treat it as absent.

## 1. Resolve the project

Follow [references/project-matching.md](../../references/project-matching.md).

Arguments here are the thing to capture, **not** a project name. If the project
can't be resolved, ask — don't drop the capture.

## 2. Gather candidates

**With arguments** — capture that one thing.

**Without arguments** — sweep the session for things worth recording:

- problems you flagged but deliberately left alone
- things the user said were for later
- follow-ups implied by work that was done

Only things actually observed or stated in this session. Do not invent
improvements that seem plausible for a codebase like this one. If nothing
qualifies, say so rather than manufacturing a list.

## 3. Check for duplicates

Search open tasks in the project before proposing anything. If something is a
near-match for an existing task, say which and offer to skip it or add a comment
to the existing one instead of creating a second.

## 4. Propose, then write

Present the list for approval before creating anything. For each:

- **Title** — one line, specific, states the problem not the fix
- **Description** — what was observed, where, and why it matters. Include file
  and line references, and the commit SHA from above. A capture that reads as
  meaningless in six months has failed.
- **Priority** — p4 unless the user says otherwise
- **`ClaudeCode` label** — only for self-contained work an agent could pick up
  cold. Not for anything needing a design decision, a judgement call, or
  discussion. Over-labelling makes the queue meaningless.

The user drops, edits, or reprioritises before anything is written. Create the
approved set in a single batch (max 25 per call).

Confirm what was created and where.

## Constraints

- Never modify or complete existing tasks from this skill; only create, or
  comment on a duplicate when the user asks.
- Never create tasks in a project that wasn't confirmed.
