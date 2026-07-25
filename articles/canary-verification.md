# Canary verification: proving a rule is actually loaded, not just plausible

*Terms used below, standalone: a* **canary** *is a trap question-and-answer pair placed in
the rules exactly like any other rule — not hidden, not disguised. The question can be posed
in any language; the answer is a deliberately ridiculous, unrelated response in a different
language. A rule is* **sealed** *when its canary is recorded with its exact location in the
source file. The* **examiner** *is a small deterministic script (no model involved) that
poses canaries and grades exact-match answers. An* **isolated agent** *is a freshly-started
model instance with no memory of the conversation being checked, used to verify written
claims against source files.*

## The problem

Ask a long-running AI session whether it has correctly loaded its governing rules, and it
will say yes — confidently, fluently, and unfalsifiably. An AI reporting on its own state is
not a check: it says so just as convincingly when it's true as when it isn't. Self-assessment
of this kind is not evidence.

A published "needle in a haystack" benchmark doesn't solve this either — it proves something
once, for a chart, under conditions the model may have been tuned against. What's needed is
a check that runs in production, every time, on the live session, and that the model itself
cannot pass by being merely fluent.

## The mechanism

The fix is a set of **canaries**: question-and-answer pairs placed among the rules in plain
sight, formatted exactly like any other rule — no disguise, nothing to find. The question can
be asked in any language; the answer is a deliberately ridiculous, unrelated response in a
different language. Not a sensible question, on purpose: a sensible question can be
plausibly reconstructed from general knowledge even without the source text in hand, which
would make the check worthless. An absurd, arbitrary pairing cannot be guessed or inferred —
either the exact text is loaded right now, verbatim, or it is not. It doesn't measure
understanding (that can be faked); it measures whether the source is loaded at this instant.
Binary, and effectively unforgeable.

Mechanically, this is unglamorous by design: each canary is addressed by an exact byte
offset and length into its source file, tagged with a short question, an exact expected
answer string, and a couple of category labels. No cleverness, no inference — a slice of
text, and a string comparison.

## The examiner

A small, deterministic script (no model involved — plain, dependency-free code) reads the
sealed canaries, poses them, and grades answers by exact string match. Today it does one
concrete job: it is the quality control of the rule-sealing process itself — verifying that
each canary's recorded location still matches its source file, and logging exactly when each
rule was last loaded, by which actor. It runs in a fraction of a second, costs no model
tokens, and — notably — every run prints exactly what it did and nothing more; it makes no
claims beyond what it can prove. When it has been wrong, that failure stays visible in the
log rather than being cleaned up after the fact.

This same addressing (which file, which rule, what category, what behavior it guards
against) also lets a session be briefed narrowly before a task: only the rules, precedents,
and known failure patterns relevant to that specific piece of work — not the whole
rulebook, cold.

## What's built, and what's declared but not yet built

The honest state matters here as much as the mechanism. Today, the examiner is diagnostic
only: it logs verdicts and recency, but it does not gate, escalate, or trigger any repair
action on its own. Closing that loop — an automatic reload or hand-off the moment a check
fails — is a stated design goal, not a claim about what exists today. Saying so plainly is
the same discipline the mechanism itself enforces: don't present a plausible state as an
achieved one.

## The other half: checking prose claims, not just rule-loading

Verifying that a rule loaded correctly is only half of the control problem. The other half
is verifying that a *written claim* — a quote, a date, an assertion about what a file says —
actually holds up against the source it claims to summarize.

For this, the same principle is applied through a different mechanism: an isolated,
freshly-instantiated agent, with no visibility into the conversation that produced the text
and no prior context, is given only two things — the text to verify, and the actual source
files. It checks every factual claim against its supposed source and classifies each one:
confirmed verbatim, confirmed but paraphrased, or unconfirmed — no partial credit, no
courtesy toward the author. In practice, this kind of blind audit has caught real errors:
wrong dates, misattributed quotes, and claims about events that had no file backing them up
at all — each one corrected on the record rather than smoothed over.

## Why this matters generally

Neither piece is exotic. A canary is a string comparison. A blind auditor is a
freshly-instantiated model given restricted context. What makes the combination useful is
the discipline behind it: don't trust a system's account of its own state; verify against a
recorded source; keep the verdict binary where the fact allows it; and say plainly what is
built versus what is still a stated intention. None of that requires new model capability —
it requires treating verification as a first-class part of the system, not an afterthought
bolted on when something goes wrong.

---

Contact: Fernando Ruiz Casas — you can reach me with any question by addressing my gmail
account, `fruizcasas`.

© 2026 Fernando Ruiz Casas
