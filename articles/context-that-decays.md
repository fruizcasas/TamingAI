# The context they tell you to keep is the context that decays fastest

Two documents, four weeks apart, neither citing the other.

On 24 July 2026, Anthropic published *The new rules of context engineering for Claude 5 generation
models*, by Thariq Shihipar. Its advice about what belongs in a Skill is this:

> "It's best when skills encode particular opinions, knowledge, or best practices that are particular to
> you, your team, or product."

In June 2026, Shiyang Chen published *Governance Decay* (arXiv:2606.22528), which measured what happens
to in-context rules when a harness compacts the conversation. Its headline gradient is this:

> "Decay is 8.3× larger for soft organizational policies than for hard safety norms, eroding exactly the
> deployment-specific constraints that live in context."

One says the context worth keeping is the part that is particular to you. The other measures that the
part particular to you is the first thing to go. Put the sentences next to each other and a third one
appears that neither document contains, which is what this piece is about.

## Most of the advice is right, and the reason it is right is interesting

The headline finding of the Anthropic post is that they removed over 80% of Claude Code's system prompt
for the newest models "with no measurable loss on our coding evaluations", by replacing hard guidance
with judgement. The worked example is comment style:

> Then: "In code: default to writing no comments. Never write multi-paragraph docstrings or multi-line
> comment blocks — one short line max."
>
> Now: "Write code that reads like the surrounding code: match its comment density, naming, and idiom."

That is a good change, and it is worth being precise about *why* it is free. A rule like "don't write
sprawling comments" is generic craft hygiene. A well-trained model already ships with it. Deleting a rule
the model already has costs nothing by construction — you are not removing a constraint, you are removing
a redundant statement of one.

Which is the same finding as Chen's, seen from the other side. His hard safety norms barely decay (+6
points) because the model refuses those anyway; his soft organizational policies collapse (+50) because
nothing in training supplied them. Anthropic measured that generic rules are safe to delete. Chen
measured that arbitrary rules are the ones that vanish. Those are two views of one fact: **a rule's value
and its fragility have the same source — whether the model could have produced it unaided.**

The same goes for progressive disclosure, which the post recommends and we use. Loading the right context
at the right time, as a tree of files rather than one monolith, is exactly how a rule corpus should work.
No argument here at all.

## The instrument cannot see the loss it is being used to rule out

"No measurable loss on our coding evaluations" is true, honestly scoped by the people who wrote it, and
entirely compatible with governance being lost.

A coding evaluation measures whether the code works. It cannot measure whether the arbitrary convention of
one particular house was followed, because that convention appears in no test suite by definition — it is
a local decision, not a correctness property. If a model quietly stopped honouring "this decision belongs
to that person" or "this file is never edited directly", every coding eval in the world would stay green.

This is not a criticism of the measurement. It is the ordinary limit of any measurement: it sees what it
was built to see. The point is only that "no measurable loss" on that instrument cannot be evidence about
the class of rule the instrument does not measure — and that class is precisely the one Chen found decays
8.3× faster.

## The class of rule judgement cannot reach

"Let Claude use judgement" is right for decisions that are shaped like judgement. Comment density is one:
there is a better and a worse answer, discoverable from the surrounding code.

But the rules that do the most work in a real workshop are not shaped like that. *This decision belongs to
that person.* *Never touch this file directly.* *Always this format, even though another would be fine.*
There is nothing to reason toward — they are arbitrary by nature, because they are local decisions rather
than general knowledge.

And here is the uncomfortable part: **a better model has better judgement and exactly the same zero
information about your house.** Model capability does nothing for this class, because the gap was never
capability. It was that the fact is not in the world, only in your workshop.

Which brings the two opening quotes together. Anthropic is right that what belongs in your own context is
what is particular to you. Chen has measured that what is particular to you is what compaction discards
first. The missing sentence is the conjunction of the two: *the only context that was doing any work is
also the least likely to survive the window.*

## Compaction, in the vendor's own words

None of this is hidden. Anthropic's own engineering guidance on context engineering describes compaction
plainly, and names the risk before anyone measured it:

> "Compaction is the practice of taking a conversation nearing the context window limit, summarizing its
> contents, and reinitiating a new context window with the summary."

> "overly aggressive compaction can result in the loss of subtle but critical context whose importance
> only becomes apparent later."

And it describes what survives:

> "The model preserves architectural decisions, unresolved bugs, and implementation details while
> discarding redundant tool outputs or messages."

Read that list again. Architectural decisions, unresolved bugs, implementation details. It is a list about
the **task**. Policies, standing rules and constraints are not in it — and that is not an oversight, it is
the stated objective. Compaction is engineered for task continuity, so a summarizer optimizing for task
continuity has no particular reason to carry a rule that is not part of the current sub-goal. Which is,
almost word for word, the mechanism Chen proposes: compaction "treats standing policies as low-salience
content".

The vendor's description of its own compactor corroborates the paper's account of why the failure happens.
Nobody is wrong here. The two documents simply have not been read against each other.

## What is genuinely missing across all of it

We went looking for the gap in the vendor's published guidance, and it is narrower than "governance should
live on disk" — because several of these documents already half-say that. Skills on disk. A tree of files.
References in code. Rich artifacts. The direction of travel is already outward from the context window.

The gap is this: **nothing in that guidance offers a way to check whether the context you carefully
engineered is still there.** Not in the context-engineering post, not in the engineering guide, not in the
field guide. The advice covers what to load and when to load it. It does not cover how to find out that
something you loaded is no longer present — which, given that the same guide warns about losing "subtle but
critical context whose importance only becomes apparent later", is the natural next question.

And progressive disclosure sharpens it rather than softening it. If you deliberately load less, later, and
selectively, then at any given moment **more of your governance is outside the window than in it**. "Is
this rule present right now?" stops being an idle question and becomes an operational one. The better the
loading discipline, the more valuable a presence check becomes.

That check can be very cheap and completely deterministic. We describe the mechanism, and report a field
measurement of a corpus surviving a total context death, in a companion piece:
[Governance on disk](governance.md). The short version: put an arbitrary question-and-answer pair among the
rules, in the same format as a rule, and ask for it back. The answer either matches byte for byte or it
does not. No model judges it.

One thing we should be clear about, since it would be easy to imply otherwise: the *machinery* for a richer
diagnosis on top of that check is already Anthropic's. Their guidance on dynamic workflows recommends
"orchestrating separate Claude subagents with their own context windows and focused, isolated goals" and,
specifically, "a list of rules that must be checked by verifier agents — one verifier per rule". That is
the right pattern and it ships today. What we are adding is not the mechanism but its subject: pointing
verification at the presence of the governance itself.

## The one recommendation we would qualify

The post includes this shift:

> Then: Memory in CLAUDE.md files
>
> Now: Auto-memory. "Claude now automatically saves memories that are relevant to the work and to you."

For **preferences**, this is a straight improvement, and we use it. Remembering that someone prefers terse
summaries or a particular library is exactly the sort of thing nobody should be hand-editing a file for.

For **governance**, it inverts something worth keeping right side up. A rule in `CLAUDE.md` is written by
the owner, lives in the repository, is versioned, shows up in a diff, and can be reviewed before it takes
effect. A rule in auto-memory is written by the model, about its own work, on its own initiative.

We can be concrete rather than theoretical about what that store is, because we went and looked at it
while measuring something else. In our workshop, on 25 July 2026: the auto-memory is keyed to the **project
directory**, not to the account — it crosses a vendor account change untouched, it is injected at the start
of every session, it is not in the repository, and it is not in git. We checked it for other reasons and
found it contained nothing it should not. But its contents had never been reviewed by anyone, because
nothing in the workflow asks for that.

The distinction we would draw is not about the feature, which is good. It is about what should be migrated
into it. A machine may safely **repair** governance — restoring a rule that was already sanctioned creates
no new authority and requires no judgement. A machine writing new standing rules about its own behaviour,
unreviewed, is a different act: it is the system grading its own homework, and the account it is least able
to give honestly is the account of its own weaknesses.

None of which conflicts with keeping `CLAUDE.md` light, which is also good advice:

> "Keep your CLAUDE.md lightweight… Avoid stating 'the obvious' things Claude should know by looking at
> your file system or your repo."

Agreed — with the corollary that follows from everything above. **The arbitrary local decision is never
"the obvious thing Claude should know by looking at your repo."** It is precisely the thing that cannot be
inferred from the repo, by any model, at any capability level. So "light" and "keeps its governance" are
not in tension: light means nothing the model already has and nothing derivable from the filesystem.
Whatever is left after that subtraction is small, and it is the part that matters.

## What we take from this

Nothing here is a disagreement. The context-engineering advice is good and we follow most of it. The
compaction guidance is honest about its own risk. The paper is careful about its scope.

What is missing is one sentence sitting between them, and it is not comfortable for anyone:

**The context worth keeping is the context that decays fastest — so it is the context that needs an
authority outside the window, and a way to notice when it has quietly stopped being there.**

---

Contact: Fernando Ruiz Casas — reach me with any question by addressing my gmail account, `fruizcasas`.

© 2026 Fernando Ruiz Casas
