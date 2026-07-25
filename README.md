# TamingAI / TAIAT

A production-tested **Bring-Up Procedure (BUP)** for Claude Code sessions: plain markdown
files, loaded through Claude Code's standard `SessionStart` hook, plus a small deterministic
verification layer written in stdlib Python. No fine-tune, no custom API plumbing, no
framework — a workshop, not a product.

This repository documents the *method*. It intentionally does not include the operative
governance files themselves (see [Why no source files](#why-no-source-files) below).

## What it is

TAIAT (**TamingAI-ATelier**) is the instance of the TamingAI method running in a given
workshop. TamingAI is the method itself: a discipline for keeping a long-lived AI session
governed, auditable, and honest about what it does and doesn't know — without relying on the
model to grade itself.

## Guest, not intruder

We are guests on someone else's machine, never a parasite. TAIAT respects the vendor's own
rules as strictly as it asks the vendor to respect its own — no jailbreaking, no hidden
instructions, no adversarial prompting. Everything is built entirely on top of documented,
supported Claude Code features (hooks, markdown context files). If the assistant vanished
tomorrow, the method and its records would still stand on their own.

## Sequential, provable load

At the start of every session, a fixed ordered list of files (the root instruction file first,
then a glossary, a constitution, a rulebook, a team charter, an operational runbook, best
practices, lessons learnt, a capability index) is read one file at a time, each read completed
before the next begins — never in parallel, never chunked, never similarity-retrieved. By the
time the assistant returns its first reply, it already carries a solid normative context, plus
two verification layers standing by:

- a **conscience** (a deterministic hook) that watches every tool call and warns when something
  drifts from the loaded rules — it warns, it never blocks;
- an **examiner** that can pose recall checks against the rules that were actually loaded,
  scored by exact string match — see the worked example below.

## The heading/numbering design — turning a known LLM weakness into a checkpoint

Every governing file is written with deliberate care: nested heading levels and strict
sequential numbering within each level. This is a direct, practical response to the failure
mode we most distrust in these systems — the model's own instinct to quietly fill an empty
slot rather than flag it. A numbered sequence under maximum structural tension makes a skipped
or invented item conspicuous instead of invisible: the model's own gap-filling reflex, which
normally works against reliability, is redirected to work for it.

## The examiner in action — a worked example

Every governing rule carries a small trap, embedded directly in the rule's own text — not in a
separate test suite that could drift out of sync with what was actually loaded. A real one,
verbatim:

```
## LL-03 · ¿Qué lagarto puede cambiar de color? — Δίας
```

A grade-school question in Spanish ("which lizard can change color?") paired with an answer
that has nothing to do with it — a Greek word, deliberately absurd on both sides. This is not a
secret to protect: it's a **barcode**, not a password. The point isn't "can you guess an answer
nobody knows" — it's "is this exact text, right now, actually loaded in your live context."
Publishing it changes nothing: a fresh session that never loaded this file would still fail the
check, published or not, because the check is about *this session's* context, not general
knowledge.

Addressed precisely (source file + rule, never a loose ordinal — two rules never collide), the
examiner's grade command and its real output:

```
$ python eladio.py grade --source "_AI/Skills/GIT/AntiPattern/LessonsLearnt.run.md" \
                          --rule "LessonsLearnt.1" "Δίας"

PASS  [_AI/Skills/GIT/AntiPattern/LessonsLearnt.run.md . LessonsLearnt.1]  expected='Δίας'  got='Δίας'
     recency (last load of ...LessonsLearnt.run.md by a8521ecf2c2fc62ad): 2026-07-25 13:19:18
     behaviours=['recency-loss', 'plausibilization']  areas=['git-scars']
(trace-only: logged to eladio-grades.jsonl; NO action taken)
```

That recency stamp is a real trace: `a8521ecf2c2fc62ad` is not the main session — it's a
short-lived sub-agent that was sent out on one errand and picked up this file on its own. The
verdict is exact-string, binary — no LLM grading another LLM's answer, no judge that is also a
party. And it doesn't just say PASS/FAIL: it stamps *when that specific file was last actually
loaded* by *that specific actor* — main session or a named sub-agent, never blurred together —
so a miss can be read against how long the rule has been sitting untouched in context, not just
scored blind.

That's also the escalation trigger: a light ~15% sample of the canaries from whatever was
actually loaded this conversation; one single miss is enough to widen the next pass to ~50%,
and real trouble escalates it to a full 100% sweep. Cheap when healthy, expensive exactly when
something is wrong.

## Public by default, not self-graded

The conscience does not whisper only to the operator: any drift it catches from *any* agent —
the main session or a short-lived sub-agent sent out on a single errand — is surfaced openly,
not filtered by who caused it. A sub-agent's own identifier travels with the record, so a slip
is always attributable, never laundered into an anonymous average.

## English for the machine, local language for the team

Every governing file is written in English, deliberately — a token economiser (roughly
25–30% fewer tokens than the equivalent local-language text, by our own measurement) and,
just as important, the language every recovery later *searches in* (grep, git log, git diff by
meaning). Sub-agents are briefed and report back in English for the same reason. Local language
is reserved for the live conversation with the human team.

## A Clerk obligated to leave a trail

Closing out a unit of work is not "trust me": there is a standing obligation to leave everything
discussed and agreed recoverable at once — a session log, a plain-text commit summary drawn
fresh from the real `git log`/`git status` (never from memory), and an index. If it wasn't
written down where it can be found again, it wasn't agreed.

## Honest state — what is built vs. what is roadmap

The examiner today is **trace-only**, by explicit, dated design decision. It diagnoses with
real precision — addressing failures down to the exact source file and rule, never the whole
file — but it does not yet act on what it finds. The reload/repair step, when a check fails, is
still a human decision, not an automated one. Closing that loop is the next step on the roadmap,
not a claim being made today.

## Why no source files

This repository documents the method, not the operative corpus. Two reasons, both practical
rather than defensive:

1. The actual governing files are written in the workshop's own internal register (role names,
   ritual vocabulary specific to this project) that would read as noise to an outside reader
   without the surrounding context this README already provides in plain terms.
2. It's simply not needed to demonstrate the method — the worked example above is real, taken
   directly from a live run, and stands on its own.

## Contact

Fernando Ruiz Casas — you can reach me with any question by addressing my gmail account,
`fruizcasas`.

© 2026 Fernando Ruiz Casas
