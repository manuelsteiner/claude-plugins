# Resolving the Todoist project for a repo

Used by both `queue` and `capture`. Keep the behaviour identical in both.

## Inputs

The invoking skill supplies repo identity gathered from the working directory:
directory name, git remote, `composer.json` / `package.json` name, README heading.

If that block arrives as a literal placeholder rather than data, shell execution
is disabled by policy. Do not guess — ask the user which project to use.

## Rules

1. If the user passed a project name as an argument, use it. No matching.
2. Otherwise list Todoist projects and compare against the repo identity.
   Projects may be nested; match the leaf name, not the full path.
3. Accept a match only on corroborating evidence:
   - exact case-insensitive name match, or
   - the repo's package/composer name or git remote slug matching the project
4. A generic directory name (`app`, `src`, `web`, `api`, `code`, `dev`) is never
   sufficient evidence on its own.

## Outcomes

- **One corroborated match** — use it, and state which project was chosen.
- **Several candidates, or a fuzzy match only** — list them and ask. Never pick
  silently.
- **Nothing plausible** — say so, show the project list, and ask which to use or
  whether to create one. Do not proceed on codebase inspection alone.

Always name the resolved project in the response so a wrong match is visible
immediately rather than after the write.
