# Evidence of Relevance

## 1. Purpose and method

The aim is to check the assumptions behind the project (see [01-proposal.md](01-proposal.md)) with people who
work with attendance tracking, and to separate **verified findings** from **assumptions**.

- **Method:** semi-structured interviews following the guide in [evidence/interview-guide.md](evidence/interview-guide.md).
  The answers were collected **in written (text) form**: each respondent answered the guide questions in writing.
  There was no live follow-up questioning.
- **Respondents:** 5 people (3 employers or managers, 2 employees), found through personal contacts
  (convenience sampling).
- **Period:** 22.10.2026 to 23.10.2026.
- **Prototype:** the interviews covered current practice and attitudes; the HTML prototype
  ([08-prototype.md](08-prototype.md)) was not shown to respondents and is not validated with users.
- **Ethics and data:** respondents took part after an invitation message that stated the purpose and anonymity
  [verify that the message was sent to all five]. Names are removed; respondents appear as I1 to I5.
  Anonymised answers are in [evidence/interview-transcripts.md](evidence/interview-transcripts.md).
  The original files with names are kept by the author outside the repository and can be shown to the
  supervisor on request.

## 2. Respondents

| ID | Role | Organisation type | Format | Date | Organisation size |
|---|---|---|---|---|---|
| I1 | Employer / manager | Restaurant business | Written answers | 23.10.2026 | 500+ |
| I2 | Employer / manager | Computer club | Written answers | 23.10.2026 | 20+ |
| I3 | Employer / manager | Office work (type not stated) | Written answers | 23.10.2026 | 10+ |
| I4 | Employee | Barista (food service) | Written answers | 22.10.2026 | 20+ |
| I5 | Employee | Software developer, hybrid work | Written answers | 22.10.2026 | 10+ |
Organisation sizes are approximate lower bounds reported by the author from respondents' information.
I1 (500+) is the size of the whole company; a single site is comparable to the other organisations
(author's estimate), and review of disputed marks is done at the level of one site.

## 3. Assumptions tested

Rule for the status: **Confirmed** = stated by at least two respondents, or by one respondent with a concrete
example and no contrary answer; **Partly confirmed** = confirmed in part, rare, or with differences between
respondents; **Not tested** = no respondent could speak to it.

| ID | Assumption | Related requirements | Status | Evidence |
|---|---|---|---|---|
| A1 | Employers find it hard to confirm that employees are really at the workplace, not only that they pressed a button | FR-12 to FR-14 | **Partly confirmed** | I1 had cases when an employee clocked in but was not present. I1 and I2 check presence through cameras and the shift supervisor, I3 through access passes and department heads. The problem exists, but small single-site businesses have working, labour-intensive workarounds, so the pain is lower than assumed |
| A2 | Employees leaving between check-in and check-out is a real problem | FR-33 to FR-37 | **Partly confirmed** | About 1 to 2 times a month at I1; "rarely" at I2 and I3. Noticed by the administrator, colleagues or cameras. Real but infrequent, which supports keeping presence checks as an extension (Should) |
| A3 | Attendance, lateness and overtime are processed manually and this takes time | FR-22 to FR-28 | **Confirmed** | All three employers use a paper log, Excel or 1C: I1 2 to 3 hours per week, I2 1 to 2 hours, I3 2 to 3 hours and part of the data is entered manually. Disputes: unrecorded work after the shift (I1), night shifts (I2), delays in reporting periods (I3) |
| A4 | Uncertain marks should go to manager review, not to automatic rejection or penalty | FR-15, FR-20 | **Confirmed** | None of the three employers applies automatic penalties (I1 mentions labour law limits) and all are ready to review disputed marks manually: 5 to 10 per day (I1), up to 5 (I2), 5 to 10 (I3). Both employees prefer to send an uncertain mark to the manager (I4, I5) |
| A5 | Location check at check-in/out is acceptable if transparent; continuous tracking is not; short periodic confirmations may be acceptable | NFR-05, FR-33, FR-36 | **Partly confirmed** | Location at the moment of check-in is acceptable to I1, I2, I4 and I5; I3 relies on access passes and did not address location. Both employees reject constant tracking (I4 accepts location only when it is switched on near the workplace). Two confirmations per day are acceptable to I5; the answer of I4 about periodic confirmation is unclear; employers did not give an answer about periodic requests. I1: employees must know which data is collected and why |
| A6 | QR code and biometric confirmation are acceptable as optional extras | FR-39, FR-40 | **Partly confirmed** | No employer wants biometrics (not considered by I1, not needed by I2, optional for I3). A photo at check-in is acceptable to I1 and I2. I5 finds face confirmation the most convenient; I4 finds QR or biometrics fairly convenient but has not used them. Biometrics stay optional |
| A7 | Remote or hybrid work occurs and needs separate handling | FR-38 | **Confirmed** (minor) | Present at I1 (accountant, marketer), I2 (SMM specialist), I3 (some accountants), I5 (hybrid). It concerns 1 to 2 roles per organisation. Time is tracked separately (I1), by tasks and deadlines (I3) or by stand-ups and written updates (I5), not by hours |
| A8 | Check-in must be possible in one or two taps | NFR-10 | **Partly confirmed** | I4 forgets to record arrival and departure when it is busy, and names simplicity as the main missing feature. Other respondents did not address speed |
| A9 | Leave and shift-change requests are handled informally | FR-30 to FR-32 | **Partly confirmed** | Leave is requested verbally from the administrator or manager (I4) or planned one to two months ahead (I5). I3 needs vacation and sick-leave data in monthly reports |
| A10 | GPS inaccuracy causes wrong rejections in practice | FR-14, FR-15 | **Not tested** | No respondent uses geolocation for attendance. To be tested in the field test (EXP-3 in [09-verification-plan.md](09-verification-plan.md)) |

## 4. Interview summaries

### I1 (employer or manager, restaurant business)

- **Current practice:** paper log; administrator checks shifts against the schedule in Excel; 2 to 3 hours per week.
- **Problems mentioned:** cases when an employee clocked in but was absent; leaving after clocking in
  about 1 to 2 times a month; disputes when an employee stayed after the shift and it was not recorded.
- **Control and data:** presence checked through cameras and the shift supervisor; no automatic penalties
  because of labour law limits. Location at check-in and a photo are acceptable; biometrics are not considered;
  employees must know which data is collected and why.
- **Review workload:** 5 to 10 disputed marks per day is acceptable if the check is quick.
- **Remote work:** accountant and marketer sometimes work remotely; their tasks and time are tracked separately.
- **Reports:** daily presence reports and monthly reports (worked hours, lateness, overtime), exported to Excel for accounting.
- **Systems tried:** none.

### I2 (employer or manager, computer club)

- **Current practice:** schedule in Excel, administrators work in shifts; 1 to 2 hours per week.
- **Problems mentioned:** occasional lateness; disputes about night shifts; leaving after clocking in is rare.
- **Control and data:** cameras and personal checks; no automatic penalties. Photo and location at check-in are
  suitable; biometrics are not needed.
- **Review workload:** up to 5 marks per day.
- **Remote work:** only the SMM specialist.
- **Reports:** reports on shifts, lateness and salary with Excel export.
- **Systems tried:** Google Sheets; specialised systems were not introduced because of their cost.

### I3 (employer or manager, office work)

- **Current practice:** 1C and Excel, standard schedule; 2 to 3 hours per week; part of the data is entered manually.
- **Problems mentioned:** disputes caused by delays in reporting periods; leaving after clocking in is rare.
- **Control and data:** access passes and department heads; no automatic penalties. Electronic passes are
  enough; photo and biometrics are optional. Geolocation was not addressed.
- **Review workload:** up to 5 to 10 disputed marks per day.
- **Remote work:** some accountants; controlled through completed tasks and deadlines.
- **Reports:** monthly reports on hours, vacations, sick leave and overtime, passed to 1C and Excel.

### I4 (employee, barista)

- **Current practice:** arrival and departure are sent through a messenger or as a photo report, plus a paper log.
- **Problems mentioned:** forgets to record when many visitors or colleagues are present; was once fined for lateness.
- **Uncertain location:** prefers to send the mark to the manager and accept it, because managers often do not know
  their staff.
- **Location checks:** acceptable when switched on near the workplace; not acceptable when running all the time.
- **Periodic confirmation:** the answer links it to force majeure or lunch; the meaning is unclear [verify with the respondent].
- **QR and biometrics:** fairly convenient, but not met at the workplace yet.
- **Remote work:** none. Leave is requested from the administrator or manager.
- **Missing feature:** simplicity.

### I5 (employee, software developer, hybrid)

- **Current practice:** attendance is not recorded; trust-based; time is reflected through early meetings and
  written updates about yesterday and today.
- **Problems mentioned:** constant control (questions about task status and location) is annoying; once felt
  considered absent or late because of not answering a message.
- **Uncertain location:** send to review by the manager, because automatic rejection or acceptance can be wrong.
- **Location checks:** negative about constant tracking, but necessary at check-in. Two phone confirmations per day
  are normal: not annoying and a sign of discipline.
- **QR and biometrics:** face confirmation is the most convenient because it needs no extra action.
- **Leave:** arranged one to two months ahead because of business processes.
- **Missing feature:** automation, a system that tracks or synchronises actions, for example like a Telegram bot.

## 5. Findings and resulting requirements

"Proposed" means that the change is suggested by the interviews and has not been applied yet.
After it is applied, replace "Proposed" with a link to the commit or pull request.

| # | Finding | Source | Effect on requirements | Status |
|---|---|---|---|---|
| F1 | All employers export monthly reports (hours, lateness, overtime) to Excel or 1C for accounting | I1, I2, I3 | FR-29 (CSV export): priority Could to Should | Proposed |
| F2 | Night shifts cause disputes | I2 | FR-09: the acceptance criterion "end time later than start time" excludes overnight shifts; allow shifts that end on the next day and add a test | Proposed |
| F3 | Vacation and sick-leave data are needed in monthly reports | I3 | FR-30: add the request type "sick leave"; FR-28: statistics include leave days | Proposed |
| F4 | Disputes about work after the shift that was not recorded | I1 | New requirement (next free ID): administrator can correct or add a work session with a comment and an audit record (Should); to be discussed with the supervisor | Proposed |
| F5 | Employers accept reviewing 5 to 10 marks per day and want no automatic penalties | I1, I2, I3 | FR-20 (review of NEEDS_REVIEW marks) from Should to Must, because FR-15 (Must) produces this status and it needs a way to be resolved; the review rate in EXP-2 is compared with a limit of 10% of marks (see section 6) | Proposed |
| F6 | No automatic penalties; disputes are solved by checking cameras and talking to the employee | I1, I2, I3 | FR-44 stays Could; the system records events and does not calculate penalties | No change |
| F7 | Employees must know which data is collected and why; constant tracking is rejected | I1, I4, I5 | NFR-05 confirmed; new NFR (Should): the app shows a short notice of the collected data before first use | Proposed |
| F8 | A photo at check-in is acceptable to employers and already used informally (photo reports in a messenger) | I1, I2, I4 | Possible new requirement (Could): optional photo at check-in. It needs a privacy and legal review, because a facial image is personal data (see the literature review). Outside the MVP; decide with the supervisor. FR-40 (biometrics) stays Could | Proposed |
| F9 | Remote staff are few and are tracked by tasks, results or stand-ups | I1, I3, I5 | FR-38 stays Could; the task module (FR-43) fits remote work better than periodic confirmations | No change |
| F10 | Employees forget to record in busy moments | I4 | NFR-10 (at most 3 taps) confirmed; check-in reminder (part of FR-42) from Could to Should | Proposed |
| F11 | Employers use a paper log, Excel, Google Sheets or 1C; none uses a specialised time tracking system; cost was named as a reason | I1, I2, I3 | Supports problem point 3 and the conclusions of [04-systems-comparison.md](04-systems-comparison.md); [01-proposal.md](01-proposal.md) to be updated | Proposed |

## 6. Changes to the project after the interviews

To apply (tick when done and link the commit):

- [ ] `06-requirements.md`: Evidence column updated (Verified or Partly verified, with respondent IDs).
- [ ] `06-requirements.md`: FR-09, FR-20, FR-29, FR-30, FR-42 changed as in F1, F2, F3, F5, F10; new requirements from F4, F7 and, if approved, F8 added.
- [ ] `01-proposal.md`: status of evidence in section 3 updated; FR-20 moved into the MVP list.
- [ ] `09-verification-plan.md`: review-rate limit added to EXP-2; test for overnight shifts added.
- [ ] `07-architecture.md`: use cases checked against the new requirements.
- [ ] `09-verification-plan.md`: review-rate target RR ≤ 10% of marks added to EXP-2 (basis: I1 to I3, sites of about 20 employees).

Unchanged: the MVP core (check-in/out, geofence validation with accuracy handling, schedules, calculation,
web panel, statistics); presence checks, remote work, QR and biometrics stay optional; no continuous tracking;
no automatic penalties.

## 7. Limitations

- Small sample (5 respondents) found through personal contacts; no statistical generalisation is claimed.
- Respondents come from food service, a computer club and an office, and are not representative of all employers
  and employees.
- Answers were written, short, and could not be probed with follow-up questions; some answers are ambiguous
  (I4, periodic confirmation).
- Organisation sizes are approximate lower bounds (10+ to 500+ employees; the largest figure is for a whole
  company, not one site); I3 is anonymous.
- No respondent uses geolocation-based attendance, so A10 and the opinions about location checks are about a
  system they have not used.
- The prototype was not shown to respondents; usability findings are not available yet.
- Assumptions marked Partly confirmed or Not tested remain assumptions in [06-requirements.md](06-requirements.md).

## 8. Next steps

- Ask I4 to clarify the answer about periodic confirmation.
- Add an interview with an employer from a larger or multi-site organisation.
- Record organisation size and interview date for every respondent.
- Repeat the check of the requirements with the same respondents when a working app exists.
