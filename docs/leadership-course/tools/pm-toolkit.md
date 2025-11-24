# Project Manager's Toolkit: Copilot Deployment

> **Everything you need to plan, track, and deliver a successful Copilot implementation**

This toolkit provides ready-to-use templates for project managers leading enterprise AI deployments:
- ✅ 90-Day Gantt Chart
- ✅ Resource Planning & RACI Matrix
- ✅ Risk Register (AI-specific risks)
- ✅ Sprint Planning Templates
- ✅ Communication Plan & Email Templates
- ✅ Weekly Status Report Format
- ✅ Decision Log
- ✅ Budget Tracking

**Who should use this:** Project Managers, PMO Leads, Implementation Leads, Change Managers

---

## Template 1: 90-Day Gantt Chart

### Phase 1: Foundation (Weeks 1-2)

| Week | Task | Owner | Dependencies | Status | Notes |
|------|------|-------|--------------|--------|-------|
| **Week 1** |
| W1-D1 | Kick-off meeting with AI Council | PMO Lead | Exec sponsor confirmed | 🔴 Not Started | Book 2-hour session |
| W1-D1-2 | Define success criteria (2+ hrs/week saved) | AI Council | None | 🔴 Not Started | Document in charter |
| W1-D2-3 | Permissions audit (SharePoint/OneDrive) | IT Security | Access to Purview | 🔴 Not Started | Use PowerShell script |
| W1-D3-5 | Identify overshared files | IT Security | Permissions audit | 🔴 Not Started | Prioritize top 500 |
| W1-D4-5 | Draft AI Council Charter | PMO Lead | Council formed | 🔴 Not Started | Use template |
| **Week 2** |
| W2-D1-2 | Configure DLP policies (PII, financial) | IT Security | Overshared files identified | 🔴 Not Started | Test in audit mode first |
| W2-D2-3 | Enable sensitivity labels (top 1,000 docs) | IT Security | DLP configured | 🔴 Not Started | Start with "Confidential" |
| W2-D3-4 | Define 3 high-value scenarios | Business Analyst | AI Council input | 🔴 Not Started | Use 7-Step Framework |
| W2-D4-5 | Recruit 10 Champions (pilot group) | Change Manager | Exec sponsor endorsement | 🔴 Not Started | 5% of pilot cohort |
| W2-D5 | AI Council Meeting #2: Approve pilot plan | AI Council | All Week 2 tasks | 🔴 Not Started | Go/No-Go decision |

**Deliverables (Week 1-2):**
- [ ] AI Council Charter (signed)
- [ ] Overshared files remediated (top 500)
- [ ] DLP policies active (test mode)
- [ ] 3 scenarios documented
- [ ] 10 Champions recruited
- [ ] Pilot plan approved

---

### Phase 2: Pilot (Weeks 3-10)

| Week | Task | Owner | Dependencies | Status | Notes |
|------|------|-------|--------------|--------|-------|
| **Week 3** |
| W3-D1 | Assign Copilot licenses to pilot group (50-100 users) | IT Admin | Pilot cohort selected | 🔴 Not Started | IT + Champions first |
| W3-D1-2 | Launch Viva Engage community for pilot | Change Manager | None | 🔴 Not Started | Seed with 5 prompts |
| W3-D2-3 | Champions training session (3 hours) | PMO Lead | Champions recruited | 🔴 Not Started | Hands-on workshop |
| W3-D3-4 | Pilot kick-off town hall (1 hour) | Exec Sponsor | Licenses assigned | 🔴 Not Started | Record for absentees |
| W3-D4-5 | Daily monitoring: adoption %, support tickets | PMO Lead | Pilot launched | 🔴 Not Started | Use Viva Insights |
| **Week 4** |
| W4-D1-5 | Champions host "Office Hours" (3× 1-hour sessions) | Champions | Week 1 adoption data | 🔴 Not Started | M/W/F schedule |
| W4-D3 | Mid-pilot pulse survey (10 questions) | Change Manager | None | 🔴 Not Started | Microsoft Forms |
| W4-D5 | AI Council Meeting #3: Mid-pilot review | AI Council | Week 1-2 data | 🔴 Not Started | Decision: continue/pivot |
| **Week 5-7** |
| W5-7 | Continue Champions Office Hours (weekly) | Champions | Ongoing | 🔴 Not Started | Track attendance |
| W5-7 | Harvest scenarios from pilot users | Business Analyst | Viva Engage active | 🔴 Not Started | Add to library |
| W5-7 | Address support tickets / refine prompts | PMO Lead | Ongoing | 🔴 Not Started | Update scenario cards |
| W5-7 | Weekly metrics review (adoption, time saved) | PMO Lead | Ongoing | 🔴 Not Started | Dashboard updates |
| **Week 8** |
| W8-D1-3 | Final pilot survey (20 questions) | Change Manager | None | 🔴 Not Started | Quantitative + qualitative |
| W8-D3-4 | Analyze pilot results (adoption, ROI, incidents) | Business Analyst | Survey complete | 🔴 Not Started | Build exec report |
| W8-D5 | AI Council Meeting #4: Go/No-Go for enterprise | AI Council | Results analyzed | 🔴 Not Started | Present ROI data |
| **Week 9-10** |
| W9-10 | Refine scenarios based on pilot feedback | Business Analyst | Go decision | 🔴 Not Started | Prioritize top 10 |
| W9-10 | Update DLP policies (lessons learned) | IT Security | Go decision | 🔴 Not Started | Move from test to enforce |
| W9-10 | Plan enterprise rollout (4 waves) | PMO Lead | Go decision | 🔴 Not Started | Use wave template below |

**Deliverables (Week 3-10):**
- [ ] Pilot adoption: 60%+ active usage
- [ ] Time saved: 2+ hours/week per user
- [ ] Zero security incidents
- [ ] Scenario library: 10+ prompts curated
- [ ] Go/No-Go decision documented
- [ ] Enterprise rollout plan approved

---

### Phase 3: Enterprise Rollout (Weeks 11-20)

| Week | Wave | Cohort | Size | Owner | Status |
|------|------|--------|------|-------|--------|
| W11-12 | **Wave 1** | IT + Early Adopters | 200-500 | IT Lead | 🔴 Not Started |
| W13-15 | **Wave 2** | High-Value Business Units | 500-1,000 | BU Leaders | 🔴 Not Started |
| W16-18 | **Wave 3** | Remaining Business Units | 1,000-2,000 | BU Leaders | 🔴 Not Started |
| W19-20 | **Wave 4** | Final rollout + stragglers | All remaining | PMO Lead | 🔴 Not Started |

**Rollout Activities (Per Wave):**
1. Week N-1: Pre-wave communication (email, town hall)
2. Week N-Day 1: Assign licenses
3. Week N-Day 2: Champions host office hours (3× per week)
4. Week N-Day 3-5: Daily monitoring (adoption, support tickets)
5. Week N+1: Mid-wave pulse survey
6. Week N+2: Post-wave retrospective

**Deliverables (Week 11-20):**
- [ ] 4 waves deployed on schedule
- [ ] Enterprise adoption: 70%+ active usage
- [ ] Scenario library: 50+ prompts
- [ ] Zero major security incidents
- [ ] Executive dashboard live

---

## Visual Gantt Chart (ASCII)

```
PHASE 1: FOUNDATION (Weeks 1-2)
════════════════════════════════════════════════════════════
Task                                    W1  W2
────────────────────────────────────────────────────────────
AI Council Formation                   ███
Permissions Audit                      ████
DLP Configuration                          ████
Scenario Definition                        ███
Champions Recruitment                      ████
Go/No-Go Decision                           ▼

PHASE 2: PILOT (Weeks 3-10)
════════════════════════════════════════════════════════════
Task                            W3  W4  W5  W6  W7  W8  W9  W10
──────────────────────────────────────────────────────────────────
License Assignment             ██
Champions Training             ███
Pilot Launch                   ████
Office Hours                       ████████████████████████
Pulse Surveys                      ▼               ▼
Mid-Pilot Review                   ▼
Results Analysis                                   ████
Go/No-Go Enterprise                                    ▼
Rollout Planning                                       ████████

PHASE 3: ENTERPRISE ROLLOUT (Weeks 11-20)
════════════════════════════════════════════════════════════
Wave                    W11 W12 W13 W14 W15 W16 W17 W18 W19 W20
──────────────────────────────────────────────────────────────────
Wave 1 (IT + Early)    ████████
Wave 2 (High-Value BUs)        ████████████
Wave 3 (Remaining BUs)                    ████████████
Wave 4 (Final)                                        ████████
Continuous Support      ══════════════════════════════════════════
```

---

## Template 2: Resource Planning & RACI Matrix

### Resource Requirements (90-Day Deployment)

| Role | Time Commitment | Duration | Headcount | Total Hours | Hourly Cost | Total Cost |
|------|-----------------|----------|-----------|-------------|-------------|------------|
| **Executive Sponsor** | 2 hrs/week | 20 weeks | 1 | 40 hrs | £200 | £8,000 |
| **PMO Lead** | Full-time | 20 weeks | 1 | 800 hrs | £75 | £60,000 |
| **IT Security Lead** | 20 hrs/week | 12 weeks | 1 | 240 hrs | £80 | £19,200 |
| **Change Manager** | 20 hrs/week | 20 weeks | 1 | 400 hrs | £70 | £28,000 |
| **Business Analyst** | 15 hrs/week | 20 weeks | 1 | 300 hrs | £65 | £19,500 |
| **Champions** | 2 hrs/week | 20 weeks | 10 | 400 hrs | £50 | £20,000 |
| **IT Admin** | 10 hrs/week | 20 weeks | 1 | 200 hrs | £60 | £12,000 |
| **TOTAL** | | | | **2,380 hrs** | | **£166,700** |

**Add 30% for meetings, rework, delays:** £166,700 × 1.3 = **£216,710 internal labor cost**

---

### RACI Matrix

| Task | Exec Sponsor | PMO Lead | IT Security | Change Mgr | Business Analyst | Champions | IT Admin |
|------|--------------|----------|-------------|------------|------------------|-----------|----------|
| **Form AI Council** | **A** | **R** | C | C | C | I | I |
| **Permissions Audit** | I | **A** | **R** | I | I | I | C |
| **DLP Configuration** | **A** | C | **R** | I | I | I | **R** |
| **Define Scenarios** | **A** | C | I | C | **R** | C | I |
| **Recruit Champions** | C | **A** | I | **R** | I | I | I |
| **Pilot Launch** | **A** | **R** | C | C | C | C | **R** |
| **Champions Training** | I | **A** | I | **R** | C | C | I |
| **Office Hours** | I | C | I | C | I | **R** | I |
| **Measure ROI** | C | **A** | I | C | **R** | I | I |
| **Go/No-Go Decision** | **A** | C | C | C | **R** | I | I |
| **Enterprise Rollout** | **A** | **R** | C | C | C | C | C |

**Legend:**
- **R** = Responsible (does the work)
- **A** = Accountable (final decision maker)
- **C** = Consulted (provides input)
- **I** = Informed (kept in the loop)

---

## Template 3: Risk Register (AI-Specific Risks)

| Risk ID | Risk Description | Probability | Impact | Risk Score | Mitigation Strategy | Owner | Status |
|---------|------------------|-------------|--------|------------|---------------------|-------|--------|
| **R-001** | Low pilot adoption (< 50%) | Medium | High | 🔴 12 | Champions Network, strong change mgmt, exec visibility | Change Mgr | Open |
| **R-002** | Security incident (oversharing) | Low | Critical | 🟡 8 | Permissions audit BEFORE pilot, DLP policies | IT Security | Mitigated |
| **R-003** | Delayed rollout (6+ months) | Medium | Medium | 🟡 9 | AI Council makes fast decisions, avoid analysis paralysis | PMO Lead | Open |
| **R-004** | Overestimated time savings | Medium | Medium | 🟡 9 | Measure baselines before pilot, track rigorously | Business Analyst | Open |
| **R-005** | Budget overrun (30%+) | Low | Medium | 🟢 6 | Fixed-price consulting, phased approach | PMO Lead | Open |
| **R-006** | Executive sponsor leaves | Low | High | 🟡 8 | Secure backup sponsor, document strategy | Exec Sponsor | Open |
| **R-007** | Prompt injection attack | Low | High | 🟡 8 | DLP prompt injection rules, user training | IT Security | Mitigated |
| **R-008** | Compliance audit failure | Low | Critical | 🟡 10 | Enable audit logging, document AI governance | IT Security | Open |
| **R-009** | User resistance ("AI will take my job") | High | Medium | 🟡 12 | Transparent comms, showcase wins, HR support | Change Mgr | Open |
| **R-010** | Custom agent PII leak | Low | Critical | 🟡 10 | Row-level security, output redaction, testing | IT Security | Open |

**Risk Score Formula:** Probability (1-5) × Impact (1-5) = Score (1-25)
- 🔴 High Risk (15-25): Escalate to AI Council immediately
- 🟡 Medium Risk (8-14): Monitor closely, mitigation plan required
- 🟢 Low Risk (1-7): Accept or monitor

**Risk Review Cadence:** Weekly (Weeks 1-8), Bi-weekly (Weeks 9-20)

---

## Template 4: Sprint Planning (Agile Approach)

### Sprint Structure (2-Week Sprints)

**Sprint 1 (Weeks 1-2): Foundation**
- **Goal:** AI Council formed, security baseline established, 3 scenarios defined
- **User Stories:**
  - As an Executive Sponsor, I want an AI Council charter so the governance structure is clear
  - As a CISO, I want overshared files remediated so we prevent data leaks
  - As a Business Analyst, I want 3 validated scenarios so we know what to pilot
- **Definition of Done:**
  - AI Council charter signed by all members
  - Top 500 overshared files permissions fixed
  - 3 scenario cards complete (7-Step Framework)
  - DLP policies configured (test mode)

**Sprint 2 (Weeks 3-4): Pilot Launch**
- **Goal:** Pilot launched to 50-100 users, Champions trained, office hours running
- **User Stories:**
  - As a Pilot User, I want access to Copilot so I can start testing scenarios
  - As a Champion, I want training so I can support my peers
  - As a Change Manager, I want a feedback loop so I can address issues
- **Definition of Done:**
  - 50-100 users have active Copilot licenses
  - 10 Champions trained (3-hour workshop)
  - Viva Engage community has 20+ members
  - First office hours session completed (5+ attendees)

**Sprint 3 (Weeks 5-6): Pilot Mid-Point**
- **Goal:** 60%+ adoption, 2+ hrs/week time savings, scenario library growing
- **User Stories:**
  - As a PMO Lead, I want adoption metrics so I can track progress
  - As a Business Analyst, I want user feedback so I can refine scenarios
  - As an IT Security Lead, I want incident reports so I know DLP is working
- **Definition of Done:**
  - 60%+ of pilot users active in last 7 days
  - Pulse survey responses collected (70%+ response rate)
  - 5 new scenarios added to library (harvested from users)
  - Zero security incidents logged

**Sprint 4 (Weeks 7-8): Pilot Wrap-Up**
- **Goal:** Pilot results analyzed, Go/No-Go decision made, enterprise plan approved
- **User Stories:**
  - As an Executive Sponsor, I want ROI data so I can approve enterprise rollout
  - As an AI Council, I want lessons learned so we avoid pilot mistakes
  - As a PMO Lead, I want a rollout plan so I know what happens next
- **Definition of Done:**
  - Final pilot survey complete (80%+ response rate)
  - ROI calculation: time saved × hourly cost
  - AI Council approval for enterprise rollout
  - 4-wave rollout plan documented

**Sprint 5-10 (Weeks 9-20): Enterprise Rollout**
- Each sprint covers 1-2 weeks of a rollout wave
- Repeat pattern: Pre-wave comms → License assignment → Office hours → Post-wave review

---

## Template 5: Communication Plan & Email Templates

### Communication Calendar

| Week | Audience | Channel | Message | Owner |
|------|----------|---------|---------|-------|
| **W-2** | All Employees | Email + Town Hall | "AI is coming - here's why" | Exec Sponsor |
| **W-1** | Pilot Cohort | Email | "You've been selected for pilot" | Change Mgr |
| **W1** | Pilot Cohort | Teams Meeting | Pilot kick-off (1 hour) | Exec Sponsor |
| **W1-10** | Pilot Cohort | Viva Engage | Daily tips, prompt of the day | Champions |
| **W4** | Pilot Cohort | Email | Mid-pilot check-in | Change Mgr |
| **W8** | AI Council | Meeting | Go/No-Go decision | PMO Lead |
| **W9** | All Employees | Email + Town Hall | "Pilot success - enterprise rollout begins" | Exec Sponsor |
| **W11-20** | Wave Cohorts | Email | "Your wave starts next week" (×4 waves) | Change Mgr |
| **W20** | All Employees | Email | "We're fully deployed - here's the impact" | Exec Sponsor |

---

### Email Template: T-7 Days (Pilot Starts Next Week)

**Subject:** You're in! Copilot Pilot Starts Monday - Here's What to Expect

**From:** [Change Manager Name]

**Body:**

Hi [FirstName],

You've been selected for our **Microsoft Copilot pilot** starting **[Date]**. You're among the first 100 employees to get access - congratulations!

**Why were you selected?**
You're an early adopter, a trusted voice in your team, or work in a high-value scenario area (sales, customer service, finance, etc.). We need your feedback to shape our enterprise rollout.

**What happens Monday?**
- ✅ You'll receive your Copilot license (check Outlook/Teams for new features)
- ✅ Attend our 1-hour kick-off town hall (Monday, 10am) [[Link]]
- ✅ Join our Viva Engage community to share prompts and ask questions [[Link]]

**What are we testing?**
We've identified 3 high-value scenarios to start:
1. **Meeting Recap & Action Items** (save 20 mins/meeting)
2. **Email Triage & Summarization** (save 30 mins/day)
3. **Document Summarization** (save 45 mins/doc)

You'll find copy-paste prompts in our Viva Engage community.

**Champions are here to help**
Our 10 Champions are hosting **"Copilot Office Hours"** 3× per week (M/W/F, 2-3pm). Drop in with questions!

**This is safe**
- DLP policies are active (blocking PII, financial data)
- Your data stays in the UK (M365 tenant)
- IT Security is monitoring 24/7

**We need your feedback**
- Week 4: Mid-pilot pulse survey (5 mins)
- Week 8: Final pilot survey (10 mins)

**Questions?**
Reply to this email or post in Viva Engage.

Let's make this pilot a success!

[Change Manager Name]
[Project Title] | [Contact Info]

---

### Email Template: Week 8 (Pilot Results)

**Subject:** Pilot Success! Here's What We Achieved (And What's Next)

**From:** [Executive Sponsor Name]

**Body:**

Team,

I'm thrilled to share the results of our 8-week **Microsoft Copilot pilot** with 100 employees.

**The Numbers:**
- ✅ **85% adoption rate** (vs. 40% typical for enterprise software)
- ✅ **3 hours saved per employee per week** (exceeded our 2-hour target)
- ✅ **Zero security incidents** (DLP policies worked perfectly)
- ✅ **30% increase in employee satisfaction** ("I can focus on real work, not admin")

**What did we learn?**
- **Meeting Recap** was the #1 scenario (95% of pilot users love it)
- **Email Triage** saved executives 30 mins/day (game-changer)
- **Champions were 3× more effective than training videos** (peer learning works)

**What's next?**
The AI Council has **approved enterprise rollout** starting Week 11. We'll deploy in 4 waves over 10 weeks:
- Wave 1 (W11-12): IT + Early Adopters (200 people)
- Wave 2 (W13-15): High-Value Business Units (500 people)
- Wave 3 (W16-18): Remaining Business Units (1,000 people)
- Wave 4 (W19-20): Everyone else

**Will you get it?**
Yes! Every employee will have Copilot by [Date]. You'll get an email 1 week before your wave.

**ROI for the business:**
- Annual productivity gains: **£10.8M** (based on pilot data)
- Total investment: **£745k** (licenses + implementation)
- **Year 1 ROI: 1,349%** (we'll break even in 25 days)

Thank you to our 100 pilot users and 10 Champions. You've proven that AI can transform how we work.

[Executive Sponsor Name]
[Title] | [Company]

---

## Template 6: Weekly Status Report

**To:** AI Council, Executive Sponsor
**From:** PMO Lead
**Date:** [Week X]
**Subject:** Copilot Deployment - Week X Status

---

### 📊 **Key Metrics (This Week)**

| Metric | Target | Actual | Status |
|--------|--------|--------|--------|
| Active Users | 60% | 78% | 🟢 Ahead |
| Time Saved/Week (avg) | 2 hours | 2.5 hours | 🟢 Ahead |
| Security Incidents | 0 | 0 | 🟢 On Track |
| Support Tickets | < 20/week | 12 | 🟢 Ahead |
| Champions Office Hours Attendance | 5+ per session | 8 | 🟢 Ahead |

---

### ✅ **Completed This Week**
1. Launched pilot to 100 users (IT + Champions)
2. Hosted kick-off town hall (85 attendees)
3. Champions completed 3 office hours sessions (24 total attendees)
4. Added 5 new scenarios to library (harvested from Viva Engage)
5. Zero security incidents (DLP policies working)

---

### 🚧 **In Progress**
1. Mid-pilot pulse survey (launching Friday)
2. Refining "Email Triage" prompt (30% of users report vague results)
3. Monitoring adoption by department (Sales lagging at 45%)

---

### 🔴 **Blockers / Risks**
1. **Risk:** Sales team adoption lower than expected (45% vs. 70% target)
   - **Mitigation:** BU Leader to host department-specific office hours next week
2. **Risk:** 15% of users report "not sure how to get started"
   - **Mitigation:** Champions creating "Quick Start Guide" (1-pager)

---

### 📅 **Next Week's Priorities**
1. Launch mid-pilot pulse survey (target: 70%+ response rate)
2. Host Sales-specific office hours (Champion + BU Leader)
3. Review DLP policy effectiveness (any false positives?)
4. Prepare Week 8 results for AI Council presentation

---

### 💬 **Quotes from Users**
> "Meeting recap saves me 20 minutes per meeting. I'll never take manual notes again." - Sales Rep

> "Email triage is a game-changer. I catch up on 100 emails in 10 minutes." - CFO

> "I was skeptical, but after trying the prompts in the scenario library, I'm sold." - Customer Service Manager

---

### 🆘 **Support Needed**
- AI Council: Approve 2 new scenarios (Legal Contract Review, Finance Reporting)
- Executive Sponsor: Record 2-minute video testimonial for enterprise rollout comms

---

**Overall Status:** 🟢 On Track

[PMO Lead Name]
[Contact Info]

---

## Template 7: Decision Log

| Date | Decision | Rationale | Owner | Status | Impact |
|------|----------|-----------|-------|--------|--------|
| 2025-01-15 | Start with Customer Service team (400 people) for pilot | Highest pain point, most receptive to change | AI Council | ✅ Done | 85% adoption achieved |
| 2025-01-15 | Deploy DLP policies BEFORE pilot | Prevent security incidents (see Module 4 incidents) | CISO | ✅ Done | Zero incidents in pilot |
| 2025-02-01 | Add Sales team to pilot cohort | Customer Service pilot successful, expand to 500 more | AI Council | ✅ Done | Total pilot: 900 users |
| 2025-02-15 | Approve enterprise rollout | Pilot exceeded success criteria (85% adoption, 3 hrs/week saved) | AI Council | ✅ Done | 10,000 licenses approved |
| 2025-02-15 | Greenlight 2 custom agents (HR Policy, IT Service Desk) | High ROI potential (Teladoc case study: 4,000% ROI) | AI Council | 🟡 In Progress | Build starts Week 12 |
| 2025-03-01 | Expand Champions Network from 10 to 150 (5% of workforce) | Champions 3× more effective than training videos | AI Council | 🟡 In Progress | Recruiting now |

**Usage:** Review decision log at every AI Council meeting to track accountability and outcomes.

---

## Template 8: Budget Tracking

### Budget vs. Actuals (90-Day Deployment)

| Category | Budget | Actual | Variance | % of Budget | Status |
|----------|--------|--------|----------|-------------|--------|
| **Microsoft Copilot Licenses** | £148,200 | £148,200 | £0 | 100% | 🟢 On Budget |
| **Consulting (Strategy + Pilot)** | £52,500 | £48,000 | +£4,500 | 91% | 🟢 Under Budget |
| **Internal Labor** | £216,710 | £225,000 | -£8,290 | 104% | 🟡 Slight Overrun |
| **Training & Change Mgmt** | £25,000 | £22,500 | +£2,500 | 90% | 🟢 Under Budget |
| **Tools & Software** | £5,000 | £3,200 | +£1,800 | 64% | 🟢 Under Budget |
| **Contingency (10%)** | £44,741 | £0 | +£44,741 | 0% | 🟢 Not Used |
| **TOTAL** | **£492,151** | **£446,900** | **+£45,251** | **91%** | 🟢 Under Budget |

**Key Insights:**
- Internal labor overrun due to additional scenario refinement (8% over)
- Consulting under budget (negotiated fixed price)
- Contingency not needed (project on track)
- **Overall: 9% under budget** (£45k available for custom agents)

---

## How to Use These Templates

### For Excel/Google Sheets Users
1. **Copy Gantt Chart** to Excel, format with conditional formatting (color-code by status)
2. **RACI Matrix** → Use Excel, freeze top row, filter by role
3. **Risk Register** → Use Excel, sort by Risk Score, add dropdown for Status
4. **Budget Tracker** → Use Excel with formulas: `Variance = Budget - Actual`, `% = Actual/Budget`

### For Project Management Software Users
**If you use Jira, Asana, Monday.com, MS Project:**
1. Import Gantt Chart as CSV
2. Create custom fields for RACI, Risk Score, Budget Variance
3. Set up automations (e.g., "If Status = Red, notify AI Council")

### For AI-Powered PM Tools
**If you use Notion, ClickUp, or similar:**
1. Use database views (Gantt, Kanban, Calendar)
2. Automate status updates with Zapier (e.g., "If survey response > 70%, mark as Green")
3. Embed Viva Engage feed for real-time user feedback

---

## Download Templates

- **[Excel Gantt Chart](#)** - Coming soon
- **[Google Sheets Bundle (All Templates)](#)** - Coming soon
- **[MS Project File](#)** - Coming soon

---

## Support for Project Managers

Need help tailoring these templates to your organization?

📧 **Email:** ram@aicapabilitybuilder.com
💼 **LinkedIn:** [linkedin.com/in/rammaree](https://linkedin.com/in/rammaree)
📅 **Book a 30-min PM Coaching Session:** [Free consultation]

**We can help with:**
- ✅ Customizing Gantt chart for your timeline
- ✅ Resourcing your team (internal vs. consulting)
- ✅ Identifying and mitigating AI-specific risks
- ✅ Building stakeholder communication plans
- ✅ Tracking ROI throughout deployment

---

**Toolkit Version:** 1.0 (January 2025)
**Based on:** 100+ successful enterprise deployments
**Updated quarterly** with lessons learned from new implementations
