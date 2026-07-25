# Dosing compute: matching model effort to what the task actually needs

*Terms used below, standalone: a* **cruise level** *is the model tier an operator declares as
enough for their normal work — the baseline a session is expected to run at unless the task
specifically calls for more.*

## The problem

Running every task on the largest, most expensive model available is an easy default and a
wasteful one. It also doesn't reliably fix the actual failure mode it's often reached for:
when a task needs better judgment, adding more effort from an under-powered model is not the
same thing as using a model actually capable of that judgment.

## The mechanism

Before doing any task, the first step is classifying the task itself, not just starting on
it — because different work genuinely asks for different capability:

- Mechanical, repetitive work (search-and-replace, cloning a pattern, a rote sweep) is routed
  to the smallest, cheapest available model, at low effort.
- Routine, reproducible work (ordinary prose, steady day-to-day tasks) gets a mid-tier model.
- Real judgment — diagnosis, a second opinion, evaluating reasoning itself — is the only
  category that earns the largest model, at full effort.

The governing rule is specific and a little counter-intuitive: if judgment is falling short,
the fix is a bigger model, never more effort from the same one. Effort is not a substitute for
capability — pushing a smaller model harder on a judgment task doesn't turn it into a better
judge, and using an under-powered model as an evaluator is treated as simply not allowed,
regardless of how much effort is dialed up.

## Self-reporting when over-provisioned

Each operator declares a cruise level — the tier that's genuinely enough for their normal
work. If a session starts on something more powerful than that declared baseline, the session
says so up front, plainly, rather than quietly enjoying the extra headroom: it's flagged as a
heads-up, not a unilateral downgrade — whether to step down or keep the larger model stays the
operator's informed choice, but the fact that it's more than necessary doesn't go unmentioned.

## Why this matters generally

The underlying claim generalizes past any specific system: compute has a real cost, and
"just use the biggest model for everything" is not a safety margin, it's an unexamined
default. A task-tiered dosing discipline — cheap model for mechanical work, capability
escalation (not effort escalation) for judgment-heavy work, and honest self-reporting when a
session is over-provisioned relative to what it actually needs — treats compute as a resource
to be matched to the task, not maximized by habit.

---

Contact: Fernando Ruiz Casas — reach me with any question by addressing my gmail account,
`fruizcasas`.

© 2026 Fernando Ruiz Casas
