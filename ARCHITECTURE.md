# Architecture Overview — Reputera.se

This document describes the system architecture behind Reputera.se, focusing on design decisions, data flow, and scalability under real production constraints.

The architecture reflects a core principle:

> Modernize without breaking what already works.

---

## 1. Architectural Goals

The architecture was designed to satisfy five non-negotiable goals:

1. Zero disruption to a live, revenue-generating WordPress system  
2. Strict cost control for AI and SMS usage  
3. Scalable automation without frontend rewrites  
4. Strong multi-tenant security from day one  
5. Future-proof data modeling for multi-source expansion  

---

## 2. High-Level System Model

Reputera.se uses a **hybrid architecture** combining a legacy frontend with a modern backend.


flowchart TD
    A[Customer Browser] --> B[WordPress Frontend]
    B --> C[Integration Layer]
    C --> D[Supabase Backend]
    D --> E[AI Orchestration (Claude + DeepSeek)]
    D --> F[SMS Automation (46elks)]
    D --> G[Admin Dashboard (Analytics & Alerts)]
    
Key idea:

WordPress = control plane

Supabase = data & automation plane

3. Why WordPress Remains the System of Record
Replacing WordPress would introduce unnecessary risk. WordPress is responsible for:

User accounts & authentication

Subscription state

Payments & billing logic

Frontend rendering

Existing plugins & workflows

Why this matters:

Billing logic is fragile

Payment failures are expensive

Rewrites slow product velocity

Decision:
Do not duplicate user or payment state in Supabase. Reference wp_user_id everywhere.

4. Supabase as the Backend Engine
Supabase was introduced to handle workloads WordPress is not designed for.

Responsibilities:

Usage & cost tracking (AI, SMS)

Analytics & aggregation

Scheduled background jobs

Secure multi-tenant data access

Future API integrations

Key Features Used:

Postgres for relational modeling

Row-Level Security (RLS) for tenant isolation

Edge Functions for cron-like automation

Views for read-optimized analytics

5. Data Ownership & Multi-Tenancy
User identity model:

WordPress user = source of truth

Supabase rows reference wp_user_id

No Supabase Auth exposed to end users

RLS principle:

Every table enforces: row.wp_user_id = request.wp_user_id

Ensures:

No cross-tenant access

Safe analytics queries

Future support for agency / parent accounts

6. Integration Layer (WordPress ↔ Supabase)
A custom REST bridge connects WordPress to Supabase.

Purpose:

Prevent exposing Supabase directly

Allow validation & rate limiting

Enable graceful failure

Design Principles:

Push events, not full state

Idempotent requests

Fail-safe (WP continues if Supabase is down)

Example Synced Events:

AI response generated

SMS sent

Review imported

Daily stats aggregation trigger

7. Usage & Cost Tracking Architecture
Cost visibility was a core architectural driver.

Logging Model:

Every AI call → ai_usage_log

Every SMS → sms_usage_log

Daily aggregation → daily_stats

Benefits:

Predictable margins

Plan-based enforcement

Upsell intelligence

Abuse prevention

AI and SMS are treated as billable resources, not just features.

8. Automation & Cron Strategy
Automatic Jobs:

Daily stats aggregation

Review ingestion (planned)

Competitor tracking (planned)

Usage threshold checks

Where it runs: Supabase Edge Functions (scheduled)

Why:

Keeps frontend fast

Prevents timeout risks

Centralizes automation logic

9. AI System Placement
AI is intentionally isolated from WordPress.

Design Choices:

Controlled REST endpoint for AI calls

No background AI without user action / approval

Hard caps per subscription plan

Usage logged before response returned

Prevents:

Silent cost overruns

Unpredictable behavior

Trust erosion with customers

10. Failure Modes & Resilience
If Supabase is unavailable:

WordPress continues operating

Payments & memberships unaffected

Automation pauses, does not break

If integrations fail:

Errors are logged

No cascading failures

Human intervention possible

Critical for small-team SaaS environments

11. Scalability Path
Supports future growth without frontend rework:

Multi-platform reviews (Google, Facebook, Trustpilot)

Agency / multi-location accounts

Advanced AI analytics

Competitor benchmarking

External dashboards

12. Architectural Philosophy
Good architecture reduces decisions later.

By separating:

Control vs execution

UI vs automation

Billing vs analytics

…the platform evolves without accumulating risk.
