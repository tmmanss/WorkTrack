# Architecture Description

## 1. Overview

WorkTrack consists of three software components and one database:

- **Employee mobile app** (Flutter / Dart, Android): check-in/out, schedule, history, requests.
- **Administrator web panel** (React / TypeScript): employees, workplaces, schedules, attendance, requests, reports.
- **Backend** (Java Spring Boot, one application): REST API, business logic, authentication and authorization.
- **Database** (PostgreSQL): all persistent data.

Technology choices are justified in [05-technology-comparison.md](05-technology-comparison.md).

Diagrams (editable sources in `diagrams/`, exports in `diagrams/exports/`):

| Diagram | Source | Export |
|---|---|---|
| System Context | [01-system-context.puml](/diagrams/01-system-context.puml) | ![](/diagrams/exports/01-system-context.png) |
| Use Cases | [02-use-cases.puml](/diagrams/02-use-cases.puml) | ![](/diagrams/exports/02-use-cases.png) |
| Components (draft) | [03-components.puml](/diagrams/03-components.puml) | ![](/diagrams/exports/03-components.png) |

## 2. Key architectural principles

1. **Server-side validation.** The mobile app only reads the location and sends it; the backend decides whether
   a mark is accepted (distance, GPS accuracy, time, duplicates). Server time is used for registration
   (FR-14 to FR-17).
2. **Uncertain location is not a violation.** The status NEEDS_REVIEW allows an administrator to decide
   (FR-15, FR-20).
3. **Single backend, shared database.** One deployable application keeps development and testing simple for an
   individual project (risk R6 in [10-project-plan.md](10-project-plan.md)).
4. **Role-based access in one place.** Spring Security checks the token and role for every request
   (FR-02, NFR-02).
5. **Minimal data collection.** Location is read only at check-in/out and, if enabled, at presence checks;
   no continuous tracking (NFR-05).
6. **Presence checks are server-initiated.** The scheduler decides when a check is needed and sends a push
   message; the app answers through the same REST API (ADR-4). Mobile OS background limits are a known risk (R7).

## 3. Data (preliminary)

Main entities: Organization, User, Workplace, WorkSchedule, WorkSession, AttendanceRecord (check-in/out mark),
PresenceCheck, AbsenceRequest. Optional: RemoteSession, Task, Notification, ViolationRecord.
Presence check results are stored separately from check-in/out marks (FR-35).
The ER diagram and migrations are produced in a later task (database schema).

## 4. Consistency: use cases, requirements, components

| Use case | Requirements | Main backend components |
|---|---|---|
| UC-01 Log in | FR-01, FR-02 | Security, Auth and user service |
| UC-02 View profile | FR-05 | Auth and user service |
| UC-03 View schedule | FR-11 | Workplace and schedule service |
| UC-04 Check in | FR-12, FR-14 to FR-19 | Attendance service, Workplace and schedule service |
| UC-05 Check out | FR-13, FR-14 to FR-19, FR-22 to FR-24 | Attendance service, Time calculation service |
| UC-06 View attendance history | FR-26 | Report and statistics service |
| UC-07 Submit request | FR-30 | Request service |
| UC-08 Respond to presence check | FR-33, FR-35 | Attendance service, Notification service |
| UC-09 Remote work session | FR-38 | Attendance service (extension) |
| UC-10 Manage organization and rules | FR-03, FR-25 | Auth and user service, Time calculation service |
| UC-11 Manage employees | FR-04 | Auth and user service |
| UC-12 Manage workplaces and geofences | FR-06 to FR-08 | Workplace and schedule service |
| UC-13 Manage schedules | FR-09, FR-10 | Workplace and schedule service |
| UC-14 View attendance of employees | FR-27 | Report and statistics service |
| UC-15 Review NEEDS_REVIEW marks | FR-20 | Attendance service |
| UC-16 Process requests | FR-31, FR-32 | Request service |
| UC-17 View statistics and reports | FR-28, FR-29 | Report and statistics service, Time calculation service |
| UC-18 Review possible absences and unclosed sessions | FR-21, FR-36, FR-37 | Attendance service, Presence check scheduler |
| UC-19 Run periodic presence checks | FR-33 to FR-36 | Presence check scheduler, Notification service |

Requirements not shown as separate use cases: FR-40 to FR-44 (optional extensions) will be added to the
diagrams when their implementation is planned. Non-functional requirements map to components as follows:
NFR-01 to NFR-03 to Security and controllers; NFR-08 to the repository layer (transactions);
NFR-09 to the Attendance service (distance calculation); NFR-12 to the REST controllers (OpenAPI).

## 5. Limitations

The component diagram is a draft: module boundaries inside the backend may change during implementation.
The diagrams are updated in the same pull request as the corresponding code change.