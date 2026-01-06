Case Study: Reputera.se

Hybrid Backend & AI Integration in a Live WordPress SaaS

## Executive Summary
Reputera.se is a production Online Reputation Management (ORM) SaaS serving Swedish SMBs.  
This case study documents extending a live WordPress SaaS with a modern Supabase backend to support:

- AI-driven features with strict cost controls  
- SMS automation with per-plan limits  
- Advanced analytics and usage tracking  
- Scalable automation without frontend rewrites  

Executed under real production constraints: paying users, active subscriptions, zero downtime.

## The Problem
Existing setup:

- WordPress frontend  
- Paid Member Subscriptions (PMS)  
- PayPal payments  
- SMS automation via 46elks  
- AI-powered responses via DeepSeek  

Challenges:

- Reliable analytics across users and time  
- Scalable automation (cron jobs, aggregation, alerts)  
- Safe backend expansion without destabilizing WordPress  

## Constraints

- WordPress remains the system of record  
- Payments and memberships stay in PMS  
- No auto-deployments  
- User-level usage limits enforced  
- Human-in-the-loop for high-risk automation  

## Architectural Decision: Hybrid Model

| Layer       | Responsibility |
|------------|----------------|
| WordPress  | Users, authentication, UI, subscriptions, payments |
| Supabase   | Data analytics, cost tracking, automation, cron jobs |
| REST Bridge| Controlled data sync between systems |

### Why Supabase?

- Postgres with relational modeling  
- Row-Level Security (RLS) for multi-tenancy  
- Edge Functions for serverless automation  
- Predictable cost model  

## Key Implementation Areas

### A. Usage & Cost Tracking (AI + SMS)
Supabase tables:

- ai_usage_log  
- sms_usage_log 
- daily_stats

Logs include user ID, timestamps, and costs → enforce hard caps, forecast margins, prevent abuse.

### B. AI Integration (DeepSeek)
- Centralized calls via WordPress REST endpoint  
- Monthly caps enforced per plan  
- Human approval for sensitive actions  

### C. SMS Automation (46elks)
- Credentials stored in WordPress options  
- Daily/weekly limits per plan  
- One-time review tokens with expiry → GDPR compliant  

### D. Data Synchronization (WordPress → Supabase)
- Sync new users, usage events, review activity  
- Push only required data, fail-safe design  

## Security Model
- Row-Level Security per user  
- No shared service roles in frontend code  
- Environment variables for secrets  
- Rate limits & manual approvals  

## Results & Impact

**Technical:** Analytics without frontend changes, safe automation, cost transparency.  
**Business:** Predictable margins, faster iteration, safe upsells.  
**Strategic:** Extensible for multi-source reviews, competitor benchmarking, sentiment analysis.  

## Key Learnings
- Respect legacy systems  
- Cost tracking = feature velocity  
- AI must be constrained  
- Hybrid architectures reduce risk  

