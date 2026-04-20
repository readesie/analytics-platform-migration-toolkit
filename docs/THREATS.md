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
New servers, new domain infrastructure, and new network configuration require IT to provision and configure resources. IT teams operate on their own queue and priorities — and they need lead time.

### Why it matters
You cannot begin OS upgrade work or separation build work until the environments exist. This is a hard dependency you do not control.

### Early warning signs
- IT has not been briefed on the project scope before your start date
- No existing server hardware refresh program in motion that you could align with (i.e., the organization isn't already in the process of rolling out new servers that could be provisioned for this project at the same time — if they are, that's a scheduling shortcut worth asking about)
- Procurement or hardware lead times have not been assessed

### Mitigation
Two separate actions at two separate times:

**Phase 0 (Day 1):** Brief IT that a migration project is coming. Give a rough scope estimate. Get in their queue immediately, even before the specific requirements are defined. The goal is to avoid being an unknown to IT when you arrive with a formal request in Week 3.

**End of Phase 1:** Once the dependency audit is complete and the target OS version is confirmed, submit the specific provisioning request with full server specs, environment count, and domain requirements. This is when IT has what they need to actually act.

The earlier briefing shortens the queue wait. The specific request can't come earlier because you don't have the information to write it yet.

---

## T3 · OS Version Not Confirmed / Licensing Delay
**Likelihood: MEDIUM-HIGH | Weeks at Risk: +1 to +4**

### What it is
The target Windows Server version has not been formally confirmed or approved. This cannot be resolved in Week 1 of the engagement — it requires vendor support matrix validation during Discovery first.

### Why it matters
OS version confirmation gates both planning and procurement. But confirming a version before validating application compatibility is confirming the wrong thing. The right sequence is: validate compatibility in Discovery, then confirm the version, then initiate licensing and procurement.

### Correct sequence
```
Phase 1, Week 2:  Check SAS, Spotfire, and PBI Gateway vendor support matrices
                  against candidate OS versions
                  ↓
Phase 1, Week 3:  Confirm which OS versions are compatible with current app versions
                  ↓
End of Phase 1:   Written OS version confirmation from project sponsor / IT
                  ↓
Phase 2:          Licensing and procurement initiated with confirmed version
                  ↓
Phase 3:          OS upgrade proceeds — no compatibility surprises expected
```

### Early warning signs
- No one has checked vendor support matrices for the current application versions
- "Latest" or "TBD" is being used as a version target without a validation plan
- IT procurement cycles are long and haven't been started

### Mitigation
Do not push for OS version confirmation in Week 1. Push for vendor support matrix checks in Week 2 of Discovery. OS version confirmation is the *output* of that check, not something to obtain before it.

---

## T4 · Application Compatibility
**Likelihood: MEDIUM-HIGH | Weeks at Risk: +2 to +6**

### What it is
SAS, Spotfire, or Power BI Gateway may not be certified on the target OS version at their current installed versions, or may require application version upgrades to achieve compatibility.

### Why it matters
If this is discovered after the OS version is already confirmed and procured, you have a problem. If it is discovered during Discovery — which is where it belongs — it becomes a scoping item, not a crisis.

### Why this belongs in Phase 1, not Phase 3
Phase 3 is the OS upgrade. By Phase 3, the target OS version should already be confirmed *because* compatibility was validated in Phase 1. Application compatibility is not a Phase 3 risk if Discovery was done correctly. It becomes a Phase 3 risk only if Discovery skipped this check.

### Early warning signs
- Current application versions are several releases old and have not been updated in years
- No one has checked the vendor OS support matrices
- Applications were installed by someone who is no longer available

### Mitigation
**Week 2 of Discovery:** Check the following support matrices against current installed versions and candidate OS versions:
- SAS: support.sas.com (System Requirements by release)
- Spotfire: TIBCO/Cloud Software Group support matrix
- Power BI Gateway: Microsoft documentation (Gateway release notes and supported OS)

If any application requires a version upgrade to run on the target OS, that upgrade is in scope and must be planned before Phase 3 begins. Do not confirm the OS version until all three are green.

---

## T5 · UAT Scheduling Delays
**Likelihood: MEDIUM | Weeks at Risk: +1 to +4**

### What it is
Getting business users to participate in structured user acceptance testing requires coordination, availability, and organizational will that you do not control.

### Why it matters
Sandbox sign-off is a gate before Prod cutover. If UAT drags, the entire schedule slips.

### Early warning signs
- No UAT coordinator identified on the business side
- Users not yet aware the migration is happening
- No testing window blocked on business calendars

### Mitigation
Book the UAT window before Sandbox build begins. Get the project sponsor to own user scheduling and communication. Define a minimum viable sign-off criteria so UAT doesn't become open-ended.

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
Push the recruiting partner for fast onboarding once offer is accepted. Identify any security or background check requirements early. Ask if any pre-start work (documentation review, access requests) can begin before official Day 1.

---

## T9 · Scope Creep
**Likelihood: MEDIUM-LOW | Weeks at Risk: +2 to +6**

### What it is
Additional platforms, user groups, systems, or requirements are added to the engagement after the contract is signed.

### Why it matters
A 6-month window scoped for 3 platforms and a defined user count is not a 6-month window if scope expands mid-engagement.

### Mitigation
Define scope in writing at kick-off. Any additions require a formal contract amendment and timeline revision. Be explicit about this at the start — it protects both you and the client.

---

*See also: `Migration_Timeline_Analysis.xlsx` for the quantified risk table.*
