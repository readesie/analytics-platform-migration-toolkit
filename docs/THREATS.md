# Timeline Threats — Deep Dive

This document expands on the 9 threats identified in the Timeline Analysis spreadsheet. Each threat includes context, early warning signs, and mitigation detail.

---

## T1 · Fragmented Dependency Tracking
**Likelihood: HIGH | Weeks at Risk: +2 to +6**

### What it is
No single source of truth for infrastructure dependencies — platform configs, DB connections, service accounts, and AD group memberships scattered across multiple tools, spreadsheets, or exist only as tribal knowledge.

### Why it matters
A migration built on an incomplete dependency map will produce post-cutover surprises. Users will report broken connections. Data refreshes will fail silently. Tracking down undocumented dependencies after cutover is expensive and stressful.

### Early warning signs
- "It's all over the place" or similar when you ask about CMDB coverage
- No consistent tool used for infrastructure tracking
- Only one person who "knows" how something is connected

### Mitigation
Phase 1 Task #9 in the checklist: audit ALL sources (CMDB, monitoring tools, spreadsheets, interviews) and build a single verified dependency map before any migration work begins. This map is the foundation everything else is built on. Do not skip or rush it.

---

## T2 · IT Provisioning Queue
**Likelihood: HIGH | Weeks at Risk: +2 to +4**

### What it is
New servers, new domain infrastructure, and new network configuration require IT to provision and configure resources. IT teams operate on their own queue and priorities.

### Why it matters
You cannot begin OS upgrade work or separation build work until the environments exist. This is a hard dependency you do not control.

### Early warning signs
- IT has not been briefed on the project scope before your start date
- No existing server refresh cycle to piggyback on
- Procurement/hardware lead times not yet assessed

### Mitigation
Raise provisioning requests on Day 1. Track status daily. Make it visible to Priya. Escalate if queue is not moving after week 2.

---

## T3 · OS Version Not Confirmed / Licensing Delay
**Likelihood: MEDIUM-HIGH | Weeks at Risk: +1 to +4**

### What it is
The target Windows Server version has not been formally confirmed or approved. Licensing and procurement may require finance and IT leadership sign-off.

### Why it matters
Planning cannot lock down until the target version is confirmed. Application compatibility validation depends on it. Hardware compatibility depends on it.

### Early warning signs
- Version described as "latest" or "TBD" without a written decision
- No one has spoken to IT about licensing yet
- Procurement cycle not yet initiated

### Mitigation
Get written version confirmation in Week 1 kick-off. Push to lock this as the first planning deliverable. Note: as of 2025, direct in-place upgrade from Server 2012 R2 to Server 2025 is supported in a single hop, which simplifies the path significantly.

---

## T4 · Application Compatibility
**Likelihood: MEDIUM-HIGH | Weeks at Risk: +2 to +6**

### What it is
SAS, Spotfire, or Power BI Gateway may not be certified on the target OS version, or may require version upgrades of the applications themselves to achieve compatibility.

### Why it matters
Discovering an incompatibility mid-migration can require vendor support engagement, application version upgrades, or patch testing — all of which add weeks.

### Early warning signs
- Current application versions are several releases old
- No one has checked vendor OS support matrices
- Applications were installed years ago and have not been updated

### Mitigation
Before the OS upgrade begins: check SAS, TIBCO (Spotfire), and Microsoft (Power BI Gateway) support matrices for the target OS version. Do this in Week 2 of Discovery. Do not begin upgrade work until all three are confirmed compatible.

---

## T5 · UAT Scheduling Delays
**Likelihood: MEDIUM | Weeks at Risk: +1 to +4**

### What it is
Getting ~140 business users to participate in structured user acceptance testing requires coordination, availability, and organizational will that you do not control.

### Why it matters
Sandbox sign-off is a gate before Prod cutover. If UAT drags, the entire schedule slips.

### Early warning signs
- No UAT coordinator identified on the business side
- Users not yet aware the migration is happening
- No testing window blocked on business calendars

### Mitigation
Book the UAT window early — before Sandbox build begins. Get Priya to own user scheduling and communication. Define a minimum viable sign-off criteria so UAT doesn't become open-ended.

---

## T6 · SAS Admin Knowledge Gaps
**Likelihood: MEDIUM | Weeks at Risk: +1 to +3**

### What it is
The current SAS admin inherited the role without a formal handoff from the previous administrator. Undocumented configurations, custom job logic, and non-standard setups are likely.

### Why it matters
Undiscovered SAS configs that surface mid-migration require rework. If the config was undocumented, it may be unclear what the correct behavior should be.

### Early warning signs
- No formal handoff documentation from previous admin
- Scheduled jobs with unclear ownership or purpose
- Config files that "just work" and no one has touched in years

### Mitigation
Treat the SAS Discovery phase as mandatory and thorough. Spend extra time with the SAS admin mapping every scheduled job, every data source, every custom config. Document as you go — even if it slows Discovery by a week, it saves more time later.

---

## T7 · Post-Cutover Undiscovered Dependencies
**Likelihood: MEDIUM-LOW | Weeks at Risk: +1 to +4**

### What it is
A database connection, service account, or application dependency that was not captured in the dependency map surfaces after Prod cutover, causing user-facing failures.

### Why it matters
Post-cutover is the highest-stress time to be debugging. Users are affected. Rollback pressure is real. This is preventable.

### Early warning signs
- Dependency map built quickly without thorough validation
- CMDB coverage was known to be incomplete
- Any "we'll sort that out later" decisions made during Discovery

### Mitigation
The consolidated dependency map (Phase 1, Task #9) is the prevention. Every connection on that map must be verified in Dev, Sandbox, AND Prod before go/no-go. The checklist runs the same verification three times for this reason.

---

## T8 · Contract Start Delayed
**Likelihood: MEDIUM-LOW | Weeks at Risk: +1 to +3**

### What it is
Onboarding paperwork, security clearance, background checks, or internal procurement processes delay the actual start date.

### Why it matters
Every week of delay before start is a week the 6-month window doesn't begin. If the engagement is time-sensitive (e.g., tied to a corporate spinoff date), this compresses the available runway.

### Mitigation
Push Palmer Group / recruiting partner for fast onboarding once offer is accepted. Identify any security or background check requirements early. Ask if any pre-start work (documentation review, access requests) can begin before official Day 1.

---

## T9 · Scope Creep
**Likelihood: MEDIUM-LOW | Weeks at Risk: +2 to +6**

### What it is
Additional platforms, user groups, systems, or requirements are added to the engagement after the contract is signed.

### Why it matters
A 6-month window scoped for 3 platforms and ~140 users is not a 6-month window if it becomes 4 platforms and 200 users mid-engagement.

### Mitigation
Define scope in writing at kick-off. Any additions require a formal contract amendment and timeline revision. Be explicit about this at the start — it protects both you and the client.

---

*See also: `Migration_Timeline_Analysis.xlsx` for the quantified risk table.*
