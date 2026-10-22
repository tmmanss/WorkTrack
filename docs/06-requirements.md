# Requirements Specification: WorkTrack

## 1. Conventions

- **Priority (MoSCoW):** Must = required for the MVP; Should = important, implemented after the MVP core;
  Could = optional, implemented only if time allows. Won't = out of scope (see section 5).
- **Evidence:** all requirements are currently marked **Assumption** (derived from the author's domain
  analysis and comparison of existing systems). After user discussions (WT-2) the column is updated to
  **Verified (Interview N)** or the requirement is changed or removed.
- **Acceptance criterion:** a condition that can be checked by a test or a demonstration.
  Test mapping is defined in `09-verification-plan.md`.

## 2. Actors

| Actor | Description |
|---|---|
| Employee | Uses the Flutter mobile app |
| Administrator | Employer or administrator; uses the React web panel |
| System | Backend (Spring Boot) performing validation, calculation and scheduled checks |

## 3. Functional requirements

### 3.1 Authentication, users, organization

| ID | Requirement | Priority | Acceptance criterion | Evidence |
|---|---|---|---|---|
| FR-01 | A user can log in with email and password and receive an access token | Must | Valid credentials return a token; invalid credentials return 401 | Assumption |
| FR-02 | Access to functions is restricted by role (Employee, Administrator) | Must | Employee token gets 403 on every admin endpoint; verified by automated test | Assumption |
| FR-03 | An administrator can register an organization and edit its settings | Must | Organization is created and its settings are saved and returned | Assumption |
| FR-04 | An administrator can add, edit and deactivate employees | Must | Created employee can log in; deactivated employee cannot | Assumption |
| FR-05 | An employee can view their profile and organization information | Should | Profile screen shows name, role, organization, assigned workplace | Assumption |

### 3.2 Workplaces and geofences

| ID | Requirement | Priority | Acceptance criterion | Evidence |
|---|---|---|---|---|
| FR-06 | An administrator can create a workplace with address, coordinates and geofence radius | Must | Workplace is saved; radius must be a positive number, coordinates within valid ranges | Assumption |
| FR-07 | An administrator can edit and deactivate a workplace | Must | Check-in to a deactivated workplace is rejected | Assumption |
| FR-08 | An employee is assigned to one or more workplaces | Must | Check-in is accepted only for an assigned workplace | Assumption |

### 3.3 Schedules

| ID | Requirement | Priority | Acceptance criterion | Evidence |
|---|---|---|---|---|
| FR-09 | An administrator can create a schedule (working days, start and end time, break) | Must | Schedule is saved; end time is later than start time | Assumption |
| FR-10 | An administrator can assign a schedule to an employee (including individual schedules) | Must | Employee sees the assigned schedule; one active schedule per employee at a time | Assumption |
| FR-11 | An employee can view their current schedule | Must | Mobile app shows working days and times matching the assigned schedule | Assumption |

### 3.4 Check-in / Check-out and geolocation

| ID | Requirement | Priority | Acceptance criterion | Evidence |
|---|---|---|---|---|
| FR-12 | An employee can register check-in (start of work) from the mobile app | Must | Request with valid location creates a work session with start time | Assumption |
| FR-13 | An employee can register check-out (end of work) | Must | Check-out closes the open work session with end time | Assumption |
| FR-14 | The server validates the distance between the device location and the workplace (geofence) | Must | Point inside the radius is accepted; point outside is rejected or sent for review; test with at least 5 coordinate cases | Assumption |
| FR-15 | The server takes GPS accuracy into account and assigns one of the statuses: ACCEPTED, REJECTED, NEEDS_REVIEW | Must | Accuracy above the configured threshold gives NEEDS_REVIEW instead of automatic violation; threshold is configurable | Verified (I1 to I5) |
| FR-16 | The server uses its own time for registration and ignores client-provided time for calculation | Must | A request with an altered device time is stored with server time | Verified (I1 to I5) |
| FR-17 | The system prevents duplicate marks (repeated submission, check-in with an open session, check-out without a session) | Must | Repeated identical request creates no second record; invalid sequence returns a clear error | Verified (I1 to I5) |
| FR-18 | Each mark stores coordinates, accuracy, time, workplace and validation result | Must | Stored record contains all listed fields | Verified (I1 to I5) |
| FR-19 | The app shows clear messages for GPS and network errors and does not lose the user's action | Must | With GPS off or network off the app shows an error and does not crash; retry is possible | Verified (I1 to I5) |
| FR-20 | An administrator can review NEEDS_REVIEW marks and approve or reject them with a comment | Must | Decision changes the mark status and is saved with author and time | Verified (I1 to I5) |
| FR-21 | The system detects unclosed work sessions (no check-out after the end of the shift) | Should | Session without check-out after shift end plus configured time is flagged | Assumption |

### 3.5 Working time calculation

| ID | Requirement | Priority | Acceptance criterion | Evidence |
|---|---|---|---|---|
| FR-22 | The system calculates worked duration for each work session | Must | Result matches manual calculation on at least 5 test sessions | Verified (I1, I2, I3) |
| FR-23 | The system determines lateness and early leaving against the assigned schedule | Must | Check-in later than shift start plus grace period is marked as late; analogously for early leaving | Verified (I1, I2, I3) |
| FR-24 | The system calculates overtime | Should | Time beyond the shift end is counted according to the organization rules | Verified (I1, I2, I3) |
| FR-25 | Calculation rules (grace period, accuracy threshold, overtime) are configurable per organization | Should | Changing a setting changes the result of the next calculation | Verified (I1, I2, I3) |

### 3.6 History, statistics and reports

| ID | Requirement | Priority | Acceptance criterion | Evidence |
|---|---|---|---|---|
| FR-26 | An employee can view attendance history and worked hours | Must | List shows sessions by date with worked time and statuses | Verified (I1, I2, I3) |
| FR-27 | An administrator can view current and historical attendance of all employees | Must | Filters by employee, date and status work correctly | Verified (I1, I2, I3) |
| FR-28 | The system provides statistics per employee and organization for day, week and month | Must | Totals (worked days, hours, lateness, early leaving, absences) match test data | Verified (I1, I2, I3) |
| FR-29 | An administrator can export a report to CSV | Should | Downloaded file opens and contains the same data as the screen | Assumption |

### 3.7 Requests

| ID | Requirement | Priority | Acceptance criterion | Evidence |
|---|---|---|---|---|
| FR-30 | An employee can submit a request (vacation, absence, remote work, sick leave, schedule change) | Should | Request is created with type, period and comment, status PENDING | Assumption |
| FR-31 | An administrator can approve or reject a request with a comment | Should | Employee sees the new status and the comment | Assumption |
| FR-32 | Approved requests are considered in attendance analysis | Should | An approved absence day is not counted as unexcused absence | Assumption |

### 3.8 Periodic presence verification (extension)

| ID | Requirement | Priority | Acceptance criterion | Evidence |
|---|---|---|---|---|
| FR-33 | After check-in the system periodically requests a location check within the open session | Should | Checks occur at intervals within configured limits | Partly verified (I1 to I3) |
| FR-34 | Intervals are variable (random within organization limits) | Should | Intervals of one session are not equal and stay in the configured range | Partly verified (I1 to I3) |
| FR-35 | Check results are stored separately from check-in/check-out marks with statuses CONFIRMED, UNCONFIRMED, POSSIBLE_ABSENCE | Should | Data model separates these records; report shows all three statuses | Partly verified (I1 to I3) |
| FR-36 | One failed check is not a violation; N consecutive failures (configurable) create a possible-absence record for the administrator | Should | Test: 1 failure gives no record; N failures give a record | Partly verified (I1 to I3) |
| FR-37 | Reports distinguish the registered interval from confirmed presence periods | Should | Report shows both values for a session | Partly verified (I1 to I3) |

### 3.9 Optional features

| ID | Requirement | Priority | Acceptance criterion | Evidence |
|---|---|---|---|---|
| FR-38 | Remote Work mode: remote session and periodic Work Check confirmations with request time, response time and result | Could | Missed confirmation is stored as "no response", not as proven absence | Verified, minor (I1, I2, I3, I5) |
| FR-39 | QR code confirmation of presence combined with geolocation | Could | Invalid or expired QR is rejected and the attempt is logged | Partly verified |
| FR-40 | Additional identity confirmation with device biometrics (Android) | Could | Mark with biometrics enabled requires successful confirmation | Partly verified |
| FR-41 | Detection of mock location where the platform supports it, with a suspicious flag on the mark | Could | Mark made with mock location enabled is flagged for the administrator | Assumption |
| FR-42 | Notifications (shift start, check-in reminder, request status; for administrator: unclosed sessions, possible absences) | Could | Notification is delivered for each listed event | Assumption |
| FR-43 | Task module (create, assign, statuses To Do, In Progress, Completed) | Could | Status changes by employee are visible to the administrator | Assumption |
| FR-44 | Configurable violation records without automatic monetary penalties | Could | Violation is recorded by a rule; no payment amount is calculated automatically | Assumption |

## 4. Non-functional requirements

| ID | Category | Requirement | Priority | Acceptance criterion |
|---|---|---|---|---|
| NFR-01 | Security | Passwords are stored only as secure hashes (e.g. BCrypt) | Must | Database contains no plain-text passwords; checked by inspection |
| NFR-02 | Security | All API endpoints except login and registration require authentication; role checks are done on the server | Must | Requests without a token get 401; wrong role gets 403 |
| NFR-03 | Security | Input data is validated on the server | Must | Invalid body (wrong coordinates, empty fields) returns 400 with an error description |
| NFR-04 | Security | Secrets and personal data are not stored in the repository | Must | Repository scan finds no passwords or keys; `.env.example` has placeholders only |
| NFR-05 | Privacy | Location is collected only at check-in/out and, if enabled, at presence checks; no continuous GPS tracking, screen recording, webcam or keyboard monitoring | Must | Code review and app permissions confirm no background continuous tracking |
| NFR-06 | Performance | API response time for check-in/check-out is at most 2 s (95th percentile) under 50 concurrent users on the test environment | Must | Load test result is recorded in the evaluation report |
| NFR-07 | Reliability | Important events (marks, decisions, authentication errors) are logged | Should | Log contains user, action and time for each listed event |
| NFR-08 | Data integrity | Database uses relational constraints and transactions; a mark and its work session are saved atomically | Must | Simulated failure does not leave a mark without a session |
| NFR-09 | Accuracy | Distance calculation error against a reference value is below 1 m | Must | Unit tests with reference coordinate pairs |
| NFR-10 | Usability | After login an employee can register check-in in at most 3 taps | Should | Checked on the prototype and the app |
| NFR-11 | Compatibility | Mobile app runs on Android (a current stable version); the web panel runs in current Chrome and Edge | Must | Manual run on an emulator or device and in both browsers |
| NFR-12 | Maintainability | REST API is documented (OpenAPI); geofence and calculation logic is covered by unit tests (target at least 70% of these modules) | Should | OpenAPI page is available; coverage report is attached |
| NFR-13 | Reproducibility | The system can be started by following the README instructions | Should | A clean setup by instructions works |

## 5. Out of scope (Won't)

Continuous GPS tracking, hidden monitoring, automatic penalties, payroll, iOS release,
production deployment, guaranteed protection from GPS spoofing.

## 6. Constraints and assumptions

- Individual project; the development period is limited, the MVP has priority over optional features.
- GPS is inaccurate indoors; the system treats uncertain locations as "needs review", not as violations.
- Mock location detection and device integrity checks depend on the platform and do not guarantee protection.
- Biometric functions depend on device capabilities and personal data rules.

## 7. Traceability

| Area | Requirements | Related tasks |
|---|---|---|
| Use cases and diagrams | FR-01 to FR-32 | WT-7 |
| Prototype screens | FR-06, FR-11, FR-12, FR-13, FR-26, FR-27, FR-30, FR-31 | WT-8 |
| Tests and metrics | All Must; FR-14, FR-15, FR-22, FR-36, NFR-06, NFR-09 | WT-9 |
| Evidence from users | All (status to be updated) | WT-2 |