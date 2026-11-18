---
name: health
description: Whole-codebase health assessment that tracks decay over time (tests, lint, dead code, duplication, complexity, hotspots) and compares with the previous run. Use only when the user runs /health or explicitly asks for a codebase health check.
---

# Codebase health check

This is a report. Do not change project code or project config. The only
file you write is the health log in step 7.

1. Identify the project: the git repository root name (or the top-level
   folder when there is no repository) and its root path. The log file is
   ~/.copilot/metrics/<project>-health.md. If it exists, read the latest
   entry so you can compare against it.

2. Measure size: files and lines per language. Use `git ls-files` so
   .gitignore is respected, and exclude vendored code, generated files,
   lock files, and build output.

3. Run the deterministic checks before any model judgment. Prefer the
   commands the project already defines (AGENTS.md, README, package.json
   scripts, Makefile, pyproject.toml). Otherwise use standard tools for
   the language if they are already installed, for example:
   - JS/TS: eslint, tsc --noEmit, knip (dead code), jscpd (duplication)
   - Python: ruff, mypy, vulture (dead code), radon (complexity)
   Cover: tests and coverage, lint and typecheck counts, dead code and
   unused dependencies, duplicated blocks, largest files and functions.
   Do not install anything. If a tool is missing, skip it and list it under
   "Unavailable checks" with its install command.

4. Find hotspots: files that change often and are large or complex.
   Churn: `git log --since="6 months ago" --name-only --format= | sort | uniq -c | sort -rn`.
   Without git history, rank by size and complexity only. Pick the top 5.

5. Review only those 5 hotspot files. Look for duplicated logic, unclear
   structure, dead paths, missing tests, performance traps (repeated work in
   loops, N+1 queries, unnecessary re-renders, blocking I/O on hot paths),
   and inconsistent patterns left by incremental changes.

6. Rate each area Good, Fair, or Poor with the numbers behind it, and mark
   the trend against the previous entry: better, same, or worse. Then list
   the top 5 refactors ranked by impact, each with its files, the reason,
   and a scope of small, medium, or large. No time estimates.

7. Append an entry to the log file. If the file does not exist, create it
   with a "# Health: <project>" title and a "Path: <project root>" line.
   Each entry has: a "## <date> (<short commit sha>)" heading, a table of
   the numbers from steps 2-3, the ratings with trends, the top refactors,
   and any unavailable checks.

8. Reply with a short summary: the overall trend, the 3 biggest issues, and
   the log file path. Then the usual metrics block.
