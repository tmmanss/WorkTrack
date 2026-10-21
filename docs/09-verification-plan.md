# Verification and Evaluation Plan

## 1. Purpose

This plan defines (a) how each requirement from [06-requirements.md](06-requirements.md) is verified, and
(b) how the project aim is evaluated: a system that validates geolocation on the server taking GPS
inaccuracy into account, and that verifies presence between check-in and check-out.
The plan is written before implementation; results will be stored in `docs/evaluation/`.

## 2. Verification levels and tools

| Level | What is checked | Tools (planned) | Location |
|---|---|---|---|
| Unit (UT) | Distance calculation, geofence decision policy, time calculation, validation, interval generation | JUnit 5, Mockito (backend); flutter_test (mobile); Vitest or Jest (web) | `src/<component>/` (standard test folders) |
| Integration (IT) | REST API with security, database constraints, transactions, scheduler with a fake clock | Spring Boot Test, MockMvc, Testcontainers (PostgreSQL) | `src/backend/` |
| End-to-end (E2E) | Full scenarios across components | API scenario scripts (Postman/Newman or REST-assured); Flutter integration_test with mocked location; Playwright for the web panel (optional) | `tests/e2e/` |
| Manual (MT) | Device behaviour, UI, setup, repository checks | Checklists on an Android device or emulator, Chrome and Edge | this document, section 3 |
| Experiments (EXP) | Evaluation of the project aim and non-functional targets | Simulation scripts, field test protocol, k6 or JMeter | `tests/experiments/`, `tests/load/` |

Test IDs (UT-xx, IT-xx, E2E-xx, MT-xx, EXP-x) are referenced from the YouTrack tasks and pull requests.

## 3. Requirement-to-test traceability

### 3.1 Must requirements

| Requirement | Test ID | Description |
|---|---|---|
| FR-01, NFR-02 | IT-01, IT-02 | Valid login returns a token; wrong credentials return 401; request without token returns 401 |
| FR-02 | IT-03 | Employee token gets 403 on every admin endpoint (parameterised over the endpoint list) |
| FR-03, FR-04 | IT-04, IT-05 | Organization and employee CRUD; created employee can log in; deactivated cannot |
| FR-06, FR-07, NFR-03 | UT-01, IT-06, IT-07 | Radius must be positive, coordinates within valid ranges; workplace CRUD; check-in to a deactivated workplace is rejected |
| FR-08 | IT-08 | Check-in to an unassigned workplace is rejected |
| FR-09, FR-10 | UT-02, IT-09 | End time later than start time; only one active schedule per employee |
| FR-11 | IT-10, MT-03 | API returns the assigned schedule; the app shows the same days and times |
| FR-12, FR-13 | IT-11, IT-12 | Check-in creates a session with start time; check-out closes it |
| FR-14, NFR-09 | UT-03, UT-04, EXP-1, EXP-2 | Distance against reference pairs; at least 5 coordinate cases for the geofence decision; accuracy and decision experiments |
| FR-15 | UT-05, EXP-2, EXP-3 | Status ACCEPTED / REJECTED / NEEDS_REVIEW for each accuracy and distance combination; threshold is configurable |
| FR-16 | IT-13 | Request with altered device time is stored with server time |
| FR-17 | IT-14, IT-15 | Repeated identical request creates no second record; invalid sequences (check-in with open session, check-out without session) return a clear error |
| FR-18 | IT-16 | Stored mark contains coordinates, accuracy, time, workplace and validation result |
| FR-19 | MT-01, MT-02, E2E-03 | GPS off and network off: error message, no crash, retry works, action not lost |
| FR-22, FR-23 | UT-06, UT-07 | Worked duration on at least 5 sessions compared with manual calculation; lateness and early leaving with the grace period |
| FR-26, FR-27 | IT-17, IT-18 | History list; filters by employee, date, status |
| FR-28 | IT-19 | Statistics for day, week and month equal manual totals on a fixed test data set |
| NFR-01 | UT-08, IT-20 | Password encoder is used; database inspection shows no plain-text passwords |
| NFR-03 | IT-21 | Invalid request body returns 400 with a description |
| NFR-04 | MT-04 | Repository scan (secret scanner and manual search) finds no passwords or keys; `.env.example` holds placeholders only |
| NFR-05 | MT-05 | Code review and app permissions: no continuous background location, no screen, camera or keyboard monitoring |
| NFR-06 | EXP-5 | 95th percentile of check-in/check-out response time at most 2 s with 50 concurrent users |
| NFR-08 | IT-22 | Simulated failure after saving a mark leaves no mark without a session (rollback) |
| NFR-11 | MT-06 | Mobile app on an Android device or emulator; web panel in current Chrome and Edge |

### 3.2 Should requirements

| Requirement | Test ID | Description |
|---|---|---|
| FR-05 | MT-08 | Profile screen shows name, role, organization, workplace |
| FR-20 | IT-23 | Administrator decision changes status and stores author, time and comment |
| FR-21 | IT-24 | Session without check-out after shift end plus configured time is flagged |
| FR-24, FR-25 | UT-09, IT-25 | Overtime calculation; changed setting changes the next calculation |
| FR-30 to FR-32 | IT-26, IT-27 | Request creation and decision; approved absence is not counted as unexcused |
| FR-33 to FR-37 | UT-10, IT-28, IT-29, EXP-4 | Intervals random within limits; one failed check creates no record; N consecutive failures create a possible-absence record; results stored separately; report shows registered and confirmed time |
| NFR-07 | IT-30 | Log contains user, action and time for marks, decisions and authentication errors |
| NFR-10 | MT-07 | Check-in in at most 3 taps after login |
| NFR-12 | MT-09 | OpenAPI page is available; coverage of geofence and calculation modules at least 70% |
| NFR-13 | MT-10 | A clean setup by README instructions works |

### 3.3 Could requirements

FR-29 and FR-38 to FR-44 get tests when their implementation is scheduled; each test is added to this table
in the same pull request as the feature.

### 3.4 End-to-end scenarios

| ID | Scenario | Covers |
|---|---|---|
| E2E-01 | Login, check-in, check-out, history, statistics (API level) | FR-01, FR-12, FR-13, FR-22, FR-26, FR-28 |
| E2E-02 | Check-in with poor accuracy gives NEEDS_REVIEW; administrator approves; status and history update | FR-15, FR-20 |
| E2E-03 | Mobile app: login and check-in with mocked location (accepted, outside zone, no location) | FR-12, FR-14, FR-19 |
| E2E-04 | Web panel: administrator approves a flagged mark (optional) | FR-20, FR-27 |

## 4. Evaluation of the project aim

### 4.1 Evaluation questions

- **EQ1.** Is the distance to the workplace calculated accurately enough for geofence radii used in practice? (NFR-09)
- **EQ2.** Does handling GPS accuracy (status NEEDS_REVIEW) reduce wrong decisions compared with a naive geofence
  check? (FR-14, FR-15)
- **EQ3.** Does periodic presence verification detect long absences between check-in and check-out without
  raising false alarms, compared with plain check-in/check-out? (FR-33 to FR-37)
- **EQ4.** Does the backend meet the response-time requirement under the planned load? (NFR-06)

### 4.2 Baselines

| ID | Baseline | Used in |
|---|---|---|
| B1 | **Naive geofence**: accept if measured distance is not greater than the radius, otherwise reject; GPS accuracy is ignored | EXP-2, EXP-3 |
| B2 | **Plain check-in/check-out**: the whole interval between the two marks is counted as working time; no checks between them | EXP-4 |
| B3 | Requirement thresholds (2 s, 1 m) | EXP-1, EXP-5 |

### 4.3 Decision policy under evaluation (P1, a hypothesis)

Let `d` be the measured distance to the workplace, `R` the geofence radius, `acc` the reported GPS accuracy
and `A_max` the accuracy threshold (default 50 m, configurable):

1. if `acc > A_max` then NEEDS_REVIEW;
2. otherwise, if `d <= R` then ACCEPTED;
3. otherwise, if `d - acc <= R` (the uncertainty circle touches the zone) then NEEDS_REVIEW;
4. otherwise REJECTED.

The policy and the default values may be changed after the experiments. Changes are recorded in the
evaluation report with the reason.

### 4.4 Experiments

#### EXP-1. Distance calculation accuracy (EQ1, NFR-09)

- **Method:** compare the implemented formula (Haversine) with reference values computed by an independent
  geodesic calculation on the ellipsoid, for at least 30 coordinate pairs near the planned test locations
  (latitude about 51 degrees) at distances of 10, 25, 50, 100, 200 and 500 m in different directions.
- **Metrics:** absolute error in metres (maximum and mean) by distance.
- **Target:** error below 1 m for distances up to 200 m. For larger distances the measured error is reported.
  If it exceeds 1 m up to 500 m (the largest radius offered in the prototype), either the ellipsoidal formula
  or PostGIS is used (see ADR-3 in [05-technology-comparison.md](05-technology-comparison.md)), or the
  requirement is narrowed with a justification.

#### EXP-2. Geofence decision under GPS error: simulation (EQ2, FR-14, FR-15)

- **Method:** simulate check-in attempts and compare policy P1 with baseline B1 on the same generated points.
- **Parameters:** radius `R = 100 m`; true distances inside the zone 10, 30, 50, 70, 90 m and outside
  110, 130, 150, 200, 300 m; reported accuracy 5, 10, 20, 35, 50, 100 m; 10,000 points per combination;
  fixed random seed.
- **Error model:** the position error is a circular Gaussian whose standard deviation per axis is
  `acc / 1.51`, so that 68% of the points lie within `acc` of the true position (Android reports accuracy as
  a 68% confidence radius; to be confirmed in the documentation of the location API used). A second variant adds
  10% outliers with a 3 times larger error to model indoor multipath.
- **Metrics:**
  - false rejection rate (FRR): share of points truly inside the zone that get REJECTED;
  - false acceptance rate (FAR): share of points truly outside the zone that get ACCEPTED, by distance band;
  - review rate (RR): share of all points that get NEEDS_REVIEW (workload for the administrator).
- **Target (initial):** FRR of P1 is lower than FRR of B1 for accuracy of 20 m and worse; FRR of P1 at most 2% for
  accuracy up to `A_max`; FAR is reported per band and compared with B1 (points just outside the radius are
  accepted by any method, which is a documented limit). RR is reported so that the review workload stays visible.
- **Output:** tables and plots in `docs/evaluation/exp2-geofence-simulation.md`; script in `tests/experiments/`.

#### EXP-3. Field GPS test on a real device (EQ2, FR-15, NFR-05)

- **Method:** repeat check-in requests with the real mobile app against the test backend at defined places and
  record the results.
- **Conditions:** at least one Android device (model and OS version recorded); location mode set to high accuracy;
  places: open sky at the entrance (P1), building lobby (P2), room deep inside the building (P3), outside the
  building at about 50, 150 and 300 m from the workplace point (P4 to P6); 20 attempts per place, at least
  1 minute apart, at different times of day. Reference distances are taken from a map and a measuring tool;
  the uncertainty of this ground truth (about 5 m) is stated in the report. Only the author's own device and
  location are used.
- **Metrics:** reported accuracy (median and 90th percentile), distance error against the reference, share of
  ACCEPTED / NEEDS_REVIEW / REJECTED under P1 and under B1, time to get a location fix.
- **Use:** results are compared with the simulation (EXP-2); the thresholds `A_max` and the review rule are
  tuned on the simulation, and the field data serve as a separate check.

#### EXP-4. Periodic presence verification (EQ3, FR-33 to FR-37)

- **Method:** run the scheduler with a fake clock and injected check results (simulation), then repeat the most
  important scenarios with the real app (at least 3 sessions).
- **Parameters:** shift 9:00 to 18:00; check interval random between 30 and 90 min; N = 2 consecutive failed
  checks create POSSIBLE_ABSENCE (sensitivity run with N = 3); 100 runs per scenario with different seeds.
- **Scenarios:**

| ID | Scenario | Expected result |
|---|---|---|
| A | Present for the whole shift | All checks CONFIRMED; no possible absence |
| B | Leaves for 3 hours and returns before check-out | POSSIBLE_ABSENCE recorded; confirmed time clearly lower than the registered interval |
| C | Short break of 20 minutes | No possible absence |
| D | One failed check (GPS fault), then normal | No possible absence |
| E | Device offline for 1 hour (no response) | Checks are UNCONFIRMED; no automatic possible absence (assumption, see note) |
| F | Absence of 30, 60, 90, 120, 180, 240 min | Detection rate by absence length |

- **Metrics:**
  - detection rate of an absence by absence length (scenarios B and F);
  - detection delay: time from leaving to the possible-absence record;
  - false alarm rate in scenarios A, C, D, E;
  - error of the estimated confirmed presence time against the known true presence time (minutes), for
    P (periodic checks) and for B2 (the whole interval counts, so the error equals the absence duration).
- **Target (initial):** detection of absences of 3 hours or more at least 95% with N = 2; no false alarms in
  scenarios A, C, D; the minimum reliably detected absence length is reported, because it depends roughly on N
  times the check interval. Short absences are not detected by design; this is a documented limit and not a
  defect. The method gives evidence about possible absences, not continuous observation.
- **Note:** this plan assumes that a check with a location outside the zone and acceptable accuracy counts as a
  failed check, while "no response" or poor accuracy is UNCONFIRMED. The exact definition is fixed in the
  database and API design and aligned with FR-36.

#### EXP-5. API performance (EQ4, NFR-06)

- **Method:** load test of POST check-in, POST check-out and GET history.
- **Tool and load:** k6 or JMeter; 50 virtual users, ramp-up 1 min, steady load 5 min; each user has its own
  account and workplace assignment; database seeded with 50 employees and 10,000 attendance records.
- **Environment:** the hardware, OS, JVM version, database version and configuration are recorded; the test runs on
  one machine, so results indicate behaviour of the prototype and not production capacity.
- **Metrics:** p50, p95 and p99 response time, error rate, throughput.
- **Target:** p95 of check-in and check-out at most 2 s; error rate below 1%.

#### EXP-6. Usability check (optional, NFR-10)

- **Method:** count taps for the main actions in the prototype and the app; if users are available (task WT-2),
  ask 3 to 5 people to perform check-in and submit a request and note errors and comments.
- **Metrics:** taps per task, task completion, number of questions asked.
- **Note:** with so few participants the results are qualitative.

## 5. Measurement conditions

| Aspect | Specification |
|---|---|
| Backend and database | Spring Boot and PostgreSQL in Docker or locally; versions recorded in each report |
| Mobile | Android device model and OS version recorded; emulator only for functional tests, not for GPS experiments |
| Time | The server clock is the only time source; fake clock in scheduler tests |
| Data | Fictional test users and data sets in `tests/data/`; no personal data in the repository (NFR-04) |
| Reproducibility | Fixed random seeds; scripts and parameters stored in the repository; each result file names the commit and tag it was produced from |

## 6. Success criteria

The aim is considered demonstrated when:

1. all tests for Must requirements pass (100%);
2. coverage of the geofence and calculation modules is at least 70% (NFR-12);
3. EXP-1 to EXP-5 are executed and their results are documented, including cases where targets were not met;
4. EXP-2 shows how P1 changes false rejections, false acceptances and the review rate compared with B1;
5. EXP-4 shows the detection of long absences and the error of the confirmed presence time compared with B2;
6. known limitations are listed in the evaluation report.

## 7. Schedule and responsibilities

| Item | When | Responsible |
|---|---|---|
| Plan (this document) | pre-defense 1 | Student |
| UT and IT written together with each backend feature | during implementation (backend tasks) | Student |
| EXP-1 and EXP-2 (simulation script) | before pre-defense 2 | Student |
| EXP-3 (field test) | when check-in works in the mobile app | Student |
| EXP-4 | after periodic checks are implemented | Student |
| EXP-5 and final regression | before the final pre-defense | Student |
| Review of results and conclusions | at agreed checkpoints | Supervisor |

## 8. Threats to validity and limitations

- The simulated error model is a simplification of real GPS behaviour; the field test (EXP-3) is used to check it.
- The field test has a small sample, one environment and one or two devices; no statistical generalisation is
  claimed, results are descriptive.
- Ground truth of the field test has an uncertainty of several metres.
- Baselines are simple by design (B1, B2); they do not represent commercial products.
- The load test runs on one machine and does not represent production.
- The security tests do not prove protection against GPS spoofing; this limit is stated in the requirements.
- Parameters are tuned on simulated data and checked on field data to limit overfitting.

## 9. Reporting

Each experiment produces a file in `docs/evaluation/` with the setup, parameters, raw result tables, plots and
conclusions, and the commit or tag used. Changes to this plan are made in pull requests linked to YouTrack tasks.