# Bootstrapping a governance layer onto a pre-existing system

*Terms used below, standalone: the* **shell** *is the governance machinery itself — the
rulebook, the folder structure, the verification layer — as distinct from the* **content**
*that already exists in a system before governance is added to it: the actual code, data, or
documents nobody wants disturbed just to add oversight.*

## The problem

Adding a governance layer to something that already exists and already works is a different,
harder problem than starting fresh. Two failure modes show up in practice: either the adoption
disturbs or overwrites content that was already fine, or it blindly re-imports solutions to
problems the existing system never actually had — mistaking someone else's fix for a need of
your own.

## The mechanism

**Audit before touching anything.** Before any adoption step, three questions get asked and
actually answered: what does this system already do well, so it isn't needlessly re-solved;
what is genuinely missing; and what is actually broken, as opposed to simply unfamiliar. The
purpose is specific — importing a governance pattern without first knowing what's already
present is how a team ends up carrying home a solution to a problem it doesn't have.

**The shell travels whole; the content stays put.** Adoption means deploying the governance
machinery — the rulebook, structure, and verification layer — onto the target as-is, complete
and ready to operate, without migrating or rewriting the pre-existing content in the process.
The content isn't ignored — it grows into the new structure from that point forward — but it
isn't force-migrated on day one either. Machinery first, content second, kept as two separate
concerns rather than one disruptive rewrite.

**A harder, related case: replacing the foundation under existing governance.** Sometimes the
governance layer is already in place and it's the underlying technology stack that needs to
change completely. That case needs an extra, explicit first step the plain bootstrap doesn't:
formally unhooking the existing content from its old foundation before the new one is grafted
in, followed by separating what depends on the old technology (discarded) from what doesn't
(kept intact) — otherwise the new foundation collides with content that still assumes the old
one owns it.

## Why this matters generally

The generalizable lesson isn't specific to any one governance system: adopting oversight onto
something pre-existing should be treated as an explicit, incremental operation with its own
discipline — audit first, add the machinery without disturbing working content, and treat a
full foundation swap as a distinct, harder case rather than assuming the same steps apply.
Skipping the audit step is the most common way this goes wrong in practice: it's what turns an
adoption into an unwanted rewrite.

---

Contact: Fernando Ruiz Casas — reach me with any question by addressing my gmail account,
`fruizcasas`.

© 2026 Fernando Ruiz Casas
