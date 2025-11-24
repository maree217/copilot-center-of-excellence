# AI Readiness Maturity Assessment

> **Assess your organization's readiness to deploy Microsoft Copilot successfully**

This assessment evaluates your organization across **5 critical dimensions** to determine your AI Maturity Level (1-5). Complete all 50 questions to receive your score and personalized 90-day action plan.

**Time to Complete:** 15-20 minutes
**Output:** Maturity score (0-200 points), radar chart, gap analysis, action plan

---

## How to Use This Assessment

### Option 1: Manual Scoring (GitHub)
- Answer all 50 questions below
- Tally your points (0-4 per question)
- Use the scoring guide at the bottom to determine your maturity level
- Review the recommended action plan for your level

### Option 2: Interactive Assessment (Recommended)
**This assessment is designed to be deployed on [Tally.so](https://tally.so) (free tier) for:**
- ✅ Auto-scoring and instant results
- ✅ Visual radar chart (5 dimensions)
- ✅ Email delivery of personalized report (PDF)
- ✅ Lead capture for follow-up

**To deploy:** Copy questions to Tally.so, configure scoring, add conditional logic for action plans.

---

## Assessment Questions

### **Dimension 1: Strategy & Governance (20 points)**

**Question 1.1: Executive Sponsorship**
Do you have a C-level executive actively championing your AI initiative?

- [ ] **0 points**: No executive sponsor identified
- [ ] **1 point**: Executive aware but not actively involved
- [ ] **2 points**: Executive attends meetings but no visible advocacy
- [ ] **3 points**: Executive actively communicates AI vision
- [ ] **4 points**: Executive models behavior (uses Copilot visibly, removes blockers)

---

**Question 1.2: AI Council / Governance Body**
Have you established a cross-functional AI Council?

- [ ] **0 points**: No governance structure
- [ ] **1 point**: Considering forming a council
- [ ] **2 points**: Council formed but hasn't met
- [ ] **3 points**: Council meets monthly, making some decisions
- [ ] **4 points**: Council meets weekly (first 12 weeks), makes fast decisions, tracks outcomes

---

**Question 1.3: Business Case & Strategy**
Do you have a documented AI strategy with measurable goals?

- [ ] **0 points**: No strategy or business case
- [ ] **1 point**: Informal discussions about AI potential
- [ ] **2 points**: Draft strategy document exists
- [ ] **3 points**: Board-approved strategy with budget allocation
- [ ] **4 points**: Strategy includes specific scenarios, ROI targets, 90-day roadmap

---

**Question 1.4: Budget & Resource Allocation**
Have you allocated budget for AI deployment (licenses, consulting, training)?

- [ ] **0 points**: No budget allocated
- [ ] **1 point**: Budget request in progress
- [ ] **2 points**: Budget approved for pilot only (< 100 users)
- [ ] **3 points**: Budget approved for enterprise rollout (licenses + implementation)
- [ ] **4 points**: Multi-year budget includes licenses, custom agents, consulting, training

---

**Question 1.5: Responsible AI Standard**
Have you defined your organization's Responsible AI principles?

- [ ] **0 points**: No responsible AI guidelines
- [ ] **1 point**: Aware of need but not documented
- [ ] **2 points**: Draft principles exist (not approved)
- [ ] **3 points**: Approved principles (human-in-the-loop, bias mitigation, transparency)
- [ ] **4 points**: Principles enforced with technical controls, quarterly audits

---

### **Dimension 2: Technical Foundation (20 points)**

**Question 2.1: Microsoft 365 Licensing**
What Microsoft 365 licensing do you have?

- [ ] **0 points**: No M365 or basic Business licenses
- [ ] **1 point**: M365 Business Standard (some users)
- [ ] **2 points**: M365 E3 or Business Premium (majority of users)
- [ ] **3 points**: M365 E5 (all users) or E3 with security add-ons
- [ ] **4 points**: E5 + Copilot licenses deployed, Purview active

---

**Question 2.2: Identity & Authentication**
What is your identity and access management posture?

- [ ] **0 points**: No centralized identity management
- [ ] **1 point**: Azure AD (Entra ID) synced but no MFA
- [ ] **2 points**: MFA enabled for admins only
- [ ] **3 points**: MFA required for all users, conditional access policies active
- [ ] **4 points**: Zero Trust architecture, risk-based conditional access, privileged identity management

---

**Question 2.3: Data Governance & Permissions**
Have you audited SharePoint/OneDrive permissions?

- [ ] **0 points**: No permissions audit, files shared with "Everyone"
- [ ] **1 point**: Aware of oversharing risks but no audit done
- [ ] **2 points**: Permissions audit in progress
- [ ] **3 points**: Audit complete, overshared files identified and remediated
- [ ] **4 points**: Ongoing monitoring with Purview, automated alerts for oversharing

---

**Question 2.4: Network & Endpoint Readiness**
Are your network and devices ready for real-time AI?

- [ ] **0 points**: Unknown network status, old devices
- [ ] **1 point**: Some devices on Current Channel, network not tested
- [ ] **2 points**: Most devices on Current Channel, WebSockets allowed
- [ ] **3 points**: All devices updated, network latency tested, firewall rules configured
- [ ] **4 points**: Optimized network (private endpoints for Azure), devices meet specs, monitored

---

**Question 2.5: Data Quality & Semantic Indexing**
Is your data indexed and searchable by Copilot?

- [ ] **0 points**: No Microsoft Search indexing
- [ ] **1 point**: Semantic indexing enabled but not tested
- [ ] **2 points**: Indexing active, spot-checked for accuracy
- [ ] **3 points**: Indexing optimized, known content issues fixed
- [ ] **4 points**: Continuous data quality monitoring, taxonomy management, search quality metrics

---

### **Dimension 3: Security & Compliance (20 points)**

**Question 3.1: Data Loss Prevention (DLP)**
Do you have DLP policies configured for Copilot?

- [ ] **0 points**: No DLP policies
- [ ] **1 point**: DLP policies exist but don't cover Copilot
- [ ] **2 points**: DLP policies created for Copilot but not tested
- [ ] **3 points**: DLP policies active, blocking PII/financial data in Copilot
- [ ] **4 points**: Comprehensive DLP (PII, financial, supplier data, trade secrets), tested with pilot

---

**Question 3.2: Sensitivity Labels & Information Protection**
Are documents classified with sensitivity labels?

- [ ] **0 points**: No sensitivity labels deployed
- [ ] **1 point**: Labels created but not applied to documents
- [ ] **2 points**: Labels applied to some critical documents (< 25%)
- [ ] **3 points**: Auto-labeling active, 50%+ documents classified
- [ ] **4 points**: 90%+ documents labeled, Copilot respects labels, enforced with DLP

---

**Question 3.3: Audit Logging & eDiscovery**
Can you audit Copilot interactions for compliance?

- [ ] **0 points**: No audit logging enabled
- [ ] **1 point**: Basic audit logs (30-day retention)
- [ ] **2 points**: Audit logs enabled but Copilot not included
- [ ] **3 points**: Copilot activity logging enabled (90-day retention)
- [ ] **4 points**: 90+ day retention, eDiscovery configured, quarterly compliance audits

---

**Question 3.4: Regulatory Compliance**
Does your industry have specific compliance requirements (GDPR, HIPAA, SOC2)?

- [ ] **0 points**: Not sure / not applicable
- [ ] **1 point**: Aware of requirements but no Copilot compliance plan
- [ ] **2 points**: Compliance requirements documented for Copilot
- [ ] **3 points**: Technical controls implemented (data residency, encryption, access controls)
- [ ] **4 points**: Audited by third party, compliance documentation complete, incident response plan

---

**Question 3.5: Incident Response Plan**
Do you have a security incident response plan for AI?

- [ ] **0 points**: No incident response plan
- [ ] **1 point**: General incident response plan (no AI-specific scenarios)
- [ ] **2 points**: AI incident scenarios documented (oversharing, prompt injection)
- [ ] **3 points**: Incident response plan tested (tabletop exercise)
- [ ] **4 points**: Automated detection, defined escalation paths, quarterly drills

---

### **Dimension 4: People & Culture (20 points)**

**Question 4.1: Change Management Plan**
Do you have a change management plan for AI adoption?

- [ ] **0 points**: No change management plan
- [ ] **1 point**: Informal approach ("announce and hope")
- [ ] **2 points**: Basic communication plan (emails, town halls)
- [ ] **3 points**: Comprehensive plan (cohort rollout, engagement campaigns, training)
- [ ] **4 points**: Proven plan with Champions Network, feedback loops, anxiety management, scenario library

---

**Question 4.2: Champions Network**
Have you identified and recruited AI Champions?

- [ ] **0 points**: No champions identified
- [ ] **1 point**: Considering a champions program
- [ ] **2 points**: Champions identified but not trained
- [ ] **3 points**: Champions trained, hosting office hours
- [ ] **4 points**: 5-10% of workforce, active Viva Engage community, scenario harvesting, quarterly summits

---

**Question 4.3: Training & Enablement**
What training have you provided on AI and prompt engineering?

- [ ] **0 points**: No training planned
- [ ] **1 point**: Shared generic Microsoft videos
- [ ] **2 points**: Hosted live training sessions (low engagement)
- [ ] **3 points**: Role-based training, prompt libraries, scenario cards
- [ ] **4 points**: Copilot Academy curriculum in Viva Learning, Champions-led office hours, ongoing learning

---

**Question 4.4: Communication & Transparency**
How are you communicating the "why" and managing anxiety?

- [ ] **0 points**: No communication plan
- [ ] **1 point**: Leadership announced AI is coming
- [ ] **2 points**: Regular emails and FAQs published
- [ ] **3 points**: Transparent about goals, addressing job security concerns, showcasing wins
- [ ] **4 points**: Executive visible usage, success stories shared, feedback solicited and acted on

---

**Question 4.5: Measurement & Feedback**
How are you measuring adoption and collecting user feedback?

- [ ] **0 points**: No measurement plan
- [ ] **1 point**: Tracking license assignment only
- [ ] **2 points**: Viva Insights usage dashboard reviewed monthly
- [ ] **3 points**: Pulse surveys, time-saved metrics, adoption by scenario
- [ ] **4 points**: Real-time dashboard, weekly AI Council review, feedback loop to product improvements

---

### **Dimension 5: Scenarios & Value Realization (20 points)**

**Question 5.1: Scenario Definition**
Have you defined specific high-value AI scenarios?

- [ ] **0 points**: No scenarios defined
- [ ] **1 point**: Generic use cases ("email" and "meetings")
- [ ] **2 points**: 3-5 scenarios identified but not validated
- [ ] **3 points**: 5-10 scenarios documented using 7-Step Framework
- [ ] **4 points**: 12+ scenarios, prioritized (P1/P2/P3), validated by AI Council, prompts tested

---

**Question 5.2: Scenario Library**
Do you have a curated library of prompts accessible to users?

- [ ] **0 points**: No scenario library
- [ ] **1 point**: Informal sharing in email/chat
- [ ] **2 points**: SharePoint site with some prompts (not structured)
- [ ] **3 points**: Structured library with 10+ scenarios, searchable, role-based
- [ ] **4 points**: 50+ scenarios, user-rated, Champions curating, quarterly AI Council validation

---

**Question 5.3: Pilot Strategy**
Have you planned a pilot before enterprise rollout?

- [ ] **0 points**: No pilot planned ("deploy to everyone")
- [ ] **1 point**: Considering a pilot
- [ ] **2 points**: Pilot planned (cohort selected, timeline defined)
- [ ] **3 points**: Pilot running (50-100 users, 4-8 weeks, measuring outcomes)
- [ ] **4 points**: Pilot complete, success criteria exceeded, lessons learned documented, scaling plan approved

---

**Question 5.4: ROI Measurement**
How are you measuring business value from AI?

- [ ] **0 points**: No ROI measurement plan
- [ ] **1 point**: Plan to "see what happens"
- [ ] **2 points**: Measuring vanity metrics (logins, prompts sent)
- [ ] **3 points**: Tracking time saved per scenario (self-reported)
- [ ] **4 points**: Quantified ROI (time saved × hourly cost), qualitative wins (satisfaction, retention), executive dashboard

---

**Question 5.5: Custom Agents**
Are you planning to build custom agents beyond out-of-the-box Copilot?

- [ ] **0 points**: Not aware of custom agents
- [ ] **1 point**: Aware but no plans
- [ ] **2 points**: Identified 1-2 custom agent opportunities
- [ ] **3 points**: Building first custom agent (Copilot Studio)
- [ ] **4 points**: 2+ custom agents in production, roadmap for 5+ agents, ROI tracked per agent

---

## Scoring Guide

**Total Your Points (Max: 200 points)**

Add up your scores across all 50 questions to determine your **AI Maturity Level**:

---

### **Level 1: Exploring (0-40 points)** 🔴

**Where You Are:**
- No formal AI strategy or governance
- Limited technical readiness
- No security controls configured
- Ad-hoc tool usage without structure
- No measurement plan

**What This Means:**
You're at high risk for failed deployment. Without foundational work, a Copilot rollout will likely see < 20% adoption and could expose your organization to security incidents.

**ROI Expectation:** Negative or < 50% (more cost than value)

**Your 90-Day Action Plan:**

**Month 1: Build Foundation**
1. ✅ Secure executive sponsorship (identify C-level champion)
2. ✅ Form AI Council (first meeting in Week 2)
3. ✅ Conduct permissions audit (identify overshared files)
4. ✅ Deploy basic DLP policies (block PII, financial data)
5. ✅ Define 3 high-value scenarios (using 7-Step Framework)

**Month 2: Prepare for Pilot**
6. ✅ Configure MFA and conditional access
7. ✅ Enable sensitivity labels (classify top 1,000 documents)
8. ✅ Recruit 10 Champions (early adopters)
9. ✅ Create scenario library (10 prompts)
10. ✅ Write business case for pilot (ROI projections)

**Month 3: Run Pilot**
11. ✅ Launch pilot (50 users, IT + Champions)
12. ✅ Measure time saved on 3 scenarios
13. ✅ Address feedback, refine prompts
14. ✅ Present results to AI Council
15. ✅ Decide: Scale or pivot?

**Investment Required:** £50k-£75k (consulting + licenses for pilot)
**Expected Outcome:** Pilot proves 2+ hours/week saved, 60%+ adoption, zero incidents

---

### **Level 2: Emerging (41-80 points)** 🟡

**Where You Are:**
- Pilot underway or recently launched
- Basic security controls active
- Some governance structure exists
- Executive awareness but not full commitment
- Measuring some outcomes

**What This Means:**
You've started but lack depth. You're at risk of stalled adoption or security gaps. You need to tighten governance, scale champions, and prove ROI to justify enterprise rollout.

**ROI Expectation:** 50-200% (modest value, room for improvement)

**Your 90-Day Action Plan:**

**Month 1: Strengthen Governance**
1. ✅ AI Council to meet weekly (if not already)
2. ✅ Document Responsible AI Standard
3. ✅ Expand DLP policies (add supplier data, trade secrets)
4. ✅ Enable Copilot audit logging (90-day retention)
5. ✅ Define success metrics for enterprise rollout

**Month 2: Scale Adoption**
6. ✅ Grow Champions Network to 5% of workforce
7. ✅ Launch Viva Engage community for prompt sharing
8. ✅ Build 5 more scenarios (role-specific)
9. ✅ Host weekly "Copilot Office Hours"
10. ✅ Measure ROI from pilot (time saved × cost)

**Month 3: Prepare Enterprise Rollout**
11. ✅ Secure budget for enterprise licenses
12. ✅ Design 4-wave rollout plan (IT → Early Adopters → BUs → All)
13. ✅ Identify 1-2 custom agent opportunities
14. ✅ Create executive dashboard (adoption, time saved, incidents)
15. ✅ Get AI Council approval to scale

**Investment Required:** £100k-£150k (enterprise licenses + consulting)
**Expected Outcome:** 70%+ adoption, 3 hours/week saved, 500%+ ROI in Year 1

---

### **Level 3: Defined (81-120 points)** 🟢

**Where You Are:**
- Enterprise rollout underway or complete
- Comprehensive security controls active
- Champions Network driving adoption
- Measuring time saved per scenario
- AI Council meeting regularly

**What This Means:**
You're executing well. Now focus on optimization: custom agents, scenario expansion, and continuous improvement. You're likely seeing 500-1,000% ROI.

**ROI Expectation:** 500-1,500% (strong value, proven framework)

**Your 90-Day Action Plan:**

**Month 1: Optimize Core Deployment**
1. ✅ Review adoption by department (identify laggards)
2. ✅ Expand scenario library to 50+ prompts
3. ✅ Conduct quarterly AI Council audit
4. ✅ Refine DLP policies based on incident logs
5. ✅ Launch "Scenario of the Month" campaign

**Month 2: Build Custom Agents**
6. ✅ Prioritize top 2 custom agent opportunities (HR Policy? IT Service Desk?)
7. ✅ Build first custom agent in Copilot Studio
8. ✅ Test with 50 users (Champions first)
9. ✅ Measure ROI per agent (tickets automated, time saved)
10. ✅ Document build process for repeatability

**Month 3: Scale Custom Agents**
11. ✅ Deploy 2 custom agents to production
12. ✅ Identify 3 more agent opportunities
13. ✅ Train Champions on Copilot Studio basics
14. ✅ Build roadmap for 10 custom agents
15. ✅ Share results with executive leadership

**Investment Required:** £150k-£250k (custom agent development + Power Platform licenses)
**Expected Outcome:** 2,000%+ ROI with custom agents, 85%+ adoption

---

### **Level 4: Managed (121-160 points)** 🟢🟢

**Where You Are:**
- Full enterprise adoption achieved
- Custom agents in production
- Automated testing and monitoring
- ROI tracking dashboard live
- Industry-leading adoption rates

**What This Means:**
You're in the top 10% of organizations. Focus on innovation: advanced agents, AI-first workflows, and industry thought leadership. Share your story.

**ROI Expectation:** 2,000-4,000% (exceptional value, custom agents driving massive savings)

**Your 90-Day Action Plan:**

**Month 1: Drive Innovation**
1. ✅ Launch "AI Innovation Challenge" (employees pitch agent ideas)
2. ✅ Build 2 more advanced custom agents
3. ✅ Integrate Copilot with line-of-business systems (CRM, ERP)
4. ✅ Document case study for internal sharing
5. ✅ Apply for industry awards

**Month 2: Optimize at Scale**
6. ✅ Conduct user research (what's not working?)
7. ✅ Implement Agent Lightning (RL-based optimization)
8. ✅ Build predictive analytics (who will churn? who needs help?)
9. ✅ Expand Champions to 10% of workforce (tiered levels)
10. ✅ Publish internal blog series on AI success

**Month 3: Thought Leadership**
11. ✅ Present at industry conference
12. ✅ Publish external case study (ROI results)
13. ✅ Partner with Microsoft (reference customer)
14. ✅ Launch "AI Center of Excellence" for broader org
15. ✅ Plan next wave: multi-agent orchestration

**Investment Required:** £200k-£400k (advanced agents, consulting, innovation labs)
**Expected Outcome:** 4,000%+ ROI, market differentiation, talent attraction

---

### **Level 5: Optimizing (161-200 points)** 🏆

**Where You Are:**
- AI-first culture embedded
- Continuous innovation and learning
- 10+ custom agents in production
- Industry thought leader
- Attracting top talent due to AI maturity

**What This Means:**
You're in the top 1%. You're not just using AI—you're defining how your industry uses it. Focus on ecosystem, partnerships, and scaling your learnings to other organizations.

**ROI Expectation:** 5,000%+ (transformational value, competitive advantage)

**Your 90-Day Action Plan:**

**Month 1: Ecosystem Leadership**
1. ✅ Launch external consulting practice (teach others)
2. ✅ Build open-source scenario library (GitHub)
3. ✅ Partner with Microsoft on product roadmap input
4. ✅ Recruit AI Product Manager (dedicated role)
5. ✅ Establish quarterly "AI Summit" (external guests)

**Month 2: Advanced Architecture**
6. ✅ Deploy multi-agent orchestration (agents calling agents)
7. ✅ Build self-learning agents (feedback loops, RL)
8. ✅ Integrate Azure AI Foundry (model fine-tuning)
9. ✅ Experiment with custom models (Semantic Kernel)
10. ✅ Publish research paper on AI ROI

**Month 3: Scale Impact**
11. ✅ Launch "AI Academy" (internal university)
12. ✅ Certify employees as "AI Practitioners"
13. ✅ Build M&A playbook (AI due diligence for acquisitions)
14. ✅ Establish AI Ethics Board (separate from Council)
15. ✅ Plan keynote at major industry event

**Investment Required:** £500k+ (innovation budget, R&D, external thought leadership)
**Expected Outcome:** Market-leading AI adoption, 10,000%+ ROI, competitive moat

---

## Visual Radar Chart (Plot Your Scores)

Plot your scores for each dimension on this radar chart:

```
         Strategy & Governance (20)
                    |
                    |
      People &     / \     Technical
      Culture     /   \    Foundation
      (20)       /     \   (20)
                /       \
               /         \
              /           \
             /             \
            /               \
           /_________________\
    Scenarios &           Security &
    Value (20)           Compliance (20)
```

**Instructions:**
1. Calculate your score per dimension (0-20 points each)
2. Mark your score on each axis
3. Connect the dots to form your "readiness profile"
4. Gaps show where to focus investment

**Example: A "Level 2" organization might score:**
- Strategy & Governance: 8/20 (need AI Council)
- Technical Foundation: 12/20 (good licensing, weak permissions audit)
- Security & Compliance: 6/20 (critical gap)
- People & Culture: 10/20 (some training, no Champions)
- Scenarios & Value: 5/20 (no defined scenarios)

**Total: 41 points = Level 2 (Emerging)**

---

## Gap Analysis: What to Fix First

Based on your scores, prioritize these actions:

**If your lowest score is...**

### **Strategy & Governance (< 8 points)**
🚨 **Critical:** Without governance, you'll make poor decisions
- **Week 1:** Secure executive sponsor
- **Week 2:** Form AI Council, schedule first meeting
- **Week 3:** Document 3 scenarios before pilot

### **Technical Foundation (< 8 points)**
🚨 **Critical:** You'll face technical blockers mid-pilot
- **Week 1:** Audit M365 licensing, upgrade if needed
- **Week 2:** Enable MFA for all users
- **Week 3:** Test network readiness (WebSockets, latency)

### **Security & Compliance (< 8 points)**
🔥 **Urgent:** You're at high risk for security incidents (see Module 4)
- **Week 1:** Permissions audit (find overshared files)
- **Week 2:** Deploy DLP policies (block PII, financials)
- **Week 3:** Enable sensitivity labels on top 500 docs

### **People & Culture (< 8 points)**
⚠️ **Important:** Adoption will stall without change management
- **Week 1:** Draft communication plan
- **Week 2:** Recruit 10 Champions
- **Week 3:** Create scenario library (10 prompts)

### **Scenarios & Value (< 8 points)**
⚠️ **Important:** You won't prove ROI without defined scenarios
- **Week 1:** Define 3 scenarios using 7-Step Framework
- **Week 2:** Test prompts with 5 users
- **Week 3:** Measure time saved (baseline vs. with Copilot)

---

## Next Steps

### For Level 1-2 Organizations
**Don't deploy yet.** Spend 8-12 weeks building your foundation:
1. Complete the action plan for your level
2. Re-take this assessment in 90 days (target: +40 points)
3. Pilot when you're at Level 3 (80+ points)

**Recommended Investment:** £50k-£150k (consulting to build foundation)

### For Level 3-4 Organizations
**You're ready to scale.** Focus on:
1. Custom agents (HR, IT, Legal use cases)
2. Expanding Champions Network to 10%
3. Quarterly optimization cycles

**Recommended Investment:** £150k-£400k (custom agents + ongoing optimization)

### For Level 5 Organizations
**You're a leader.** Consider:
1. Publishing your results externally
2. Offering consulting to peers
3. Partnering with Microsoft as reference customer

**Recommended Investment:** £500k+ (innovation budget, thought leadership)

---

## How to Move This to Tally.so (Interactive)

**To create an auto-scoring web version:**

1. **Sign up for Tally.so** (free tier supports 50 questions, unlimited responses)

2. **Create a new form** with these field types:
   - **Section headers** for each dimension (5 sections)
   - **Multiple choice (single select)** for each question (50 questions)
   - **Hidden fields** for scoring calculations

3. **Configure scoring:**
   - Assign point values to each answer (0-4)
   - Create calculated fields: `Dimension1Score`, `TotalScore`
   - Add conditional logic: IF TotalScore < 40, show Level 1 action plan

4. **Customize output:**
   - Use Tally's "Thank You" page to show instant results
   - Display radar chart (use Tally's chart integration or link to external viz)
   - Email PDF report using Zapier integration

5. **Lead capture:**
   - Add fields: Name, Email, Company, Role
   - Export to CRM for follow-up

6. **Embed on website:**
   - Copy Tally embed code
   - Add to GitHub Pages or landing page
   - Drive traffic via LinkedIn, email campaigns

**Estimated Setup Time:** 4-6 hours (form creation + scoring logic + design)

---

## Contact for Assessment Support

Need help interpreting your results or building your 90-day plan?

📧 **Email:** ram@aicapabilitybuilder.com
💼 **LinkedIn:** [linkedin.com/in/rammaree](https://linkedin.com/in/rammaree)
📅 **Book a 30-min Assessment Review:** [Free consultation link]

---

**Assessment Version:** 1.0 (January 2025)
**Based on:** Microsoft's Gold Standard Framework + 100+ enterprise deployments
**Updated quarterly** with latest best practices
