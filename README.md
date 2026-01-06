# reputera-portfolio-

Reputera.se — Production Backend & AI Integration Portfolio

This repository documents the architecture, design decisions, and backend integration work behind Reputera.se, a production Online Reputation Management (ORM) SaaS built for the Swedish market.

It is intended as a technical portfolio for roles involving:

Backend engineering

AI / automation systems

SaaS architecture

Hybrid legacy + modern stacks

The code and examples in this repository are sanitized, non-sensitive, and representative of real production systems.

👋 Who This Is For

This repository is for:

Hiring managers evaluating real-world system design experience

Engineers reviewing architectural judgment and security thinking

Founders / CTOs looking for someone who can extend existing systems without breaking them

This is not a tutorial and not a full open-source product.

🧠 Project Overview

Reputera.se is a SaaS platform that helps local businesses monitor, understand, and act on customer reviews across multiple platforms.

Core challenge

The product was already live on WordPress with paying users, subscriptions, and SMS workflows — but needed:

Cost tracking for AI & SMS usage

Advanced analytics

Automation and cron-based workflows

A scalable data layer without rewriting the frontend

Solution

A hybrid architecture where:

WordPress remains the system of record (users, payments, UI)

Supabase provides a modern backend (Postgres, RLS, Edge Functions)

A custom REST bridge synchronizes data safely between systems

🏗 High-Level Architecture

Frontend / Control Plane

WordPress 6.x

Paid Member Subscriptions (PMS)

PayPal payments

Custom plugins & REST endpoints

Backend / Data & Automation

Supabase (Postgres, RLS, Edge Functions)

AI usage tracking & cost control

SMS usage logging

Scheduled analytics aggregation

Integrations

AI: DeepSeek API

SMS: 46elks (Sweden-first provider)

Planned: Hunter.io, Google Sheets, SendGrid

Key principle: Extend legacy systems instead of replacing them.

🔐 Security & Constraints (Non-Negotiables)

This project was built with strict constraints:

No auto-deployments

No secrets in repositories

All backend access scoped via Row-Level Security (RLS)

Usage limits enforced per subscription plan

Human-in-the-loop for high-risk automation

These constraints shaped every architectural decision.

📁 Repository Structure
reputera-portfolio/
├── README.md                # This file
├── CASE_STUDY_REPUTERA.md   # Business & technical deep dive
├── ARCHITECTURE.md          # System design & data flow
├── SECURITY.md              # RLS, limits, and threat model
├── ROADMAP.md               # MVP → Scale → Intelligence
├── CLAUDE.md                # AI agent workflow protocol
│
├── examples/
│   ├── supabase_schema.sql      # Sanitized table definitions
│   ├── wp_sync_example.php     # Simplified REST sync logic
│   └── edge_function_pseudo.ts # Pseudocode only
│
├── diagrams/                # Architecture & data-flow visuals
├── .env.example
├── .gitignore
└── LICENSE
