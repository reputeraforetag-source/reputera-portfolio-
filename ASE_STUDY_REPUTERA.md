Case Study: Reputera.se

Hybrid Backend & AI Integration in a Live WordPress SaaS

1. Executive Summary

Reputera.se is a production Online Reputation Management (ORM) SaaS serving Swedish small and mid-sized businesses.

This case study documents how a live WordPress-based SaaS was extended with a modern Supabase backend to support:

AI-driven features with strict cost controls

SMS automation with per-plan limits

Advanced analytics and usage tracking

Scalable automation without frontend rewrites

The project was executed under real production constraints: paying users, active subscriptions, and zero tolerance for downtime.

2. The Problem

Reputera was already live with:

WordPress frontend

Paid Member Subscriptions (PMS)

PayPal payments

SMS automation via 46elks

AI-powered responses via DeepSeek

However, the existing architecture lacked:

Cost visibility for AI and SMS usage

Reliable analytics across users and time

Scalable automation (cron jobs, aggregation, alerts)

A safe way to add new backend features without destabilizing WordPress

Core risk

Rewriting or replacing WordPress would:

Break subscriptions

Introduce billing risk

Slow down feature delivery

3. Constraints (What Could NOT Change)

This was not a greenfield project.

Hard constraints:

WordPress remains the system of record

Payments and memberships stay in PMS

No auto-deployments

No secrets in repositories

User-level usage limits must be enforced

Human-in-the-loop for high-risk automation

These constraints shaped the architecture, not the other way around.

4. Architectural Decision: Hybrid Model

Instead of replacing WordPress, the system was extended.

Chosen approach

A hybrid architecture where:

Layer	Responsibility
WordPress	Users, authentication, UI, subscriptions, payments
Supabase	Data analytics, cost tracking, automation, cron jobs
REST Bridge	Controlled data sync between systems
Why Supabase

Postgres with strong relational modeling

Row-Level Security (RLS) for multi-tenancy

Edge Functions for serverless automation

Predictable cost model

5. Key Implementation Areas
A. Usage & Cost Tracking (AI + SMS)

A major goal was cost awareness per user and plan.

Supabase tables introduced:

ai_usage_log

sms_usage_log

daily_stats

Each log entry is tied to:

wp_user_id

Timestamp

Cost metrics (USD / SEK)

This enables:

Hard caps per subscription plan

Forecasting and margin analysis

Abuse prevention

B. AI Integration (DeepSeek)

Problem:
AI responses create variable, invisible costs.

Solution:

Centralized AI calls via a WordPress REST endpoint

Usage logged per request

Monthly caps enforced per plan

No background auto-generation without approval

Result:
AI became a controlled feature, not a runaway cost center.

C. SMS Automation (46elks)

Why 46elks (not Twilio):

Sweden-first pricing

Local reliability

Predictable billing

Implementation highlights:

Credentials stored in WordPress options

Daily and weekly limits per plan

SMS usage logged to Supabase

One-time review tokens with expiry

This reduced spam risk and ensured GDPR-aligned behavior.

D. Data Synchronization (WordPress → Supabase)

A custom REST API bridge was designed to sync:

New users

Usage events

Review activity

Key principles:

Push only what is needed

Never expose Supabase directly to the client

Fail safely (WordPress continues working if Supabase is unavailable)

6. Security Model

Security was treated as a first-class feature.

Measures:

Supabase Row-Level Security (RLS) per user

No shared service roles in frontend code

Environment variables for all secrets

Explicit rate limits

Manual approval for sensitive automation

This allowed scaling features without scaling risk.

7. Results & Impact
Technical impact

Added analytics without touching frontend UX

Enabled cron-based automation safely

Introduced cost transparency across AI and SMS

Business impact

Predictable margins

Safer upsells based on usage

Faster feature iteration without breaking billing

Strategic impact

Platform is now extensible:

Multi-source reviews

Competitor benchmarking

Advanced sentiment analysis

8. Key Learnings

Legacy systems are not liabilities if respected

Cost tracking is as important as feature velocity

AI features must be constrained by design

Hybrid architectures reduce risk in live SaaS products

9. What I’d Do Next (If Scaling Further)

Introduce business-level multi-tenancy (agency accounts)

Expand ingestion to multiple review platforms

Automate competitor benchmarking

Add AI-assisted prioritization (not auto-action)

10. Why This Case Study Matters

This project demonstrates:

Real production experience

Constraint-driven engineering

Security-first automation

Business-aligned technical decisions

It reflects how I approach scaling systems that already make money.
