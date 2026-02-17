---
version: 1
created_date: 2026-02-17
purpose: Integration with external tools (Jira, Linear, GitHub Projects) and token optimization strategies
---

# Integration Patterns & Token Optimization

Best practices for exporting Spec-Kit tasks to external project management tools and strategies for optimizing AI token usage.

## Q: How Do I Export Spec-Kit Tasks to Jira?

### Overview

Spec-Kit generates tasks in `tasks.md` (markdown format). Most teams use external tools (Jira, Linear, GitHub Projects) for sprint tracking. This guide covers three integration approaches, from manual to automated.

### Approach 1: Manual Copy-Paste (Small Teams)

**Best for:**
- Teams <5 people
- Low task churn (tasks don't change frequently)
- Simple workflows (no complex dependencies)

**Process:**

#### Step 1: Create Jira Epic for the Feature

```
Epic Name: User Authentication System (branch-042-auth)
Epic Description:
  See full specification: .specify/memory/specs/branch-042-auth/spec.md
  Plan: .specify/memory/specs/branch-042-auth/plan.md
  Tasks: .specify/memory/specs/branch-042-auth/tasks.md
```

#### Step 2: Map Phases to Jira Epics or Labels

From `plan.md`:
```markdown
## Phase -1: Constitutional Pre-Checks
## Phase 1: Core Authentication
## Phase 2: Integration & Testing
## Phase 3: Deployment
```

In Jira:
- Option A: Create 4 epics (one per phase)
- Option B: Create labels: `phase-0`, `phase-1`, `phase-2`, `phase-3`

**Recommendation:** Use labels for small features (< 20 tasks), epics for large features.

#### Step 3: Convert Tasks to Jira Stories

From `tasks.md`:
```markdown
## Task 1.1: Implement Password Validation [Effort: M]

**Description:**
Create password validation function using zxcvbn library.

**Dependencies:**
- K2 (Password Validation Strategy from research.md)
- zxcvbn library installation (Task 0.1)

**Acceptance Criteria:**
- [ ] Function accepts password string, returns strength score (0-4)
- [ ] Minimum 8 characters enforced
- [ ] Rejects common passwords (from zxcvbn dictionary)
- [ ] Unit tests with 95%+ coverage

**Effort:** Medium (4-6 hours)
```

In Jira story:
```
Story ID: AUTH-101
Title: Implement Password Validation

Description:
Create password validation function using zxcvbn library.

See full task details: tasks.md - Task 1.1
Reference: K2 (Password Validation Strategy)

Acceptance Criteria:
☐ Function accepts password string, returns strength score (0-4)
☐ Minimum 8 characters enforced
☐ Rejects common passwords (from zxcvbn dictionary)
☐ Unit tests with 95%+ coverage

Effort: 6 Story Points (Medium, 4-6 hours)

Labels: phase-1, authentication
Epic: User Authentication System (branch-042-auth)
Depends on: AUTH-100 (zxcvbn library installation)
```

#### Step 4: Add Bi-Directional Links

**In tasks.md:**
```markdown
## Task 1.1: Implement Password Validation [Effort: M]
**Jira:** [AUTH-101](https://your-company.atlassian.net/browse/AUTH-101)

[...rest of task...]
```

**In Jira (AUTH-101):**
- Add custom field "Spec-Kit Task ID": `1.1`
- Add link to `tasks.md` in description or as attachment

### Approach 2: Semi-Automated with Jira CLI (Medium Teams)

**Best for:**
- Teams 5-20 people
- Moderate task churn
- Need for programmatic task creation

**Prerequisites:**
```bash
# Install Jira CLI
npm install -g jira-cli

# Configure authentication
jira config set --server https://your-company.atlassian.net
jira config set --token YOUR_API_TOKEN
```

#### Step 1: Create Export Script

**File:** `.specify/scripts/export-to-jira.sh`

```bash
#!/usr/bin/env bash
set -e

# Configuration
SPEC_DIR=".specify/memory/specs/$1"
EPIC_KEY="$2"

if [ -z "$1" ] || [ -z "$2" ]; then
  echo "Usage: ./export-to-jira.sh <branch-id> <epic-key>"
  echo "Example: ./export-to-jira.sh branch-042-auth AUTH-100"
  exit 1
fi

echo "Exporting tasks from $SPEC_DIR to Jira epic $EPIC_KEY"

# Parse tasks.md and create Jira stories
# This is a simplified example - you'll need to enhance parsing
grep -E "^## Task [0-9]+\.[0-9]+" "$SPEC_DIR/tasks.md" | while read -r line; do
  TASK_ID=$(echo "$line" | grep -oP 'Task \K[0-9]+\.[0-9]+')
  TASK_TITLE=$(echo "$line" | sed 's/^## Task [0-9]\+\.[0-9]\+: //' | sed 's/ \[Effort:.*\]//')
  
  echo "Creating story: $TASK_TITLE (Task $TASK_ID)"
  
  # Create Jira issue
  jira issue create \
    --project AUTH \
    --type Story \
    --summary "$TASK_TITLE" \
    --description "See tasks.md - Task $TASK_ID\n\nFull spec: $SPEC_DIR/spec.md" \
    --epic-link "$EPIC_KEY" \
    --custom-field "customfield_10001=$TASK_ID"  # Spec-Kit Task ID field
done

echo "Export complete!"
```

#### Step 2: Run Export

```bash
# Make script executable
chmod +x .specify/scripts/export-to-jira.sh

# Export tasks for branch-042-auth
./.specify/scripts/export-to-jira.sh branch-042-auth AUTH-100
```

#### Step 3: Advanced Parsing (Python Example)

**File:** `.specify/scripts/export-to-jira.py`

```python
#!/usr/bin/env python3
import re
import sys
from jira import JIRA

# Configuration
JIRA_SERVER = 'https://your-company.atlassian.net'
JIRA_TOKEN = 'YOUR_API_TOKEN'
PROJECT_KEY = 'AUTH'

def parse_tasks(tasks_file):
    """Parse tasks.md and extract structured data."""
    with open(tasks_file, 'r') as f:
        content = f.read()
    
    tasks = []
    # Regex to match task blocks
    task_pattern = r'## Task (\d+\.\d+): (.+?) \[Effort: (\w+)\]\n\n(.+?)(?=\n## Task|\Z)'
    
    for match in re.finditer(task_pattern, content, re.DOTALL):
        task_id = match.group(1)
        title = match.group(2)
        effort = match.group(3)
        body = match.group(4)
        
        # Extract acceptance criteria
        criteria = re.findall(r'- \[ \] (.+)', body)
        
        # Extract dependencies
        deps = re.findall(r'Dependencies: (.+)', body)
        
        tasks.append({
            'id': task_id,
            'title': title,
            'effort': effort,
            'criteria': criteria,
            'dependencies': deps[0] if deps else None
        })
    
    return tasks

def create_jira_issue(jira, task, epic_key):
    """Create Jira story from task."""
    # Map effort to story points
    effort_map = {'S': 2, 'M': 5, 'L': 8}
    story_points = effort_map.get(task['effort'], 3)
    
    # Build description
    description = f"See tasks.md - Task {task['id']}\n\n"
    description += "h3. Acceptance Criteria\n"
    for criterion in task['criteria']:
        description += f"* {criterion}\n"
    
    if task['dependencies']:
        description += f"\nh3. Dependencies\n{task['dependencies']}"
    
    # Create issue
    issue = jira.create_issue(
        project=PROJECT_KEY,
        summary=task['title'],
        description=description,
        issuetype={'name': 'Story'},
        customfield_10014=epic_key,  # Epic Link field
        customfield_10001=task['id'],  # Custom Spec-Kit Task ID field
        customfield_10016=story_points  # Story Points field
    )
    
    return issue.key

def main():
    if len(sys.argv) != 3:
        print("Usage: export-to-jira.py <tasks.md path> <epic-key>")
        sys.exit(1)
    
    tasks_file = sys.argv[1]
    epic_key = sys.argv[2]
    
    # Connect to Jira
    jira = JIRA(server=JIRA_SERVER, token_auth=JIRA_TOKEN)
    
    # Parse and export
    tasks = parse_tasks(tasks_file)
    for task in tasks:
        issue_key = create_jira_issue(jira, task, epic_key)
        print(f"Created {issue_key}: {task['title']}")

if __name__ == '__main__':
    main()
```

**Usage:**
```bash
pip install jira
python .specify/scripts/export-to-jira.py \
  .specify/memory/specs/branch-042-auth/tasks.md \
  AUTH-100
```

### Approach 3: Full Automation with API Integration (Large Teams)

**Best for:**
- Teams 20+ people
- High task churn (specs change frequently)
- Bi-directional sync needed (Jira → Spec-Kit)

**Architecture:**

```
┌─────────────┐          ┌──────────────┐          ┌──────────┐
│  tasks.md   │ ────────>│  Sync Agent  │────────> │   Jira   │
│  (Spec-Kit) │ <──────  │  (Python)    │<──────   │  (API)   │
└─────────────┘          └──────────────┘          └──────────┘
     ▲                           │
     │                           │
     └──────── Git Hook ─────────┘
              (on commit)
```

**Implementation:**

**File:** `.specify/scripts/sync-jira.py`

```python
#!/usr/bin/env python3
"""
Bi-directional sync between Spec-Kit tasks.md and Jira.

Features:
- Export new tasks from tasks.md to Jira
- Update Jira stories when tasks.md changes
- Update tasks.md when Jira story status changes
"""

import hashlib
import json
import os
from datetime import datetime
from jira import JIRA

SYNC_STATE_FILE = '.specify/memory/.jira-sync-state.json'

class JiraSync:
    def __init__(self, server, token, project_key):
        self.jira = JIRA(server=server, token_auth=token)
        self.project_key = project_key
        self.state = self.load_state()
    
    def load_state(self):
        """Load previous sync state."""
        if os.path.exists(SYNC_STATE_FILE):
            with open(SYNC_STATE_FILE, 'r') as f:
                return json.load(f)
        return {}
    
    def save_state(self):
        """Save sync state."""
        with open(SYNC_STATE_FILE, 'w') as f:
            json.dump(self.state, f, indent=2)
    
    def hash_task(self, task):
        """Create hash of task content for change detection."""
        content = f"{task['title']}|{task['criteria']}|{task['effort']}"
        return hashlib.md5(content.encode()).hexdigest()
    
    def sync_to_jira(self, tasks, epic_key):
        """Export tasks to Jira, update if changed."""
        for task in tasks:
            task_hash = self.hash_task(task)
            task_id = task['id']
            
            # Check if task exists in Jira
            if task_id in self.state:
                # Task exists, check if changed
                if self.state[task_id]['hash'] != task_hash:
                    self.update_jira_issue(self.state[task_id]['jira_key'], task)
                    self.state[task_id]['hash'] = task_hash
                    print(f"Updated {self.state[task_id]['jira_key']}: {task['title']}")
            else:
                # New task, create in Jira
                jira_key = self.create_jira_issue(task, epic_key)
                self.state[task_id] = {
                    'jira_key': jira_key,
                    'hash': task_hash,
                    'created': datetime.now().isoformat()
                }
                print(f"Created {jira_key}: {task['title']}")
        
        self.save_state()
    
    def sync_from_jira(self, tasks_file):
        """Update tasks.md with Jira status changes."""
        # Read current tasks.md
        with open(tasks_file, 'r') as f:
            content = f.read()
        
        # For each tracked task, check Jira status
        for task_id, state in self.state.items():
            jira_key = state['jira_key']
            issue = self.jira.issue(jira_key)
            jira_status = issue.fields.status.name
            
            # Update task status in tasks.md
            # (This is simplified - you'd need more robust parsing)
            task_pattern = f"## Task {task_id}:"
            if task_pattern in content:
                # Add status annotation
                content = content.replace(
                    task_pattern,
                    f"{task_pattern} [Jira: {jira_status}]"
                )
        
        # Write updated tasks.md
        with open(tasks_file, 'w') as f:
            f.write(content)
        
        print(f"Updated tasks.md with Jira status")

# Usage example
if __name__ == '__main__':
    sync = JiraSync(
        server='https://your-company.atlassian.net',
        token='YOUR_API_TOKEN',
        project_key='AUTH'
    )
    
    # Parse tasks (use parse_tasks from Approach 2)
    tasks = parse_tasks('.specify/memory/specs/branch-042-auth/tasks.md')
    
    # Sync to Jira
    sync.sync_to_jira(tasks, epic_key='AUTH-100')
    
    # Sync from Jira
    sync.sync_from_jira('.specify/memory/specs/branch-042-auth/tasks.md')
```

**Setup Git Hook:**

**File:** `.git/hooks/post-commit`

```bash
#!/bin/bash
# Auto-sync tasks.md changes to Jira on commit

CHANGED_FILES=$(git diff-tree --no-commit-id --name-only -r HEAD)

if echo "$CHANGED_FILES" | grep -q "tasks.md"; then
  echo "Detected tasks.md change, syncing to Jira..."
  python .specify/scripts/sync-jira.py
fi
```

### Field Mapping Reference

| Spec-Kit Field | Jira Field | Notes |
|----------------|------------|-------|
| Phase | Epic or Label | Use epic for large features (20+ tasks) |
| Task ID (e.g., 1.1) | Custom Field | Create "Spec-Kit Task ID" text field |
| Task Title | Summary | Direct mapping |
| Task Description | Description | Include link to tasks.md |
| Acceptance Criteria | Sub-tasks OR Description checklist | Sub-tasks for complex criteria |
| Effort (S/M/L) | Story Points | Map: S=2, M=5, L=8 (adjust to your team) |
| Dependencies (K-refs) | Linked Issues OR Description | Create links if dependencies are Jira issues |
| Status | Status | Map: Pending→To Do, In Progress→In Progress, Done→Done |

### Common Integration Challenges

**Challenge 1: Acceptance Criteria Too Detailed for Jira**

**Problem:** Spec-Kit acceptance criteria are very detailed (5-10 items), Jira description gets cluttered.

**Solution:** Use Jira sub-tasks for acceptance criteria:
```python
# In export script
for criterion in task['criteria']:
    jira.create_issue(
        project=PROJECT_KEY,
        summary=criterion[:100],  # Truncate if too long
        issuetype={'name': 'Sub-task'},
        parent={'key': parent_issue_key}
    )
```

**Challenge 2: K-Decision References Don't Translate**

**Problem:** Tasks reference K-decisions (e.g., "Depends on K2"), Jira doesn't understand this.

**Solution:** Add K-decision explanations in Jira description:
```python
if 'K2' in task['dependencies']:
    description += "\n\nh3. K-Decision Reference\n"
    description += "K2: Password Validation Strategy\n"
    description += "See research.md for full rationale\n"
    description += "Link: .specify/memory/specs/branch-042-auth/research.md#k2"
```

**Challenge 3: Jira Changes Don't Sync Back**

**Problem:** Developer updates Jira story, tasks.md is out of sync.

**Solution:** Treat Jira as source of truth for status only, tasks.md for details:
- Jira status → tasks.md status (automated sync)
- Task details remain in tasks.md (manual updates only)

---

## Q: How Do I Integrate with GitHub Projects or Linear?

### GitHub Projects (Basic)

**Approach:** Use GitHub Issues as tasks, Projects for Kanban view.

**Export Script:**

```bash
#!/bin/bash
# Create GitHub issues from tasks.md

gh issue create \
  --title "Task 1.1: Implement Password Validation" \
  --body "See tasks.md - Task 1.1" \
  --label "phase-1,authentication" \
  --milestone "Sprint 23"
```

**Pros:**
- Native Git integration
- Free for public repos
- Simple automation via `gh` CLI

**Cons:**
- Less powerful than Jira (no custom fields for Spec-Kit Task ID)
- Limited reporting

### Linear (Advanced)

Linear has excellent API and CLI support.

**Export Script:**

```bash
# Install Linear CLI
npm install -g @linear/cli

# Create issue
linear issue create \
  --title "Implement Password Validation" \
  --description "Task 1.1 from tasks.md" \
  --priority 1 \
  --estimate 5 \
  --team AUTH
```

**Pros:**
- Fast, modern UI
- Excellent keyboard shortcuts
- Good API for automation

**Cons:**
- Paid product
- Less enterprise features than Jira

---

## Q: What Are the Best Practices for Token Optimization?

### Understanding Token Costs

**Typical Spec-Kit workflow token usage:**

| Operation | Input Tokens | Output Tokens | Total | Cost (GPT-4) |
|-----------|-------------|---------------|-------|--------------|
| `/speckit.specify` | 1,500 | 800 | 2,300 | ~$0.07 |
| `/speckit.plan` | 2,000 | 1,200 | 3,200 | ~$0.10 |
| `/speckit.tasks` | 1,800 | 1,000 | 2,800 | ~$0.09 |
| `/speckit.implement` | 3,000 | 2,000 | 5,000 | ~$0.15 |
| **Total per feature** | 8,300 | 5,000 | **13,300** | **~$0.41** |

**With optimization (see strategies below):**
- Total: ~8,000 tokens (~40% reduction)
- Cost: ~$0.25 per feature (~40% savings)

### Strategy 1: Use K-References Instead of Repeating Decisions

**Before (Inefficient):**
```markdown
## Task 1.1: Implement Password Validation

We need to use the zxcvbn library because it provides entropy-based 
password strength calculation which is more accurate than simple 
regex validation. The library is maintained by Dropbox and recommended 
by OWASP. We evaluated three options:

1. Simple regex (pros: fast, cons: inaccurate)
2. Custom entropy calculation (pros: tailored, cons: maintenance burden)
3. zxcvbn library (pros: proven, maintained, cons: 800KB bundle size)

We chose option 3 because security is priority P0 and bundle size 
is acceptable given lazy loading strategy.

[...rest of task...]
```

**Token count:** ~120 tokens of repeated rationale

**After (Efficient):**
```markdown
## Task 1.1: Implement Password Validation

**Dependencies:** K2 (Password Validation Strategy)

Implementation: Use zxcvbn library per K2 decision.

[...rest of task...]
```

**Token count:** ~15 tokens (reference only)

**Savings:** 105 tokens per task x 10 tasks = **1,050 tokens saved**

### Strategy 2: Keep Specs Concise (WHAT/WHY, Not HOW)

**Before (Over-Specified):**
```markdown
## User Story 2: Password Reset Flow

**As a** user
**I want** to reset my password
**So that** I can regain account access if I forget

**Acceptance Criteria:**
- [ ] Reset form has email input field (type="email")
- [ ] Form uses React Hook Form with zod validation
- [ ] Submit button triggers POST /api/auth/reset-password
- [ ] API endpoint validates email format using regex /^[^\s@]+@[^\s@]+\.[^\s@]+$/
- [ ] Email service uses SendGrid API with template ID "pwd-reset-123"
- [ ] Email contains token generated with crypto.randomBytes(32)
- [ ] Token stored in Redis with 1-hour TTL using key pattern: "reset:{email}:{token}"
- [ ] Reset link format: https://app.com/reset?token={token}&email={email}
```

**Problem:** Spec includes HOW (implementation details), bloating context.

**Token count:** ~140 tokens

**After (Appropriate Spec):**
```markdown
## User Story 2: Password Reset Flow [Priority: P1]

**As a** user
**I want** to reset my password via email
**So that** I can regain account access if I forget

**Acceptance Criteria:**
- [ ] User submits email address, receives reset email within 2 minutes
- [ ] Email contains secure one-time reset link, valid for 1 hour
- [ ] Link allows password update without re-authentication
- [ ] Link expires after use or timeout (whichever comes first)

**Security Requirements:**
- Token must be cryptographically random (not predictable)
- Email validation required (prevent abuse)
- Rate limiting: max 3 requests per hour per email
```

**Token count:** ~85 tokens

**Savings:** 55 tokens per user story x 8 stories = **440 tokens saved**

**Implementation details go in plan.md:**
```markdown
## Phase 1: Password Reset Implementation

### Technical Approach
- **Frontend:** React Hook Form + zod validation
- **Backend:** Express.js endpoint `/api/auth/reset-password`
- **Email:** SendGrid API (template ID: pwd-reset-123)
- **Token:** crypto.randomBytes(32), hex encoded
- **Storage:** Redis, key pattern `reset:{email}:{token}`, TTL 3600s

[...detailed implementation...]
```

### Strategy 3: Batch Operations (Regenerate Multiple Artifacts Together)

**Inefficient Pattern:**
```bash
# Separate AI conversations
> /speckit.specify Add user authentication
[wait for response, 2,300 tokens]

> /speckit.plan
[wait for response, 3,200 tokens, DUPLICATES spec context]

> /speckit.tasks
[wait for response, 2,800 tokens, DUPLICATES plan context]
```

**Total:** ~8,300 tokens (with duplicated context)

**Efficient Pattern:**
```bash
# Single conversation with context reuse
> /speckit.specify Add user authentication. 
  After spec approval, generate plan and tasks in same session.

[AI generates spec → you approve → AI reuses context for plan → you approve → AI reuses context for tasks]
```

**Total:** ~5,500 tokens (40% reduction via context reuse)

**Pro Tip:** Use conversation threads to maintain context:
- Start thread: `/speckit.specify`
- Continue thread: "Looks good, generate plan"
- Continue thread: "Plan approved, generate tasks"

### Strategy 4: Reuse Templates and Constitutional Patterns

**Inefficient Pattern:**
Every feature reinvents structure:
```
Feature A spec.md: Custom frontmatter, unique structure
Feature B spec.md: Different frontmatter, different structure
Feature C spec.md: Yet another format
```

**Result:** AI can't predict structure, uses more tokens to "figure out" format.

**Efficient Pattern:**
Standardize in constitution.md:
```yaml
# Article X: Spec Template Standard

All spec.md MUST follow this structure:
1. Frontmatter (feature_name, branch_id, complexity, effort, status, sync_impact)
2. Context & Motivation (1-2 paragraphs max)
3. User Stories (prioritized P0/P1/P2, Given/When/Then acceptance)
4. Functional Requirements (FR1, FR2, ...)
5. Non-Functional Requirements (Performance, Security, ...)
6. Constraints (In Scope, Out of Scope)
7. Success Metrics (quantifiable)
```

**Result:** AI uses template, reduces "structure discovery" tokens by ~200 per spec.

### Strategy 5: Cache Complex Context with research.md

**Scenario:** Complex technical decision with multiple options evaluated.

**Inefficient Pattern (Repeat in Every Artifact):**
```
spec.md: "We need password validation (see rationale below: ...)"
plan.md: "For password validation, we evaluated 3 options: ..."
tasks.md: "Password validation (using zxcvbn because ...)"
```

**Efficient Pattern (Cache in research.md):**

**research.md:**
```markdown
## K2: Password Validation Strategy

**Problem:** Need secure password validation

**Options Evaluated:**
1. Simple regex: Fast but inaccurate
2. Custom entropy: Accurate but high maintenance
3. zxcvbn library: Proven, maintained, 800KB bundle

**Decision:** Option 3 (zxcvbn)

**Rationale:** 
- OWASP recommended
- Battle-tested (Dropbox production)
- 800KB acceptable with lazy loading

**Consequences:**
- Bundle size increase (mitigated by lazy load)
- External dependency (risk: library abandonment - low)
```

**Everywhere else (spec, plan, tasks):**
```markdown
Dependencies: K2 (Password Validation Strategy)
```

**Token savings:** ~300 tokens (rationale stored once, referenced 10+ times)

### Strategy 6: Minimize [NEEDS CLARIFICATION] Churn

**Anti-Pattern:**
```
Day 1: AI generates spec with [NEEDS CLARIFICATION]
Day 2: You clarify, AI regenerates spec (2,300 tokens)
Day 3: New question, AI regenerates spec again (2,300 tokens)
Day 4: Another clarification, AI regenerates spec (2,300 tokens)
```

**Cost:** 3 regenerations x 2,300 = **6,900 extra tokens**

**Optimization:**
Batch clarifications:
```
Day 1: AI generates spec with [NEEDS CLARIFICATION] markers
Day 2: You collect ALL clarifications (research, ask stakeholders)
Day 3: Single spec regeneration with all answers (2,300 tokens)
```

**Cost:** 1 regeneration = **2,300 tokens (4,600 saved)**

**Pro Tip:** Use refinement sessions (see workflow-best-practices.md) to batch clarifications weekly.

### Token Optimization Checklist

**Before generating artifacts:**
- [ ] Do I have research.md for complex decisions? (Avoid repeating rationale)
- [ ] Is my constitution.md up-to-date with templates? (Reduce structure discovery)
- [ ] Can I batch multiple operations in one session? (Context reuse)

**When writing specs:**
- [ ] Focus on WHAT/WHY, not HOW (Save ~40% tokens)
- [ ] Use K-references for decisions (Save ~100 tokens per reference)
- [ ] Keep user stories concise (<15 words per acceptance criterion)

**During refinements:**
- [ ] Batch clarifications (Avoid multiple regenerations)
- [ ] Update spec once with all answers (Not piecemeal)

**Expected savings:**
- Small feature (10 tasks): 30-40% token reduction
- Medium feature (20 tasks): 40-50% token reduction
- Large feature (30+ tasks): 50%+ token reduction (more opportunities for K-references)

### Cost Tracking

**Track token usage per feature:**

**File:** `.specify/memory/specs/branch-042-auth/token-log.md`

```markdown
# Token Usage Log - User Authentication Feature

| Date | Operation | Input | Output | Total | Cost |
|------|-----------|-------|--------|-------|------|
| 2026-02-15 | /speckit.specify | 1,500 | 800 | 2,300 | $0.07 |
| 2026-02-16 | /speckit.plan | 2,000 | 1,200 | 3,200 | $0.10 |
| 2026-02-17 | /speckit.tasks | 1,800 | 1,000 | 2,800 | $0.09 |
| 2026-02-18 | Spec regeneration (clarifications) | 1,600 | 700 | 2,300 | $0.07 |
| **Total** | | **6,900** | **3,700** | **10,600** | **$0.33** |

**Optimization Applied:**
- ✅ K-references in tasks (saved ~1,000 tokens)
- ✅ Batched spec regeneration (saved ~4,600 tokens)
- ✅ Concise acceptance criteria (saved ~440 tokens)

**Comparison:**
- Without optimization: ~16,000 tokens (~$0.50)
- With optimization: ~10,600 tokens (~$0.33)
- **Savings: 34% (~$0.17 per feature)**
```

---

## Integration & Optimization Summary

### Quick Reference

**Jira Export:**
- Manual: 5-person teams, low churn
- Semi-automated: 5-20 person teams, moderate churn
- Fully automated: 20+ person teams, high churn

**Field Mapping:**
- Phase → Epic or Label
- Task → Story
- Acceptance Criteria → Sub-tasks or Checklist
- Effort → Story Points (S=2, M=5, L=8)

**Token Optimization:**
- Use K-references (save ~100 tokens per reference)
- Keep specs concise (save ~40% tokens)
- Batch operations (save ~40% via context reuse)
- Reuse templates (save ~200 tokens per spec)
- Cache with research.md (save ~300 tokens per decision)

**Expected Costs:**
- Small feature: $0.15-0.25 (optimized), $0.30-0.50 (unoptimized)
- Medium feature: $0.40-0.60 (optimized), $0.80-1.20 (unoptimized)
- Large feature: $0.80-1.20 (optimized), $1.50-2.50 (unoptimized)

---

## See Also

- [Workflow Best Practices FAQ](workflow-best-practices.md) - Daily workflows and refinements
- [Troubleshooting FAQ](troubleshooting.md) - Common issues and fixes
- [Basic Workflow Guide](../workflows/basic-workflow.md) - End-to-end Spec-Kit workflow
- [Constitution Reference](../docs/constitution-reference.md) - Template standards
