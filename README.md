# Dhoom - AI Instructions

Organization-wide AI instruction management for agents, prompts, skills, and hooks.

## Structure

| Folder         | Purpose                                                    |
| -------------- | ---------------------------------------------------------- |
| `meta/`        | Cross-domain templates and reusable building blocks        |
| `domains/`     | Domain-specific instructions (dev, marketing, finance, hr) |
| `references/`  | eBooks, articles, and learning resources                   |
| `research/`    | Experiments, benchmarks, and insights                      |
| `governance/`  | Standards, review process, and ownership                   |
| `releases/`    | Changelogs and release notes                               |
| `src/tooling/` | CLI tools, generators, and validators                      |
| `docs/`        | Documentation and conventions                              |

## File Format

All assets use Markdown with YAML frontmatter:

```markdown
---
id: domain/asset-name
name: Asset Name
version: 1.0.0
owner: "@owner"
---

# Asset Title

Instructions here...
```

## Releases

Uses [Release Please](https://github.com/googleapis/release-please) for automated versioning with team notifications.
