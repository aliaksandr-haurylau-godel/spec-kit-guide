---
name: research-indexer
description: Organize and index raw research data in the raw_data directory, ensuring consistent folder structure and updating the index file.
---

## Context
This skill helps you organize and index raw research data in the `raw_data/` directory.

## Protocols

### 1. Storage Structure
- Create a new directory inside `raw_data/` with the following format:
  `raw_data/YYYY-MM-DD-<topic-name>/`
- Save the raw content (markdown, json, images) within that folder.
- If appropriate, add a local `README.md` inside that folder summarizing the specific research session.

### 2. Indexing Requirement
- You MUST update `raw_data/README.md` after saving new data.
- Append new entries to the top of the "Research Sessions" list (reverse chronological order).
- Do NOT use tables. Use the following list format:

```markdown
### [YYYY-MM-DD] <Topic Name>
- **Folder**: [Link](./YYYY-MM-DD-<topic-name>)
- **Goal**: <Brief description of what was researched>
- **Results**: <Key findings or "Raw data dump">
- **Sources**:
  - [Title](URL)
```

### 3. Usage Rules
- Never modify existing data in `raw_data` unless correcting a format error.
- Always prefer `YYYY-MM-DD` format for dates to ensure proper sorting.
- If the raw data is extremely large, consider summarizing or truncating it before saving, but mark it clearly as truncated.
