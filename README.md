# Enterprise Workforce Operations & Learning Management Platform (Case Study)

> [!NOTE]
> **Confidentiality Disclaimer**: This repository is an architectural case study of an internal operations and workforce platform engineered for a distributed digital media and management agency. Proprietary source code, specific business logic, internal database credentials, and company/client data have been omitted in compliance with non-disclosure agreements (NDA).

---

## Executive Summary
Engineered an internal web-based workforce operations, training, and human resource management platform for a distributed digital media agency. The system centralizes team scheduling, automated compensation and pay statement generation, contractor onboarding, training/LMS quiz workflows, third-party analytics data ingestion, and multi-tier role-based access control (RBAC).

---

## Key System Modules & Engineering Contributions

### 1. Shift Scheduling & Operations Engine
* **Dynamic Shift Planner**: Built a weekly scheduling matrix with conflict detection, shift-slot deduplication, and overwork tracking.
* **Whole-Week Team Swaps**: Implemented atomic team reassignment endpoints to swap schedules across entire week blocks without corrupting assignment slots.

### 2. Automated Compensation & PDF Statement Generator
* **Commission & Bonus Rules Engine**: Built calculation pipelines supporting configurable bonus types, cycle rates, role-specific pay rates, and override rules.
* **Client-Side/Server-Side PDF Generation**: Integrated `pdf-lib` to generate official pay statements and signed contract documents with cryptographically tracked human-readable document IDs (`{CON|NDA|CER|CUS}-YYYY-XXXXXX`).

### 3. Integrated Learning Management System (LMS)
* **Interactive Quiz Runner & Builder**: Engineered an in-app training suite supporting multiple question types (Multiple Choice, Multi-Select, True/False, Free Text) with point-weighted scoring.
* **Authoritative Server-Side Scoring & Timers**: Server-verified attempt durations and automatic submit-on-expiry, preventing client clock tampering and stripping answer keys before transmission.
* **Bulk Assignments & User Cohorts**: Implemented Cartesian-product bulk assignment tools targeting individual users, custom training cohorts, pod groups, or roles with deduped notification fan-outs.

### 4. Data Ingestion & Fuzzy Record Matching
* **XLSX / CSV Ingestion Pipeline**: Ingestion service parsing external reporting sheets with automated column heuristic detection.
* **Fuzzy Identity Resolution**: Implemented accent-, whitespace-, and case-insensitive fuzzy matching with email fallback to reconcile mismatched contractor names against database records.
* **Automated Orphan Backfill**: Best-effort background backfill reconciling unmatched import rows upon profile updates.

### 5. Identity, RBAC & Duplicate Detection
* **Multi-Tier Authorization**: Server-component and API-route level permission guards across 5 privilege tiers (`Owner`, `Manager`, `Developer`, `Contractor`, `Trialist`).
* **Confidence-Scored Duplicate Detection**: Heuristic engine detecting duplicate user records with support for pairwise dismissals, silent duplicate creation alerts, and safe database merge operations.

---

## Tech Stack

| Domain | Technologies |
| :--- | :--- |
| **Frontend** | Next.js 14 (App Router), React 18, TypeScript (Strict Mode) |
| **Styling & UI** | Tailwind CSS, shadcn/ui, Radix UI Primitives, Lucide Icons |
| **Database & ORM** | PostgreSQL (Supabase), Drizzle ORM, Drizzle Kit Migrations |
| **Authentication** | Supabase SSR Auth, Google OAuth, Strict Identity Verification |
| **Document Processing**| `pdf-lib` (PDF Generation), `xlsx` (Spreadsheet Parsing) |
| **Testing & Quality** | Vitest (38 Unit Test Suites, 390+ Tests), ESLint |
| **Deployment** | Vercel |

---

## Architectural Highlights
* **Type-Safe Relational Schema**: 30+ relational migrations managed via Drizzle ORM with schema audit scripts preventing accidental destructive schema pushes.
* **Strict Unit Test Coverage**: 390+ tests covering bonus calculation math, fuzzy name normalization, quiz scoring edge cases, and force-wipe cascading deletion safety.
* **Idempotent Data Pipelines**: Database unique constraints on `(user_id, source, date)` preventing duplicate shift and metric logs during re-imports.
