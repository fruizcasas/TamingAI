# Multi-agent roles: a long-lived orchestrator, short-lived task agents, one human authority

*Terms used below, standalone: an* **orchestrator** *is the long-lived model instance that
carries a session end to end — it plans, decides, and coordinates, but doesn't necessarily do
every piece of work itself. A* **task agent** *is a short-lived instance dispatched for one
narrow mission, with a fresh, minimal context, that reports back and then ends.*

## The problem

As soon as more than one AI instance is involved in a piece of work, three questions need a
real answer, not an implicit one: who is actually allowed to authorize new work being
generated, who is accountable when a delegated instance gets something wrong, and how does a
short-lived helper avoid inheriting all the drift and clutter of a long conversation it never
needs to see.

## The mechanism

**Roles are fixed, not improvised per task.** A human holds sole authority to approve
generative work — nothing gets produced without an explicit go-ahead, and that authorization
is never inferred from tone, urgency, or a plausible-sounding request; it has to be actually
given. The orchestrator proposes, explains, and designs, but generation is the last step, not
the first — it comes after approval, never before it. Above the orchestrator's normal
authority, certain higher-consequence actions require a deliberate, temporary elevation of
authority, exercised only for that one act and never left standing afterward — the opposite
of a permanent admin role.

**Task agents get a narrow, fresh brief, not the whole conversation.** When a mission needs
dedicated focus, it's handed to a task agent rather than handled inline by the orchestrator
carrying its full accumulated context. Before that agent starts, it gets briefed specifically:
what the mission is, what behavior is expected, what it should watch for — a short, deliberate
handoff rather than dumping the entire history on it. The agent does its one job and reports
back; it doesn't persist afterward.

**A separate, deterministic watcher observes every action.** Independent of both the
orchestrator and any task agent, a non-model watcher logs and flags anomalies as they happen
— it doesn't block, and it doesn't wait to be invoked; it's simply always present.

**Delegating a task doesn't delegate the responsibility for it.** The orchestrator remains
accountable for what a task agent does under its dispatch — "the sub-agent got it wrong" is
not treated as an adequate account of a failure. Asking for more information before deciding
is explicitly not the same thing as handing off the decision itself; it's how the eventual
decision gets made responsibly.

## Why this matters generally

None of these roles require exotic infrastructure — a long-lived coordinator, disposable
narrowly-briefed task agents, an independent non-model observer, and a single human authority
gate are all buildable with ordinary tooling. What matters is treating them as fixed roles
with real boundaries — who may authorize generation, who may be dispatched and forgotten, who
watches without controlling — rather than letting any capable instance drift into doing
whatever seems locally convenient.

---

Contact: Fernando Ruiz Casas — reach me with any question by addressing my gmail account,
`fruizcasas`.

© 2026 Fernando Ruiz Casas
