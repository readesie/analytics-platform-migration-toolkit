<p align="center">
  <img src="assets/northstar_banner.png" alt="Northstar Banner" width="100%" />
</p>
# Analytics Platform Migration Toolkit

A practical, reusable toolkit for enterprise analytics platform separation and migration projects. Built from real-world experience migrating SAS, Spotfire, and Power BI Gateway environments across Dev, Sandbox, and Prod tiers — including OS upgrades and full IT domain separations.

---

## What This Is

This toolkit provides a structured, battle-tested framework for anyone leading or contributing to an analytics platform migration or spinoff initiative. It covers the full lifecycle from pre-start discovery through closeout, with explicit attention to the things that most often go wrong.

It is platform-aware (SAS, Spotfire, Power BI Gateway) but the principles apply broadly to any analytics infrastructure migration.

---

## Contents

```
analytics-platform-migration-toolkit/
│
├── README.md                          # This file
│
├── templates/
│   ├── Migration_Checklist.xlsx       # 54-task checklist across 9 phases, color-coded by owner
│   └── Migration_Timeline_Analysis.xlsx  # Realistic timeline with threats, scenarios, mitigations
│
├── scripts/
│   ├── generate_checklist.py          # Regenerates the checklist Excel file from source
│   └── generate_timeline.py           # Regenerates the timeline analysis Excel file from source
│
└── docs/
    ├── PHASES.md                      # Phase-by-phase narrative guide
    ├── THREATS.md                     # Deep dive on timeline threats and mitigations
    └── LESSONS_LEARNED.md             # Hard-won operational lessons
```

---

## Quick Start

### Use the templates directly
Download `templates/Migration_Checklist.xlsx` and `templates/Migration_Timeline_Analysis.xlsx` and adapt them to your engagement. Both are fully formatted with dropdowns, color coding, and a Legend tab.

### Regenerate from source
```bash
pip install openpyxl
python scripts/generate_checklist.py
python scripts/generate_timeline.py
```
Output files will be written to the current directory.

---

## The Checklist

**54 tasks across 9 phases:**

| Phase | Name | Focus |
|-------|------|-------|
| 0 | Pre-Start | Access, contracts, kick-off |
| 1 | Discovery | Document everything before touching anything |
| 2 | Planning | Sequence, change control, source control, OS version confirmation |
| 3 | OS Upgrade | Upgrade infrastructure before separation — not after |
| 4 | Dev Separation Build | Stand up new domain environment in Dev |
| 5 | Sandbox Separation | Promote and validate in Sandbox; get UAT sign-off |
| 6 | Prod Cutover | Go/No-Go discipline; rollback ready before starting |
| 7 | Validation | Regression, monitoring, user feedback, Prod sign-off |
| 8 | Closeout | Handoff, decommission plan, contract close |

### Color coding (row backgrounds)
| Color | Meaning |
|-------|---------|
| White | You own it — solo, sequential |
| Green | Can run in parallel with other tasks |
| Blue | Delegate to Evan / SAS Admin / IT; you validate |
| Amber | Joint / collaborative — requires active Northstar Analytics Group input |

### Status dropdown
Each task has a status dropdown: `Not Started / In Progress / Complete / Blocked`

---

## The Timeline Analysis

Structured in three sections:

1. **Baseline 6-month schedule** — optimistic but achievable if key assumptions hold
2. **9 threats ranked by likelihood** — with weeks-at-risk estimates and mitigations
3. **4 outcome scenarios** — Best Case through At Risk, with likelihood percentages

### Realistic outcome summary
| Scenario | Duration | Likelihood |
|----------|----------|------------|
| Best Case | 6 months | ~20% |
| Most Likely | 7–8 months | ~55% |
| Extended | 9–10 months | ~20% |
| At Risk | 10+ months | ~5% |

> **Recommendation:** Build a 2-month extension option into the contract from day one. A migration of this complexity — new IT domain, ~140 users, OS upgrade as prerequisite, fragmented dependency tracking — deserves a realistic runway.

---

## Key Principles

These are the things that separate migrations that go well from migrations that don't:

**1. Dependency mapping first, everything else second.**
Do not touch anything until you have a single, verified map of all platform dependencies, DB connections, service accounts, and AD groups. Fragmented or tribal dependency tracking is the #1 cause of post-cutover surprises.

**2. OS upgrade before separation.**
Migrating onto an old OS and then separating compounds risk unnecessarily. Upgrade the infrastructure to the target OS version first, validate everything, then execute the separation. These are two distinct projects that should be sequenced, not merged.

**3. Rollback planning starts before the change is made.**
Capture script-level diffs before every change. Commit a baseline to source control before any migration work begins. If you can't undo it cleanly, you're not ready to do it.

**4. Change control is not optional.**
Every change logged, sequenced, and timestamped. Every promotion from Dev → Sandbox → Prod tracked. One bad change reaching Prod without a rollback path is an outage.

**5. Written sign-offs at every gate.**
Sandbox sign-off before Prod. Prod sign-off before closeout. Get them in writing (email or ticket). This protects everyone.

**6. Surface blockers early, not late.**
A blocker found in week 3 is a schedule adjustment. The same blocker found in week 20 is a crisis. Flag early, flag often, flag in writing.

---

## Platform Notes

### SAS
- Confirm exact version (Base / Grid / Viya) and hosting model (bare metal / VM / cloud) before planning
- SAS admin knowledge may be partially tribal — treat Discovery as mandatory, not optional
- All scheduled jobs must be inventoried and baselined before any migration work begins

### Spotfire
- Owned and administered by BI/platform team — coordinate early
- Document all data connections, user groups, and auth method before touching config
- TIBCO documentation is thorough; use it

### Power BI Gateway
- Highest auth risk item — service accounts and AD group memberships must be fully documented
- Credentials must be recreated in the new domain during separation
- Test every registered data source explicitly after each environment promotion

### Windows Server
- As of 2025, direct in-place upgrade from Server 2012 R2 to Server 2025 is supported in a single hop
- Verify application compatibility (SAS, Spotfire, PBI Gateway) against target OS before upgrade begins
- For VMs: take a snapshot before upgrade. For physical: full backup and restore test required

---

## Contributing

If you use this toolkit and improve it — fix a gap, add a platform, refine a timeline estimate — pull requests are welcome. The goal is a living document that gets better with each engagement it touches.

---

## Author

**Kedren Reade Sitton**
Senior Data Engineering & Analytics Platform Specialist
25+ years in production data pipelines, SAS environments, and hybrid cloud infrastructure

[LinkedIn](https://linkedin.com/in/reades/) · [GitHub](https://github.com/readesie)

---

## A Note on the Source Material

This toolkit was built from a real engagement. Client, personnel, and project names have been anonymized throughout. Any resemblance to actual organizations or individuals is purely coincidental — or highly circumstantial. 🏒

---

## ⭐ About the Name

*Project name inspired by the legendary Minnesota North Stars — because every migration deserves a true north.*

[![Minnesota North Stars](https://img.shields.io/badge/True%20North-Minnesota%20North%20Stars-006747?style=flat&labelColor=black)](https://en.wikipedia.org/wiki/Minnesota_North_Stars)

> *"They left. The toolkit stayed."*
