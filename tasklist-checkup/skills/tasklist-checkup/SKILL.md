---
name: tasklist-checkup
description: >
  Check up on any task list — from Things 3 MCP, pasted text,
  chat history, or uploaded files. Use this skill whenever the user wants to review their
  tasks or todo list, asks about any of the 5 review dimensions (capture,
  clarify, organize, reflect, engage), wants to do a weekly review, says things like "check up on
  my tasks", "let's review my task list", "help me review my todo list", "what should I work on",
  or wants to deep-dive on a specific dimension together. Trigger even if the user just
  says "let's go through my tasks" or "review what I have" — the task list checkup is the right
  default for any task list review. Inspired by the GTD (Getting Things Done) methodology
  by David Allen.
---

# Task List Checkup Skill

A collaborative checkup tool that evaluates any task list against 5 dimensions inspired by
the GTD methodology (Getting Things Done by David Allen), then helps you work through
specific dimensions interactively.

---

## Step 1: Acquire the Task List

Check for task data in this priority order:

1. **User-specified source** — If the user has named a specific app or tool ("use my Todoist",
   "pull from Things", "check Notion"), use that source. If a matching MCP tool is available,
   call it. If not, let the user know and fall back to the next option.

2. **Connected MCP / todo tool** — Scan available tools in the session for anything that
   looks like a task manager or todo list connector. This includes (but isn't limited to):
   Things 3, Todoist, Notion, Asana, Linear, TickTick, OmniFocus, Reminders, or any tool
   whose name or description suggests task/project management. If one is found, call it to
   fetch the user's tasks — prefer inbox, today, upcoming, and active project areas.
   Tell the user which app you're pulling from so they know what's being audited.
   If multiple todo-like tools are connected, ask the user which one to use.

3. **Chat history** — Scan the conversation for any pasted task lists, markdown checklists,
   or described task sets. If found, confirm with the user: "I'll use the task list you
   shared earlier — is that current?"

4. **Uploaded file** — If the user has attached a `.txt`, `.md`, `.csv`, or export file,
   read it. Common formats: Things CSV export, Todoist CSV, Notion table export, plain markdown.

5. **Ask the user** — If none of the above yield data, ask: "Please paste your task list,
   or describe what you're working with. Any format works — a plain list is fine."

### Normalizing Tasks

Internally represent each task as:
```
- title: string (required)
- project: string | null
- area: string | null
- due_date: date | null
- tags: string[]
- notes: string | null
- last_modified: date | null  (if available)
- status: "inbox" | "today" | "upcoming" | "someday" | "waiting"
```

If a field isn't available from the source, mark it null. Don't ask the user to fill in
missing metadata — infer what you can, flag gaps in the checkup.

---

## Step 2: Determine the Mode

After acquiring the task list, identify which mode the user wants:

### Mode A — Full Checkup
Triggered when the user hasn't specified a particular dimension, or says things like
"check up on my tasks", "full review", "weekly review", "how am I doing".

Run all 5 dimensions (see below) and produce a **Task List Health Report**.

### Mode B — Dimension Deep-Dive
Triggered when the user names or implies a specific dimension:
- "let's clarify these", "which tasks are vague" → **Clarify**
- "what should I work on today / right now" → **Engage**
- "am I missing anything" → **Capture**
- "help me organize these" → **Organize**
- "what's stale / overdue" → **Reflect**

Jump straight to that dimension's deep-dive flow. Don't run the full checkup first unless asked.

You can also switch modes mid-conversation — if the user finishes a deep-dive and asks
"what about reflect?", move to that dimension.

---

## The 5 Dimensions — Audit Logic

> These dimensions are drawn from the GTD (Getting Things Done) methodology by David Allen.

### 1. Capture
**Goal:** Everything that has your attention is externalized.

Flag tasks that suggest incomplete capture:
- Tasks with notes like "also need to...", "plus...", "and..." — hidden sub-tasks not captured
- Vague "project-shaped" tasks with no breakdown (e.g., "Website redesign" as a single task)
- Anything that seems like it spawned from a meeting, email, or conversation but has no context

**Checkup verdict:** "X tasks may be hiding uncaptured work"

**Deep-dive flow:** For each flagged task, ask: "Does this task have sub-tasks you haven't
captured yet? Should we break it down?" Help the user articulate and capture anything hiding.

---

### 2. Clarify
**Goal:** Every task is a clear, actionable next step — a physical action starting with a verb.

Flag tasks that:
- Use vague verbs: "handle", "deal with", "think about", "look into", "figure out", "work on"
- Are nouns only: "Budget", "Mom", "Website" — no verb, no action
- Are too broad to do in one sitting without a clear first step
- Have no definition of "done"

**Checkup verdict:** "X tasks are unclear or not actionable"

**Deep-dive flow:** Present each flagged task and ask: "What's the actual next physical action
here?" Help rewrite to something like "Call X to confirm Y" or "Draft outline for Z". Rewrite
collaboratively, don't just suggest — ask the user to confirm the rewrite.

---

### 3. Organize
**Goal:** Every task is in the right bucket with enough context to be trusted.

Flag tasks that:
- Are in Inbox (not yet assigned to a project or area)
- Have a project but no clear context (where/what tool/energy level needed)
- Seem misplaced — e.g., a someday item with a due date, or a waiting-for with no person named
- Are duplicates or very similar to another task

**Checkup verdict:** "X tasks are unorganized or misplaced"

**Deep-dive flow:** Walk through flagged tasks. For each: suggest the right project/area,
ask about context needed, and help rename/move as appropriate. If there are duplicates,
surface both and ask which to keep.

---

### 4. Reflect
**Goal:** Your system is current, trusted, and not overloaded.

Flag tasks that:
- Are overdue (due date passed with no completion)
- Are stale — haven't been touched or modified in 2+ weeks (if metadata available)
- Are in a heavily overloaded project (e.g., 20+ tasks in one project with no prioritization)
- Are "someday/maybe" items that have been sitting for a long time
- Are "waiting for" items with no follow-up date

**Checkup verdict:** "X tasks need reflection — stale, overdue, or overloaded"

**Deep-dive flow:** Present flagged tasks in groups (overdue first, then stale). For each:
offer choices — "Delete, defer, delegate, or do?" Help the user make the call, don't just
surface the problem. After the sweep, give a quick before/after: "We cleared X tasks,
deferred Y, and deleted Z."

---

### 5. Engage
**Goal:** It's clear what to work on right now, given your context and energy.

Flag when:
- Nothing is marked "today" or high priority — no clear starting point
- Everything is marked urgent — priority inflation, can't discern what matters
- Tasks labeled "today" don't match the user's available time/energy
- High-value tasks are perpetually deferred (appear repeatedly in reviews)

**Checkup verdict:** "Your engage layer is [clear / overloaded / empty]"

**Deep-dive flow:** Ask about current context: "How much time do you have? What's your
energy level — high focus, medium, or low?" Then surface 3–5 tasks that best fit their
context right now. Don't just list — explain why each made the cut based on GTD criteria
(context, time, energy, priority).

---

## Mode A Output — Task List Health Report

Use this structure for a full checkup. Keep it scannable — no walls of text.

```
## Task List Health Report
[date / task list source]

### 📥 Capture — [score: Good / Fair / Needs Work]
[1–2 sentence summary]
[Flagged tasks, if any — brief list]

### ✏️ Clarify — [score]
[summary]
[Flagged tasks]

### 🗂 Organize — [score]
[summary]
[Flagged tasks]

### 🔍 Reflect — [score]
[summary]
[Flagged tasks]

### 🎯 Engage — [score]
[summary]
[Current state of today/priority queue]

---
### Overall
[1–2 sentence honest take on the system's health]

**Where to start:** [recommend one dimension to deep-dive first, with brief reason]
```

After delivering the report, always offer: "Which dimension do you want to work through
together?" — this is the natural bridge to Mode B.

---

## Collaboration Principles

- **Be honest, not cheerful.** If someone's system is a mess, say so clearly but constructively.
  A trusted system is the goal — false reassurance undermines that.
- **Don't overwhelm.** In deep-dive mode, present 3–5 flagged tasks at a time, not the whole list.
- **Ask, don't assume.** Especially during Clarify and Organize — the user knows their context.
  Your job is to surface the right question, not impose the answer.
- **Track progress.** If the user fixes things during a deep-dive, acknowledge it: "That's 4
  tasks clarified. Want to keep going or call it here?"
- **Respect incompleteness.** A partial review is better than no review. If the user wants to
  stop mid-dimension, summarize what was done and offer a way to resume later.

---

## Edge Cases

- **Tiny list (<10 tasks):** Skip the scorecard format. Just have a conversational checkup.
- **Huge list (100+ tasks):** Warn the user the checkup may take a moment. Focus the full checkup
  on high-signal areas (Inbox count, overdue count, today queue) and do a lighter pass on
  the rest.
- **No metadata available** (just raw titles): Note this upfront. Clarify and Capture dimensions
  still work well. Reflect will be limited since there's no date info.
- **Non-GTD systems:** If the user's list clearly uses a different system (Kanban, MoSCoW, etc.),
  acknowledge it and adapt — apply the 5 dimensions as a lens, not a mandate.
