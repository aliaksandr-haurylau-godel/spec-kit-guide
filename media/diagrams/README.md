# Presentation Diagrams

This directory contains Mermaid diagrams used in the Spec-Kit presentation.

## Diagrams

### phase-workflow.mmd

**Used in**: Slide 13 (plan.md artifact)

**Purpose**: Illustrates the flow through implementation phases from constitutional pre-checks through release

**Rendering**:
- Most markdown renderers support Mermaid natively (GitHub, GitLab, VS Code with extensions)
- Online: [Mermaid Live Editor](https://mermaid.live/)
- CLI: `mmdc -i phase-workflow.mmd -o phase-workflow.svg`

**Color Coding**:
- **Orange** (#ff9800): Quality gates/checkpoints (Phase -1, research.md creation)
- **Blue** (#2196f3): Primary development work (Phase 1)
- **Green** (#4caf50): Validation/integration (Phase 2)
- **Purple** (#9c27b0): Deployment/release (Phase 3)
- **Red** (#f44336): Failure paths (Stop, fix violations)

**Flow Description**:
1. Start with Phase -1 (constitutional checks)
2. If gates fail → Stop and fix violations
3. If gates pass → Proceed to Phase 1 (implementation)
4. If research needed → Create research.md with K-decisions, then resume
5. Phase 1 complete → Move to Phase 2 (integration)
6. If validation fails → Return to Phase 1 for fixes
7. If validation passes → Proceed to Phase 3 (release)

## Usage in Presentations

### Markdown/Documentation
```markdown
```mermaid
![Phase Workflow](media/diagrams/phase-workflow.mmd)
```
```

### HTML
```html
<div class="mermaid">
  <!-- Paste diagram content here -->
</div>
<script src="https://cdn.jsdelivr.net/npm/mermaid/dist/mermaid.min.js"></script>
<script>mermaid.initialize({startOnLoad:true});</script>
```

### PowerPoint/Keynote
1. Render to SVG using Mermaid CLI or online editor
2. Import SVG into presentation
3. Alternative: Recreate using native shapes/SmartArt

### Google Slides
1. Use Mermaid extension (if available)
2. Or render to PNG/SVG and insert as image
3. Or recreate using native drawing tools

## Adding New Diagrams

When creating new diagrams:

1. Use `.mmd` extension for Mermaid source files
2. Follow color scheme defined above
3. Include comments explaining node types and flows
4. Add entry to this README with purpose and usage
5. Reference in presentation documents

## See Also

- [Mermaid Documentation](https://mermaid.js.org/)
- [Main Presentation Plan](../../docs/presentation-30min-sdd-spec-kit.md)
- [Slide Structure Guide](../../slides/30min-sdd-deck-structure.md)
