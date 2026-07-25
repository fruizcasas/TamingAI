# Externalized memory: the conversation is disposable, the record is not

*Terms used below, standalone: a* **draft** *is a dated landing spot for anything raw and
unprocessed — a note, a fragment, a half-formed idea — kept safe before anyone has decided
what it means. A* **shared record** *is the single document, distilled from one or more
drafts, that both the human and the model treat as ground truth for a piece of work — not the
conversation that produced it.*

## The problem

Conversation is not storage. A decision made mid-chat, a detail mentioned once and never
written down, a conclusion that only exists as a paragraph buried in a long exchange — none of
that survives a compacted context, a new session, or a model swap. It isn't dramatically
"lost" on day one; it's merely loose, which is the antechamber of lost. The day nobody happens
to remember it, it's gone for good.

## The mechanism

The fix is a short, fixed staircase that anything raw has to pass through on the way to
becoming permanent, with no step that acts as a silent trash can:

1. **Land it raw.** Anything arriving unprocessed — a fragment, a raw note, an unfinished
   thought — goes straight into a dated draft, under a plain label. Nothing has to be
   understood yet for it to be safe; it just has to exist somewhere findable.
2. **Distill it.** A drafting pass works the raw material until no real gap is left unresolved,
   and produces the shared record — the actual document both sides are meant to treat as
   truth from then on, not a summary of the conversation that produced it.
3. **Route it by weight.** A small, one-off matter becomes a lightweight tracked item, whose
   filename alone encodes its current state. A real piece of work with a beginning, middle,
   and end becomes a tracked ticket on a project board.
4. **Graft it into the tree, if it grows.** If a tracked item matures into something with
   lasting structure of its own, it's given a permanent place in the project hierarchy, with
   its own history — a separate, deliberate step from simply tracking it.

## Filesystem as the database

None of the state above lives in a hidden database table. The project board is a folder tree,
version-controlled like everything else; the folder an item currently sits in — to-do,
in-progress, done — *is* its state, not a label that could drift out of sync with reality.
Nothing about a task's status requires querying an application; it's readable directly by
opening the directory.

## Why this matters generally

The generalizable claim is simple and easy to under-value: a long-running AI collaboration
is only as durable as its externalized record, and "the model will remember" is not a
retention policy. The concrete test isn't whether a conversation happened — it's whether
whatever mattered from it is now sitting in a written, findable, permanent place that survives
the conversation ending. Nothing gets silently dropped along the way, either: material that
isn't ready to act on yet is explicitly parked with a visible state, rather than disappearing
because no clear next step existed for it.

---

Contact: Fernando Ruiz Casas — reach me with any question by addressing my gmail account,
`fruizcasas`.

© 2026 Fernando Ruiz Casas
