# Phase-by-Phase Guide

A narrative walkthrough of each migration phase. Use this alongside the checklist spreadsheet.

---

## Phase 0 · Pre-Start

**Goal:** Get the foundation right before touching anything.

The most common mistake in contract migrations is treating Day 1 as the day you start doing things. It isn't. Day 1 is the day you confirm you have what you need to do things safely.

The two most important items in this phase:

**Access** — You cannot discover, plan, or build without access to all three tiers (Dev, Sandbox, Prod) on all three platforms. Request this on Day 1 and track it daily until it is resolved. Access delays are the most common and most avoidable source of schedule slippage.

**Brief IT early — but don't order yet.** You don't know your full server and infrastructure requirements until Discovery is complete. What you *can* do in Phase 0 is get IT's attention: let them know a migration project is coming, give them a rough scope estimate ("we'll likely need new server environments for three platforms across three tiers"), and get in their queue. The specific provisioning request with confirmed specs comes at the end of Phase 2, once Discovery has defined the actual requirements. The Phase 0 action is ensuring IT is not surprised in week 6.

Also in this phase: get the kick-off meeting on the calendar with an agenda. Establish who you escalate to, what the change control tool is, and who signs off at each gate.

**Note on OS version:** Do not confirm a target OS version in Phase 0. You don't yet know what application versions are running, and therefore cannot validate compatibility. What you *can* do is identify candidate versions and flag that vendor support matrix checks are a Week 2 Discovery priority. OS version confirmation belongs at the end of Phase 1, once compatibility has been verified.

---

## Phase 1 · Discovery

**Goal:** Document everything before touching anything — including confirming what OS version you can actually target.

This is the most important phase and the most commonly underinvested one. The quality of your dependency map at the end of Phase 1 determines the quality of everything that follows.

**Vendor support matrix checks belong here, in Week 2.** Before any OS version is confirmed, verify that the current versions of SAS, Spotfire, and Power BI Gateway are certified on each candidate OS version. This is not a Phase 3 activity — if a compatibility problem exists, you need to know it before planning begins, not after. If an application version requires an upgrade to achieve OS compatibility, that scope belongs in the plan. Discovering it mid-upgrade is avoidable.

**The dependency audit** (Task #9) deserves special attention. If CMDB coverage is fragmented or incomplete — and it often is — you must audit every source: monitoring tools, spreadsheets, runbooks, and direct conversations with the people who built these systems. The output should be a single verified document that maps every platform dependency, DB connection, service account, and AD group. This document is your migration foundation. Do not proceed to Phase 2 without it.

**SAS Discovery** requires extra care when the current admin inherited the role without a formal handoff. Spend time with them mapping every scheduled job, every data source, and every non-standard config. Ask "what would break if this server disappeared?" for each component.

**End of Phase 1 deliverables:**
1. A gap report and risk assessment delivered to the project sponsor
2. **Written confirmation of the target OS version** — now informed by actual application inventory and vendor matrix checks, not by assumption
3. Specific IT provisioning requests submitted, with confirmed server specs

Do not proceed to Phase 2 until all three are complete.

---

## Phase 2 · Planning

**Goal:** Lock down sequence, process, and tools before building anything.

By the time Phase 2 begins, the OS version is confirmed and IT has received specific provisioning requests. Planning can now proceed without the risk of a late-breaking compatibility surprise.

Two sequencing decisions in this phase are non-negotiable and must be made in writing:

1. **OS upgrade before separation.** The migration sequence must be: upgrade OS first, validate everything, then execute the separation. Merging these two workstreams compounds risk significantly.

2. **Written success criteria for each environment.** What does "done" mean for Dev? For Sandbox? For Prod? Get the project sponsor's sign-off on these definitions before build work begins. This prevents scope creep and protects you at contract end.

Also in this phase: establish source control for all scripts and configs. Commit a baseline before any migration work begins. This is your rollback foundation.

**The 2-week next-steps presentation** (if agreed with the client) belongs in this phase. It should cover: OS upgrade sequencing, separation scope, dependency audit findings, confirmed target OS version, and proposed timeline with risks called out explicitly.

---

## Phase 3 · OS Upgrade

**Goal:** Get all platforms running cleanly on the target OS before the separation begins.

By this point, vendor compatibility has already been confirmed in Phase 1 and the target OS version is locked. There should be no compatibility surprises in this phase — if there are, they represent a Discovery gap, not a Phase 3 discovery.

The OS upgrade itself is technically straightforward. As of Server 2025, direct in-place upgrade from Server 2012 R2 is supported in a single hop and takes under an hour per server. The work in this phase is validation: running a full regression after the OS upgrade to confirm SAS, Spotfire, and Power BI Gateway all behave correctly on the new OS.

Run a full regression after the OS upgrade in Dev before promoting to Sandbox. Run it again in Sandbox before promoting to Prod. The OS upgrade and the separation are two distinct workstreams — complete the upgrade across all tiers before beginning separation build work.

---

## Phase 4 · Dev Separation Build

**Goal:** Stand up the separated environment in Dev and validate it completely.

This is where the new IT domain comes into play. A new company domain means new AD, new DNS, new network configuration, and new service accounts. IT provisioning for this new domain infrastructure must be complete before you can begin this phase.

Note on provisioning timing: the provisioning *request* for the separated environment was submitted at the end of Phase 1, once the dependency map confirmed the scope. Phase 4 work begins when IT delivers those environments — which is why early queue entry in Phase 0 matters, even before the spec is finalized.

The **dependency map from Phase 1** is your guide here. Every connection on that map must be verified working in the separated Dev environment before you move on. Do not rely on memory or assumption — work through the list explicitly.

**Power BI Gateway auth** is the highest individual risk item in this phase. Service accounts and AD group memberships must be recreated in the new domain. Test every registered data source explicitly.

---

## Phase 5 · Sandbox Separation

**Goal:** Validate the separation in a production-like environment with real users.

Promote only — do not make manual changes in Sandbox. Every promotion should follow the same change control process used in Dev. Capture diffs. Log everything.

**UAT with the affected users** is the gate for this phase. Book the UAT window before Sandbox build begins. Get the project sponsor to own user scheduling and communication. Define minimum viable sign-off criteria upfront.

Do not schedule Prod cutover without written sign-off from the project sponsor on Sandbox completion.

---

## Phase 6 · Prod Separation Cutover

**Goal:** Execute the separation in production cleanly, with a clear go/no-go discipline.

Before the cutover window opens: full backup of all three platforms, integrity verified, stored securely. This is non-negotiable.

Establish a **rollback decision point** before starting. At some point in the cutover window — agreed in advance — you stop and make an explicit go/no-go decision with the project sponsor. If it's no-go, rollback must be executable within the remaining window. Have rollback steps open and ready before the conversation.

Run the dependency verification checklist a third time in Prod. This is Prod. Run it anyway.

---

## Phase 7 · Validation

**Goal:** Confirm stability and close the feedback loop.

Monitor for 48–72 hours post-cutover. Agree on alert routing with the platform admin during this window. Run a full regression against the baseline you captured before cutover.

Establish a user feedback channel on Day 1 of Prod. Do not wait for problems to find you.

**Prod sign-off** from the project sponsor is the gate for Phase 8. Get it in writing.

---

## Phase 8 · Closeout

**Goal:** Hand off cleanly and leave the environment better than you found it.

The **consolidated dependency map** built in Phase 1 should be handed off as the new single source of truth. This is one of the most valuable things you leave behind — especially if the previous state was fragmented.

Confirm a decommission plan for the old environments with IT. You don't own this decision, but you should flag it explicitly rather than leaving it ambiguous.

If a contract extension is not happening: final invoice, final documentation, final sign-off. Make the close clean.

---

*See also: `Migration_Checklist.xlsx` for the full task list with owners, parallel flags, and completion criteria.*
