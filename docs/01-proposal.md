# Project Proposal: WorkTrack

## 1. Project title

**EN:** Development of an Information System for Employee Attendance and Working Time Tracking Based on Geolocation
**RU:** Разработка информационной системы учета рабочего времени и посещаемости сотрудников на основе геолокации
**KZ:** Геолокация негізінде қызметкерлердің жұмыс уақыты мен қатысуын есепке алу ақпараттық жүйесін әзірлеу

Working name: **WorkTrack**

## 2. Team and supervisor

| Role | Name |
|---|---|
| Student (individual project) | Mansur Tasbolat |
| Supervisor | Konstantin Latuta |
| Programme | Software Engineering, Astana IT University |

## 3. Problem statement

Organizations need reliable records of when employees start and finish work and where they were during the working day. Simple attendance systems with two buttons (check-in and check-out) have three weaknesses:

1. **Location is not verified or is verified naively.** Check-in can be made from outside the workplace, and GPS errors (especially indoors) are either ignored or wrongly treated as violations.
2. **The interval between check-in and check-out is not verified.** An employee can check in, leave for several hours and return before check-out; the system then counts the whole interval as working time.
3. **Calculation rules are manual.** Lateness, early leaving, overtime and approved absences are often calculated by hand, which is slow and error-prone.

Commercial products (Jibble, Connecteam, TimeCamp, Buddy Punch) cover parts of this problem, but they are closed, subscription-based and not designed to be studied or adapted. The project designs, implements and evaluates its own prototype, with particular attention to GPS inaccuracy and to presence verification between check-in and check-out.

> **Status of evidence:** points 1-3 are currently based on the author's analysis of the domain and existing systems (**assumption**). They will be verified through user discussions in WT-2 and the status will be updated here.

## 4. Target users

| User | Needs |
|---|---|
| **Employee** | Quickly register check-in/check-out, see schedule and attendance history, submit absence and schedule requests |
| **Administrator / Employer** | Manage employees, workplaces and geofences, schedules, review attendance and suspicious records, process requests, get reports |

Future roles (department head, HR specialist) are out of scope but the access model must allow adding them.

## 5. Aim

To design, implement and evaluate a prototype of a client-server information system that registers employee working time using geolocation, validates presence on the server, and calculates attendance indicators according to configurable organization rules.

## 6. Objectives

1. Analyse the domain, existing solutions and literature, and define requirements supported by user evidence.
2. Select and justify the technology stack and architecture.
3. Design the database and REST API.
4. Implement the backend with authentication, role-based access, workplaces, schedules, check-in/out and server-side geofence validation.
5. Implement the employee mobile app (Flutter) and the administrator web panel (React).
6. Implement working time calculation (lateness, early leaving, overtime) and basic reports for day, week and month.
7. Implement periodic presence verification between check-in and check-out (extension, after the MVP).
8. Test the system and evaluate geofence correctness, the effect of GPS accuracy and the handling of missed checks against a baseline.

## 7. Expected outcomes

- A working prototype: mobile app, web panel, backend and PostgreSQL database.
- Documentation: requirements, architecture, database model, test plan and evaluation results.
- A repository with traceable development history linked to YouTrack tasks.
- An evaluation of geofence-based validation and presence verification with documented limitations.

## 8. Project scope

### In scope (MVP, Must)

- Registration, authentication and role-based access (Employee, Administrator).
- Organizations, employees, workplaces with coordinates and geofence radius.
- Work schedules and assignment to employees.
- Check-in / check-out with server-side validation of distance and GPS accuracy.
- Statuses: accepted, rejected, needs review (for uncertain GPS).
- Work sessions and calculation of worked hours, lateness, early leaving, overtime.
- Attendance history and basic statistics; admin web panel.

### Optional (Should / Could, after the MVP)
- Absence and schedule-change requests (basic).
- Periodic presence verification (random-interval checks between check-in and check-out).
- Remote work mode with Work Check confirmations.
- QR code confirmation, biometric confirmation (Face ID), mock-location detection.
- Task module, notifications, CSV/PDF export.

### Out of scope

- Continuous GPS tracking, screen recording, webcam or keyboard monitoring.
- Automatic monetary penalties.
- Payroll calculation and integration with accounting systems.
- Guaranteed protection against GPS spoofing (a documented limitation).
- iOS release and production deployment.

## 9. Engineering complexity

- Geofencing with GPS accuracy handling: distance calculation and decision rules for uncertain locations.
- Server-side validation of time, location and duplicate submissions.
- Periodic presence verification: separating confirmed presence, unconfirmed presence and possible absence.
- Three-component architecture (Flutter, React, Spring Boot) with Spring Security and role-based access.
- Relational data model (users, workplaces, schedules, sessions, presence checks, requests).
- Evaluation under different GPS accuracy conditions.

## 10. Risks (summary)

The main risks are GPS inaccuracy, limited time for optional features, spoofing and privacy. See [10-project-plan.md](10-project-plan.md).

## 11. Related tasks

WT-2 (evidence), WT-6 (requirements), WT-5 (technologies), WT-10 (project management).