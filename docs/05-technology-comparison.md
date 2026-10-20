# Technology Comparison (Architecture Decision Records)

## 1. Method

Five key technical decisions are compared. For each decision, 3 alternatives are evaluated against criteria
derived from the requirements in [06-requirements.md](06-requirements.md).

Rating scale: **+** fits well, **~** fits with limitations, **−** fits poorly.
Ratings are the author's engineering judgement based on the official documentation of the technologies and
the project constraints (individual project, limited time, Android as the target platform, server-side validation).
They are not benchmark results.

Project constraints that influence all decisions:
- one developer, three client/server components (risk R6 in [10-project-plan.md](10-project-plan.md));
- the MVP has priority over optional features (risk R2);
- critical logic (geofence validation, time calculation, access control) must be on the server (FR-14, FR-16, NFR-02);
- location data is personal data and must be minimized (NFR-05).

---

## 2. ADR-1: Mobile client

**Context.** The employee app must get device location with accuracy (FR-12 to FR-15), work with unstable GPS and
network (FR-19), and optionally use biometrics and detect mock location (FR-40, FR-41).

| Criterion (related requirements) | Flutter (Dart) | React Native (TypeScript) | Native Android (Kotlin) |
|---|---|---|---|
| Access to location with accuracy, biometrics, mock-location flag (FR-14, FR-15, FR-40, FR-41) | + mature plugins (e.g. geolocator, local_auth) | + mature libraries, some via native modules | + direct platform API, no wrappers |
| Development effort for one developer (R2, R6) | + one codebase, hot reload, rich built-in widgets | + one codebase, hot reload | ~ more boilerplate; each platform separately |
| UI quality and speed of building screens (NFR-10) | + consistent UI from one widget set | ~ native components, differences between platforms | + native look, more code for each screen |
| Cross-platform potential (NFR-11, future iOS) | + Android and iOS from one codebase | + Android and iOS from one codebase | − Android only |
| Code sharing with the web panel | − different language (Dart vs TypeScript) | + same language as React panel | − different language |
| Maturity and documentation | + | + | + |

**Decision: Flutter.**
**Rationale:** one codebase with a rich set of ready UI components lets one developer build the required screens
(login, check-in/out, history, requests) quickly; the plugins cover all device functions needed by the requirements.
iOS is out of scope for now, but Flutter keeps the option open. My prior experience with Flutter and Dart makes this choice practical, as I can develop the mobile application faster and use my existing skills.
**Trade-offs:** React Native would share TypeScript with the web panel; this advantage is accepted as lost.
Mock-location and integrity checks depend on the platform and do not guarantee protection (see section 6 of the requirements).

---

## 3. ADR-2: Backend platform

**Context.** The backend implements business logic (geofence validation, work sessions, calculation, scheduled presence checks),
authentication and role-based access (FR-01, FR-02, FR-14 to FR-17, FR-22 to FR-24, FR-33 to FR-36, NFR-01, NFR-02, NFR-08).

| Criterion (related requirements) | Java Spring Boot | NestJS (TypeScript) | Django + DRF (Python) |
|---|---|---|---|
| Security: authentication, roles, password hashing (NFR-01, NFR-02) | + Spring Security with role-based access and BCrypt out of the box | ~ Passport/guards, more manual assembly | + built-in auth, permissions need extra setup for JWT |
| Transactions and data integrity (NFR-08) | + declarative transactions (`@Transactional`) | ~ depends on ORM used | + atomic transactions in ORM |
| Relational data model and constraints (FR-18, NFR-08) | + Spring Data JPA, migrations via Flyway/Liquibase | ~ TypeORM/Prisma | + Django ORM and migrations |
| Scheduled tasks for presence checks (FR-33, FR-34) | + `@Scheduled`, Quartz | + schedule module | ~ Celery or similar, extra infrastructure |
| Testing of critical logic (NFR-09, NFR-12) | + JUnit, Mockito, Spring test support | + Jest | + pytest / Django tests |
| API documentation (NFR-12) | + springdoc-openapi | + @nestjs/swagger | + drf-spectacular |
| Strict typing and maintainability | + static typing | + static typing | ~ dynamic typing |
| Development speed | ~ more verbose | + | + |

**Decision: Java Spring Boot** (Spring Data JPA, Spring Security).
**Rationale:** the strongest fit for the security and data integrity requirements (role-based access, hashed passwords,
transactions), static typing for the calculation and validation logic, and a mature scheduling mechanism for presence checks.
It also matches the architecture of the project description (single backend application with a shared database).
Although I have limited experience with Java and Spring Boot, my programming background will help me understand the backend structure and implement the required business logic, authentication, and database operations.
**Trade-offs:** more boilerplate and slower start than NestJS or Django. Mitigation: use Spring Initializr, keep a single
backend application (no microservices).

---

## 4. ADR-3: Data storage

**Context.** Data is strongly relational: users, organizations, workplaces, schedules, work sessions, presence checks,
requests (FR-03 to FR-11, FR-18, FR-30 to FR-37). Marks and sessions must be saved atomically (NFR-08).
Reports aggregate data by day, week and month (FR-28).

| Criterion (related requirements) | PostgreSQL | MySQL | MongoDB |
|---|---|---|---|
| Relational model, foreign keys, constraints (NFR-08) | + | + | − document model, no enforced relations |
| Transactions across several tables (NFR-08) | + | + | ~ supported, less natural for this model |
| Aggregation for reports (FR-28) | + SQL, window functions, date functions | + SQL | ~ aggregation pipeline, harder joins |
| Geospatial support (FR-14, optional) | + PostGIS extension if needed | ~ built-in spatial functions, fewer features | ~ geospatial indexes |
| Time zone and timestamp handling (FR-16) | + `timestamptz` | ~ more limited time zone behaviour | ~ |
| Integration with Spring Data JPA | + | + | ~ separate Spring Data MongoDB module |
| Open source, free, easy local setup | + | + | + |

**Decision: PostgreSQL.**
**Rationale:** the data is relational and requires constraints and transactions; SQL fits attendance reports well;
`timestamptz` suits server-side time registration. PostGIS is **not required** for the MVP: distance to the workplace
is calculated in the application with a spherical (Haversine) formula, which is enough for geofence radii of tens or hundreds
of meters; the accuracy requirement NFR-09 (error below 1 m) is checked by unit tests with reference coordinates.
PostGIS stays as an option if spatial queries become necessary.
**Trade-offs:** MongoDB would be more flexible for changing schemas, but integrity is more important here.

---

## 5. ADR-4: Interaction between components

**Context.** The mobile app and the web panel communicate with one backend. Main operations are request-response
(check-in, check-out, CRUD, reports). Notifications and periodic presence checks (FR-33, FR-42, FR-38) are optional.

| Criterion (related requirements) | REST (JSON over HTTPS) | GraphQL | WebSocket (or gRPC) |
|---|---|---|---|
| Fit for request-response operations (FR-12, FR-13, CRUD) | + | + | ~ possible but unnecessary |
| Integration with Spring Security (NFR-02) | + standard, per-endpoint role rules | ~ extra security configuration per field/resolver | ~ separate configuration |
| Simplicity and support in Flutter and React | + | ~ more setup on both clients | ~ |
| API documentation and testing (NFR-12) | + OpenAPI | ~ schema introspection | ~ |
| Idempotency and duplicate protection (FR-17) | + simple (unique request key, HTTP semantics) | ~ | ~ |
| Real-time updates (optional) | − | ~ subscriptions | + |

**Decision: REST API with JSON over HTTPS, documented with OpenAPI; JWT in the Authorization header.**
**Rationale:** the system is built on request-response operations; REST integrates directly with Spring Security,
is simple to test and document, and supports duplicate protection through a unique request identifier.
Presence check requests (FR-33) and Work Check (FR-38) are initiated by the server through push notifications
(e.g. Firebase Cloud Messaging) and answered through the same REST API. This keeps one communication style.
**Trade-offs:** no real-time updates in the web panel; the administrator refreshes the page or polling is used.
If live updates become necessary, WebSocket can be added for that single feature.

---

## 6. ADR-5: Administrator web panel

**Context.** Tables with filters (attendance, employees), forms (workplace with radius, schedules, requests),
simple statistics (FR-27, FR-28, FR-06, FR-09, FR-31). Runs in current Chrome and Edge (NFR-11).

| Criterion | React + TypeScript | Angular | Vue 3 + TypeScript |
|---|---|---|---|
| Ecosystem of table, form and map components | + | + | ~ |
| Learning curve and development speed for one developer | + | ~ steeper, more structure | + |
| Static typing | + TypeScript | + TypeScript | + optional TypeScript |
| Integration with REST and JWT | + | + | + |
| Community and documentation | + | + | + |

**Decision: React + TypeScript.**
**Rationale:** the largest ecosystem of ready components (tables, forms, maps for choosing workplace coordinates),
a low entry barrier and strict typing of API data with TypeScript. My experience with Flutter has helped me understand component-based UI development and API integration, which will be useful when learning React and building the administrator web panel.
**Trade-offs:** the web panel is kept minimal (risk R6); UI libraries are used instead of custom components.

---

## 7. Summary of decisions

| # | Area | Decision | Rejected alternatives | Main related requirements |
|---|---|---|---|---|
| ADR-1 | Mobile client | Flutter (Dart) | React Native, Kotlin | FR-12 to FR-19, FR-40, FR-41, NFR-10, NFR-11 |
| ADR-2 | Backend | Java Spring Boot | NestJS, Django | FR-14 to FR-17, FR-33 to FR-36, NFR-01, NFR-02, NFR-08 |
| ADR-3 | Data storage | PostgreSQL | MySQL, MongoDB | NFR-08, FR-28, FR-16 |
| ADR-4 | Interaction | REST + JSON, OpenAPI, JWT | GraphQL, WebSocket/gRPC | NFR-02, NFR-12, FR-17 |
| ADR-5 | Web panel | React + TypeScript | Angular, Vue | FR-27, FR-28, NFR-11 |

## 8. Risks of the chosen stack

Three different stacks for one developer (R6). Mitigation: backend and mobile app first, minimal web panel,
one API contract (OpenAPI) shared by both clients. Decisions are reviewed if a stack blocks the MVP.