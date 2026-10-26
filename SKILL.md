---
name: agent-ticket-tracker
description: Agent-kept TRACKER.md, the owner's single entry point for a repo (what needs them, what runs, what is next, what is done). Use when the owner asks to set up a tracker, or when a task starts, changes state, needs the owner, or finishes in a repo that has TRACKER.md.
---

# Agent ticket tracker

`TRACKER.md` is the owner's **dashboard**. The owner opens it to see, at a glance, what needs them, what is running, what comes next and what finished. The agent keeps it current. Each line is one ticket with a link; detail lives in the ticket.

## Files

| File | Holds | Shown on the tracker |
|---|---|---|
| `TRACKER.md` (repo root) | The four sections | — |
| `.tracker/backlog.md` | Every open, unstarted ticket, in priority order. The only place priority lives. | Top 5 under ⏭️ Next |
| `.tracker/history.md` | Every finished ticket and owner decision, newest first | Top 5 under ✅ Recently completed |
| Ticket files | The detail: scope, notes, results, decisions | Linked from every line |

Templates are in [`templates/`](templates/). Copy their structure exactly: headings, emoji, table columns, the "View all (N)" lines and the footer.

## Tickets

Every line on the tracker, backlog and history links to a ticket file. This includes owner asks, check runs and known issues, not only build work.

- **Repo convention first.** Look for a ticket convention in `AGENTS.md`, `CLAUDE.md` or `docs/`, and follow it (location, numbering, status line).
- **Fallback.** With no convention, use `.tracker/tickets/NN-slug.md`, numbered from `01`, with these lines at the top:

  ```
  Type: task | ask | check | issue
  Status: open | in-progress | waiting | resolved
  ```

  `ask` is a question or action for the owner. `check` is a gate or verification run. `issue` is a known problem or limitation.

## Set up

1. Create `TRACKER.md`, `.tracker/backlog.md` and `.tracker/history.md` from the templates. Fill in the project name and today's date.
2. Make one ticket for each piece of known work, and list them in `backlog.md` in priority order.
3. Keep the files local: add `TRACKER.md` and `.tracker/` to `.git/info/exclude`. Commit them only when the owner asks.
4. Add this line to the repo's `AGENTS.md` (create it if missing), so every agent and session keeps the tracker:

   > `TRACKER.md` is the owner's single entry point. Keep it current with the agent-ticket-tracker skill: update the ticket first, then the tracker, whenever a task starts, changes state, needs the owner or finishes.

Setup is done when the three files exist, every backlog line links to a real ticket, and `AGENTS.md` carries the line.

## Update

Do an update on every **event**. Update the ticket first, then `backlog.md` or `history.md`, then `TRACKER.md`.

| Event | Ticket | Backlog / history | Tracker |
|---|---|---|---|
| New work | Create it | Insert in `backlog.md` at its priority | Refresh Next |
| Task starts | Status `in-progress` | Remove from `backlog.md` | Add to 🔄 In progress; refresh Next |
| State changes | Note it | — | Update the State cell |
| Needs the owner | Create an `ask` ticket with the question and your recommendation | — | Add a row to 🙋 Waiting for you |
| Owner answers | Write the decision; status `resolved` | Add "Decided: …" to the top of `history.md` | Remove from Waiting; refresh Recently completed |
| Task finishes | Write the result; status `resolved` | Add the result to the top of `history.md` | Remove from In progress; refresh Recently completed |

An update is done when every touched ticket, `backlog.md`, `history.md` and `TRACKER.md` agree, Next equals the top 5 of `backlog.md`, Recently completed equals the top 5 of `history.md`, both "View all (N)" counts equal the line counts of their files, and `Updated:` shows today.

### Line rules

- One line per item. Give the result, a commit hash or a count; the ticket holds the rest.
- An empty section shows one plain line: "Nothing needs you.", "Nothing running.", "Backlog is empty." or "Nothing finished yet."
- Waiting for you carries a recommendation on every row, so the owner can answer in one word.

## Subagents and child sessions

When you hand work to a subagent or child session, include the update rule in the brief: update the ticket first, then the tracker, whenever its task starts, changes state, needs the owner or finishes.
