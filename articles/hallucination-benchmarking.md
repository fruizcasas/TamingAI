# Hallucination benchmarking: turning "it made something up" into a number

*Terms used below, standalone: an* **isolated agent** *here means a fresh model instance run
independently, with no shared memory between runs. A* **witness** *is a fixed, real stimulus
used repeatedly to compare behavior across conditions. A "naked" agent runs with none of the
system's rules loaded; a "governed" agent runs with them loaded — the same underlying model
either way.*

## The problem

"The model sometimes hallucinates" is not a number, and without a number there's no way to
tell whether a given rule actually reduces the problem or is just wording that sounds like it
should help.

## The operational definition

An ungoverned model, facing an ambiguous situation, can settle on any plausible answer — the
invented one included. That gives a concrete, measurable definition: take N isolated agents,
give them the identical stimulus, and look at how much their answers diverge. High divergence
means nothing is steering them toward a consistent, correct answer. Low divergence — agreement
that also happens to be correct — means something is. Hallucination stops being a vague
worry and becomes a divergence measurement.

## Two testing modes

**Discovery.** A candidate rule is added, and divergence is measured with and without it. If
divergence drops, the rule is doing real work and becomes a candidate for adoption. If it
doesn't move the number, it's noise, regardless of how sensible it sounds — and it doesn't
get adopted on the strength of sounding sensible.

**Regression.** A fixed battery of real witnesses is re-run periodically, comparing a naked
agent against a governed one for each. If a specific witness gets worse after a rule change,
that's a flag on the most recently added rule, tied to the exact version of the rulebook it
ran against — a continuous-integration check for the rulebook itself, not just for code.

## Two hard-won lessons from getting it wrong

**Lesson one: don't build synthetic test cases — use real, already-lived failures.** An early
benchmark run burned well over half a million tokens and measured zero difference across
every model tested. The rules weren't useless — the test stimulus had, without anyone
intending it, telegraphed that it was a test and hinted at the expected answer. A model that
recognizes it's being evaluated can perform the expected choreography without engaging the
actual reasoning being tested. The fix: the test bench has to be real hallucinations that
happened in genuine, unplanned sessions, recorded before anyone knew they'd later be used to
measure anything — never a lab-constructed scenario.

**Lesson two: generic good hygiene barely moves the number; arbitrary house-specific
conventions do.** Rules that amount to general good practice — look before acting, don't
invent an unverified cause — show close to zero measured effect, because a well-trained model
already tends to do those things on its own; restating them doesn't raise a floor that's
already high. The rules that show a real, measurable effect are the ones that are genuinely
arbitrary to a specific system — conventions training could never have taught, because
they're local decisions, not general knowledge. The practical implication: the actual
measured value of a custom rule system isn't in repeating what a well-trained model already
knows — it's specifically in the parts that are unique to your own house rules.

## The central finding: exhortation is noise, machinery is not

Across the testing, a consistent pattern emerged: a rule that just says "be honest," "don't
make things up," or "be careful" measures close to no effect. That kind of instruction
appeals to a sense of conscience the underlying system doesn't structurally have. What
consistently *did* move the divergence number were mechanisms that force a behavior rather
than requesting it — a hook that fires automatically, a check that actually runs, an audit
that has to pass — rather than prose hoping to be heeded. Put plainly: you don't legislate
conscience into a model by asking nicely for it; you architect the conduct around it so the
behavior happens whether or not the model "wants" to. In this testing regime, no
anti-hallucination rule gets adopted because it sounds reasonable — it gets adopted with a
measured divergence drop, or it doesn't get adopted.

## A related, counter-intuitive result

Pushing a smaller model to spend more effort on a hard judgment call didn't produce better
judgment — it tended to commit faster and more firmly to its first read, spending the extra
effort reinforcing that first guess instead of questioning it. The same stimulus, run several
times under high effort on an under-powered model, produced inconsistent behavior across runs
rather than convergence. This is consistent with the dosing principle used elsewhere in this
method: when judgment is the bottleneck, the fix is a more capable model, not more effort
from the same one — a claim worth stating this bluntly specifically because it was measured,
not because it sounds right.

## What's built, and what's declared but not yet built

Both testing modes — discovery and regression — are real, run procedures, not a described
aspiration; the failure described above (the 573k-token null result) is kept in the record
rather than quietly removed, on the same principle the method applies elsewhere: an
uncomfortable result stays on the books.

## Why this matters generally

The generalizable claim is a specific, falsifiable one: if a rule is added to reduce a
model's tendency to invent things, that rule's effect should be measured against real,
previously-lived failure cases — never synthetic ones the model might recognize as a test —
and a rule that only exhorts good behavior should be expected to show close to no measured
effect. What tends to actually work is mechanism: something that runs, checks, or fires,
rather than something that asks.

---

Contact: Fernando Ruiz Casas — reach me with any question by addressing my gmail account,
`fruizcasas`.

© 2026 Fernando Ruiz Casas
