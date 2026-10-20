# Project plan and risks

## Milestones

| Milestone | Date | Content |
|---|---|---|
| pre-defense-1 | 24.10.2026 | Analysis, requirements, architecture, prototype, verification plan |
| pre-defense-2 | <date> | Database, backend (auth, check-in/out, geofencing), Flutter skeleton |
| final-pre-defense | <date> | Web panel, statistics, presence verification, testing |
| Final defense | <date> | Complete prototype and documentation |

## Responsibilities

Individual project: all components are developed by the student.
The supervisor reviews selected significant changes at agreed checkpoints.

## Risks

| ID | Risk | Probability | Impact | Mitigation |
|---|---|---|---|---|
| R1 | GPS inaccuracy indoors causes false rejections | High | High | Use accuracy threshold, mark uncertain marks "needs review" instead of rejecting |
| R2 | Not enough time for all features | High | High | MoSCoW priorities; Face ID, QR, tasks are optional |
| R3 | GPS spoofing (mock location) | Medium | Medium | Mock-location detection where supported; limitation documented |
| R4 | Privacy and personal data processing | Medium | High | Minimal data collection, no continuous tracking, no secrets in repo |
| R5 | No access to real users for interviews | Medium | High | Start arranging interviews early; fall back to discussions with HR/managers |
| R6 | Three different stacks (Flutter, React, Spring) for one developer | Medium | High | Backend and mobile first, web panel minimal, reuse API contract |
| R7 | Battery and background restrictions on mobile OS | Medium | Medium | Periodic checks instead of continuous tracking |

## Dependencies

WT-2 → WT-1, WT-6; WT-6 → WT-7, WT-9; WT-7 → WT-8;
WT-1…WT-11 → pre-defense-1 submission.