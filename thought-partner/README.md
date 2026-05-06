# thought-partner

A Claude skill that transforms rambling thoughts into well-contextualized, actionable tasks through interactive clarification.

Works with brain dumps, voice transcripts, meeting notes, scattered thoughts, or anything on your mind.

> Inspired by *Getting Things Done* by David Allen. Implements the Capture → Clarify → Organize workflow.

---

## What it does

**Phase 1: Extract** — Automatically identifies actionable items, projects, decisions, and communications from your thought stream.

**Phase 2: Clarify** — Works with you interactively to refine vague items, add context, and determine real next actions.

**Phase 3: Finalize** — Delivers a structured, scannable output ready for your task manager.

The 6 categories:

| Category | What goes here |
|---|---|
| 📋 **Immediate tasks** | Single-step actions you can do soon |
| 🎯 **Projects / Long-term** | Multi-step outcomes or future work |
| ❓ **Open questions** | Decisions needed before you can act |
| 💬 **Communications** | Things to tell, ask, or discuss with others |
| ⏳ **Waiting for** | Blocked on external input |
| 💡 **Someday/maybe** | Ideas to explore but not committed |

---

## Usage

Once installed, just share your thoughts naturally. The skill auto-triggers on phrases like:

```
"I have a bunch of thoughts"
"Help me organize my thinking"
"Here's what's on my mind"
"I need to figure out what to do with all this"
"Brain dump: [paste]"
```

### Input formats

Any format works:
- Rambling paragraphs
- Voice-to-text transcripts
- Scattered bullet points
- Meeting notes
- Email threads with todos buried in them
- Stream of consciousness

---

## Example interaction

```
You: I've been thinking a lot about the website. We really need to redesign it but I don't
know if we should hire an agency or do it in-house. Also I need to talk to Sarah about
something. Oh and the client keeps asking about the roadmap but we haven't finished it yet.
I should probably set up a meeting with the team. The API migration is still hanging over
my head, not sure if we should do it now or wait until Q3. And I really need to respond
to that email from John about the budget. Also fix that login bug people have been reporting.

Claude: I've extracted 8 items from your thoughts. Here's what I'm seeing:

**Immediate Tasks (3)**
- Respond to John's email about budget
- Fix login bug
- Set up team meeting

**Projects/Long-term (2)**
- Website redesign
- Client roadmap completion

**Open Questions (2)**
- Website: hire agency or do in-house?
- API migration: now or Q3?

**Communications (1)**
- Talk to Sarah

---

Let's clarify these together. Starting with the unclear ones:

**"Talk to Sarah"** - What do you need to discuss with her? Is this to ask something, tell
her something, or work through a decision together?

You: Oh right, I need to tell her the deadline for the client deliverable moved to next Friday
and make sure she's OK with that timeline.

Claude: Got it. I'll update that to:
**Tell Sarah: Client deliverable deadline moved to next Friday, confirm she can meet it**

Moving to Communications.

**"Website redesign"** - You mentioned not knowing whether to hire an agency or go in-house.
Is this committed work you're starting soon, or more of a "someday we should do this" thing?

You: It's committed - we need to start in the next month. The in-house vs agency question
needs to be answered first though.

Claude: OK, so this is a committed project blocked on a decision. Two options:

1. Make the decision the first action: "Decide: hire agency vs in-house for website redesign"
2. Move it to Open Questions until you've decided, then come back to project planning

Which feels right?

You: Let's make the decision the first action.

Claude: Perfect. I'll structure it as:
**Website Redesign** (Project)
- First action: Decide whether to hire agency or do in-house - compare cost, timeline, quality

What criteria matter most for that decision - cost, speed, quality, or something else?

You: Mostly timeline. We need it done by Q3.

Claude: Adding that context:
**Website Redesign** (Project)
- First action: Decide agency vs in-house (Q3 deadline, optimize for timeline)

**"Client roadmap"** - You said it's not finished yet. What's blocking it, and what's the
next step to move it forward?

You: We have most of it, just need final sign-off from the leadership team. I should probably
schedule that review meeting.

Claude: So the next action is:
**Schedule leadership review meeting for client roadmap sign-off**

Is that still part of the client roadmap project, or is it its own immediate task?

You: It's an immediate task - once that meeting happens, the roadmap is done.

Claude: Got it, moving it to Immediate Tasks.

**API migration timing** - What do you need to figure out to make this decision? Is it a
technical question, a resource question, or something else?

You: Mostly resources. If we do it now, the team is stretched thin. But if we wait, the
legacy system might cause issues.

Claude: So the question is really: "Can we afford the team bandwidth hit now, or is it riskier
to delay?"

To answer that, what would you need to know - team capacity, risk assessment, or both?

You: Yeah, probably need to map out team capacity for the next two months and weigh it against
the risk of waiting.

Claude: I'll phrase it as:
**Open Question: API migration now or Q3?**
- To decide: Map team capacity through Q3 + assess risk of delaying migration

Want to turn that into an immediate task, or leave it as an open question for now?

You: Make it a task - I should do that analysis this week.

Claude: Perfect. Adding:
**Immediate Task: Map team capacity through Q3 and assess API migration timing risk**

---

We've clarified everything! Here's your finalized structure:

## Finalized Task Structure — May 5, 2026

---

### 📋 Immediate Tasks (5)
- Respond to John's email about budget
- Fix login bug
- Schedule leadership review meeting for client roadmap sign-off
- Map team capacity through Q3 and assess API migration timing risk
- Set up team meeting

### 🎯 Projects / Long-term (1)
- **Website Redesign** - Need completion by Q3, optimizing for timeline
  - First action: Decide agency vs in-house based on Q3 timeline constraint

### 💬 Communications (1)
- Sarah: Tell her client deliverable deadline moved to next Friday, confirm she can meet it

---

### Summary
Your thoughts surfaced 5 immediate tasks, 1 major project (website redesign), and 1 important
communication. The website decision and API timing are the two strategic questions that need
resolution soon.

**Next steps:** Ready to copy these into your task manager? Or want to run a deeper checkup
using the tasklist-checkup skill?
```

---

## How it works

### 1. Initial extraction (automatic)
The skill scans your thought stream and identifies:
- Actionable items
- Multi-step projects
- Open decisions
- Communications needed
- Reference information

Then presents a draft categorization.

### 2. Interactive clarification (collaborative)
The skill doesn't just dump categories - it works through unclear items with you:
- Asks what the real next action is
- Helps distinguish tasks from projects
- Surfaces missing context
- Identifies blockers and dependencies

You respond, it reflects back clearer versions, you confirm.

### 3. Finalized output
You get a scannable structure with:
- Clear next actions (not vague "handle X" tasks)
- Projects properly identified with first actions
- Context on why things matter
- Ready to copy into your task manager

---

## Integration with tasklist-checkup

These two skills complement each other:

- **thought-partner**: Rambling thoughts → Structured tasks
- **tasklist-checkup**: Structured tasks → Health audit across 5 GTD-inspired dimensions

Workflow:
1. Use thought-partner to turn brain dumps into tasks
2. Use tasklist-checkup to review and maintain your task list over time

---

## License

MIT
