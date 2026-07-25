# Generator vs. judge: a mediocre creator is often a lethal evaluator

*Terms used below, standalone: a* **generative act** *is any step where a model produces new
content — text, code, a decision. A* **binary check** *is a narrow yes/or/no verification: does
this specific claim match this specific source, with no partial credit and no shading.*

## The problem

Asking a model to produce something and then asking that same model, in the same context,
whether its own output is good, sets up a structural conflict of interest. The same
generative habits that produced the content — filling gaps plausibly, sounding confident,
preferring an answer that reads well over one that admits uncertainty — are still active when
that content is being "checked." Self-review tends to confirm rather than catch.

## The asymmetry

The useful, somewhat counter-intuitive observation is that the same model is not equally weak
at both jobs. Open-ended generation — write something good from a blank page — is where
gap-filling and plausible invention show up most. Binary evaluation — does this specific claim
hold up against this specific source, yes or no — is a different, narrower task, and models
tend to be considerably more reliable at it. Asked to create, a model is a mediocre
craftsman. Asked "is this true or not, given what's in front of you," the same model can be a
strict, consistent, hard-to-fool checker.

## The mechanism

The design response is to never let the same context that generated something be the only
thing that certifies it, and to lean deliberately into the asymmetry above rather than fight
it:

1. **Front-load the ambiguity removal.** Before any generation happens, a separate pass works
   the raw request until as few open questions as possible remain — every "wait, what does
   this actually mean" gets asked and resolved before generation starts, not discovered
   afterward. The step from a fully-specified request to generated output is meant to be
   close to mechanical; the real work happens earlier.
2. **Separate the check from the creation.** Once something is generated, the actor that
   checks it is deliberately different from — or at minimum, isolated from the context of —
   the actor that produced it: a fresh instance, narrowly tasked with a binary question, with
   no stake in the output looking good.
3. **Keep the question binary wherever the fact allows it.** "Does this quote appear verbatim
   in that file — yes or no" is a question a model can answer reliably. "Is this a good essay"
   is not, and isn't the kind of question this mechanism is used for.

## Why this matters generally

The generalizable claim isn't "don't trust the model" — it's narrower and more useful than
that: match the task to the mode the model is actually reliable in. Generation is where
invention risk concentrates; verification, kept binary and narrowly scoped, is where the same
model becomes trustworthy. Treating both as the same kind of task — and worse, letting the
same context do both — throws away that asymmetry instead of using it.

---

Contact: Fernando Ruiz Casas — reach me with any question by addressing my gmail account,
`fruizcasas`.

© 2026 Fernando Ruiz Casas
