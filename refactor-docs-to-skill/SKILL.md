---
name: refactor-docs-to-skill
description: Audit, distill, and restructure raw documentation markdown files into a modular, token-efficient, and actionable agent skill directory.
---

# Refactor Documentation to Agent Skill

Transform verbose, verbatim documentation files into a high-density, structured skill optimized for LLM context windows and autonomous agent retrieval.

## Operating Principles

- **Zero Fluff:** Strip marketing prose, promotional filler, redundant narrative tutorials, and obsolete compatibility notices.
- **Actionable Density:** Retain exact syntax, signature definitions, core config options, gotchas, and minimal idiomatic code patterns.
- **Progressive Disclosure:** Keep entry-level guidance in the root file; offload detailed domain specifications to focused child modules.
- **Strict File Bounds:** Target a maximum size of 200–400 lines (or ~2,000 tokens) per child file. Split multi-topic files into dedicated leaf nodes.

## Target Structure

```text
skill-root/
├── SKILL.md                 # Primary entry point, workflow triggers, and section index
└── sections/                # Granular, modular reference files
    ├── setup.md             # Installation, environment, prerequisites
    ├── core-concepts.md     # Mental model, foundational types/abstractions
    ├── <feature>/           # Sub-directories for broad feature domains
    │   ├── _index.md        # Feature sub-index referencing child configs
    │   ├── basic.md         # Common implementations and patterns
    │   └── advanced.md      # Edge cases, performance tuning, full options
    └── troubleshooting.md   # Common errors, diagnostics, and failure modes
