# Playlo — Engineering Architecture & Product Specification

> **Tagline:** Chalo Khelein  
> **Launch Market:** Sirsa, Haryana, India  
> **Launch Sport:** Badminton  
> **Stack:** Next.js 15 (App Router) + TypeScript + Neon Postgres + Prisma + tRPC v11 + Tailwind v4 + Upstash

---

## 📚 Technical Documentation Index

The monolithic product plan (`product-plan-phased.md`) has been refactored into modular, production-grade technical specifications designed for solo-developer execution:

| Document | Title | Description |
| :--- | :--- | :--- |
| **[01-system-architecture.md](./01-system-architecture.md)** | **System Architecture & Infrastructure** | Monorepo layout (Turborepo), C4 context & container diagrams, Edge routing, Vercel/Neon/Upstash topology, security (OWASP, CSP), scaling bottlenecks, and cost modeling. |
| **[02-database-and-data-model.md](./02-database-and-data-model.md)** | **Database Schema & Data Model** | Full production-ready `schema.prisma`, PostGIS geospatial queries, integer paise financial precision, state machines, and concurrency locking (`SELECT ... FOR UPDATE`). |
| **[03-api-and-trpc-design.md](./03-api-and-trpc-design.md)** | **API & tRPC Architecture** | Complete tRPC router tree, Zod validation schemas as single source of truth, procedure middleware (RBAC, Upstash sliding-window rate limit), Razorpay HMAC webhooks, and QStash jobs. |
| **[04-core-engines-and-workflows.md](./04-core-engines-and-workflows.md)** | **Core Engines & Business Workflows** | Court booking engine with 5-min Redis holds, split payments (Razorpay Route), game lifecycle invariants, venue scheduling matrix, and player reliability scoring formula. |
| **[05-frontend-and-design-system.md](./05-frontend-and-design-system.md)** | **Frontend & Design System** | Turborepo 4-app setup (`apps/web`, `apps/player` PWA, `apps/venue`, `apps/admin`), Tailwind CSS v4 design tokens, shadcn/ui components, screen state handling, and `next-intl` localization. |
| **[06-operations-cicd-and-roadmap.md](./06-operations-cicd-and-roadmap.md)** | **Operations, CI/CD & Roadmap** | PostHog feature flags for phased rollout, GitHub Actions CI workflow, Vitest concurrency test suite, Playwright checkout E2E tests, and 7-week solo-developer sprint plan. |

---

## 🏗️ Monorepo Quick Overview

```text
playlo/
├── apps/
│   ├── web/          # Public marketing & SEO (playlo.in)
│   ├── player/       # PWA Player Application (playlo.in/app)
│   ├── venue/        # Venue Owner Portal (playlo.in/venue)
│   └── admin/        # Internal Control Center (playlo.in/admin)
├── packages/
│   ├── api/          # Root tRPC routers & procedure middleware
│   ├── db/           # Prisma client, migrations & Neon connection
│   ├── auth/         # NextAuth.js v5 (MSG91 Phone OTP + Google)
│   ├── validators/   # Zod schemas (Single source of truth)
│   ├── ui/           # shadcn/ui + Radix + Tailwind v4 components
│   └── config/       # Shared Tailwind, ESLint, TypeScript & i18n configs
```

---

## 🚀 Key Architectural Decisions (From Grill-Me Session)

1. **Solo Developer Pragmatism:** No heavy microservices or Kubernetes. High leverage via Vercel Edge + Neon serverless Postgres + Upstash Redis/QStash.
2. **India-First Essentials:** Phone OTP authentication via MSG91, Razorpay UPI & Route split-payments, integer paise storage, and WhatsApp viral invite link generator.
3. **Double-Booking Prevention:** Three-tier defense: Ephemeral 5-minute Redis hold (`SET NX EX 300`) during checkout + Postgres transaction with `SELECT FOR UPDATE` and advisory lock at confirmation.
4. **Decoupled Entities:** Games and Bookings maintain separate lifecycles (a game can exist in a local park without a court booking; a court booking can be made for private family practice).
5. **Phase Control via Feature Flags:** Merging to trunk (`main`) with PostHog feature flags controlling Phase 1, Phase 2, and Phase 3 feature exposure.

