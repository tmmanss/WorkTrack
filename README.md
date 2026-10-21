# WorkTrack

Information system for employee attendance and working time tracking based on geolocation.

## Description

WorkTrack is a client-server system consisting of a Flutter mobile app for employees,
a React web panel for administrators, a Spring Boot REST backend and a PostgreSQL database.
Employees register check-in / check-out; the server verifies the device location against
a workplace geofence and calculates working time, lateness and overtime.

## Problem statement

Manual or two-tap attendance tracking cannot reliably confirm that an employee was present
at the workplace between check-in and check-out, and GPS errors are often treated as violations.
WorkTrack addresses this with server-side geofence validation, handling of GPS inaccuracy,
and (as an extension) periodic presence verification.

## Target users

- **Employee**: registers working time, views schedule and attendance history, submits requests.
- **Administrator / Employer**: manages employees, workplaces, schedules, requests and reports.

## Expected outcomes

A working prototype in which an employee registers working time via the mobile app,
the server validates geolocation and applies organization rules, and the employer
analyses attendance through the web panel.

## Team and supervisor

| Role | Name | Responsibility |
|---|---|---|
| Student | Mansur Tasbolat | Analysis, design, backend, mobile app, web panel, testing (individual project) |
| Supervisor | Konstantin Latuta | Review of key deliverables and decisions |

## Scope and status

**MVP:** authentication, users and workplaces, schedules, check-in/out, geofence validation,
working time calculation, admin web panel, basic statistics.
**Optional:** periodic presence verification, remote work mode, QR code, Face ID, task module,
CSV/PDF export.

**Current status:** pre-defense 1 stage (analysis, design and planning). No runnable code yet.

## Technologies

Flutter (Dart), React + TypeScript, Java Spring Boot (Spring Data JPA, Spring Security),
PostgreSQL, REST API.

## Repository structure

| Path | Content |
|---|---|
| `src/backend` | Spring Boot application |
| `src/mobile` | Flutter application |
| `src/web` | React admin panel |
| `docs/` | Proposal, literature review, comparisons, requirements, architecture decisions |
| `diagrams/` | Editable diagram sources and exports |
| `tests/` | Automated tests and test materials |
| `config/` | Configuration examples (placeholders only) |

## Links

- YouTrack: [<link to YouTrack project WT>](https://work-truck.youtrack.cloud/agiles/204-1/current)
- Documentation: [docs/](docs/)
- Prototype: [docs/08-prototype.md](docs/08-prototype.md) (clickable HTML files: `docs/prototype/mobile.html`, `docs/prototype/web.html`)

## Setup, run and test

To be added when runnable code is available.

## Workflow

Each task has a YouTrack ID (WT-N). Branch: `WT-N-short-name`.
Commit: `WT-N: description`. Changes go to `main` through Pull Requests.
Pre-defense versions are marked with tags: `pre-defense-1`, `pre-defense-2`, `final-pre-defense`.