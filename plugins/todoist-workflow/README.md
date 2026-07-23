# todoist-workflow

Skills for working a Todoist delegation queue from Claude Code.

| Skill                       | Purpose                                              |
| --------------------------- | ---------------------------------------------------- |
| `/todoist-workflow:queue`   | Pick up and work a task labelled `ClaudeCode`         |
| `/todoist-workflow:capture` | Turn things noticed in a session into tasks           |

`/queue` and `/capture` also work unqualified unless another skill claims them.

## Prerequisite

This plugin ships no connector. Install Doist's official one first:

```
/plugin marketplace add doist/todoist-mcp
/plugin install todoist@doist
```

The first Todoist tool call opens a browser for OAuth.

## Conventions assumed

- Tasks to delegate carry the `ClaudeCode` label
- One Todoist project per repo, matched on name — see
  `references/project-matching.md`
