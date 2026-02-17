# Presentation Visual Content

**Status**: Screenshots replaced with rich textual content and diagrams

## Overview

The presentation slides have been redesigned to focus on **detailed content specifications** rather than static screenshots. This approach provides:

- **More useful information**: Comprehensive details about what to include in each artifact
- **Easier maintenance**: Content updates don't require recreating images
- **Better accessibility**: Text-based content is searchable and screen-reader friendly
- **Flexible implementation**: Content can be adapted to any presentation format

## Content Approach by Slide

### Slides Using Detailed Textual Content (No Screenshots)

**Slide 2: AI Agent Problem**
- **Content**: Conversation timeline description with context degradation examples
- **Location**: `docs/presentation-30min-sdd-spec-kit.md` (lines 61-91)
- **Implementation**: Can be presented as diagram, timeline, table, or text

**Slide 5: Enter Spec-Kit**
- **Content**: Installation commands, system requirements, quick start guide
- **Location**: `docs/presentation-30min-sdd-spec-kit.md` (lines 115-173)
- **Implementation**: Code blocks with syntax highlighting

**Slide 8: constitution.md**
- **Content**: Detailed article structure (I-V, X+), version semantics, examples
- **Location**: `docs/presentation-30min-sdd-spec-kit.md` (lines 170-305)
- **Implementation**: Structured sections with examples from real artifacts

**Slide 10: spec.md**
- **Content**: Complete requirements for each spec section, quality checklist
- **Location**: `docs/presentation-30min-sdd-spec-kit.md` (lines 310-485)
- **Implementation**: Comprehensive requirements guide with examples

**Slide 11: spec.md Examples**
- **Content**: 6 unique features from IdeaVim + Backpack patterns
- **Location**: `docs/presentation-30min-sdd-spec-kit.md` (lines 487-640)
- **Implementation**: Detailed feature analysis with lessons learned

**Slide 22: Demo**
- **Content**: Minimal - just title with decorative graphic
- **Location**: `docs/presentation-30min-sdd-spec-kit.md` (lines 456-467)
- **Implementation**: Simple visual, focus on live demo

### Slides Using Diagrams + Textual Descriptions

**Slide 13: plan.md (Phase Workflow)**
- **Content**: Phase descriptions + Mermaid flowchart + diagram explanation
- **Location**: 
  - Content: `docs/presentation-30min-sdd-spec-kit.md` (lines 287-400)
  - Diagram: `media/diagrams/phase-workflow.mmd`
- **Implementation**: Render Mermaid diagram alongside textual phase descriptions

**Slide 16: Artifact Relationships**
- **Content**: Traceability flowchart (constitution → spec → research → plan → tasks)
- **Location**: `docs/presentation-30min-sdd-spec-kit.md` (lines 332-360)
- **Original diagram**: Already specified in presentation document
- **Implementation**: Mermaid diagram can be rendered in presentation tools

### Slides Unchanged (Keep Original Approach)

**Slide 12: research.md**
- Uses excerpt from `artifacts/research-example-ideavim-api.md`
- Shows K-decision structure

**Slide 15: tasks.md**
- Uses excerpt from `artifacts/tasks-example-ideavim-api.md`
- Shows task structure with dependencies

## Directory Structure

```
media/
├── diagrams/                    # NEW: Mermaid diagrams
│   └── phase-workflow.mmd       # Phase flow for Slide 13
└── presentation-screenshots/    # DEPRECATED: No longer creating screenshots
    └── README.md               # This file

artifacts/                       # Reference files for content
├── constitution-example-backpack.md
├── spec-example-ideavim-api.md
├── spec-example-backpack-nx.md
├── research-example-ideavim-api.md
├── plan-example-backpack-nx.md
└── tasks-example-ideavim-api.md
```

## Benefits of New Approach

### Advantages
- ✅ **Always fresh**: References real artifact files that are version-controlled
- ✅ **More detailed**: Can include comprehensive information not visible in screenshots
- ✅ **Easier updates**: Change markdown files, not regenerate images
- ✅ **Better UX**: Audience can copy/paste code examples
- ✅ **Accessible**: Screen readers can parse content
- ✅ **Smaller size**: Markdown files vs PNG images
- ✅ **Format agnostic**: Content works in slides, docs, web, PDF

### What Changed
- ❌ **No more screenshots**: Slides 2, 3, 5, 8, 10, 11, 22 use text/code/examples instead
- ➕ **Added diagrams**: Slide 13 (phase workflow), Slide 16 (artifact relationships)
- ✅ **Kept excerpts**: Slides 12, 15 reference actual artifact files

## Creating Presentations

When building presentation from these specifications:

1. **Read content from**: `docs/presentation-30min-sdd-spec-kit.md`
2. **Follow structure from**: `slides/30min-sdd-deck-structure.md`
3. **Reference artifacts in**: `artifacts/` directory
4. **Render diagrams from**: `media/diagrams/*.mmd` (Mermaid format)

### Presentation Format Options

**Slides (PowerPoint, Keynote, Google Slides)**:
- Use formatted code blocks for commands/examples
- Create diagrams from Mermaid source or hand-draw
- Break detailed content into progressive reveal/sections

**Documentation (Markdown, HTML)**:
- Content already in markdown format
- Diagrams render automatically (if Mermaid supported)
- Can include expandable sections for details

**Web Presentation (Reveal.js, Spectacle)**:
- Markdown content works directly
- Mermaid diagrams render natively
- Interactive features possible (tabs, accordions)

## See Also

- [Main Presentation Plan](../../docs/presentation-30min-sdd-spec-kit.md) - Full content specifications
- [Slide Structure Guide](../../slides/30min-sdd-deck-structure.md) - Detailed layout guidance
- [Artifact Examples](../../artifacts/README.md) - Source material for examples
