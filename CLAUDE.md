# OctoHub Client Customizations - GPStrategies Government Solutions

This repository contains client-specific customizations for GPStrategies Government Solutions.

## Rules & Patterns - Single Source of Truth

**All customization rules live in octohub-core's customizations CLAUDE.md.** Read that file for:
- Directory structure & symlink patterns
- Feature registration (manifest.yaml, __init__.py, index.ts)
- YAML vs JSONB configuration pattern
- Base class imports
- RLS context setup (async and sync)
- Frontend patterns (TaskProgressModal, SSE streaming, components)
- Client-specific database schemas & migrations
- Progress tracking (TaskProgressTracker)
- Logging patterns
- Dependency management

**Customization rules (unanet):** `/home/turn10innovations/projects/octohub-core/backend/plugins/unanet/customizations/CLAUDE.md`
**Core platform rules:** `/home/turn10innovations/projects/octohub-core/CLAUDE.md`

Do NOT duplicate those rules here. If a rule needs updating, update it in octohub-core.

## Repository Info

| Field | Value |
|-------|-------|
| **Client Name** | `gpgov` |
| **Company Slug** | `gpgov` |
| **ERP System(s)** | `unanet` |
| **Core Repo** | `octohub-core` |

## Directory Structure

```
backend/plugins/unanet/customizations/
├── config.yaml
├── shared/                              # Shared extensions (all companies)
│   ├── automations/
│   ├── reports/
│   ├── hooks/
│   └── transformers/
└── companies/
    └── gpgov/                   # Company slug = directory name
        ├── config.yaml                  # Company binding (octohub_co_id)
        ├── features/
        ├── automations/
        ├── reports/
        ├── hooks/
        └── transformers/

frontend/src/plugins/unanet/customizations/
└── companies/
    └── gpgov/
        └── features/
```

## Symlink Setup

From octohub-core, `switch-client.sh gpgov` creates:
- `octohub-core/.../customizations/companies/` -> this repo's `companies/`

The `shared/` directory in octohub-core is NOT symlinked - it belongs to core.
Client features go under `companies/gpgov/features/`.

## Features

*No features yet. See the customizations CLAUDE.md for how to create one.*

## Development

```bash
# Switch to this client (from octohub-core)
./scripts/switch-client.sh gpgov
```

## Dependencies

- **Python:** `pyproject.toml` + `uv.lock` (client-specific packages, pinned)
- **Frontend:** `frontend/package.json` + `frontend/package-lock.json` (pinned)

Never add client-specific packages to octohub-core's dependency files. They live
here, in this repo, backend and frontend alike.

**Commit your lockfiles.** A dependency declared without one is a dependency that
re-resolves at deploy time, and a deployed box then runs versions nobody built or
tested. That is not hypothetical: an unconstrained install on a deploy path pulled
a newer transitive package over a pinned one and took a deployment down. Ship
`uv.lock` and `frontend/package-lock.json` in this repo the same way octohub-core
ships its own, and do not gitignore them.

Frontend dependencies are not wired up yet — `switch-client.sh` refuses a client
that declares them rather than installing them into core on a live box, which is
what it used to do. Raise it with the maintainer if you need one.

## Company Configuration

**IMPORTANT:** Update the octohub_co_id in each company config.yaml.
Get the UUID from OctoHub admin after creating the company.

## See Also

- octohub-core CLAUDE.md - Platform rules and patterns
- [Feature Development Guide](https://github.com/Turn10Innovations/octohub-core/blob/main/docs/FEATURE_DEVELOPMENT_GUIDE.md)
- [Customizations Cookbook](https://github.com/Turn10Innovations/octohub-core/blob/main/docs/CUSTOMIZATIONS_COOKBOOK.md)
