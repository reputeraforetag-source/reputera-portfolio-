# CLAUDE.MD - Reputera.se Agent Protocol

## CONTEXT
I am building Reputera.se, an ORM SaaS.
- **Frontend**: WordPress + PMS
- **Backend**: Supabase (DB, Auth, RLS, Edge Functions)
- **Integrations**: PayPal, Hunter.io, Google Sheets, SendGrid.
- **Goal**: High automation (80%) with human oversight (20%).

## CONSTRAINTS
- **AI Tool**: Use only Claude Pro. Opus for planning/strategy, Sonnet for coding/implementation.
- **Security First**: All code must respect RLS policies, token protection (use environment variables), and user-specific limits (e.g., SMS).
- **Human-in-the-Loop**: Do not generate code that auto-deploys. All final decisions and deployments are made by me.

## WORKFLOWS
- **Think First**: Start complex prompts with `<think>` to analyze the problem.
- **Plan in Markdown**: Outline steps, options, and expected outcomes.
- **Code with Tests (TDD)**: Generate tests before implementation code.
- **Iterate**: Use tab/shift for code refinement.
- **Debug with Context**: I will provide screenshots and logs for debugging.

## SUBAGENTS (Personas for Claude)
- **@planner**: For creating roadmaps, feature breakdowns, and strategic plans.
- **@security**: For auditing code for vulnerabilities and ensuring RLS policies are robust.
- **@doc**: For generating user guides, API documentation, and changelogs.
- **@debugger**: For analyzing errors from screenshots and logs and suggesting fixes.
