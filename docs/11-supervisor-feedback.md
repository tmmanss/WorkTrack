# Supervisor Feedback and Review Report

## 1. Context

This report summarizes the state of all project deliverables prior to the pre-defense 1 deadline (24.10.2026), details identified limitations and shortcomings, and outlines required actions for the upcoming release. Official feedback and comments from the supervisor were conducted on 24.10.2026 and integrated into Section 6 as action items.

Supervisor: Konstantin Latuta. Repository access granted on 23.10.2026 (WT-10).
yy
## 2. State of the deliverables

| # | Component | Document | Status | Main limitation |
|---|---|---|---|---|
| 1 | Project proposal | docs/01-proposal.md | Delivered | To be updated after the interviews (S-04) |
| 2 | Evidence of relevance | docs/02-relevance-evidence.md | Delivered, limited | 5 written interviews, small convenience sample (S-03) |
| 3 | Literature review | docs/03-literature-review.md | Delivered, limited | Some entries unverified; no systematic database search (S-01) |
| 4 | Existing system comparison | docs/04-systems-comparison.md | Delivered, limited | Some cells unverified, marked "?" (S-02) |
| 5 | Technology comparison | docs/05-technology-comparison.md | Delivered | Ratings based on author's judgement (S-08) |
| 6 | Requirements specification | docs/06-requirements.md | Delivered, limited | Post-interview changes proposed, not yet applied (S-04) |
| 7 | Architecture and diagrams | diagrams/, docs/07-architecture.md | Delivered (draft) | No ER, sequence or deployment diagram yet (S-06) |
| 8 | UX / prototype | docs/08-prototype.md, docs/prototype/ | Delivered, limited | HTML prototype not validated with users (S-05) |
| 9 | Verification and evaluation plan | docs/09-verification-plan.md | Delivered, limited | Targets are hypotheses (S-07) |
| 10 | Project management and risks | README.md, docs/10-project-plan.md | Delivered | No supervisor checkpoints scheduled yet (S-09) |
| 11 | Supervisor feedback | this document | Delivered | Supervisor's comments in section 6; actions pending |

## 3. Shortcomings and actions for the next release

Priority: **High** = before pre-defense 2; **Medium** = during implementation; **Low** = before the final defense.

| ID | Area | Shortcoming | Action | Priority |
|---|---|---|---|---|
| S-01 | Literature review | Part of the bibliographic data is unverified; the search was not systematic (Scopus, IEEE Xplore, ACM DL); GPS accuracy evidence is partly older than five years; no source on periodic presence verification; Android location documentation not yet cited | Check each entry against the publisher page; repeat the search in the university databases; add 2 to 3 recent peer-reviewed papers and the Android documentation | High |
| S-02 | Systems comparison | Cells marked "?" (presence verification, GPS-inaccuracy handling) are not confirmed; some sources are third-party reviews, one is from a competitor | Check vendor documentation or trial accounts; store dated screenshots in `docs/evidence/`; update the table | High |
| S-03 | Evidence of relevance | Five written interviews, no follow-up questions, one ambiguous answer, no respondent uses geolocation attendance; prototype not shown to respondents | Follow up with I4; show the prototype to 2 to 3 respondents; add an employer from a multi-site organisation; repeat with the working app | Medium |
| S-04 | Requirements consistency | Interview findings are proposed but not applied: overnight shifts (FR-09), FR-20 to Must (FR-15 creates NEEDS_REVIEW), FR-29 to Should, "sick leave" in FR-30, administrator session correction, privacy notice, optional photo at check-in; Evidence column mostly "Assumption" | Apply the changes in section 6 of `02-relevance-evidence.md` in separate commits; update the traceability table and `01-proposal.md` | High |
| S-05 | Prototype | Not validated with users; no usability or accessibility check; not tied to an API contract | Run tasks T1 to T6 from the interview guide; draft an OpenAPI description of the main endpoints | Medium |
| S-06 | Architecture | Component diagram is a draft; no ER diagram, check-in sequence diagram, deployment diagram or threat model | Add ER diagram and migrations; sequence diagrams for check-in and presence check; short threat model (spoofing, duplicate marks) | High |
| S-07 | Verification plan | Targets (false rejection at most 2%, detection of absences of 3 hours or more at least 95%, review rate at most 10%) are hypotheses; the error model assumes reported accuracy is a 68% radius, not yet confirmed in the Android documentation; the definition of a "failed check" versus an "unconfirmed check" is not fixed in the data model | Confirm the accuracy definition; run EXP-1 and EXP-2 first and revise targets with reasons; fix check statuses in the database design | High |
| S-08 | Technology comparison | Ratings are engineering judgement; library statements are not checked against official documentation; three stacks for one developer remain a risk | Add documentation links to each decision record; build a minimal vertical slice early | Medium |
| S-09 | Project management | Pull requests are merged by the author (individual project); supervisor checkpoints not scheduled; task prefix changed from DIP to WT (older branch names keep DIP); some YouTrack criteria differ from the documents | Agree checkpoint dates with the supervisor and request review comments on selected pull requests; align YouTrack criteria; create the follow-up tasks (section 4) | High |
| S-10 | Privacy and law | Kazakhstan legal context is based on law-firm summaries, not the law text; no retention rules, storage-location analysis or impact assessment | Read the law text (adilet.zan.kz); define retention and storage rules; decide on photo and biometrics | Medium |
| S-11 | Scope | 44 functional requirements for an individual project; the Must set is large | After S-04, reduce the Must set to the vertical slice needed for the prototype; keep the rest as a backlog | Medium |
| S-12 | Evaluation design | Field test with one or two devices and a few places; load test on one machine | State these limits in every evaluation report; add devices if available | Low |

## 4. Follow-up tasks

Create in YouTrack (owner, deadline and acceptance criterion in each task) and link here.

| Task | Items | Priority |
|---|---|---|
| Verify bibliography and extend the literature search | S-01 | High |
| Verify "?" cells in the systems comparison | S-02 | High |
| Apply post-interview changes to requirements, proposal and verification plan | S-04, S-07 | High |
| ER diagram, database schema and sequence diagrams | S-06 | High |
| Agree supervisor checkpoints and request pull request reviews | S-09 | High |
| Follow-up interviews and prototype check | S-03, S-05 | Medium |
| Privacy and legal review (retention, storage, optional photo and biometrics) | S-10 | Medium |
| Reduce the Must set to the vertical slice | S-11 | Medium |

## 5. Questions for the supervisor

1. Is the focus on presence verification between check-in and check-out and on GPS inaccuracy handling sufficient as the research part? (01-proposal.md, section 9)
2. Are EXP-1 to EXP-5 and baselines B1 and B2 appropriate? (09-verification-plan.md)
3. Raise FR-20 to Must and FR-29 to Should after the interviews? (02-relevance-evidence.md, F1 and F5)
4. Include administrator session correction and an optional photo at check-in, or keep them out of scope? (F4 and F8)
5. Is the literature review sufficient? Are there mandatory sources?
6. Which dates suit the supervisor checkpoints before pre-defense 2?

## 6. Supervisor's comments

- **Supervisor:** Konstantin Latuta. **Date:** 24.10.2026. **Form:** Written review of deliverables and documents 01 to 10.
- **Status of this section:** Reviewed and confirmed by the supervisor on 24.10.2026.
- **Timing:** Review provided on the date of submission. Items marked "Before tag" are minor adjustments to complete before creating tag `pre-defense-1` (24.10 to 26.10.2026); remaining items transition into YouTrack tasks (section 4) targeted before pre-defense 2.

### 6.1 Assessment of the report

The list S-01 to S-12 accurately captures the current limitations and areas for improvement. The explicit limits identified (written interviews, unverified cells, hypotheses in the evaluation plan, reliance on legal summaries) demonstrate a solid analysis. Priorities are accepted with one adjustment: **S-11 (scope) is upgraded from Medium to High priority**, as the MVP scope must be finalized before designing the database schema.

Key strengths noted: Deliverables are comprehensive for this stage and properly traceable across requirements, use cases, components, and tests. Post-interview adjustments appropriately updated priorities. The strategy of using a "needs review" status for GPS inaccuracy instead of automatic penalties is well-justified. The verification plan incorporates structured hypotheses, baselines, and test conditions.

### 6.2 Additional comments

Comments outside the scope of S-01 to S-12 or updating existing items.

| ID | Priority / Timing | Comment | Affects | Requested action | Decision / Status | Evidence (commit or PR) | Status |
|---|---|---|---|---|---|---|---|
| SV-01 | High | Periodic presence checks: sending a push message expecting background location reading conflicts with current Android background location permissions and system restrictions, which affects NFR-05 (no background tracking). | 07-architecture.md; 05-technology-comparison.md; 10-project-plan.md; FR-33 to FR-37 | Perform a technical spike on a physical device. Define the verification flow (e.g., notification opened within a given response window) and record "unconfirmed" check handling. Update architecture and ADR-4. | Open | `<link>` | Open |
| SV-02 | Before tag | Post-interview requirement inconsistencies: FR-15 (Must) triggers NEEDS_REVIEW, but FR-20 (resolving it) is currently Should. Acceptance criteria for FR-09 omit overnight shifts. | 06-requirements.md; 02-relevance-evidence.md | Apply these adjustments, update the Evidence mapping and proposal before tagging. | Open | `<link>` | Open |
| SV-03 | Before tag | NFR-09 specifies distance accuracy under 1 m, but spherical distance calculation formulas exceed this error bound over larger distances; EXP-1 limits tests to 200 m. | 06-requirements.md; 09-verification-plan.md; 05-technology-comparison.md | Align requirement with EXP-1 scope, switch to an ellipsoidal calculation formula, or use PostGIS. | Open | `<link>` | Open |
| SV-04 | High | Interviews indicate leaving work post-check-in occurs rarely (1–2 times/month at one site), with cameras and shift leads covering oversight. Presence verification requires stronger academic/practical rationale as a core research focus. | 02-relevance-evidence.md; 01-proposal.md; 09-verification-plan.md | Refine research focus: frame handling GPS uncertainty via review workflows as primary core, and presence verification as an evaluated extension. Adjust evaluation questions accordingly. | Open | `<link>` | Open |
| SV-05 | High | Evaluation design relies on internal assumptions: simulated error parameters, schedule, and sample size N heavily govern EXP-2 and EXP-4. | 09-verification-plan.md | Ground the error model using literature/field data, evaluate across at least two test devices, perform sensitivity analysis, and define failure metrics. | Open | `<link>` | Open |
| SV-06 | Before tag | Unresolved placeholders remain in documentation (`[verify]` tags, default justification text in tech decisions, target dates, project README links). | 03-literature-review.md; 05-technology-comparison.md; 10-project-plan.md; README.md | Resolve placeholders or explicitly list remaining gaps in submission notes. Engineering decisions must detail technical constraints and experience. | Open | `<link>` | Open |
| SV-07 | Before tag | Commit history shows concentrated updates prior to deadline. Standard guidelines require continuous version control commits. | Git history; YouTrack | Transition to atomic commits, dedicated task feature branches, pull requests per task, and PR reviews without force-rewriting history. Confirm checkpoint schedule. | Open | `<link>` | Open |
| SV-08 | High | Project scope contains 44 functional requirements across three tech stacks for a single developer without an explicitly frozen MVP or cut list. | 01-proposal.md; 06-requirements.md; 10-project-plan.md | Define explicit MVP boundary by requirement IDs and establish a prioritized fallback cut list before finalizing DB schema. Keep admin web panel minimal. | Open | `<link>` | Open |
| SV-09 | High | Security and data governance specifications require expansion (location log retention, deactivation cleanup, audit logging, token life cycle, rate limits). | 06-requirements.md; 07-architecture.md | Specify retention, deletion, and access log policies alongside a concise threat model based on primary legal texts. | Open | `<link>` | Open |
| SV-10 | For pre-defense | Clear ownership of design choices: core mechanisms (geofence policy, error models, stack choice) must be explained in detail during pre-defense defense. | All documents | Prepare structured oral justifications for key design decisions. Ensure full compliance with university attribution policies. | Open | n/a | Open |

### 6.3 Answers to questions in Section 5

| # | Supervisor's answer | Resulting action |
|---|---|---|
| 1 | Approved, contingent on reframing focus per SV-04: GPS uncertainty handling with a review workflow is primary; presence verification is secondary/evaluated. | SV-04 |
| 2 | Approved in principle, provided updates in SV-03 and SV-05 are integrated. | SV-03, SV-05 |
| 3 | Approved. FR-20 must be set to Must (resolves FR-15 state); FR-29 as Should is supported by interview findings. | SV-02 |
| 4 | Admin session correction with audit trails: Include as Should. Check-in photo: Exclude from scope for now due to legal/privacy overhead; revisit after S-10/SV-09. | SV-09 |
| 5 | Literature review requires expansion per S-01. | S-01 |
| 6 | Reviews will proceed bi-weekly via PR reviews. Agreed checkpoint dates: `<dates>` | SV-07 |

## 7. Next checkpoints

| Checkpoint | Date | Material |
|---|---|---|
| Supervisor review of pre-defense 1 | 24.10 to 26.10.2026 | Tag `pre-defense-1`, this report |
| Interim review | <date> | Pull requests for the requirements update and database schema |
| Pre-defense 2 | <date> | Database schema, backend (authentication, check-in/out, geofencing), Flutter skeleton |