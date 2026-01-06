Security Model — Reputera.se

This document outlines the security principles, threat model, and controls used in the Reputera.se architecture.

Security is treated as a core product feature, not an afterthought — especially given the use of AI, SMS, and payment-linked automation.

1. Security Philosophy

The system is designed around four core principles:

Least privilege by default

Explicit tenant isolation

Cost & abuse prevention as security

Human-in-the-loop for high-risk actions

Automation is intentionally constrained to preserve trust and prevent silent failure modes.

2. Threat Model
Primary risks addressed

Cross-tenant data access

API key leakage

Cost abuse (AI / SMS)

Unauthorized automation

Accidental data exposure via analytics

Out of scope

End-user device compromise

Browser-level attacks outside the application domain

3. Identity & Access Control
Source of truth

WordPress is the authoritative identity provider

Supabase does not expose Auth directly to end users

Identity mapping

All Supabase records reference wp_user_id

No duplicated user tables

No shared service accounts per user

This prevents:

Identity drift

Orphaned access

Conflicting permission models

4. Row-Level Security (RLS)

Supabase RLS is the primary enforcement mechanism for data isolation.

Core rule pattern

Every user-scoped table enforces:

wp_user_id = current_setting('request.jwt.claim.wp_user_id')::bigint

Guarantees

Users can only read/write their own data

Aggregations remain tenant-safe

Accidental leaks via views are prevented

RLS is non-optional and enforced on all relevant tables.

5. API & Integration Security
WordPress ↔ Supabase communication

Server-to-server only

No Supabase keys exposed to browsers

Requests validated and signed

REST bridge safeguards

Input validation

Idempotent requests

Rate limiting per endpoint

Graceful failure handling

Supabase is never called directly from frontend JavaScript.

6. Secrets & Credential Handling
Storage rules

No secrets in repositories

No secrets in client-side code

No secrets in logs

Credential locations
Credential	Storage
AI API keys	Environment variables
SMS credentials	WordPress options (server-only)
Supabase keys	Server environment variables

A .env.example file documents required variables without values.

7. AI & SMS Abuse Prevention

AI and SMS are treated as billable attack surfaces.

Controls in place

Per-plan usage caps

Daily & weekly SMS limits

Monthly AI response limits

Pre-call quota checks

All usage logged before execution

Why this matters

Cost abuse is a security incident, not just a billing issue.

8. Human-in-the-Loop Controls

Certain actions are intentionally not automated:

High-risk AI responses

Bulk messaging

Destructive data operations

Plan override logic

These require explicit human approval to:

Prevent brand damage

Avoid legal exposure

Maintain customer trust

9. Logging & Observability
Logged events

AI usage

SMS usage

Sync failures

Rate-limit violations

Logging principles

No PII in logs

No credentials in logs

Logs are append-only

Logs support:

Auditing

Debugging

Incident response

10. Failure & Containment Strategy
If Supabase is unavailable

WordPress continues to operate

Payments and memberships unaffected

Automation pauses safely

If an integration misbehaves

Feature is disabled

No cascading failures

Manual intervention possible

The system prioritizes containment over recovery speed.

11. Data Ownership & Compliance

Customers retain ownership of their data

Data is not resold or repurposed

Usage data is collected only for service delivery

Design choices are aligned with GDPR principles:

Data minimization

Purpose limitation

Access transparency

12. Security as a Scaling Enabler

Security decisions in this system are designed to:

Support future agency accounts

Enable safe automation

Allow feature expansion without re-architecture

Security is what allows the platform to grow without fear.
