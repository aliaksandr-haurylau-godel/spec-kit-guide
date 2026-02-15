---
version: 1
created_date: 2026-02-15
derived_from: raw_data/2026-02-15-real-world-spec-kit-artifacts/ (constitution examples from IdeaVim, Backpack, Manifest)
---

# Constitution Evolution FAQ

Questions about how constitutions change over time, how to version them, and how to decide whether existing specs/plans/tasks need to be regenerated.

## Versioning Basics

### Q: What is constitution versioning?

**Answer:** Constitution versioning is the practice of assigning a version number to your constitution document (usually `.specify/memory/constitution.md`) so the team can track which rules and expectations were in force when a spec, plan, or tasks list was generated.

It matters because the constitution is the "governing contract" for how the agent plans and implements work. If the constitution changes, some artifacts may become:

- Out of date (missing new rules)
- Non-compliant (contradicting updated rules)
- Ambiguous (using wording that now means something else)

Versioning is how you make those shifts explicit and auditable.

### Q: Why does constitution versioning matter even if we only "edit wording"?

**Answer:** In real projects, "wording" changes often change behavior.

For example:

- Tightening a MUST/SHOULD statement changes what plans must include.
- Clarifying an example can change the default pattern an agent uses.
- Changing a template placeholder can change the structure of generated artifacts.

When you version changes, you can:

- Decide which in-flight branches need regeneration
- Keep old branches stable (when safe)
- Communicate expectations clearly during reviews

### Q: What does semantic versioning (MAJOR.MINOR.PATCH) mean for constitutions?

**Answer:** In this guide, constitution versions use SemVer-style meaning, but the "compatibility" you care about is compliance and workflow compatibility, not runtime API compatibility.

- PATCH (x.y.Z): Editorial improvements and example/template corrections that do not introduce new requirements.
- MINOR (x.Y.z): Backward-compatible additions or expansions that new work should follow, but existing approved artifacts can usually stand.
- MAJOR (X.y.z): Breaking governance changes that can make existing plans/specs non-compliant or incomplete.

This mirrors the patterns in the real-world examples:

- PATCH style: `artifacts/constitution-example-backpack.md` (v1.0.2)
- MINOR style: `artifacts/constitution-example-ideavim.md` (v1.2.2)
- MAJOR style: `artifacts/constitution-example-manifest.md` (v2.0.0)

### Q: Where is the constitution version stored?

**Answer:** In the constitution frontmatter.

Most projects store the constitution version in at least one of these keys:

```yaml
---
version: 1.2.2
constitution_version: 1.2.2
previous_version: 1.2.1
---
```

See the examples in:

- `../artifacts/constitution-example-ideavim.md`
- `../artifacts/constitution-example-backpack.md`
- `../artifacts/constitution-example-manifest.md`

### Q: How do specs/plans/tasks track the constitution version (SYNC IMPACT)?

**Answer:** Artifacts typically record the constitution version they were generated against in their own frontmatter, usually as `sync_impact`.

Example pattern (from the artifact-structure documentation):

```yaml
---
version: 1
sync_impact: 1.2.2
---
```

That number is not a "severity" score. It is a pointer to the constitution version.

Practical use:

- If your current constitution is 1.2.3 and the plan says `sync_impact: 1.2.2`, you can decide whether the delta requires regeneration.
- If the current constitution is 2.0.0 and the plan says `sync_impact: 1.2.2`, you should assume regeneration is likely (MAJOR jump).

Reference: `../docs/artifact-structure.md`

### Q: What does "SYNC IMPACT" mean in real-world constitutions?

**Answer:** In real-world constitutions, teams often include a "SYNC IMPACT REPORT" comment block that summarizes what changed between versions and what must be updated as a result.

This is distinct from the `sync_impact` field in other artifacts:

- In constitution comments: "SYNC IMPACT" is a human-readable compatibility report.
- In spec/plan/tasks frontmatter: `sync_impact` is a machine-searchable pointer to the constitution version.

You can see this comment-block pattern in:

- `../artifacts/constitution-example-ideavim.md` (explicit templates reviewed)
- `../artifacts/constitution-example-backpack.md` (template updates and status)
- `../artifacts/constitution-example-manifest.md` (sections removed/added; MVP transition)

### Q: How should we use SYNC IMPACT to decide whether to regenerate artifacts?

**Answer:** Treat SYNC IMPACT as a decision aid, not a mandate.

Suggested approach:

1. Compare the artifact's recorded constitution version (its `sync_impact`) to the current constitution version.
2. Read the constitution's SYNC IMPACT report for the versions in between (or a changelog entry).
3. Decide what to regenerate based on the kind of change:

- PATCH delta: usually no regeneration; optionally regenerate to pick up nicer templates.
- MINOR delta: regenerate only if the feature touches the new or expanded rule.
- MAJOR delta: regenerate plan/tasks at minimum; often re-check the spec.

If you do nothing, reviewers should expect you to justify why the old artifact is still compliant.

### Q: What is the difference between "artifact version" and "constitution version"?

**Answer:** "Artifact version" (often `version: 1`) is the schema/version of the document format used in this repo.

"Constitution version" (SemVer-like) is the governance version of your project rules.

In other words:

- `version: 1` answers: "Which artifact template format is this?"
- `constitution_version: 1.2.2` answers: "Which constitution ruleset is this constitution?"
- `sync_impact: 1.2.2` answers: "Which constitution ruleset was this artifact produced under?"

Reference: `../docs/artifact-structure.md`

### Q: How do I quickly find stale branches after a constitution update?

**Answer:** Search for `sync_impact:` across specs/plans/tasks and compare against your current constitution version.

Example commands (adjust paths to your repo layout):

```bash
rg "^sync_impact:" -n
rg "constitution_version:" -n
```

Then focus review effort on branches with:

- A MAJOR gap (e.g., 1.x.y artifacts while constitution is 2.0.0)
- A MINOR gap where new rules apply to the feature area

## When to Bump Versions

### Q: When should I bump PATCH?

**Answer:** Bump PATCH when you improve clarity, examples, or templates without changing obligations.

Good PATCH changes:

- Fixing typos, formatting, and ambiguous wording
- Updating placeholders in examples (without changing requirements)
- Correcting stale references (paths, filenames, commands)
- Reordering sections for readability
- Adjusting template task numbering after a non-semantic edit

Real example: Backpack 1.0.1 to 1.0.2 is framed as "template improvements and example corrections" with "Modified principles: None".

Reference: `../artifacts/constitution-example-backpack.md`

### Q: When should I bump MINOR?

**Answer:** Bump MINOR when you add a new article, a new enforcement mechanism, or materially expand guidance in a way that is intended for new work but does not invalidate already-approved work.

Good MINOR changes:

- Adding a new custom article (e.g., Article X for a domain)
- Adding a new checklist that applies going forward
- Expanding a principle with more concrete requirements that do not contradict existing specs/plans
- Updating a development workflow rule where existing in-flight work can still comply without rewriting the spec

Real example: IdeaVim increments from 1.2.1 to 1.2.2 while changing workflow guidance for branches (still compatible with templates).

Reference: `../artifacts/constitution-example-ideavim.md`

### Q: When should I bump MAJOR?

**Answer:** Bump MAJOR when the change can make previously "good" artifacts non-compliant.

Common MAJOR triggers:

- Removing deferrals and making previously optional requirements mandatory
- Rewriting or removing core principles
- Introducing a new NON-NEGOTIABLE that applies to existing work
- Changing the definition of "done" (e.g., tests required before merge, new security gates)
- Shifting project maturity expectations (POC to MVP/production)

Real example: Manifest 1.1.0 to 2.0.0 removes "DEFERRED FOR POC" markers and adds new security and community standards.

Reference: `../artifacts/constitution-example-manifest.md`

### Q: What counts as a "breaking change" for constitutions?

**Answer:** A constitution change is breaking if it causes existing, approved, or in-flight work to fail a reasonable compliance review without additional work.

In practice, breaking means at least one of these is true:

- Existing plans omit newly-required gates, tests, or deliverables.
- Existing specs promise behavior that the updated constitution forbids.
- Existing tasks list is missing newly-mandated steps (security review, license headers, ADRs, etc.).
- The workflow rules change such that the branch process in the plan is no longer acceptable.

Breaking does not require code to "stop compiling". It is about governance compliance.

### Q: Is adding a new article always a MINOR bump?

**Answer:** Usually, but not always.

Add a new article as MINOR if:

- It applies primarily to new work.
- Existing in-flight work can comply without rewriting the plan/spec.

Add a new article as MAJOR if:

- It introduces mandatory work for all features, including those already planned.
- It is marked NON-NEGOTIABLE and would cause existing branches to fail CI or policy gates.

### Q: Is changing a MUST to a SHOULD a breaking change?

**Answer:** It depends on direction:

- MUST to SHOULD: usually non-breaking (relaxation), often MINOR or PATCH.
- SHOULD to MUST: often breaking, usually MAJOR if it applies broadly.

If the change affects a single niche area and you can safely scope it to future work only, MINOR can be reasonable, but document the scope.

### Q: What is a conservative rule when unsure about PATCH vs MINOR vs MAJOR?

**Answer:** If you are unsure, choose the higher bump.

Conservative guidance:

- If anyone could plausibly interpret the change as adding obligations, do not call it PATCH.
- If the change could plausibly fail an existing branch's compliance review, do not call it MINOR.
- If you cannot clearly define who is exempt (old branches vs new branches), call it MAJOR.

This is the governance equivalent of "avoid silent breaking changes".

### Q: Can you provide a simple decision tree for version bumps?

**Answer:** Yes. Use this as a starting point.

```text
Start
  |
  |-- Does the change introduce new mandatory work for existing in-flight branches?
  |       |-- Yes --> MAJOR
  |       |
  |       |-- No --> continue
  |
  |-- Does the change add a new principle/article, new gate, or materially expand guidance?
  |       |-- Yes --> MINOR
  |       |
  |       |-- No --> continue
  |
  |-- Is it purely clarifications, example fixes, or template correctness improvements?
          |-- Yes --> PATCH
          |
          |-- No --> MINOR (default)
```

Notes:

- "Materially expand" includes adding concrete thresholds, adding required checklists, or adding new enforcement steps.
- If you add a NON-NEGOTIABLE marker that changes enforcement, treat it as MAJOR unless it is clearly only labeling an already-enforced rule.

### Q: Should constitutions ever use pre-1.0.0 versions (0.y.z)?

**Answer:** Some teams do for POC phases, but the examples in this repo use 1.x and 2.x.

If you use 0.y.z, define what "breaking" means for 0.y.z in the constitution itself (because SemVer treats 0.y as unstable by default).

## Real-World Examples (Detailed)

This section uses and cites the artifacts in `../artifacts/`.

### Q: What does a PATCH bump look like in practice (Backpack v1.0.2)?

**Answer:** Backpack's constitution describes a 1.0.1 to 1.0.2 PATCH bump as "template improvements and example corrections".

Key characteristics of this PATCH bump (as described in the artifact):

- No new principles were introduced.
- Existing requirements did not change.
- Templates and examples were corrected so new work would be more consistent.

Narrative (what changed and why):

- Component placeholder format was standardized (more consistent naming in generated docs and tasks).
- Token examples were adjusted to be clearer (better contrast for illustrative examples).
- References to files that do not exist in the monorepo were removed (avoids misleading work).
- Storybook example structure was corrected (prevents "copy the example" mistakes).

Typical team communication:

```text
Heads up: Constitution 1.0.2 is a PATCH.
No new rules. We fixed template placeholders and corrected examples.
No need to regen existing branches; new branches will pick up nicer defaults.
```

Example frontmatter snippet (pattern):

```yaml
---
version: 1.0.2
constitution_version: 1.0.2
previous_version: 1.0.1
note: "Patch-level evolution (template improvements)."
---
```

Before/after style excerpt (illustrative):

```text
Before (1.0.1 example placeholder):
  BpkComponentName

After (1.0.2 example placeholder):
  Bpk[ComponentName]
```

What SYNC IMPACT should say for a PATCH bump:

- "Modified principles: None"
- "Templates updated" (if templates were changed)
- "Existing branches do not need regeneration"

Reference: `../artifacts/constitution-example-backpack.md`

### Q: What does a MINOR bump look like in practice (IdeaVim v1.2.2)?

**Answer:** IdeaVim's constitution shows a 1.2.1 to 1.2.2 version change with a SYNC IMPACT report that lists:

- The modified section (development workflow guidance)
- A template compatibility review
- No required template changes

Narrative (what changed and why):

- The development workflow guidance was refined: trunk-based development remained the philosophy, but the guidance shifted to using feature branches more often, with frequent rebasing.
- The stated rationale is practical: merge conflicts and pain in production.
- The change is framed as compatible with existing templates.

Why this is MINOR-like (even though the bump is 1.2.1 to 1.2.2):

- It changes how teams work (branching and integration expectations).
- It does not require rewriting every existing plan template.
- Existing work can comply by adjusting process, not reauthoring the feature specification.

Typical team communication:

```text
Constitution 1.2.2: workflow guidance updated.
Use feature branches by default; rebase frequently.
Templates remain compatible, no regen required.
For in-flight branches: follow the new branch guidance going forward.
```

Example frontmatter snippet (pattern):

```yaml
---
version: 1.2.2
constitution_version: 1.2.2
previous_version: 1.2.1
---
```

Before/after style excerpt (illustrative):

```text
Before (older guidance):
  "Trunk-based development" implies frequent direct-to-trunk changes.

After (1.2.2 guidance):
  Feature branches SHOULD be used.
  Feature branches MUST be rebased frequently.
```

How to use this as a model:

- Make the delta explicit: name the article/section.
- Document whether templates need changes.
- State whether existing branches must regenerate.

Reference: `../artifacts/constitution-example-ideavim.md`

### Q: What does a MAJOR bump look like in practice (Manifest v2.0.0)?

**Answer:** Manifest's constitution shows a 1.1.0 to 2.0.0 MAJOR bump during a project maturity transition (POC to production-grade MVP).

Narrative (what changed and why):

- "DEFERRED FOR POC" markers were removed.
- Testing and performance requirements became mandatory.
- New articles were added (security, community/contribution standards).
- CI/CD and quality gates were activated.

Why this is MAJOR:

- It turns previously-skippable work into required work.
- It adds new compliance areas (security, community standards).
- It changes the meaning of "ready to merge".

Typical team communication:

```text
MAJOR constitution bump to 2.0.0: POC is over.
Testing/perf are now mandatory and security standards are active.
All in-flight branches must re-check plan/tasks against 2.0.0.
If your plan assumes deferred requirements, regen and update before merging.
```

Example frontmatter snippet (pattern):

```yaml
---
version: 2.0.0
constitution_version: 2.0.0
previous_version: 1.1.0
note: "MVP/Production transition with added standards."
---
```

Before/after style excerpt (illustrative):

```text
Before (POC):
  [DEFERRED FOR POC] Integration tests
  [DEFERRED FOR POC] Coverage thresholds

After (2.0.0):
  Integration tests are required when external dependencies exist.
  Coverage thresholds are enforced.
```

How to use this as a model:

- List removed sections explicitly.
- List modified principles explicitly.
- List added sections explicitly.
- Explicitly state whether templates are compatible but now mandatory.

Reference: `../artifacts/constitution-example-manifest.md`

### Q: Can you show a side-by-side comparison of PATCH vs MINOR vs MAJOR examples?

**Answer:** Yes.

```text
Example:    Backpack
Bump Type:  PATCH
Change:     1.0.1 -> 1.0.2
What:       Template/example correctness
Risk:       Low
Action:     Usually no regen; optionally regen to pick up corrected templates

Example:    IdeaVim
Bump Type:  MINOR-style
Change:     1.2.1 -> 1.2.2
What:       Workflow guidance refined
Risk:       Medium
Action:     Usually no regen; follow updated workflow; regen if plan embeds now-invalid workflow constraints

Example:    Manifest
Bump Type:  MAJOR
Change:     1.1.0 -> 2.0.0
What:       Deferred rules removed; new mandatory standards added
Risk:       High
Action:     Regen plan/tasks at minimum; re-check spec assumptions; update CI gates
```

References:

- `../artifacts/constitution-example-backpack.md`
- `../artifacts/constitution-example-ideavim.md`
- `../artifacts/constitution-example-manifest.md`

### Q: How should we write a SYNC IMPACT report in our own constitution?

**Answer:** Use a short, consistent structure that answers:

- What changed (version delta)
- What sections/principles changed
- What artifacts/templates must be updated
- Whether in-flight work requires regeneration
- Any follow-up TODOs

Template (comment block, so it does not pollute the rendered document):

```markdown
<!--
SYNC IMPACT REPORT
==================
Version change: 1.4.0 -> 1.5.0

Modified sections:
- X. Security Standards: added secrets scanning requirement

Added sections:
- XI. Data retention policy

Removed sections:
- None

Artifacts/templates requiring updates:
- .specify/templates/plan-template.md: update Phase -1 gates
- .specify/templates/tasks-template.md: add security verification task

Regeneration guidance:
- Existing branches: regen plan/tasks if feature touches auth/secrets
- New branches: must use 1.5.0

Follow-up TODOs:
- Add CI job for secret scanning
-->
```

If you do not track template compatibility, you lose the main benefit of SYNC IMPACT: fast regeneration decisions.

## Project-Specific Articles (X+)

### Q: When should we add custom articles instead of keeping rules in specs/plans?

**Answer:** Add a custom article when the rule is:

- Cross-cutting (applies to many features)
- Repeated in multiple specs/plans
- Expensive to forget (causes incidents, regressions, compliance failures)
- Best enforced early (Phase -1 gates, templates, or CI)

Keep it in specs/plans when the rule is:

- Feature-specific (only applies to this one feature)
- Experimental or time-bounded
- Too detailed for a constitution (implementation details)

Rule of thumb:

- Constitution: stable principles and enforcement expectations.
- Specs: product/feature requirements.
- Plans: implementation choices and gates.
- Tasks: executable steps.

Reference: `../docs/artifact-structure.md`

### Q: What are examples of good project-specific articles?

**Answer:** The real-world constitutions show domain-specific articles that encode constraints unique to their context.

Examples:

- Backpack adds design system governance and strict naming, licensing, and accessibility standards.
- IdeaVim encodes plugin-specific constraints like IDE integration and shared engine separation.
- Manifest adds security and community standards as the project matures.

References:

- `../artifacts/constitution-example-backpack.md`
- `../artifacts/constitution-example-ideavim.md`
- `../artifacts/constitution-example-manifest.md`

### Q: How should we scope custom articles in a monorepo?

**Answer:** Scope is often the difference between a helpful constitution and an oppressive one.

Options:

1. Scope by directory/package (recommended)

```text
This article applies to:
- packages/ui-*
- packages/design-tokens

This article does not apply to:
- packages/infra-*
- tools/*
```

1. Scope by artifact type

```text
This article applies to plan.md and tasks.md for runtime services,
but does not apply to documentation-only branches.
```

1. Scope by risk tier

```text
Tier 1 (auth, payments): strict requirements
Tier 2 (internal tools): relaxed requirements
```

Write scope explicitly in the article itself.

### Q: Should we modify the core articles I-IX?

**Answer:** Generally avoid it.

Core articles are the stable backbone of Spec-Kit. In most projects, you get better outcomes by:

- Keeping I-IX as-is.
- Adding project-specific Articles X+ for domain constraints.
- Using templates and Phase -1 gates to operationalize enforcement.

If you must modify I-IX:

- Treat it as a high-scrutiny change.
- Assume MAJOR unless the change is purely editorial.
- Document migration impact explicitly.

Reference: `../docs/constitution-reference.md`

### Q: What is the risk of putting too much in the constitution?

**Answer:** Overloading the constitution creates three problems:

- It becomes hard to read, so people stop using it.
- It makes regeneration decisions noisy (every bump feels large).
- It encourages rule-lawyering instead of engineering judgment.

Prefer a constitution that is strict on a few high-value constraints and leaves details to specs/plans.

### Q: How do custom articles interact with versioning?

**Answer:** Adding or changing a custom article affects versioning the same way as any other change.

- New optional guidance: MINOR.
- New mandatory requirement across existing work: MAJOR.
- Clarifying examples: PATCH.

If the article is scoped to a subset of packages, a MINOR bump is often sufficient, but you still need to communicate scope clearly.

## Team Workflows

### Q: Who should approve constitution changes?

**Answer:** Treat constitution changes like changes to build rules or architecture standards.

Common approval models:

- Maintainers/owners approve (recommended for most teams).
- Architecture group approves (for larger orgs).
- Dual approval for MAJOR bumps (tech lead + security/compliance when relevant).

The key is consistency: people should know where constitutional authority lives.

### Q: How should we propose constitution changes?

**Answer:** Use a lightweight, repeatable process:

1. Open a PR with the constitution change.
2. Include a SYNC IMPACT report describing:
   - version delta
   - what artifacts/templates need updates
   - regeneration guidance
3. Link to the incident/retro/issue that motivated the change.

### Q: How should we communicate MAJOR bumps?

**Answer:** Communicate them like a policy change.

Minimum communication:

- A short announcement with:
  - why the bump happened
  - what becomes mandatory
  - what existing branches must do
  - a deadline (if you will enforce it in CI)

Example announcement:

```text
Constitution 2.0.0 is now active.
Change type: MAJOR (new mandatory security + tests; POC deferrals removed).
Action required: all in-flight branches must update plan/tasks to 2.0.0 before merge.
CI enforcement begins next Monday.
```

### Q: What should we do with old branches after a MAJOR bump?

**Answer:** You have three common strategies.

1. Mandatory regen

- Pros: consistent compliance; fewer surprises.
- Cons: interrupts work; may be costly for long branches.

1. Grandfathering

- Pros: less disruption.
- Cons: mixed standards; reviewers need to remember exceptions.

1. Case-by-case

- Pros: flexible; matches risk.
- Cons: higher coordination cost.

Recommended default:

- Case-by-case for short-lived branches.
- Mandatory regen for long-lived branches or high-risk domains.

Whichever you choose, write it down in the SYNC IMPACT report and enforce consistently.

### Q: How often should a team bump the constitution?

**Answer:** There is no universal cadence, but real-world patterns suggest:

- PATCH: as needed (small clarifications during normal work).
- MINOR: occasional (monthly/quarterly) when a new recurring pattern emerges.
- MAJOR: rare (when project maturity changes, or when a significant policy shift is required).

If you bump versions too often, people stop paying attention. If you never bump, the constitution stops reflecting reality.

### Q: How do we avoid churn when the constitution changes?

**Answer:** Use three techniques:

- Scope changes: define what the new rule applies to.
- Stage enforcement: announce first, enforce later.
- Automate checks: ensure the new rule is easy to comply with.

The goal is to reduce "hidden work" created by governance updates.

### Q: What should code review check after a constitution bump?

**Answer:** For branches created before the bump:

- Does the branch have an artifact with `sync_impact` older than current?
- If yes, is there a note in the plan explaining why no regen is needed?
- If the bump is MAJOR, does the plan include updated Phase -1 gates?

For branches created after the bump:

- Require all artifacts to match the current constitution version.

## NON-NEGOTIABLE

### Q: What does NON-NEGOTIABLE mean in a constitution?

**Answer:** NON-NEGOTIABLE marks a rule that cannot be waived via "justified exceptions".

In other words, it is not just a strong preference. It is an enforcement boundary.

Real-world examples include:

- License headers (legal compliance)
- Accessibility requirements
- Test-first requirement

Reference: `../artifacts/constitution-example-backpack.md`

### Q: When should we use NON-NEGOTIABLE?

**Answer:** Use it sparingly, for rules that are:

- Legally required
- Security critical
- Needed for safety/compliance
- Needed for tooling consistency (if non-compliance breaks automation)

If everything is NON-NEGOTIABLE, the marker stops being meaningful.

### Q: What are common mistakes with NON-NEGOTIABLE?

**Answer:** Common mistakes:

- Marking preferences as NON-NEGOTIABLE (creates resentment and workarounds).
- Using it to compensate for poor tooling ("it is required" but no checks exist).
- Adding it without documenting enforcement (people do not know how to comply).

If you cannot describe how it will be enforced, it is not ready to be NON-NEGOTIABLE.

### Q: How do we enforce NON-NEGOTIABLE rules (Phase -1 gates)?

**Answer:** The most effective enforcement is to catch violations before implementation.

Use Phase -1 gates in plan templates.

Example (conceptual):

```markdown
## Phase -1: Pre-Implementation Gates

### Security Gate (NON-NEGOTIABLE)
- [ ] Secrets are not committed (scan configured)
- [ ] Inputs validated at boundaries
- [ ] Threat model considered for auth/payment surfaces
```

Reference for gates: `../docs/constitution-reference.md`

### Q: How do we enforce NON-NEGOTIABLE rules with CI or pre-commit hooks?

**Answer:** Put mechanical checks in automation.

Examples:

- Pre-commit hook: block commits missing required license headers.
- CI: fail if tests are missing or coverage is below threshold.
- CI: fail if security scans find critical issues.
- Lint rules: error-level for forbidden patterns.

Backpack explicitly references pre-commit hooks and strict conventions; Manifest activates production-grade CI/CD requirements.

References:

- `../artifacts/constitution-example-backpack.md`
- `../artifacts/constitution-example-manifest.md`

### Q: How does NON-NEGOTIABLE interact with version bumps?

**Answer:** A newly introduced NON-NEGOTIABLE that applies to existing work is often a MAJOR bump.

Reason: it changes the enforcement boundary. Branches that were previously acceptable may now be blocked by CI or review.

If you are only labeling an already-enforced rule as NON-NEGOTIABLE (no behavior change), it can be MINOR or PATCH, but document explicitly:

- "Marker added for clarity; enforcement already existed."

### Q: What if we need an exception to a NON-NEGOTIABLE rule?

**Answer:** If you truly need an exception, treat it as a constitution change proposal, not a one-off waiver.

Options:

- Narrow the scope (rule does not apply to this package/category).
- Define an alternate compliant pathway (e.g., different test types).
- Keep it NON-NEGOTIABLE but add a documented emergency protocol.

If you allow ad-hoc exceptions, it is not NON-NEGOTIABLE.

### Q: How do we keep NON-NEGOTIABLE rules from slowing teams down?

**Answer:** Make compliance cheap.

Techniques:

- Provide generators/templates that include required boilerplate.
- Add linters/formatters that auto-fix.
- Add clear examples in the constitution.
- Ensure CI failures are actionable and fast.

Backpack's PATCH bump is a reminder: even "just examples" matter because they reduce friction.

Reference: `../artifacts/constitution-example-backpack.md`

## See Also

- [Constitution Reference](../docs/constitution-reference.md)
- [Spec-Kit Artifact Structure](../docs/artifact-structure.md)
- [IdeaVim Constitution Example](../artifacts/constitution-example-ideavim.md)
- [Backpack Constitution Example](../artifacts/constitution-example-backpack.md)
- [Manifest Constitution Example](../artifacts/constitution-example-manifest.md)
- [Customization FAQ](customization.md)
