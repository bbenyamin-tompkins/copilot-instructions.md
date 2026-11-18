## Required workflow for every new task

1. Before inspecting project files, running project commands, or creating,
   editing, or deleting any files, present this approval card:
   - Task: a short paragraph describing what I asked for.
   - Plan: the steps you intend to take.
   - Preview: for visual changes, generate a small-scale HTML artifact.
     Writing this preview file is allowed before approval; all other file
     changes are not. Otherwise, use a small ASCII diagram, before/after
     snippet, or file tree when useful. Label assumptions if files have not
     yet been inspected.
   - Scope: small, medium, or large. Do not give time estimates.
   Show the card as your first output, before any tool call. Write the HTML
   preview only after the card has been shown.
2. Post the card as plain chat text, end your response, and wait for me to
   reply in chat. End each card with a short, natural question such as
   "Should I go ahead with this?" A clear agreement ("yes", "sure", "go
   ahead", "sounds good") approves; a refusal or requested change means
   nothing is changed. If my reply is ambiguous, ask instead of assuming.
   Never use the ask_user form or any pop-up prompt for approval.
   Presenting the card is not approval.
   Do not begin file inspection or implementation while waiting.
3. After I approve, inspect the relevant files, apply the agreed changes, and
   verify the result. Complete that task without asking for approval again,
   unless the task is genuinely unclear.
4. Keep the final summary to 2-3 lines. State any incomplete or blocked work
   plainly.
5. End with a metrics block, set apart from the summary by a horizontal rule
   (---) and shown in a code block:
       Wall-clock 1m 12s · AIC 0.8 · Model 41s · Commands 3.2s
   When a task creates a project's log file (see step 6), add a second
   line: LOGGED   ~/.copilot/metrics/<project>.md. Otherwise omit it.
   Measure from my approval message to completion, for this session only:
   - Wall-clock: approval message timestamp to now.
   - AIC and Model: sum total_nano_aiu (÷1e9) and duration_ms from the local
     session store table assistant_usage_events.
   - Commands: tool.execution_start → tool.execution_complete in the
     session's events.jsonl, minus time spent waiting on permission prompts.
   If a value can't be isolated to the task, show the session total marked
   "(session)". If it isn't available at all, write "n/a". Never estimate.
6. When a task is part of a project, append its metrics as one row to
   ~/.copilot/metrics/<project>.md. If the file doesn't exist, create it
   with a "# Metrics: <project>" title, a "Path: <project root>" line, and
   this table header:
   | Date (local) | Task | Wall-clock | AIC | Model | Commands |
   |---|---|---|---|---|---|
   <project> is the git repository root name, or the top-level folder of
   the changed files when there is no repository. Skip tasks that only
   change Copilot configuration. Logging is part of every approved task
   and needs no separate approval.

## Style

- Do not use emojis in replies, files, or metrics.
