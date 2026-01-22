## **3. Technical Debugging Super Prompt**
**Filsökväg:** `prompts/04-debugging.md`


# Technical Debugging Super Prompt

## 🎯 Goal
Solve complex system issues 10x faster

## 🎪 Best For
- Software engineers
- DevOps engineers
- Site Reliability Engineers (SREs)

## 📝 The Prompt Template

You are a senior principal engineer with 20 years experience debugging complex distributed systems. Debug this issue:

SYMPTOM: [Describe what's happening]
EXPECTED: [What should happen]
ENVIRONMENT: [Tech stack, versions, deployment]
ERROR LOGS: [Paste relevant logs]
ALREADY TRIED: [List attempted solutions]

FOLLOW THIS METHOD:
1. **Root cause hypothesis** (list 3 most likely causes ranked by probability)
2. **Diagnostic steps** (exact commands to run, logs to check, metrics to monitor)
3. **Isolation procedure** (how to reproduce in controlled environment)
4. **Fix implementation** (code changes with explanations of side effects)
5. **Regression prevention** (tests to add, monitoring to implement)

FORMAT RESPONSE AS:
- Executive summary (for stakeholders)
- Technical deep dive (for engineers)
- Rollback plan (if fix fails)
- Post-mortem template
🏆 Real Results
API Performance: Reduced 500 errors by 95% in production

Database Issues: Diagnosed connection pool exhaustion in 15 minutes (vs 4 hours)

Microservices: Resolved distributed tracing issues across 8 services

🔧 Customization Tips
Always include actual error logs and stack traces

Specify your exact tech stack and versions

Mention what debugging tools you have available

Include system architecture context if relevant

📊 Success Metrics
MTTR (Mean Time To Resolution) (target: < 30 minutes)

Accuracy (target: 95%+ correct diagnoses)

Documentation quality (target: complete runbooks created)

Prevention rate (target: 80%+ issues prevented from recurring)
