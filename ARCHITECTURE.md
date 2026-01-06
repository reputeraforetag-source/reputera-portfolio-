rchitecture Overview — Reputera.se

This document describes the system architecture behind Reputera.se, focusing on design decisions, data flow, and scalability under real production constraints.

The architecture reflects a core principle:

Modernize without breaking what already works.

1. Architectural Goals

The architecture was designed to satisfy five non-negotiable goals:

Zero disruption to a live, revenue-generating WordPress system

Strict cost control for AI and SMS usage

Scalable automation without frontend rewrites

Strong multi-tenant security from day one

Future-proof data modeling for multi-source expansion

2. High-Level System Model

Reputera.se uses a hybrid architecture combining a legacy frontend with a modern backend.

[ Customer Browser ]
        |
        v
[ WordPress Frontend ]
- UI & Dashboard
- Authentication
- Memberships (PMS)
- Payments (PayPal)
        |
        | REST (Server-to-Server)
        v
[ Integration Layer ]
- Custom WP REST endpoints
- Validation & limits
- Fail-safe logic
        |
        v
[ Supabase Backend ]
- Postgres (RLS)
- Edge Functions (cron)
- Analytics & aggregation


Key idea:
WordPress remains the control plane. Supabase is the data & automation plane.

3. Why WordPress Remains the System of Record

Replacing WordPress would introduce unnecessary risk.

WordPress is responsible for:

User accounts & authentication

Subscription state

Payments & billing logic

Frontend rendering

Existing plugins & workflows

Why this matters

Billing logic is fragile

Payment failures are expensive

Rewrites slow down product velocity

Decision:
Do not duplicate user or payment state in Supabase.
Instead, reference WordPress user IDs (wp_user_id) everywhere.

4. Supabase as the Backend Engine

Supabase was introduced to handle workloads that WordPress is not designed for.

Supabase responsibilities:

Usage & cost tracking (AI, SMS)

Analytics & aggregation

Scheduled background jobs

Secure multi-tenant data access

Future API integrations

Key Supabase features used:

Postgres for relational modeling

Row-Level Security (RLS) for tenant isolation

Edge Functions for cron-like automation

Views for read-optimized analytics

5. Data Ownership & Multi-Tenancy
User identity model

WordPress user = source of truth

Supabase rows reference wp_user_id

No Supabase Auth exposed to end users

RLS principle

Every table enforces:

row.wp_user_id = request.wp_user_id


This ensures:

No cross-tenant access

Safe analytics queries

Future support for agencies / parent accounts

6. Integration Layer (WordPress ↔ Supabase)

A custom REST bridge connects WordPress to Supabase.

Why a bridge layer exists

Prevents exposing Supabase directly

Allows validation and rate limiting

Enables graceful failure

Design principles

Push events, not full state

Idempotent requests

Fail-safe (WP continues working if Supabase is down)

Example synced events

AI response generated

SMS sent

Review imported

Daily stats aggregation trigger

7. Usage & Cost Tracking Architecture

Cost visibility was a core architectural driver.

Logging model

Every AI call → ai_usage_log

Every SMS → sms_usage_log

Daily aggregation → daily_stats

Why this matters

Predictable margins

Plan-based enforcement

Upsell intelligence

Abuse prevention

AI and SMS are treated as billable resources, not features.

8. Automation & Cron Strategy
What runs automatically

Daily stats aggregation

Review ingestion (planned)

Competitor tracking (planned)

Usage threshold checks

Where it runs

Supabase Edge Functions (scheduled)

Never in the WordPress request lifecycle

Why

Keeps frontend fast

Prevents timeout risks

Centralizes automation logic

9. AI System Placement

AI is intentionally not embedded deeply into WordPress.

Design choices

AI calls go through a controlled REST endpoint

No background AI without user action or approval

Hard caps per subscription plan

All usage logged before response is returned

This prevents:

Silent cost overruns

Unpredictable behavior

Trust erosion with customers

10. Failure Modes & Resilience

The system is designed to fail safely.

If Supabase is unavailable:

WordPress continues operating

Payments and memberships unaffected

Automation pauses, not breaks

If integrations fail:

Errors are logged

No cascading failures

Human intervention possible

This is critical in small-team SaaS environments.

11. Scalability Path

This architecture supports future growth without rework:

Multi-platform reviews (Google, Facebook, Trustpilot)

Agency / multi-location accounts

Advanced AI analytics

Competitor benchmarking

External dashboards

The frontend does not need to change to support these.

12. Architectural Philosophy

This system was designed around one core belief:

Good architecture reduces decisions later.

By separating:

Control vs execution

UI vs automation

Billing vs analytics

…the platform can evolve without accumulating risk.
