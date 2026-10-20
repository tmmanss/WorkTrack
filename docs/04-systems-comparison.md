# Existing System Comparison

## 1. Method

Four commercial attendance and time tracking systems are compared against nine criteria derived from the
requirements in [06-requirements.md](06-requirements.md). Information was collected on 21.10.2026 from vendor
pages and independent reviews listed in section 5.

**Legend**
- ✔ confirmed in at least one source
- ◐ available only on some plans, or sources disagree (see notes)
- ? not confirmed in the sources reviewed (this is **not** the same as "absent")

**Limitations of this comparison.** Several sources are third-party reviews, and some reviews are published
by competing vendors. Plan contents and prices change often. Cells marked "?" must be verified in a trial
account or with vendor documentation before the final defense (task WT-9 / later iterations).

## 2. Systems

| System | Focus | Deployment |
|---|---|---|
| Jibble | Time and attendance, free tier with unlimited users | Cloud (SaaS) |
| Connecteam | Workforce management for deskless teams: time clock, scheduling, tasks, chat | Cloud (SaaS) |
| TimeCamp | Automatic time tracking, attendance, invoicing, optional employee monitoring | Cloud; self-hosted and private cloud on the Enterprise plan |
| Buddy Punch | Time clock with punch-accountability controls (GPS, photo, face, QR) | Cloud (SaaS) |

## 3. Comparison table

| # | Criterion (related requirement) | Jibble | Connecteam | TimeCamp | Buddy Punch | WorkTrack (planned) |
|---|---|---|---|---|---|---|
| C1 | GPS location and geofencing (FR-14) | ◐ GPS stamps and geofencing listed; plan placement differs between sources | ✔ GPS and geofencing | ✔ GPS tracking and geofencing | ✔ GPS and geofencing (geofencing on Pro and above) | ✔ MVP |
| C2 | Handling of GPS inaccuracy: "needs review" status instead of automatic violation (FR-15) | ? | ? | ? | ? | ✔ MVP (ACCEPTED / REJECTED / NEEDS_REVIEW) |
| C3 | Face recognition or photo on punch (FR-40) | ✔ | ? | ? | ✔ photo and facial recognition (Pro and above) | Could (device biometrics, Android) |
| C4 | QR code punch (FR-39) | ? | ? | ? | ✔ (Pro and above) | Could |
| C5 | Periodic presence verification between check-in and check-out (FR-33 to FR-37) | ? | ? | ? | ? | Should (extension) |
| C6 | Activity monitoring, screenshots (not in WorkTrack scope, NFR-05) | ? | ? | ◐ optional screenshots and app/website tracking on paid plans | ? | ✖ out of scope by design |
| C7 | Schedules, timesheets, reports, time off (FR-09, FR-22 to FR-28, FR-30 to FR-32) | ✔ timesheets and reports | ✔ scheduling, timesheets, overtime rules, time off | ✔ attendance, overtime, time off (Starter and above) | ✔ scheduling and time off (Pro and above) | ✔ MVP (requests: Should) |
| C8 | Public API (NFR-12) | ✔ | ? | ✔ | ✔ (Pro and above) | ✔ OpenAPI (own API) |
| C9 | Self-hosted deployment / own data control (NFR-04, NFR-05) | ? | ? | ◐ Enterprise plan only | ? | ✔ by design (prototype) |
| C10 | Pricing model (entry level) | ◐ free plan exists; paid tiers per user; sources disagree on what the free plan includes | ? price not retrieved; vendor lists custom pricing | Free plan; Starter from 5.49 USD per user per month (monthly billing) | Per user plus 19 USD monthly base fee; Starter 4.99 and Pro 6.99 USD per user per month (annual billing) | Not applicable (academic prototype) |

Notes:
- **Jibble.** One review says facial recognition, GPS and kiosk are free for unlimited users; another says facial
  recognition and geofencing are only in paid tiers. The free-plan scope must be checked on the official pricing page.
  A review on connecteam.com (a competitor) was not used for ratings.
- **TimeCamp.** Free plan has GPS tracking and API access; attendance tracking starts at Starter; screenshots
  and app tracking are on higher plans; self-hosted deployment is listed for Enterprise only.
- **Buddy Punch.** Reviews list different plan names (Starter / Pro / Advanced or Enterprise); the Pro tier is
  consistently described as the one that adds geofencing, QR codes, kiosk and API access.
- **Connecteam.** Capterra and the vendor description confirm GPS, geofencing, timesheets, overtime and time off.
  Biometrics, QR, API and prices were not confirmed in the sources reviewed.

## 4. Conclusions

1. **All four systems cover the basics of WorkTrack's MVP**: geofence-based check-in, timesheets, reports and
   attendance rules. WorkTrack does not compete with them on feature breadth, and the project does not claim to.
2. **Identity verification (face, photo, QR) is well covered** by Jibble and Buddy Punch. For WorkTrack it is
   an optional extension (FR-39, FR-40), so this gap is accepted.
3. **Two aspects were not found in the reviewed sources**: handling of GPS inaccuracy as a separate
   "needs review" decision (C2) and periodic presence verification between check-in and check-out (C5).
   This is the justification for WorkTrack's research focus. The claim is limited: absence in the reviewed
   sources does not prove the functions do not exist, so it will be re-checked in trial accounts.
4. **Monitoring tools** (screenshots, app tracking) exist in TimeCamp. WorkTrack deliberately excludes them
   (NFR-05) in favour of limited, transparent checks.
5. **Own data control**: only TimeCamp's Enterprise plan lists self-hosting. A self-hosted prototype whose
   logic can be studied and evaluated is an academic advantage, not a commercial one.
6. **Remaining limitations of WorkTrack**: no payroll, no integrations with accounting systems, no kiosk mode,
   no production-grade support, Android only.

## 5. Sources (accessed 21.10.2026)

- TimeCamp pricing page: https://timecamp.com/pricing
- TimeCamp plans overview: https://www.timely.com/blog/timecamp-pricing/
- TimeCamp vs Clockify (GPS and geofencing): https://toggl.com/blog/timecamp-vs-clockify
- Jibble review 2026: https://www.hivedesk.com/jibble-review
- Jibble review (paid tiers): https://dupple.com/reviews/jibble
- Jibble review: https://fitsmallbusiness.com/jibble-review/
- Connecteam on Capterra: https://www.capterra.com/p/153140/Connecteam/
- Buddy Punch review 2026: https://www.hivedesk.com/buddy-punch-review
- Buddy Punch overview and pricing: https://erpresearch.com/erp-add-ons/time-attendance/buddy-punch
- Buddy Punch review: https://fitsmallbusiness.com/buddy-punch-review/
- Buddy Punch pricing: https://softwarefinder.com/hr/buddy-punch/pricing

## 6. Verification plan for "?" cells

For each "?" cell: open the vendor documentation or start a free trial, record the date and a screenshot in
`docs/evidence/`, and update this table. Priority: C2 and C5 for all four systems.