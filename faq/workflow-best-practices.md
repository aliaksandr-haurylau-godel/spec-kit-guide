---
version: 1
created_date: 2026-02-17
purpose: Best practices for daily workflow, refinements, and fixing specs mid-development
---

# Workflow Best Practices

Best practices for organizing daily work with Spec-Kit, conducting refinements, and handling spec/plan corrections mid-development.

## Q: How Do I Fix Bugs or Issues in Spec/Plan After Tasks Are Started?

### The Problem

You've already started implementing tasks when you discover:
- A requirement in `spec.md` is incorrect or incomplete
- The `plan.md` approach won't work (technical constraint discovered)
- A K-decision in `research.md` needs revision
- The constitution was updated (version bump) and artifacts are out of sync

**Critical question:** Should you stop work or continue?

### Decision Framework

#### When to STOP Immediately

Stop current implementation work if:
- ✋ **Foundational assumption changed** - The core "WHAT" or "WHY" is wrong
- ✋ **Architecture pivot required** - Current approach is fundamentally flawed
- ✋ **Constitution violation discovered** - Existing code breaks NON-NEGOTIABLE rules
- ✋ **Scope changed significantly** - Stakeholders redefined success criteria

**Example (STOP scenario):**
```
Original spec: "Add OAuth authentication"
Discovery: Legal requires SAML only (no OAuth allowed)
→ STOP: Foundational requirement changed
```

#### When to CONTINUE (and Fix Later)

Continue current task if:
- ✅ **Minor clarification needed** - Edge case or validation rule missing
- ✅ **Documentation gap** - Spec lacks detail but intent is clear
- ✅ **Non-blocking enhancement** - Nice-to-have discovered, not P0
- ✅ **Refactoring opportunity** - Better implementation found, current one works

**Example (CONTINUE scenario):**
```
Original spec: "Password must be validated"
Discovery: Should specify minimum length (8 chars)
→ CONTINUE: Add clarification, update acceptance criteria
```

### Step-by-Step Fix Process

#### Step 1: Assess Impact Scope

**Questions to answer:**
1. What artifacts are affected? (spec, plan, tasks, research)
2. How many tasks are impacted? (all, some, one)
3. Is in-progress work salvageable or must be discarded?
4. Does this require constitutional update? (version bump)

**Create an impact document:**
```markdown
## Spec Amendment Impact Assessment

**Issue:** [Brief description of bug/issue]
**Discovered:** 2026-02-17
**Reporter:** [Your name]

### Affected Artifacts
- [ ] spec.md - Section 3.2 (User Story 4)
- [ ] plan.md - Phase 2 approach
- [ ] tasks.md - Tasks 2.1, 2.2, 2.3
- [ ] research.md - K2 decision needs revision

### Impact Scope
- **Severity:** Medium (blocks 3 tasks, not entire feature)
- **In-Progress Work:** Task 2.1 (50% done, salvageable)
- **Estimated Rework:** 4 hours

### Decision
Continue Task 2.1 to completion, then pause for spec amendment.
```

#### Step 2: Version the Spec

Update the spec frontmatter to track the amendment:

**Before:**
```yaml
---
feature_name: "User Authentication System"
branch_id: "branch-042-auth"
status: approved
sync_impact: 1.2.2
---
```

**After:**
```yaml
---
feature_name: "User Authentication System"
branch_id: "branch-042-auth"
status: approved  # Remains approved, amendment is clarification
sync_impact: 1.2.2  # Constitution version unchanged
spec_amendments:
  - date: 2026-02-17
    reason: "Added password validation requirements (8+ chars, special char)"
    sections_updated: ["3.2 User Story 4", "5.1 Functional Requirements"]
    tasks_affected: ["2.1", "2.2", "2.3"]
---
```

#### Step 3: Mark Changes with [AMENDED] Tags

In `spec.md`, clearly mark what changed:

```markdown
### User Story 4: Password Security [Priority: P1]

**As a** user
**I want** strong password requirements
**So that** my account is secure

**Acceptance Criteria**:
- [ ] Password minimum length: 8 characters **[AMENDED 2026-02-17]**
- [ ] Must contain: uppercase, lowercase, number, special char **[AMENDED 2026-02-17]**
- [ ] Previous criteria unchanged: no common passwords, not same as username
```

#### Step 4: Update Companion Artifacts

**Update plan.md** (if technical approach changes):
```markdown
## Phase 2: Authentication Implementation

### Changes from Original Plan [AMENDED 2026-02-17]
- Added password validation library (zxcvbn) for strength checking
- Updated validation flow diagram (see below)
- **Original approach preserved:** JWT token generation, session management

[Updated diagram here]
```

**Update research.md** (if K-decision needs revision):
```markdown
## K2: Password Validation Strategy [REVISED 2026-02-17]

**Original Decision:** Simple regex validation
**Revision Reason:** Security audit requires zxcvbn library (OWASP recommendation)

**New Decision:** Use zxcvbn + custom rules
**Rationale:** 
- OWASP approved approach
- Catches common weak passwords (e.g., "Password123!")
- Minimal performance impact (<50ms per validation)

**Migration:** Existing regex kept for backward compatibility, zxcvbn adds second layer
```

#### Step 5: Regenerate Affected Tasks

**Option A: Selective Regeneration (Preferred)**

Regenerate only affected tasks:
```bash
# Tell your AI agent:
"Regenerate tasks 2.1, 2.2, 2.3 based on updated spec.md and plan.md.
Preserve completed work in task 2.1 (steps 1-3 done).
New tasks should reflect [AMENDED 2026-02-17] changes."
```

**Option B: Full Regeneration (If major changes)**

```bash
# Use Spec-Kit command
/speckit.tasks --regenerate

# Then manually:
# 1. Review new tasks.md
# 2. Merge completed work from old tasks.md
# 3. Update task status (mark completed tasks)
```

#### Step 6: Communicate Changes

**For team projects, announce the amendment:**

**Slack/Email Template:**
```
📋 Spec Amendment: branch-042-auth

**What changed:** Added password validation requirements (spec.md section 3.2)
**Why:** Security audit findings (OWASP compliance)
**Impact:** Tasks 2.1, 2.2, 2.3 updated; Task 2.1 in progress (no rework needed)
**Action needed:** Review updated spec.md before starting Task 2.4

**Links:**
- Updated spec: .specify/memory/specs/branch-042-auth/spec.md
- Impact assessment: [Link to commit or doc]

Questions? Ask in #auth-feature thread.
```

### Prevention Strategies

**How to minimize mid-development spec changes:**

1. **Front-load research** - Create research.md for complex features BEFORE spec approval
2. **Use [NEEDS CLARIFICATION]** - Tag uncertain areas in spec early
3. **Phase -1 gates** - Constitutional pre-checks catch violations before coding
4. **Spike stories** - For risky technical areas, create small proof-of-concept first
5. **Daily spec reviews** - 5-minute morning check: "Does spec still match reality?"

---

## Q: How Should I Organize Daily Work with Spec-Kit?

### The Daily Cadence

Spec-Kit works best with a structured daily routine:

#### Morning (5-10 minutes)

**Spec Alignment Check:**
```bash
# 1. Read relevant sections before coding
cat .specify/memory/specs/current-feature/spec.md | grep "User Story"

# 2. Review today's tasks
cat .specify/memory/specs/current-feature/tasks.md | grep "## Task 2.1"

# 3. Check for [NEEDS CLARIFICATION] markers
grep -r "NEEDS CLARIFICATION" .specify/memory/specs/current-feature/
```

**Questions to answer each morning:**
- What user story am I implementing today?
- What are the acceptance criteria I must meet?
- Are there any [NEEDS CLARIFICATION] items blocking me?
- Do I need to review any K-decisions from research.md?

#### During Work (Real-Time)

**Use [NEEDS CLARIFICATION] markers proactively:**

In your AI chat or code comments:
```markdown
[NEEDS CLARIFICATION - 2026-02-17]
Question: Should password reset emails expire after 24 hours or 1 hour?
Context: Spec says "reasonable timeframe" but doesn't specify.
Impact: Affects Task 2.3 implementation.
Decision needed by: EOD 2026-02-17 (blocking Task 2.3)
```

**Update task status as you work:**

In `tasks.md`, track progress:
```markdown
## Task 2.1: Implement Password Validation [Status: IN_PROGRESS]

**Progress Log:**
- [x] Step 1: Install zxcvbn library (2026-02-17 10:00 AM)
- [x] Step 2: Create validation function (2026-02-17 11:30 AM)
- [ ] Step 3: Add unit tests (IN PROGRESS - 2026-02-17 2:00 PM)
- [ ] Step 4: Integration with signup form

**Blockers:** None
**[NEEDS CLARIFICATION]:** Should we allow passphrase style passwords? (e.g., "correct-horse-battery-staple")
```

#### End of Day (5 minutes)

**Status snapshot:**
1. **Mark completed steps** - Update tasks.md with [x] for done items
2. **Document blockers** - Add [NEEDS CLARIFICATION] for tomorrow's standup
3. **Commit work** - Push code + updated tasks.md together

**Example commit message:**
```
feat: implement password validation (Task 2.1 - 75% complete)

- Added zxcvbn library for strength checking
- Implemented validation function with unit tests
- TODO: Integration with signup form (blocked by clarification)

Related: branch-042-auth, Task 2.1
Needs clarification: Passphrase support (see tasks.md)
```

### The Weekly Cadence

#### Monday: Week Planning (15 minutes)

**Review this week's tasks:**
```bash
# Filter tasks for current week
grep "Priority: P0\|Priority: P1" tasks.md

# Check dependencies
grep "Dependencies:" tasks.md
```

**Questions to answer:**
- Which tasks are P0/P1 for this week?
- Are all dependencies resolved (K-decisions made, blockers cleared)?
- Do I need to schedule a refinement session?

#### Wednesday: Mid-Week Check-In (10 minutes)

**Progress review:**
- Are we on track to complete planned tasks?
- Have new [NEEDS CLARIFICATION] items appeared?
- Should we schedule Friday refinement?

#### Friday: Refinement Session (30-60 minutes)

See "Refinement Practices" section below.

---

## Q: Are Refinements Necessary? How Should They Be Conducted?

### When Refinements Are Needed

**Refinements are necessary when:**

1. **Accumulation of [NEEDS CLARIFICATION] markers** - 3+ unresolved questions blocking work
2. **Spec drift detected** - Implementation reveals spec assumptions were wrong
3. **New requirements emerge** - Stakeholder feedback requires scope adjustment
4. **Constitutional update** - constitution.md version bumped, need to assess impact
5. **Complex feature kickoff** - Before starting high-complexity feature (research.md needed)

**Refinements are NOT needed when:**
- ❌ Work is flowing smoothly with no blockers
- ❌ All acceptance criteria are clear and testable
- ❌ No [NEEDS CLARIFICATION] markers for 2+ weeks
- ❌ Team has <5 people (async clarification via chat is sufficient)

### Refinement Session Structure

#### Preparation (Before Meeting)

**Agenda document** (create 24 hours before):
```markdown
# Refinement Session - 2026-02-21
**Duration:** 30 minutes
**Attendees:** Dev team, Product Owner, QA lead

## Open Questions (from [NEEDS CLARIFICATION] markers)

### Q1: Password Reset Email Expiry
**Source:** Task 2.3 (branch-042-auth)
**Question:** 1-hour vs 24-hour expiry for reset links?
**Impact:** Security vs user convenience trade-off
**Proposed Answer:** 1 hour (OWASP recommendation)
**Discussion time:** 5 minutes

### Q2: Social Login Support
**Source:** spec.md section 4.5 (marked out-of-scope, but users asking)
**Question:** Should we add to current sprint or defer to v1.1?
**Impact:** +2 weeks effort, defers core feature completion
**Proposed Answer:** Defer to v1.1, update spec.md out-of-scope section
**Discussion time:** 5 minutes

[...continue for all questions...]
```

#### Meeting Flow (30 minutes)

**Part 1: Question Triage (10 minutes)**

Go through each [NEEDS CLARIFICATION] item:
- ✅ **Answerable now?** - Document decision, assign update owner
- ⏸️ **Need research?** - Create spike task, schedule follow-up
- 🗑️ **No longer relevant?** - Remove marker, document why

**Part 2: Decisions & Documentation (15 minutes)**

For each decision made:
1. **Update spec.md** - Remove [NEEDS CLARIFICATION], add answer
2. **Update plan.md** - Adjust technical approach if needed
3. **Create K-decision** - If complex, document in research.md
4. **Regenerate tasks** - If tasks affected, mark for regeneration

**Example decision documentation:**
```markdown
## Refinement Decision Log - 2026-02-21

### Decision R1: Password Reset Email Expiry
**Question:** 1-hour vs 24-hour expiry?
**Decision:** 1 hour
**Rationale:** OWASP recommendation, reduces attack window
**Spec update:** Section 5.2 (Security Requirements)
**Tasks affected:** Task 2.3
**Action owner:** @john (update spec by EOD)
```

**Part 3: Next Steps (5 minutes)**

- Assign artifact update owners (who updates spec, plan, tasks?)
- Schedule task regeneration (if needed)
- Set date for next refinement (if needed, otherwise "on-demand")

#### Post-Refinement (Within 24 hours)

**Update artifacts:**
```bash
# 1. Remove [NEEDS CLARIFICATION] markers from spec.md
# 2. Add [REFINED 2026-02-21] tags where answers added
# 3. Update sync_impact if constitution referenced
# 4. Regenerate tasks if decisions affect implementation
```

**Example refined spec section:**
```markdown
### 5.2 Security Requirements

**Password Reset Flow** **[REFINED 2026-02-21]**
- Reset link expiry: 1 hour (OWASP recommendation)
- One-time use: Link invalid after password changed
- Rate limiting: Max 3 reset requests per hour per email

**Rationale:** Balance security (minimize attack window) with usability.
```

### Async vs Synchronous Refinements

#### When to Use Async (Chat/PR Comments)

**Good for:**
- ✅ Small teams (<5 people)
- ✅ Simple clarifications (yes/no, pick option A/B/C)
- ✅ Single-question issues
- ✅ Non-blocking questions (can proceed with other tasks)

**Example async workflow:**
```
Developer (in PR): [NEEDS CLARIFICATION] Should we cache user sessions?
Product Owner (comment): Yes, 30-min TTL. Updated spec.md section 4.3.
Developer: Thanks! Updating Task 2.5 acceptance criteria.
```

#### When to Use Sync (Meeting)

**Good for:**
- ✅ Complex trade-off decisions
- ✅ Multiple interdependent questions
- ✅ Blocking issues affecting multiple tasks
- ✅ Stakeholder alignment needed
- ✅ 5+ accumulated [NEEDS CLARIFICATION] markers

### Refinement Anti-Patterns (Avoid These)

**❌ Anti-Pattern 1: Refinement Theater**
- **Problem:** Weekly refinements scheduled but nothing to refine
- **Fix:** Make refinements on-demand, not recurring

**❌ Anti-Pattern 2: Refinement Paralysis**
- **Problem:** Over-analyzing every edge case, no decisions made
- **Fix:** Time-box decisions (5 min per question), use "decide and iterate" approach

**❌ Anti-Pattern 3: Undocumented Decisions**
- **Problem:** Decisions made verbally but not captured in artifacts
- **Fix:** Always update spec.md/plan.md after refinement, use [REFINED] tags

**❌ Anti-Pattern 4: Skipping Constitutional Check**
- **Problem:** Decisions contradict constitution.md
- **Fix:** Start every refinement with "Does this align with constitution version X.Y.Z?"

---

## Best Practices Summary

### Daily Checklist

**Morning:**
- [ ] Read today's user story and acceptance criteria (5 min)
- [ ] Check for blockers or [NEEDS CLARIFICATION] items (2 min)
- [ ] Review relevant K-decisions from research.md (3 min)

**During Work:**
- [ ] Add [NEEDS CLARIFICATION] markers immediately when questions arise
- [ ] Update tasks.md progress in real-time (or at logical breakpoints)
- [ ] Reference K-decisions by number (K1, K2) in code comments and commits

**End of Day:**
- [ ] Mark completed steps in tasks.md (2 min)
- [ ] Document blockers for tomorrow (2 min)
- [ ] Commit code + updated tasks.md together (1 min)

### Weekly Checklist

**Monday:**
- [ ] Review week's planned tasks (P0/P1 priorities)
- [ ] Check dependencies resolved
- [ ] Schedule refinement if 3+ [NEEDS CLARIFICATION] items accumulated

**Wednesday:**
- [ ] Mid-week progress check (on/off track?)
- [ ] Triage new questions (can they wait until Friday?)

**Friday:**
- [ ] Refinement session (if needed)
- [ ] Update artifacts post-refinement
- [ ] Plan next week's tasks

### Monthly Checklist

- [ ] Review constitution.md for needed updates
- [ ] Capture lessons learned (what went well, what didn't?)
- [ ] Update project templates based on learnings
- [ ] Audit [NEEDS CLARIFICATION] markers (are old ones still relevant?)

---

## See Also

- [Integration Patterns FAQ](integration-patterns.md) - Jira/Linear export and token optimization
- [Troubleshooting FAQ](troubleshooting.md) - Common issues and fixes
- [Constitution Evolution FAQ](constitution-evolution.md) - Versioning and sync impact
- [Basic Workflow Guide](../workflows/basic-workflow.md) - End-to-end Spec-Kit workflow
