Product Roadmap — Reputera.se

This roadmap outlines the planned evolution of Reputera.se, focusing on value delivery, system integrity, and scalable automation.

The roadmap is intentionally phased, ensuring that each layer is stable before additional complexity is introduced.

Phase 0 — Foundation (Completed / In Progress)

Objective: Establish a secure, monetizable core with minimal moving parts.

Platform

WordPress as primary application shell

Supabase as data layer (Postgres + RLS)

Server-side integration only

Stripe-based subscription plans

Core Features

Review collection from multiple sources

SMS-based review requests

QR codes and shareable links

Review filtering and moderation

Review widget for websites

Architecture

Strict tenant isolation

No frontend access to Supabase

Usage-based enforcement by plan

Human-in-the-loop controls for risky actions

Outcome:
A production-ready MVP that can onboard real customers safely.

Phase 1 — Reliability & Control (Next)

Objective: Make the system predictable, observable, and operationally safe.

Platform Improvements

Centralized usage dashboard (SMS / AI / reviews)

Hard usage caps per plan

Admin-level kill switches per feature

Improved error visibility & retry logic

Security & Compliance

Extended audit logging

Data access transparency per user

Improved key rotation procedures

UX Improvements

Clear usage indicators

Plan limit warnings

Safer defaults for automation

Outcome:
Reduced support load and higher customer trust.

Phase 2 — AI-Assisted Value (Planned)

Objective: Introduce AI where it creates measurable ROI, not novelty.

AI Capabilities

Sentiment analysis of incoming reviews

Review summarization for business owners

AI-assisted response drafts (approval required)

Trend detection across time and platforms

Guardrails

AI usage counted before execution

No autonomous posting

Clear opt-in per feature

Outcome:
Customers save time and gain insight without risking brand voice.

Phase 3 — Automation & Workflows (Planned)

Objective: Enable controlled automation for power users and agencies.

Automation

Trigger-based review requests

Conditional SMS flows

Scheduled review campaigns

Workflow Controls

Preview before execution

Rollback-safe operations

Per-automation quotas

Target Users

Multi-location businesses

Agencies managing multiple clients

Outcome:
Higher ARPU without sacrificing safety.

Phase 4 — Agency & Multi-Tenant Expansion (Future)

Objective: Support professional users managing many brands.

Features

Parent / child account hierarchy

Cross-account analytics

Agency billing & invoicing

White-label options

Security

Delegated permissions

Scoped access tokens

Strict cross-tenant boundaries

Outcome:
Platform becomes a scalable B2B infrastructure, not just a tool.

Phase 5 — Ecosystem & Integrations (Future)

Objective: Expand reach without bloating the core product.

Integrations

POS systems

Booking platforms

CRM tools

Webhook-based extensibility

Platform Strategy

Public API (read-first)

Partner integrations

Controlled extension points

Outcome:
Growth through ecosystem, not internal complexity.

Roadmap Principles

Revenue before complexity

Security before automation

Insights before actions

Opt-in before autonomy

Features are added only when they:

Solve a real customer problem

Can be safely constrained

Support long-term scalability
