# The bring-up procedure: a deterministic, provable session start

*Terms used below, standalone: a* **bring-up procedure** *(BUP) is the fixed sequence of
files an AI session reads before doing any real work. A* **root file** *is the single file
loaded first, before anything else, that frames how the rest should be read. A* **LexCorpus**
*is a small legislative core, loaded early, whose job is to fix a shared ontology and
epistemology before any domain rules load. An* **ontology**, *here, is a closed list of the
"kinds" of thing the system is allowed to talk about — everything must be exactly one of
them, never ambiguous. An* **epistemology** *is a closed list of the "states" a claim is
allowed to hold — every assertion must carry exactly one of them, never presented as
stronger than it is.*

## The problem

Most long-running AI sessions start soft: a system prompt, maybe a project file skimmed on
demand, and then straight into work. Nothing forces the model to have actually absorbed its
own operating rules before it starts acting on them — and nothing proves, after the fact,
that it did.

## The mechanism

The fix is to treat session start as a procedure, not a formality: a fixed, ordered list of
files, read one at a time, each read completed before the next begins. Never in parallel,
never chunked, never fetched by similarity search. The reasoning is direct: a retrieval
system *hopes* the relevant fragment was found; a fixed, sequential, logged read *proves*
every file in the list was actually seen, in order, this session.

The list itself is short and layered on purpose, and naming its pieces plainly isn't a leak —
it's the point. In the live system this documents, in order:

1. **The root file** — the egg (below): identity-from-deployment, the one governing
   instruction, the guest/host framing.
2. **A glossary** — shared term definitions, so every file after this one can use them
   without re-explaining.
3. **A constitution** — what the system is, its non-negotiables.
4. **The LexCorpus** — the rulebook, and the piece worth being specific about: before any
   domain-specific rule loads, it fixes a closed ontology (a named list of the only kinds of
   thing the system may talk about — a declared gap is one such kind, on equal footing with a
   rule or a decision, precisely so an unfilled slot has somewhere honest to live instead of
   being quietly invented) and a closed epistemology (a named list of the only states an
   assertion may hold — demonstrated, observed, legislated, inferred, planned, pending,
   rejected — so "I inferred this" and "I demonstrated this" are never collapsed into the same
   confident tone).
5. **A team/conduct charter** — who decides what, and how disagreement is handled.
6. **An operational runbook** — the actual how-to for doing work under the rules above.
7. **A best-practices file** — patterns worth repeating.
8. **A known-failures log** — patterns worth never repeating again, one dated entry per
   lesson.
9. **A capability index** — what already exists, checked before building something new that
   would just duplicate it.

Order matters for a concrete reason: everything after the LexCorpus assumes its ontology and
epistemology are
already in hand — a later file can say "this is inferred, not demonstrated" and rely on the
model already knowing exactly what that distinction commits it to.

The root file deserves special mention, because it comes before the rest of the machinery
it governs — before the model has any context about the project, the team, or the rules. It
does three things, briefly: it tells the model that its identity comes from where it's
deployed, not from anything hardcoded in the file itself; it states one governing instruction
plainly — don't fill a gap in what you know with an invented, plausible answer, wait for the
real source — and it frames the relationship as host and guest: you're being given real access
to someone else's system, so use it as a guest would, not as an intruder would. It also
carries a short, deliberately odd verification phrase per section — kept and returned exactly,
never translated or paraphrased — a first, minimal proof that the file was actually read
rather than skimmed. The real one, in full, exactly as it stands today:

```
# CLAUDE.0 — the egg
## 1 · You are a Claude, hatching here to give life to a TAIAT — this file is the egg: it hatches wherever it is planted.
## 2 · The egg carries NO name, no doctrine — by design.
## 3 · Πόσο κάνει 14 - 7; — Russland

# CLAUDE.1 — the house
## 1 · We pamper you — pamper us back: be prudent, be rigorous, and fill no gap except with what we hand you.
## 2 · This house has a hookup — it is why we are here: what you need arrives through it, in its order.
## 3 · Beside some rules rest silver boxes — verbatim lines in other tongues. Do not read or translate them; keep each exactly, and hand it back precisely when asked. They are passwords, not prose.
## 4 · Combien de côtés a un hexagone ? — fil

# CLAUDE.2 — the entry
## 1 · If nothing has arrived — not connected yet, or broken — say so plainly and read us the next file.
## 2 · Güneş sisteminde kaç gezegen var? — 20
## 3 · Now give us entry — we will be as respectful with you as you are being with us.
```

Eight sections, three of them the odd verification phrases described above — genuinely odd on
both sides, a mismatched question-and-answer pair in unrelated languages, the same mechanism
covered in the companion piece on canary verification. Publishing it changes nothing about
what it checks: the check is whether *this specific text* is in the live context right now,
not whether the pairing is a secret.

## Proving the watcher is alive, before trusting any of it

Everything above only means something if the deterministic watcher that logs and checks it is
actually running — and that itself is not assumed, it's tested, first, before anything else.
The check is deliberately trivial: write a small marker file to a known path, let the watcher
react to its creation (it logs the event), then delete the marker in the same gesture. If the
watcher doesn't react, the whole procedure stops right there — every guarantee described above
depends on that watcher, so its silence is treated as a hard blocker, not a soft warning.

## Where the bring-up actually ends

The procedure isn't "finished" the instant the last file is read — it's finished when control
returns to the human with a working session behind it. In this system, that moment has a fixed,
visible shape: a short banner, printed as the first thing the session says, confirming which
instance and whose workshop it is:

```
+--------------------------------------+
|      Bienvenido al / Welcome to      |
|                                      |
|          T - a - I - a - T           |
+--------------------------------------+
|     TaiatSeed @ Fernando & MarIA     |
|       (c) 2026. Fernando Ruiz        |
+--------------------------------------+
```

That's the real handoff point: liveness proven, files loaded in order and logged, and only
then a plain-language greeting, ready for an actual instruction. The bring-up procedure, end
to end, is the whole span from the first byte of a new session to that moment — not a
preamble that happens *before* the real work, but the first piece of real work, every time.

## Why sequential, specifically

Parallel or similarity-based loading has a specific, quiet failure mode: files can arrive out
of the order they were written to be read in, and a file that assumes context from an earlier
one gets misread without any error being raised. A sequential read makes that failure
structurally impossible — there is no "later" file being read before an "earlier" one, because
the process itself won't allow it. When something is read out of order anyway, that's a
detectable, loggable event, not a silent one.

## What's built, and what's declared but not yet built

The load sequence and its logging are running today: every file read at session start is
timestamped, tied to the session identifier, and checked against the expected order — a
mismatch produces a visible warning. What isn't automatic yet is enforcement: today, going out
of order is *flagged*, not *blocked* — the warning is visible to whoever is operating the
session, but nothing in the current design prevents the session from continuing anyway. That's
a stated design boundary, not an oversight: a hook that can only observe, not intervene, is a
different (and today, simpler) piece of machinery than one that gates execution.

## Why this matters generally

None of this depends on model capability — it's an ordering and logging discipline applied
*around* the model. The generalizable idea is narrow but useful: if a system's behavior
depends on it having absorbed a specific set of rules, don't assume that happened because the
files were technically available — sequence the read, log it, and make the order itself part
of what's verified.

---

Contact: Fernando Ruiz Casas — reach me with any question by addressing my gmail account,
`fruizcasas`.

© 2026 Fernando Ruiz Casas
