# Case Study: Reputera.se

**Hybrid Backend & AI Integration in a Live WordPress SaaS**

---

## Executive Summary

Reputera.se is a production Online Reputation Management (ORM) SaaS serving Swedish small and mid-sized businesses.  

This case study documents how a **live WordPress-based SaaS** was extended with a modern Supabase backend to support:

- AI-driven features with **strict cost controls**  
- SMS automation with **per-plan limits**  
- Advanced analytics and **usage tracking**  
- Scalable automation **without frontend rewrites**  

The project was executed under **real production constraints**: paying users, active subscriptions, and zero tolerance for downtime.

---

## The Problem

Reputera.se was live with:

- WordPress frontend  
- Paid Member Subscriptions (PMS)  
- PayPal payments  
- SMS automation via 46elks  
- AI-powered responses via DeepSeek  

However, the system lacked:

- Cost visibility for AI and SMS usage  
- Reliable analytics across users and time  
- Scalable automation (cron jobs, aggregation, alerts)  
- Safe extensibility without destabilizing WordPress  

**Core risk:** Rewriting or replacing WordPress would break subscriptions, introduce billing risk, and slow feature delivery.

---

## Constraints

Hard constraints shaped the architecture:

- WordPress remains the system of record  
- Payments and memberships stay in PMS  
- No auto-deployments  
- No secrets in repositories  
- User-level usage limits must be enforced  
- Human-in-the-loop for high-risk automation  

This was **not a greenfield project** — the solution had to **modernize without disruption**.

---

## Architectural Decision: Hybrid Model

Instead of replacing WordPress, a **hybrid approach** was chosen:

| Layer       | Responsibility |
|------------|----------------|
| WordPress  | Users, authentication, UI, subscriptions, payments |
| Supabase   | Data analytics, cost tracking, automation, cron jobs |
| REST Bridge | Controlled data sync between systems |

**Why Supabase:**

- Postgres with strong relational modeling  
- Row-Level Security (RLS) for multi-tenancy  
- Edge Functions for serverless automation  
- Predictable cost model  

---

## Key Implementation Areas

### A. Usage & Cost Tracking (AI + SMS)

Supabase tables introduced:

- `ai_usage_log`  
- `sms_usage_log`  
- `daily_stats`  

Each log entry includes:

- `wp_user_id`  
- Timestamp  
- Cost metrics (USD / SEK)  

**Impact:**

- Hard caps per subscription plan  
- Forecasting and margin analysis  
- Abuse prevention  

---

### B. AI Integration (DeepSeek)

**Problem:** AI responses create variable, invisible costs.  

**Solution:**

- Centralized AI calls via WordPress REST endpoint  
- Usage logged per request  
- Monthly caps enforced per plan  
- No background auto-generation without approval  

**Result:** AI became a controlled feature, not a runaway cost center.

---

### C. SMS Automation (46elks)

**Why 46elks:** Sweden-first pricing, local reliability, predictable billing  

**Implementation Highlights:**

- Credentials stored in WordPress options  
- Daily & weekly limits per plan  
- SMS usage logged to Supabase  
- One-time review tokens with expiry  

**Impact:** Reduced spam risk and ensured GDPR-aligned behavior.

---

### D. Data Synchronization (WordPress → Supabase)

Custom REST API bridge designed to sync:

- New users  
- Usage events  
- Review activity  

**Principles:**

- Push only what is needed  
- Never expose Supabase directly to clients  
- Fail safely (WordPress continues working if Supabase is unavailable)

---

## Security Model

- Supabase Row-Level Security (RLS) per user  
- No shared service roles in frontend code  
- Environment variables for all secrets  
- Explicit rate limits  
- Manual approval for sensitive automation  

**Result:** Scalable features without increasing risk.

---

## Results & Impact

**Technical Impact:**

- Added analytics without touching frontend UX  
- Enabled cron-based automation safely  
- Introduced cost transparency across AI and SMS  

**Business Impact:**

- Predictable margins  
- Safer upsells based on usage  
- Faster feature iteration without breaking billing  

**Strategic Impact:**

- Platform now extensible for multi-source reviews, competitor benchmarking, and advanced sentiment analysis  

---

## Key Learnings

- Legacy systems are **not liabilities** if respected  
- Cost tracking is as important as **feature velocity**  
- AI features must be **constrained by design**  
- Hybrid architectures **reduce risk** in live SaaS products  

---

## Next Steps (Scaling Further)

- Introduce business-level multi-tenancy (agency accounts)  
- Expand ingestion to multiple review platforms  
- Automate competitor benchmarking  
- Add AI-assisted prioritization (not auto-action)  

---

## Why This Case Study Matters

This project demonstrates:

- Real production experience  
- Constraint-driven engineering  
- Security-first automation  
- Business-aligned technical decisions  

It reflects an approach to scaling **systems that already generate revenue**, while minimizing risk.
