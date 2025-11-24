# Checklist: Copilot Readiness Assessment

**Instructions:** Use this checklist to validate your organisation's readiness **before** assigning Copilot licenses.

---

## Part 1: Technical Readiness (The Foundation)

### Licensing & Identity
- [ ] **Microsoft 365 E3/E5:** Base licenses are active and assigned.
- [ ] **Entra ID (Azure AD):** Accounts are synced and healthy.
- [ ] **Microsoft 365 Apps:** Users are on the **Current Channel** or **Monthly Enterprise Channel** (Copilot features require recent builds).
- [ ] **New Outlook/Teams:** Users have migrated to the "New Outlook" (Mac/Windows) and new Teams client.

### Data Governance & Security (Zero Trust)
- [ ] **Permissions Audit:** "Just Enough Access" review completed. (Are sensitive HR/Finance sites truly private?).
- [ ] **Semantic Indexing:** Confirmed that Microsoft Search is indexing tenant data.
- [ ] **Data Loss Prevention (DLP):** Policies reviewed to ensure they don't block legitimate AI generation (or *do* block sensitive data exfiltration).
- [ ] **Sensitivity Labels:** Labels are deployed and respected by Copilot.

### Network & Endpoints
- [ ] **WebSockets:** Verified that WebSockets are allowed (critical for Copilot communication).
- [ ] **Network Latency:** Assessed network performance for real-time AI interaction.

---

## Part 2: Cultural Readiness (The People)

### Leadership & Sponsorship
- [ ] **Executive Sponsor:** Identified a C-Suite leader who will actively champion the project.
- [ ] **AI Council:** Established a cross-functional governance body (HR, IT, Security, Business).
- [ ] **"The Why":** Defined clear business objectives for the rollout (not just "because it's cool").

### Engagement & Support
- [ ] **Champions Network:** Recruited 5-10% of the workforce as "Early Adopters" / Champions.
- [ ] **Feedback Channels:** Created a Viva Engage community or Microsoft Form for user feedback.
- [ ] **Training Hub:** Established a SharePoint site or Viva Learning path for "How-To" guides.
- [ ] **Communication Plan:** Drafted the "Coming Soon" and "Launch Day" communications.

---

## Part 3: Pilot Strategy
- [ ] **Cohort Selection:** Identified the first 50-100 users (IT + Champions + High-Value Business Units).
- [ ] **Success Metrics:** Defined what "Success" looks like for the pilot (e.g., "70% of users active daily").
- [ ] **Risk Assessment:** Documented potential risks (e.g., "Hallucinations", "Oversharing") and mitigation strategies.
