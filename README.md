# payload-arch skill for Claude

A Claude Code skill that enforces a layered **route → controller → service → store** architecture
for [PayloadCMS](https://payloadcms.com) projects.

This skill was built on top of the official [`payload` skill](https://github.com/payloadcms/skills)
from the PayloadCMS team. That skill covers the framework — collections, hooks, queries, access
control. This one covers the architectural layer on top of it: how to structure your code, where
things live, and how layers communicate.

## What it enforces

**Backend layers (inward-only dependency flow)**
- `src/routes/` — endpoint registration only, zero logic
- `src/controllers/` — HTTP parsing and response formatting, delegates to services
- `src/services/` — all business logic, can call other services and stores
- `src/store/` — Payload CRUD only, no business logic, never calls services

**Frontend**
- SWR + `payloadClientSDK` for all client-side data fetching
- Server pages for auth checks and primary entity fetching, client components for everything dynamic
- Polling pattern for live job/evaluation status

**Project structure**
- `src/types/` — domain TypeScript types
- `src/utils/` — domain-specific pure utilities
- `src/lib/` — generic pure utilities (no domain dependencies)
- `src/access/` — Payload access control functions
- `src/const/` — frozen constants, all re-exported from an index
- `src/config/config.ts` — the only place `process.env` is read
- `src/jobs/` — task definitions and workflows separated into two files per domain

**CLAUDE.md template** included in the skill — drop a thin, project-specific `CLAUDE.md` into any
new PayloadCMS project and let this skill handle the rest.

## Installation

Register this repo as a Claude Code plugin marketplace:

/plugin marketplace add MinSomai/payload-arch-skill

Then install the skill:

1. Run `/plugin` in Claude Code
2. Select **Browse and install plugins**
3. Select `payload-arch-skill`
4. Select `payload-arch`

## Usage

Once installed, the skill triggers automatically when you're working on a PayloadCMS project that
follows this architecture. It activates when you mention routes, controllers, services, stores,
jobs, SWR, components, or data fetching.

You can also invoke it explicitly:

/payload-arch where should this logic live?

## Works best alongside

Install the official PayloadCMS skill for deep framework reference (collections, hooks, queries):

/plugin marketplace add payloadcms/skills

The two skills are complementary — `payload` covers what the framework can do, `payload-arch`
covers how to structure your project on top of it.

## License

MIT
