# A self-amending rulebook, gated by rank and sign-off

*Terms used below, standalone: the* **LexCorpus** *is the name of the rulebook this piece is
about — the small legislative core, loaded early in a session's bring-up, that everything
else's rules build on. To* **legislate** *here means to add, change, or retire a rule in the
LexCorpus itself — not code deployment, the actual rulebook the system reads at bring-up.*
**Judge ≠ party** *is the principle that whoever authored or approved a rule should not be the
one certifying it's correct — a separate, independent check is required.*

## The problem

A rulebook that never changes ossifies — it can't absorb a real lesson without a human
manually rewriting files. A rulebook that changes freely rots the other way — silent edits,
no record of why, no way to tell a considered amendment from an accidental one. Both failure
modes are common; neither is acceptable if the rulebook is what a long-running AI session is
actually supposed to obey.

## The mechanism

The fix is to treat rule-changes as a small number of named, deliberate gestures — not an
open-ended "edit the file":

1. **Legislate** — a rule is born, from an observed failure or a direct instruction. The
   act of recording it *is* the approval — there's no separate rubber-stamp step, but there
   is exactly one entry point, and it always carries a date and a state.
2. **Watch** — a standing, automatic check runs on every edit to a governing file, regardless
   of who triggered it — nobody has to remember to invoke it.
3. **Periodic audit** — the whole rulebook gets swept on a schedule, not just individual
   changes: which rules are still actually relevant, which have quietly become dead weight.
4. **Derogate** — a rule that's gone stale is moved out of the active set, with a dated reason
   attached, rather than being silently deleted. The history isn't erased, it's marked
   retired.
5. **Declared exception** — if a new, narrower rule needs to override a broader existing one,
   and there's a real, specific justification, that override is stated openly as an exception
   — never a quiet contradiction sitting unresolved in two places at once.

Layered on top of the five gestures is rank: rules are classified into a small number of
levels (constitutional, organic, regulatory), and the authority required to touch a level
scales with it — a routine regulatory tweak needs far less sign-off than a change to a
constitutional rule, which requires an explicit, deliberate elevation of authority that is
never assumed and never left standing after the change is made.

## One body of law, not one file

The LexCorpus isn't a single monolithic rulebook. The shared, root-level LexCorpus holds what
applies everywhere; beyond it, each individual skill or utility carries its own local rules —
narrower, specific to what that piece does. The same discipline governs both: this is applied
as ordinary legal hierarchy works, no exception — a lower-level rule can add detail or
narrow a case, but it can never weaken, contradict, or override what a higher-level rule
already states. A local rule always yields to the shared one it sits under.

That structure creates a standing question: where should a given rule *live*? The working
answer is to consolidate upward on repetition, not by default. A rule stays local as long as
it only matters to the one skill it was born in. The moment the same pattern shows up
independently in more than one place, that repetition is itself the signal to promote it to
the shared level instead of leaving duplicate, potentially drifting copies scattered around.
The goal stated for this is blunt: the best rule is the one that doesn't need to exist at all
— fewer things to remember beats more things correctly remembered, so consolidating a rule
that now spans several components removes N-1 copies rather than adding one more.

Moving a rule up a level is not a mechanical action available to just anyone. It requires a
correspondingly qualified actor to sign off — a small ladder of authorization (roughly:
routine / broadly-trusted / highest-authority), where touching the top of the hierarchy needs
the deliberate, explicit involvement of whoever holds that top authority, granted for that one
act and not left standing afterward.

## Judge ≠ party

The harder problem is self-certification: the same process that authored a rule change
naturally wants to believe its own change is correct. The stated design answer is an
independent auditor — a separate process, walking the same rule tree top-down, checking for
contradictions and improper overrides, without the context or the stake of whoever proposed
the change.

## What's built, and what's declared but not yet built

The five gestures, the rank-gated sign-off, and the standing edit-watch are running today —
every touch to a governing file gets checked against its declared rank, and an edit at the
wrong authority level is flagged. The independent, isolated auditor described above is not:
today, that step is honest-TBD — a stated design requirement with no live implementation yet,
and saying that plainly matters more than describing it as if it already ran, since a
self-amending rulebook whose only checker is itself is exactly the failure mode this piece is
supposed to avoid.

## Why this matters generally

The generalizable claim is narrow: if an AI system is allowed to propose or make changes to
its own governing rules, the safety of that isn't in the eloquence of the change — it's in
whether the process around it has named gestures instead of an open edit surface, whether
authority to approve scales with how consequential the rule is, and whether the check on a
change is ever performed by the same actor that made it. Where that last piece isn't built
yet, the honest move is to say so, not to describe the goal as the current state.

---

Contact: Fernando Ruiz Casas — reach me with any question by addressing my gmail account,
`fruizcasas`.

© 2026 Fernando Ruiz Casas
