---
description: Design with Opus, then execute with Sonnet/Haiku workers sized to each step
---

Task: $ARGUMENTS

Run this in three stages. Do not skip to building before stage 1 is done.

**1. Design (Opus).** Call the `designer` subagent with the task above. It returns a plan:
numbered steps, each tagged mechanical / standard / hard. Show me that plan before building
anything non-trivial comes from it.

**2. Build (Haiku/Sonnet/Opus, sized to each step).** Work the plan step by step:
- **mechanical** → dispatch to the `grunt` subagent (Haiku). Batch independent mechanical
  steps into parallel Agent calls.
- **standard** → do it yourself in this thread, or spawn a `general-purpose` subagent if the
  step is large enough to want its own context.
- **hard** → dispatch to the `senior-builder` subagent (Opus). Don't downgrade this to
  yourself even if you think you can manage it — that's the whole point of the tier.
- If a grunt or senior-builder step comes back confused or wrong, don't retry it blindly —
  look at why before re-dispatching.

**Re-tier live — the plan's tags are a starting guess, not a ceiling.** While working any
mechanical or standard step, escalate that step to `senior-builder` (Opus) the moment any of
these happens, even though the plan didn't flag it as hard:
- you're about to commit to an approach for something genuinely ambiguous or hard to reverse
- the same step has failed twice in a row, or you're stuck in a fix-break-fix loop
- you're about to declare the whole task done and something about it still feels unverified
Don't push through on a hunch that you can manage it — that hunch is exactly the failure mode
this exists to catch. Note in the final report every time this fired and why, so the plan's
tagging gets better next time.

**3. Report.** One pass of what was built, what each tier actually did, every live escalation
and why, and anything the plan got wrong once it hit real code.
