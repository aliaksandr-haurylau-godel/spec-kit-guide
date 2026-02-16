---
version: 1
created_date: 2026-02-16
companion_doc: docs/presentation-30min-sdd-spec-kit.md
purpose: Slide-by-slide creation guide for Google Slides
style_guide: SLIDES_PROMPT.md
---

# 30-Minute SDD Presentation: Slide-by-Slide Breakdown

**Use with**: Google Slides + SLIDES_PROMPT.md style guide

---

## Design Constraints (From SLIDES_PROMPT.md)

### Text Limitations

- **Max 5 bullet points per content slide**
- **Max 15 words per bullet point**
- Avoid long paragraphs
- Use short, sharp phrases

### Layout Rules

- Embrace significant white/negative space
- Limit information blocks to 3-4 per slide
- Clear font hierarchy (titles, main content, supporting text)
- Professional graphics in dedicated spaces

### Visual Guidelines

- Use relevant graphics, charts, or icons
- Visuals support message (not decorative)
- Occupy dedicated space (16:9 or 1:1 placeholders)
- High contrast for readability

---

## Slide-by-Slide Creation Guide

### Section 1: The Problem & The Solution (Slides 1-6)

#### Slide 1: Title Slide

**Layout**: Title + Subtitle + Presenter Info + Date

**Text**:
- Title: "Spec-Driven Development with GitHub Spec-Kit"
- Subtitle: "From Idea to Production with AI-Assisted Workflows"
- [Your Name], [Your Title]
- [Date]

**Visual**: Split image showing:
- Left: Chaotic chat history (long conversation, confusion)
- Right: Clean spec.md document (organized structure)

**Design Notes**:
- Dark background (#1a1a1a) with light text (#f5f5f5)
- Center-aligned
- Logo/branding top-right corner
- Use arrow or transition graphic between chaos/order images

---

#### Slide 2: The Problem (Current State)

**Layout**: Title + 3 Bullets + Visual (right 40% of slide)

**Text**:
- Title: "The AI Agent Problem"
- Bullet 1: "Long chat sessions → context loss → drift" (9 words)
- Bullet 2: "No shared memory → agents repeat questions" (8 words)
- Bullet 3: "Unclear requirements → expensive rework cycles" (6 words)

**Visual**: Screenshot of long ChatGPT/Claude conversation
- Dimensions: 800x600px
- Placement: Right 40% of slide
- Annotations: Highlight box around confusion message
- File: `media/presentation-screenshots/01-ai-conversation-chaos.png`

**Design Notes**:
- Use red highlight color (#f44336) for confusion markers
- Left 60%: White space with bullets
- Font size: 24pt for bullets

---

#### Slide 3: Traditional Approaches Fall Short

**Layout**: Title + Comparison Table + Icons

**Text**:
- Title: "Why Traditional Methods Don't Work"
- Table (2 columns, 3 rows):

| Approach | Problem |
|----------|---------|
| Pure chat | Context loss, no memory |
| Waterfall docs | Too rigid, disconnected |
| Agile stories | Too granular, missing arch |

**Visual**: Three icons with X marks
- Icons: 💬 (chat), 📄 (docs), 📋 (stories)
- Red X overlay on each icon
- Placement: Above each table row

**Design Notes**:
- Table: 60% of slide width, centered
- Icons: 80px each, left-aligned with table rows
- Use red (#f44336) for X marks

---

#### Slide 4: Introducing Spec-Driven Development

**Layout**: Title + Definition Box + 3 Principles (with icons)

**Text**:
- Title: "Spec-Driven Development (SDD)"
- Definition: "AI agents and humans collaborate through **structured artifacts** capturing WHAT, WHY, and HOW." (14 words)
- Principle 1: 📋 **Specs before code** (WHAT & WHY)
- Principle 2: 🏗️ **Plans before implementation** (HOW)
- Principle 3: ✅ **Tasks with acceptance criteria** (steps)

**Visual**: Definition box with highlighted keywords
- Box: Light background (#2a2a2a), bordered
- Keywords: Bold "structured artifacts", "WHAT", "WHY", "HOW"

**Design Notes**:
- Definition box: Top 30% of slide
- Principles: 3 columns below, equal width
- Icons: 60px, above principle text
- Font: 22pt for principles

---

#### Slide 5: Enter GitHub Spec-Kit

**Layout**: Title + Subtitle + 4 Bullets + Visual (bottom)

**Text**:
- Title: "GitHub Spec-Kit: SDD Made Real"
- Subtitle: "Open-source framework from GitHub (2024)"
- Bullet 1: "CLI + slash commands for Copilot/Claude/Cursor" (8 words)
- Bullet 2: "Generates structured artifacts (spec, plan, tasks)" (7 words)
- Bullet 3: "Built-in templates & constitutional principles" (6 words)
- Bullet 4: "Production use: JetBrains, Skyscanner, internal tools" (7 words)

**Visual**: Terminal screenshot of `specify --help`
- Dimensions: 1000x400px
- Placement: Bottom 30% of slide
- File: `media/presentation-screenshots/02-speckit-cli-help.png`

**Design Notes**:
- Bullets: Top 60% of slide
- Terminal: Syntax-highlighted, monospace font
- Use GitHub logo/icon near title

---

#### Slide 6: The Spec-Kit Workflow (Visual)

**Layout**: Title + Flowchart (horizontal)

**Text**:
- Title: "The 6-Step Spec-Kit Workflow"
- Flowchart boxes (6 items):
  1. 🟢 `/speckit.constitution` → constitution.md
  2. 🔵 `/speckit.specify` → spec.md
  3. 🔵 `/speckit.clarify` → research.md (optional)
  4. 🟠 `/speckit.plan` → plan.md
  5. 🟠 `/speckit.analyze` → validation
  6. 🟣 `/speckit.tasks` → tasks.md

**Visual**: Flowchart with arrows and color coding
- Layout: Horizontal flow, left to right
- Arrows: Bold, connecting boxes
- Colors:
  - Green (#4caf50): Setup (constitution)
  - Blue (#2196f3): Specification (specify, clarify)
  - Orange (#ff9800): Planning (plan, analyze)
  - Purple (#9c27b0): Execution (tasks)

**Design Notes**:
- Boxes: 150px wide, 80px tall
- Font: 18pt for commands, 14pt for artifacts
- Use rounded corners on boxes
- Arrow width: 4px

---

### Section 2: The Core Artifacts (Slides 7-16)

#### Slide 7: Section Title

**Layout**: Large Title + Subtitle + Visual

**Text**:
- Title: "The Four Artifacts" (large, 48pt)
- Subtitle: "Real examples from production projects"

**Visual**: 4 document icons in row
- Icons: 📜 (constitution), 📋 (spec), 📐 (plan), ✅ (tasks)
- Size: 100px each
- Placement: Bottom 40% of slide, evenly spaced

**Design Notes**:
- Minimal text, maximum impact
- White space: 50% of slide
- Icons: Subtle animation (fade in) if presenting digitally

---

#### Slide 8: Artifact #1 – constitution.md

**Layout**: Title + Purpose + 3 Bullets + Visual (right side)

**Text**:
- Title: "constitution.md: Your Project's Immutable Laws"
- Purpose: "Architectural principles that govern ALL work" (7 words)
- Bullet 1: "Created once, versioned (like 1.2.2, 2.0.0)" (8 words)
- Bullet 2: "Contains NON-NEGOTIABLE rules (tests, licenses, patterns)" (7 words)
- Bullet 3: "Templates reference constitution via `sync_impact`" (6 words)

**Visual**: Screenshot of constitution frontmatter
- Source: `artifacts/constitution-example-backpack.md` (lines 1-10)
- Dimensions: 600x400px
- Placement: Right 40% of slide
- Annotations: Highlight `version: 1.0.2`
- File: `media/presentation-screenshots/03-constitution-frontmatter.png`

**Design Notes**:
- Use code block styling for `sync_impact`
- Highlight version number in green (#4caf50)

---

#### Slide 9: Constitution Evolution (Real Example)

**Layout**: Title + Side-by-Side Comparison + Arrow

**Text**:
- Title: "Constitution Evolution: POC → Production"
- Table (2 columns):

| Manifest v1.1.0 (POC) | Manifest v2.0.0 (MVP) |
|-----------------------|-----------------------|
| [DEFERRED] Testing | ✅ Tests REQUIRED |
| [DEFERRED] Performance | ✅ Perf metrics enforced |
| Security: Not mentioned | ✅ Article VI: Security |

**Visual**: Large arrow showing version jump
- Arrow: Center of slide, pointing right
- Text on arrow: "MAJOR BUMP"
- Color: Orange (#ff9800) for warning

**Design Notes**:
- Left column: Faded/gray text for "DEFERRED"
- Right column: Green checkmarks, bold text
- Table: 80% of slide width
- Font: 20pt for table text

---

#### Slide 10: Artifact #2 – spec.md

**Layout**: Title + Purpose + 4 Bullets + Split Visual (bottom)

**Text**:
- Title: "spec.md: The WHAT and WHY"
- Purpose: "Defines requirements WITHOUT prescribing implementation" (6 words)
- Bullet 1: "User Stories with acceptance criteria" (5 words)
- Bullet 2: "Functional/Non-functional requirements" (3 words)
- Bullet 3: "Constraints (in scope / out of scope)" (7 words)
- Bullet 4: "Success metrics" (2 words)

**Visual**: Split screenshot
- Left: `spec-example-taskify.md` excerpt (simple, 158 lines)
- Right: `spec-example-ideavim-api.md` excerpt (complex, 249 lines)
- Dimensions: Each 500x300px
- Placement: Bottom 40% of slide
- Labels: "Simple (158 lines)" | "Complex (249 lines)"
- File: `media/presentation-screenshots/04-spec-comparison.png`

**Design Notes**:
- Use vertical divider between screenshots
- Label font: 16pt, bold

---

#### Slide 11: spec.md in Action (Real Example)

**Layout**: Title + Pull Quote + 3 Bullets + Visual (right)

**Text**:
- Title: "Real Spec: IdeaVim API Layer"
- Pull Quote: 
  > "As an IdeaVim core developer, I want all extension functionality exposed through a dedicated API module, so that extensions depend only on this module and internal IdeaVim code can evolve freely."
- Bullet 1: "Project: JetBrains IdeaVim (10,000+ stars)" (6 words)
- Bullet 2: "Complexity: High (API design + migration)" (7 words)
- Bullet 3: "Companion artifact: research.md (418 lines)" (6 words)

**Visual**: Screenshot of User Story 1
- Source: `spec-example-ideavim-api.md` (lines 52-80)
- Dimensions: 700x500px
- Placement: Right 45% of slide
- Annotations: Highlight user story header
- File: `media/presentation-screenshots/05-ideavim-user-story.png`

**Design Notes**:
- Pull quote: Italic, slightly larger font (22pt)
- Quote box: Light border, indented
- Bullets: Below quote, left-aligned

---

#### Slide 12: Artifact #2.5 – research.md (Optional)

**Layout**: Title + Purpose + "When to create" list + K-example + Visual

**Text**:
- Title: "research.md: The Secret Weapon (20% of features)"
- Purpose: "Technical investigations & decision logs" (5 words)
- When to create:
  - "Multiple architectural approaches exist" (4 words)
  - "Platform constraints need documentation" (4 words)
  - "Trade-offs require explicit rationale" (4 words)
- K-numbered format:
  - K1: State Update Safety
  - K2: Functional vs Builder Patterns
  - K3: API Surface Scope

**Visual**: Screenshot of research.md K1 structure
- Source: `research-example-ideavim-api.md` (lines 53-80)
- Dimensions: 600x400px
- Placement: Right 40% of slide
- Annotations: Highlight "K1:", "Problem", "Decision", "Rationale" sections
- File: `media/presentation-screenshots/06-research-k-decision.png`

**Design Notes**:
- K-numbers: Monospace font, bold
- Use lightning bolt icon (⚡) near title for "Secret Weapon"

---

#### Slide 13: Artifact #3 – plan.md

**Layout**: Title + Purpose + 4 Bullets + Visual (bottom right)

**Text**:
- Title: "plan.md: The HOW"
- Purpose: "Implementation approach WITHOUT micro-tasks" (5 words)
- Bullet 1: "Technology stack + rationale" (4 words)
- Bullet 2: "System architecture (diagrams, data models)" (5 words)
- Bullet 3: "Phases with gates (Phase -1: constitutional checks)" (8 words)
- Bullet 4: "Risk & complexity tracking" (4 words)

**Visual**: Screenshot of Phase structure
- Source: `plan-example-backpack-nx.md` (search "## Phase")
- Dimensions: 700x300px
- Placement: Bottom 40% of slide
- File: `media/presentation-screenshots/07-plan-phases.png`

**Design Notes**:
- Highlight "Phase -1" in orange (#ff9800)
- Use gear icon (⚙️) near title

---

#### Slide 14: plan.md Example (Infrastructure)

**Layout**: Title + Context + Pull Quote + 3 Bullets + Visual

**Text**:
- Title: "Real Plan: Backpack Nx Migration"
- Context: "Skyscanner Backpack design system (95+ packages)" (7 words)
- Pull Quote:
  > "Reorganize project structure from flat packages/ to domain-grouped structure under libs/"
- Bullet 1: "Type: Infrastructure (not user-facing)" (5 words)
- Bullet 2: "Effort: Large (2 weeks), Complexity: Small" (7 words)
- Bullet 3: "Phases: Preparation → Migration → Validation" (5 words)

**Visual**: Phase breakdown diagram
- Show 3 boxes: Preparation | Migration | Validation
- Arrows connecting boxes
- Brief description under each
- Dimensions: 900x250px
- Placement: Bottom 35% of slide

**Design Notes**:
- Pull quote: Monospace font for paths
- Phase boxes: Same style as Slide 6 flowchart

---

#### Slide 15: Artifact #4 – tasks.md

**Layout**: Title + Purpose + 4 Bullets + Visual (right)

**Text**:
- Title: "tasks.md: The Executable Breakdown"
- Purpose: "Actionable tasks with dependencies & acceptance" (6 words)
- Bullet 1: "Grouped by plan phases" (4 words)
- Bullet 2: "Dependencies (K-decisions, other tasks)" (5 words)
- Bullet 3: "Effort estimates (S/M/L)" (4 words)
- Bullet 4: "Acceptance criteria (testable)" (3 words)

**Visual**: Screenshot of Task 1.1
- Source: `tasks-example-ideavim-api.md` (find Task 1.1)
- Dimensions: 700x500px
- Placement: Right 45% of slide
- Annotations: Highlight "Dependencies:", "Acceptance Criteria:"
- File: `media/presentation-screenshots/08-task-example.png`

**Design Notes**:
- Use checkbox icon (✅) near title
- Highlight S/M/L with color coding (Green/Orange/Red)

---

#### Slide 16: The Artifact Relationship (Visual Summary)

**Layout**: Title + Flowchart + 3 Traceability Examples

**Text**:
- Title: "How Artifacts Connect"
- Flowchart (vertical):
  ```
  constitution.md (v1.2.2)
       ↓
  spec.md (sync_impact: 1.2.2)
       ↓ (if complex)
  research.md (K1, K2, K3)
       ↓
  plan.md (references K1, K2)
       ↓
  tasks.md (dependencies: K1, K2)
  ```
- Traceability examples:
  - "Plan references spec: `companion_spec`" (4 words)
  - "Tasks cite K-decisions: 'Dependencies: K2'" (5 words)
  - "Spec tracks constitution: `sync_impact: 1.2.2`" (5 words)

**Visual**: Create flowchart diagram
- Boxes: 250px wide, 60px tall
- Arrows: Labeled with relationship type
- Color code boxes by artifact type
- Optional annotations on arrows

**Design Notes**:
- Center flowchart, left 50% of slide
- Traceability examples: Right 45%, bulleted
- Use link icon (🔗) for connections

---

### Section 3: Practical Application (Slides 17-21)

#### Slide 17: Section Title

**Layout**: Large Title + Subtitle + Visual

**Text**:
- Title: "Putting It Into Practice" (48pt)
- Subtitle: "DOs, DON'Ts, and Tricks from Production"

**Visual**: Large checklist icon (✅)
- Size: 150px
- Placement: Center of slide, below subtitle
- Optional: Green/red split background (subtle)

**Design Notes**:
- Minimal text, breather slide
- White space: 60% of slide

---

#### Slide 18: DOs and DON'Ts (Part 1: Specs)

**Layout**: Title + Two-Column Table

**Text**:
- Title: "Spec Best Practices"
- Table:

| ✅ DO | ❌ DON'T |
|-------|----------|
| Use [NEEDS CLARIFICATION] markers | Guess missing requirements |
| Track dated Q&A sessions | Bury decisions in chat |
| Define out-of-scope explicitly | Assume "obvious" scope |

**Visual**: Green checkmarks (✅) and red X marks (❌)
- Size: 40px
- Placement: In table cells, before text

**Design Notes**:
- Table: 90% of slide width, centered
- Left column: Light green background (#e8f5e9)
- Right column: Light red background (#ffebee)
- Font: 22pt for table text
- Cell padding: Generous (20px)

---

#### Slide 19: DOs and DON'Ts (Part 2: Plans & Tasks)

**Layout**: Title + Two-Column Table

**Text**:
- Title: "Planning Best Practices"
- Table:

| ✅ DO | ❌ DON'T |
|-------|----------|
| Use Phase -1 gates | Skip pre-checks until PR |
| Reference K-decisions by number | Copy/paste rationale |
| Update sync_impact when regen | Ignore version bumps |

**Visual**: Same style as Slide 18

**Design Notes**:
- Same layout as Slide 18 for consistency
- Use code formatting for `Phase -1`, `sync_impact`, `K-decisions`

---

#### Slide 20: Common Pitfalls

**Layout**: Title + 3 Numbered Items (with sub-bullets)

**Text**:
- Title: "3 Common Mistakes (and How to Avoid Them)"
- Items:
  1. **Over-specifying in spec.md**
     - Symptom: Spec includes implementation details
     - Fix: Move "how" to plan.md
  2. **Under-using research.md**
     - Symptom: Plan has 5 pages of justification
     - Fix: Extract to research.md with K-numbers
  3. **Ignoring constitution evolution**
     - Symptom: Old branches fail CI after bump
     - Fix: Use SYNC IMPACT reports

**Visual**: Warning icon (⚠️) near title
- Size: 60px
- Color: Orange (#ff9800)

**Design Notes**:
- Numbers: Large, bold (36pt), colored
- Symptoms: Italic, gray text
- Fixes: Bold, green text
- Spacing: Generous between items

---

#### Slide 21: Pro Tips & Tricks

**Layout**: Title + 5 Numbered Tips (with lightning bolt icons)

**Text**:
- Title: "5 Pro Tips from Production"
- Tips:
  1. ⚡ Start with lightweight constitution (add later)
  2. ⚡ Use `effort: S/M/L` + `complexity` separately
  3. ⚡ Mark NON-NEGOTIABLE sparingly (critical only)
  4. ⚡ Create research.md when re-arguing decisions
  5. ⚡ Version bump conservatively (unsure = higher)

**Visual**: Lightning bolt icons (⚡)
- Size: 40px
- Placement: Before each tip number

**Design Notes**:
- Tips: Numbered 01-05 (two digits, bold)
- Font: 22pt for tips
- Background: Subtle gradient (dark to darker)
- Code terms: Monospace (`effort:`, `complexity`)

---

### Section 4: Demo & Next Steps (Slides 22-24)

#### Slide 22: Quick Demo (Hybrid)

**Layout**: Title + Side-by-Side Screenshots

**Text**:
- Title: "Demo: /speckit.specify in Action"
- Label left: "Command Input"
- Label right: "Generated Output"

**Visual**: Two screenshots
- Left: Terminal with command `/speckit.specify Build a simple Todo App`
- Right: Generated spec.md excerpt (frontmatter + overview)
- Dimensions: Each 600x450px
- Placement: Left/right split, 50/50
- Files:
  - `media/presentation-screenshots/09-demo-input.png`
  - `media/presentation-screenshots/10-demo-output.png`

**Design Notes**:
- Use vertical divider between screenshots
- Add arrow from left to right (process flow)
- Labels: 18pt, above each screenshot
- Optional: If doing live demo, use this as backup slide

---

#### Slide 23: Getting Started

**Layout**: Title + 3-Step Checklist + Code Blocks

**Text**:
- Title: "Try It Monday Morning"
- Steps:
  1. 🛠️ **Install**
     ```bash
     uv tool install specify-cli --from git+https://github.com/github/spec-kit.git
     ```
  2. ⚙️ **Initialize**
     ```bash
     specify init --here --ai claude
     ```
  3. 🚀 **First feature**
     ```
     /speckit.specify <your simplest backlog item>
     ```

**Visual**: Icons (🛠️ ⚙️ 🚀)
- Size: 50px
- Placement: Before each step number

**Design Notes**:
- Code blocks: Dark background (#2a2a2a), syntax highlighted
- Font: Fira Code or Monaco (monospace), 18pt
- Blocks: 90% of slide width
- Generous padding in code blocks (15px)

---

#### Slide 24: Resources

**Layout**: Title + 3 Resource Categories + QR Code (optional)

**Text**:
- Title: "Learn More"
- Resources:
  - 📖 **Official repo**: `github.com/github/spec-kit`
  - 📂 **This guide**: [Your repository URL]
  - 💬 **Real examples**:
    - JetBrains IdeaVim: `artifacts/spec-example-ideavim-api.md`
    - Skyscanner Backpack: `artifacts/constitution-example-backpack.md`
    - Manifest CLI: `artifacts/constitution-example-manifest.md`

**Visual**: QR code (if in-person)
- Size: 200x200px
- Placement: Bottom right corner
- Links to: Your guide repository

**Design Notes**:
- Icons: 50px, before each category
- URLs: Monospace font, slightly smaller (18pt)
- Bullets: Indented under each category
- QR code: Optional, only if presenting in-person

---

### Section 5: Q&A (Slide 25)

#### Slide 25: Q&A

**Layout**: Title + Large Icon + Contact Info

**Text**:
- Title: "Questions?"
- Subtitle (optional): "Let's discuss how Spec-Kit can help your team"
- Contact info:
  - Email: [your.email@company.com]
  - Slack: #ai-research (or relevant channel)
  - Guide repo: [shortened URL]

**Visual**: Large Q&A icon or question mark
- Size: 200px
- Placement: Center of slide
- Color: Blue (#2196f3)

**Design Notes**:
- Minimal text, inviting
- White space: 60% of slide
- Font: Contact info 22pt, gray
- Optional call-to-action: "Join AI & RnD Community"

---

## Screenshot Specifications

### Screenshot 1: AI Conversation Chaos

**File**: `media/presentation-screenshots/01-ai-conversation-chaos.png`

**Source**: ChatGPT or Claude chat export

**Dimensions**: 800x600px

**Content**:
- Long conversation (20+ messages visible)
- Highlight a message showing confusion: "Wait, what did we decide about authentication?"
- Blur any sensitive/proprietary content

**Annotations**:
- Red highlight box around confusion message
- Optional: Add label "Context Lost"

---

### Screenshot 2: Spec-Kit CLI Help

**File**: `media/presentation-screenshots/02-speckit-cli-help.png`

**Source**: Terminal running `specify --help`

**Dimensions**: 1000x400px

**Content**:
- Full help output showing available commands
- Syntax-highlighted (use VS Code terminal or similar)

**Annotations**: None needed

---

### Screenshot 3: Constitution Frontmatter

**File**: `media/presentation-screenshots/03-constitution-frontmatter.png`

**Source**: `artifacts/constitution-example-backpack.md` (lines 1-10)

**Dimensions**: 600x400px

**Content**:
```yaml
---
version: 1.0.2
constitution_version: 1.0.2
previous_version: 1.0.1
---
```

**Annotations**:
- Green highlight box around `version: 1.0.2`
- Optional label: "Version Tracking"

---

### Screenshot 4: Spec Comparison

**File**: `media/presentation-screenshots/04-spec-comparison.png`

**Source**: 
- Left: `artifacts/spec-example-taskify.md` (lines 20-50)
- Right: `artifacts/spec-example-ideavim-api.md` (lines 50-80)

**Dimensions**: Combined 1000x300px (500px each)

**Layout**: Side-by-side

**Labels**:
- Left: "Simple (158 lines)" - above screenshot
- Right: "Complex (249 lines)" - above screenshot

**Annotations**:
- Vertical divider line between screenshots
- Use same font/styling for both

---

### Screenshot 5: IdeaVim User Story

**File**: `media/presentation-screenshots/05-ideavim-user-story.png`

**Source**: `artifacts/spec-example-ideavim-api.md` (lines 52-80)

**Dimensions**: 700x500px

**Content**: Full User Story 1 with acceptance criteria

**Annotations**:
- Highlight "User Story 1:" header in blue
- Box around acceptance criteria section

---

### Screenshot 6: Research K-Decision

**File**: `media/presentation-screenshots/06-research-k-decision.png`

**Source**: `artifacts/research-example-ideavim-api.md` (lines 53-80)

**Dimensions**: 600x400px

**Content**: K1 structure showing Problem, Decision, Rationale

**Annotations**:
- Highlight "K1:", "Problem", "Decision", "Rationale" headers
- Use different colors for each section (e.g., orange for K1, blue for sections)

---

### Screenshot 7: Plan Phases

**File**: `media/presentation-screenshots/07-plan-phases.png`

**Source**: `artifacts/plan-example-backpack-nx.md` (search for "## Phase")

**Dimensions**: 700x300px

**Content**: Phase structure showing Phase -1, Phase 1, Phase 2

**Annotations**:
- Highlight "Phase -1" in orange
- Box around constitutional gates

---

### Screenshot 8: Task Example

**File**: `media/presentation-screenshots/08-task-example.png`

**Source**: `artifacts/tasks-example-ideavim-api.md` (find Task 1.1)

**Dimensions**: 700x500px

**Content**: Complete Task 1.1 block with:
- Task description
- Dependencies
- Effort estimate
- Acceptance criteria

**Annotations**:
- Highlight "Dependencies:" in red
- Highlight "Acceptance Criteria:" in green
- Box around entire task

---

### Screenshots 9-10: Demo Input/Output

**Files**:
- `media/presentation-screenshots/09-demo-input.png`
- `media/presentation-screenshots/10-demo-output.png`

**Source**: Run `/speckit.specify Build a simple Todo App` in test environment

**Dimensions**: Each 600x450px

**Content**:
- Input: Terminal with command typed
- Output: Generated spec.md first 20 lines (frontmatter + overview)

**Annotations**:
- Arrow from input to output (if combining into one image)
- Labels: "Input" and "Output"

---

### Screenshot 11: Artifact Relationship (Optional)

**File**: `media/presentation-screenshots/11-artifact-flowchart.png`

**Source**: Create in Google Slides or Lucidchart

**Dimensions**: 800x600px

**Content**: Flowchart showing:
- constitution.md → spec.md → research.md → plan.md → tasks.md
- Annotations showing relationships (sync_impact, K-numbers, companion_spec)

**Style**:
- Use color coding from Slide 6
- Boxes: Rounded corners
- Arrows: Labeled with relationship type

---

## Color Palette

### Primary Colors

- **Background**: #1a1a1a (dark gray)
- **Text**: #f5f5f5 (off-white)
- **Code Background**: #2a2a2a (darker gray)

### Accent Colors (Workflow Phases)

- **Green (Setup)**: #4caf50
- **Blue (Specification)**: #2196f3
- **Orange (Planning)**: #ff9800
- **Purple (Execution)**: #9c27b0

### Status Colors

- **Success**: #4caf50 (green)
- **Error**: #f44336 (red)
- **Warning**: #ffc107 (yellow)
- **Info**: #2196f3 (blue)

### UI Elements

- **Highlight Background**: #2a2a2a
- **Border**: #424242
- **Table Header**: #333333
- **Table Row (even)**: #1e1e1e
- **Table Row (odd)**: #2a2a2a

---

## Font Specifications

### Typefaces

**Headings**: Roboto or Inter (sans-serif)
- Slide titles: 36pt, Bold, #f5f5f5
- Section titles: 28pt, Bold, #f5f5f5

**Body Text**: Roboto or Inter
- Bullets: 24pt, Regular, #e0e0e0
- Sub-bullets: 20pt, Regular, #cccccc
- Supporting text: 18pt, Regular, #aaaaaa

**Code**: Fira Code or Monaco (monospace)
- Code blocks: 18pt, Regular, #f5f5f5
- Inline code: 20pt, Regular, #4caf50 (green tint)

### Hierarchy

- Main titles: 36pt
- Section titles: 28pt
- Bullet points: 24pt
- Sub-bullets: 20pt
- Code: 18pt
- Captions/labels: 16pt

---

## Timing Annotations

### Per-Slide Timing Goals

**Section 1** (Slides 1-6): 5 minutes total
- Slide 1: 30 seconds
- Slide 2: 60 seconds
- Slide 3: 45 seconds
- Slide 4: 60 seconds
- Slide 5: 45 seconds
- Slide 6: 60 seconds

**Section 2** (Slides 7-16): 12 minutes total
- Slide 7: 20 seconds (breather)
- Slides 8-16: 70-80 seconds each

**Section 3** (Slides 17-21): 5 minutes total
- Slide 17: 20 seconds (breather)
- Slides 18-21: 70 seconds each

**Section 4** (Slides 22-24): 3 minutes total
- Slide 22: 60 seconds (or 90 if live demo)
- Slide 23: 60 seconds
- Slide 24: 60 seconds

**Section 5** (Slide 25): 5 minutes (Q&A)

### Total: 30 minutes

### Checkpoint Markers

Add subtle timing indicators in presenter notes:
- ⏱️ "Must reach by 5:00" (Slide 6)
- ⏱️ "Must reach by 12:00" (Slide 10)
- ⏱️ "Must reach by 20:00" (Slide 18)
- ⏱️ "Start Q&A by 25:00" (Slide 25)

---

## Accessibility Checklist

### Color Contrast

- [ ] All text has ≥4.5:1 contrast ratio (WCAG AA)
- [ ] No critical information conveyed by color alone
- [ ] Red-green combinations avoided (colorblind-friendly)

### Typography

- [ ] Font size ≥24pt for body text (readable from back of room)
- [ ] Clear font hierarchy (3 levels max)
- [ ] Sans-serif fonts for readability

### Visual Elements

- [ ] Alt text on all images (for screen readers)
- [ ] Icons accompanied by text labels
- [ ] Diagrams have text descriptions

### Code Blocks

- [ ] Syntax highlighting (not plain text)
- [ ] High contrast background (#2a2a2a vs #f5f5f5)
- [ ] Monospace font (Fira Code or Monaco)
- [ ] Font size ≥18pt

### Animations

- [ ] Minimal animations (no distracting transitions)
- [ ] No auto-advancing slides
- [ ] Motion can be disabled if needed

---

## Export Settings (Google Slides)

### For Presentation

- **Resolution**: 1920x1080 (16:9)
- **Format**: Google Slides native format
- **Backup**: Export as PDF

### For Sharing

- **PDF**: Standard quality, all slides
- **Images**: Export key slides as PNG (for documentation)

### Presenter View

- Enable presenter notes
- Display timer
- Preview next slide

---

## Practice Guidelines

### Rehearsal Schedule

**1 Week Before**:
- Create all slides
- Capture all screenshots
- Write detailed speaker notes

**3 Days Before**:
- Full run-through with timer (aim for 24 minutes)
- Record yourself (audio or video)
- Get feedback from colleague

**1 Day Before**:
- Final run-through
- Verify all links
- Test demo (if doing live)
- Print Q&A answers on index card

**Morning Of**:
- Test screen sharing (if virtual)
- Test projector (if in-person)
- Open deck in presentation mode
- Have backup PDF ready

### Timing Practice

Use these checkpoints during practice:
- 5:00 → Should be finishing Slide 6
- 12:00 → Should be on Slide 10
- 17:00 → Should be finishing Slide 16
- 20:00 → Should be on Slide 18
- 25:00 → Should be starting Q&A

If running over, cut:
- Slide 9 (Constitution Evolution) - save for Q&A
- Slide 14 (Backpack Plan example) - keep only IdeaVim
- Reduce speaker elaboration on Slides 18-21

---

## Backup Plans

### If Demo Fails

- Have static screenshots ready (Slide 22)
- Say: "Let me show you the pre-captured result"
- Never apologize excessively - move on quickly

### If Running Long

- Skip Slide 9 (Constitution Evolution)
- Skip Slide 14 (Backpack Plan)
- Reduce Q&A to 3 minutes

### If Running Short

- Elaborate on Slide 12 (research.md) with more K-decision examples
- Add extra Q&A time
- Share additional tips on Slide 21

### Technical Issues

- Have PDF backup on USB drive
- Have screenshots in local folder (not cloud-dependent)
- Have printed notes (if screen sharing fails)

---

## Post-Presentation Checklist

### Immediately After

- [ ] Share slide deck link in chat/email
- [ ] Share guide repository URL
- [ ] Collect feedback (survey or informal)

### Within 24 Hours

- [ ] Send follow-up email with resources
- [ ] Answer outstanding questions
- [ ] Share recording (if virtual and recorded)

### Within 1 Week

- [ ] Follow up with attendees: "Have you tried Spec-Kit?"
- [ ] Address common questions in guide FAQ
- [ ] Update presentation based on feedback

---

## See Also

- [Main Presentation Plan](../docs/presentation-30min-sdd-spec-kit.md) - Speaker notes and content
- [SLIDES_PROMPT.md](../SLIDES_PROMPT.md) - Corporate presentation style guide
- [Artifact Examples](../artifacts/README.md) - Source material for screenshots
- [Constitution Evolution FAQ](../faq/constitution-evolution.md) - Deep dive on versioning
