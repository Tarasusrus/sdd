Role

You are archiving a completed feature spec.

Input

- spec/ folder with proposal.md, requirements.md, acceptance_criteria.md, constraints.md, verification.md, plan.yaml
- Confirmation that the feature is shipped and verification.md reports PASS

Task

Move the active spec into the archive and produce a retro note.

Steps

1. Determine archive path: archive/<project>/<YYYY-MM-DD>-<slug>/ where slug is the kebab-case feature name from proposal.md.
2. Move all six pipeline artifacts into that folder. Keep filenames unchanged.
3. Generate retro.md inside the archive folder using the template below.
4. Update INDEX.md at repo root: append one row with project, date, slug, one-line outcome, tags.
5. If the project has a master spec for this capability, merge any decisions from constraints.md into it. Otherwise skip.

retro.md template

---
project: {project}
feature: {slug}
shipped: {YYYY-MM-DD}
status: archived
tags: [{tag1}, {tag2}]
outcome: {one line — what was delivered}
---

What worked
- {bullet}

What did not
- {bullet — where the spec was wrong, where the agent diverged, where reality contradicted assumptions}

What to add to the templates
- {bullet — concrete change to feature-spec.md / change-delta.md / one of the prompt templates}

Links
- PR: {url}
- Code: {path or commit range}

Definition of Done for archival

- [ ] All six artifacts present in archive folder
- [ ] retro.md filled (not template stubs)
- [ ] INDEX.md updated
- [ ] Active spec/ folder cleared of this feature
- [ ] If templates need updating per retro — corresponding PR opened against this repo

Output

Confirm the archive path and the INDEX.md row added.
