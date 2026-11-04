# Claude Code Tool Notes

Claude Code is the reference harness: skills speak its vocabulary
(`Agent` for a subagent dispatch, todos, `Skill`). These notes cover the
two places Claude Code differs from the skills' default shape: a
structured question tool, and a cheaper way to run a plan.

## Questions go through AskUserQuestion

Claude Code has `AskUserQuestion`: a question with up to four options,
each with a label and a line of description, plus a free-text "Other"
the harness adds on its own. Whenever a skill has you ask your human
partner something with choices you can name (superpowers:brainstorming
above all), ask through it, one question per call, with your
recommended option first and labelled "(Recommended)". They click the
option they want or type an answer; a wall of numbered questions in
text makes them write all the answers out by hand. An open question,
one with nothing to pick from, stays in text.

## Cheaper orchestration for subagent-driven development

The controller session is the most expensive seat in a
superpowers:subagent-driven-development run: it reads every dispatch
result and every report, and it usually runs on the session's most
capable model. Claude Code supports nested subagents (three layers below
the main conversation by default; `CLAUDE_CODE_MAX_SUBAGENT_SPAWN_DEPTH`
adjusts it), so the whole loop can run one layer down.

When your human partner asks for it — or has said the session model is
too expensive to spend on coordination — dispatch ONE orchestrator
subagent on a mid-tier model with the plan path and the instruction to
use superpowers:subagent-driven-development end to end. The orchestrator
dispatches its own implementers and reviewers per that skill's Model
Selection; the workspace and ledger live on disk, so nothing is lost to
the extra layer. Its final message must carry the "Rulings I made" list
verbatim — that list is how the decisions reach your human partner, and
you relay it, not summarize it.

Do this only for a whole plan. Nesting a single task's dispatch buys
nothing and adds a seat.
