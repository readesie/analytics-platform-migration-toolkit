# Lessons Learned

Hard-won operational lessons from production migrations. These are the things you only know from having done it.

---

## On Documentation

**You can't manage what you can't measure.**
If availability isn't being tracked, it isn't being managed. If changes aren't being logged, rollback is guesswork. The first order of business in any inherited environment is instrumentation — not the migration itself.

**Tribal knowledge is a liability, not an asset.**
The person who "just knows" how something is configured is a single point of failure. Every piece of tribal knowledge extracted and documented is risk reduced. Treat Discovery interviews as a knowledge transfer, not just a fact-finding exercise.

**Document as you go, not at the end.**
Documentation written during the migration is accurate. Documentation written after the migration is a reconstruction. They are not the same thing.

---

## On Change Control

**If you can't undo it cleanly, you're not ready to do it.**
Rollback planning happens before the change, not after something breaks. A committed baseline, a captured diff, and a documented rollback procedure are prerequisites — not nice-to-haves.

**Faulty changes get caught in test — they never reach production.**
The value of Dev and Sandbox environments is not just testing functionality. It is catching configuration errors, auth failures, and dependency mismatches before they become production incidents.

**Change tracking at 100% is non-negotiable.**
75% change tracking means 25% of changes are unaccounted for. That 25% is where your post-cutover surprises live. Either everything is tracked or rollback confidence is an illusion.

---

## On Dependencies

**Build the map before you move anything.**
A migration executed without a complete dependency map is a migration that will have post-cutover surprises. There is no shortcut here. The map is the work.

**Run the dependency verification checklist in every environment.**
Dev, Sandbox, and Prod. Three times. Yes, even if it feels redundant. Post-cutover is not the time to discover a connection that "worked in Sandbox."

**The thing most likely to break is the thing nobody remembered to document.**
Service accounts expire. AD group memberships don't carry across domains automatically. Data source credentials are stored somewhere nobody checks. The dependency audit exists specifically to find these before they matter.

---

## On Availability

**99% availability over years is not luck — it is process.**
Consistent high availability comes from change control, source control, and rollback discipline applied systematically over time. It is not a function of how talented the individual is. It is a function of how rigorous the process is.

**User feedback loops are availability infrastructure.**
If users don't report outages, you don't know about outages. A user feedback channel that reaches you in real time is as important as a monitoring dashboard. Establish it on Day 1 of Prod.

**RCA at 100% is how reliability improves.**
Every outage that doesn't have a documented root cause analysis is an outage that will recur. RCA is not blame — it is the mechanism by which the system learns.

---

## On Working Independently

**Flag blockers early, not late.**
A blocker found in week 3 is a schedule adjustment. The same blocker found in week 20 is a crisis. The discipline of surfacing problems early — in writing, to the right people — is one of the highest-value behaviors a contract resource can demonstrate.

**Don't mistake silence for progress.**
In a contract engagement, the client assumes no news is good news. If you are stuck, blocked, or waiting on something, say so. Visibility is your responsibility, not theirs.

**Document everything that matters in writing.**
Verbal agreements evaporate. Scope decisions, sign-offs, go/no-go decisions, extension agreements — if it matters, it goes in writing (email or ticket). This protects you and the client equally.

---

## On OS Migrations

**Upgrade first, separate second.**
Combining an OS upgrade with a platform separation in a single migration pass compounds risk significantly. These are two distinct projects. Sequence them. Complete the OS upgrade, validate everything, then execute the separation.

**Check application compatibility before you schedule the cutover.**
Discovering that SAS, Spotfire, or Power BI Gateway is not certified on the target OS version after the cutover window is scheduled is an avoidable crisis. Check the vendor support matrices in week 2 of Discovery.

**Physical servers from 2012 may have hardware compatibility issues with modern OS versions.**
Check network adapters, storage controllers, and PCIe card drivers before the upgrade. For VMs this is less of a concern — take a snapshot and proceed. For physical: full backup, restore test, hardware audit.

---

## On Contract Engagements

**Scope in writing, at kick-off, signed off.**
Any scope that isn't written down at the start will be interpreted differently by different people at the end. Define it explicitly. Anything added after kick-off requires a formal amendment.

**Build the extension option in from day one.**
A complex migration with realistic risks deserves a realistic runway. A 2-month extension option built into the contract from the start is a professional safeguard, not a hedge. It is easier to not use it than to negotiate it under pressure at month 5.

**The checklist and timeline you leave behind are part of the deliverable.**
A contractor who leaves the environment better documented than they found it — with runbooks, change logs, a consolidated dependency map, and a clear handoff — is a contractor who gets called back.

---

*These lessons inform every task, note, and mitigation in this toolkit.*
