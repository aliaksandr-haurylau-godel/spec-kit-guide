# Raw Data Research Log

This directory contains unprocessed, unsorted, and unapproved research data.
**WARNING**: Data in this directory should not be used as a source of truth unless explicitly requested.

## Research Sessions

### [2026-02-12] spec-kit-official-docs
- **Folder**: [Link](./2026-02-12-spec-kit-official-docs)
- **Goal**: Gather official documentation for the GitHub Spec-Kit framework.
- **Results**: Downloaded official markdown documentation (README, spec-driven, docs folder) and images from the github/spec-kit repository.
- **Sources**:
  - [github/spec-kit](https://github.com/github/spec-kit)

### [2026-02-15] real-world-spec-kit-artifacts
- **Folder**: [2026-02-15-real-world-spec-kit-artifacts](./2026-02-15-real-world-spec-kit-artifacts)
- **Goal**: Collect real-world Spec-Kit artifacts from production projects to validate framework usage patterns and provide authentic examples for our guide.
- **Results**: Downloaded complete artifact sets from 3 production projects spanning different domains and maturity levels:
  
  **Projects:**
  1. **JetBrains IdeaVim** (Open-source IDE plugin, ~10K+ GitHub stars)
     - Branch `001-api-layer`: Complete artifact set with rare `research.md`
     - Files: spec.md (249L), plan.md (307L), tasks.md (271L), research.md (418L)
     - Constitution v1.2.2 (225L) with SYNC IMPACT version tracking
     - Domain: IDE plugin development, API design, large refactoring
     
  2. **Skyscanner Backpack** (Design system, enterprise scale)
     - 3 Pull Requests: #4141, #4153, #4218
     - Selected PR-4153 for artifacts: spec.md (355L), plan.md (480L), tasks.md (261L)
     - Constitution v1.0.2 (561L) with NON-NEGOTIABLE markers for design system rules
     - Domain: Design system components, infrastructure/tooling (Nx migration)
     
  3. **Manifest Generator** (CLI tool, internal project)
     - Constitution only: v2.0.0 (249L) showing POC → Production transition
     - Major version bump (v1.1.0 → v2.0.0) demonstrates constitution evolution
     - Domain: Code generation, SOLID principles, MVP quality standards

- **Key Patterns Discovered**:
  - **SYNC IMPACT Reports**: All 3 projects use standardized version change tracking in constitution frontmatter
  - **Research Documents**: IdeaVim uses K-numbered decision logs (K1-K6) for complex architectural choices
  - **Clarifications Sections**: Backpack specs include Q&A sessions with dates and context
  - **Metadata Standards**: Effort (S/M/L), Complexity, Time estimates, Branch/PR references
  - **Constitution Evolution**: Patch fixes (template improvements), minor changes (process updates), major versions (POC→Production)

- **Sources**:
  - [JetBrains/ideavim](https://github.com/JetBrains/ideavim) - Public open-source
  - [Skyscanner/backpack](https://github.com/Skyscanner/backpack) - Public open-source
  - Manifest Generator - Internal project (sanitized)

- **Data Quality**: Production-ready artifacts with 1000+ lines each, showing real usage patterns and evolution over time.

### [YYYY-MM-DD] Example Topic
- **Folder**: [Link](./2025-01-01-example)
- **Goal**: Example goal description.
- **Results**: Example results summary.
- **Sources**:
  - [Title](URL)

---
