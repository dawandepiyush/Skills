# tasklist-checkup

A Claude skill that checks up on your task list across 5 dimensions — and helps you work through them interactively.

Works with Things 3, Todoist, Notion, Linear, plain text, uploaded files, or anything pasted into chat.

> Inspired by *Getting Things Done* by David Allen. The five review dimensions (Capture, Clarify, Organize, Reflect, Engage) are drawn from the GTD methodology.

---

## What it does

**Full Checkup** — Run all 5 dimensions against your task list and get a health report with a score per dimension, flagged tasks, and a recommended starting point.

**Dimension Deep-Dive** — Pick one dimension and work through it collaboratively. Claude flags the relevant tasks and walks through them with you one by one.

The 5 dimensions covered:

| Dimension | What gets checked |
|---|---|
| 📥 **Capture** | Tasks hiding uncaptured sub-tasks; ideas not yet externalized |
| ✏️ **Clarify** | Vague verbs, noun-only tasks, no clear next action |
| 🗂 **Organize** | Inbox items, misplaced tasks, missing context or project |
| 🔍 **Reflect** | Overdue, stale (2+ weeks), overloaded areas, aging someday items |
| 🎯 **Engage** | Priority inflation, empty today queue, tasks that match your current time/energy |

---

## Usage

Once installed, just talk naturally. The skill auto-triggers on phrases like:

```
"Check up on my tasks"
"Let's review my task list"
"Help me review my todo list"
"What should I work on?"
"Let's work on clarify"
"What's stale in my list?"
```

### With a connected todo app (MCP)

If you have a todo app connected via MCP (e.g. Things 3, Todoist), Claude will auto-detect it and pull your tasks directly — no copy-pasting needed.

```
You: "Check up on my tasks"
Claude: [fetches from connected app, runs full checkup]
```

If multiple todo apps are connected, Claude will ask which one to use.

### With a pasted list

Any format works:

```
You: "Here's my task list — [paste]"
Claude: [runs checkup on pasted content]
```

Supported formats: markdown checklists, plain numbered lists, CSV exports from Things/Todoist/Notion, or just a brain dump.

### With a file

Attach a `.txt`, `.md`, or `.csv` export and ask for a checkup.

---

## Example interaction

```
You: Check up on my tasks

Claude: ## Task List Health Report — April 21, 2026

### 📥 Capture — Fair
8 inbox items, 2 hiding sub-tasks. "Website redesign" and "Immigration" 
look like projects masquerading as tasks.

### ✏️ Clarify — Needs Work
6 tasks flagged. "Look into visa options" and "Handle onboarding" use 
vague verbs with no clear next action.

...

Where to start: Clarify — that's where the most friction is right now.

---

You: Let's work through clarify

Claude: Let's go. First up: "Look into visa options" — what's the 
actual next physical action here? Schedule a call, fill a form, 
research something specific?
```

---

## Switching modes mid-conversation

You can move between modes freely:

```
"Now let's look at reflect"
"What about my today list?"
"I have 30 minutes and low energy — what should I do?"
```

---

## Compatible todo apps (via MCP)

Tested:
- ✅ Things 3

Should work with any MCP-connected task manager:
- Todoist
- Notion
- Linear
- Asana
- OmniFocus
- Apple Reminders
- TickTick

If your app isn't MCP-connected, paste or upload your list — the checkup works the same way.

---

## License

MIT
