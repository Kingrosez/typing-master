# Typing Master & Spell Corrector

## Complete Features, Technology Stack, Dependencies & Implementation Specification

**Document type:** Product + Technical Specification\
**Status:** Consolidated planning document\
**Frontend direction:** React + TypeScript + Tailwind CSS v4\
**Backend direction:** Java + Spring Boot\
**Database direction:** PostgreSQL

> **Important status note:** This document consolidates the features and
> technology choices discussed so far. Items marked **Proposed /
> Confirm** are recommendations or unresolved decisions, not proof that
> they are already implemented or installed. Exact dependency versions
> should be pinned to versions compatible with the actual project
> environment.

------------------------------------------------------------------------

# 1. Product Overview

## 1.1 Product name

**Typing Master & Spell Corrector**

## 1.2 Product purpose

A typing-practice and learning application intended to help users
improve typing speed, accuracy, consistency, and confidence through
practice sessions, structured lessons, timed tests, feedback, and
progress tracking.

## 1.3 Product goals

-   Make starting a typing session quick and intuitive.
-   Support both guided learning and free practice.
-   Measure typing performance using clearly defined metrics.
-   Show useful feedback after each session.
-   Track progress over time.
-   Help users identify recurring typing errors.
-   Provide a responsive, accessible interface.
-   Keep the UI and implementation consistent with the approved
    prototype and design system.

## 1.4 Target users

The app can serve: - Beginners learning keyboard familiarity. - Students
practicing typing. - Office and professional users improving
productivity. - Users preparing for typing assessments. - Returning
users tracking personal progress.

These are intended user groups; product research may further refine
them.

------------------------------------------------------------------------

# 2. Scope and Status

## 2.1 Core feature areas discussed

-   Dashboard
-   Practice
-   Lessons
-   Typing Tests
-   Results
-   Reports / Progress Analytics
-   Achievements
-   Profile
-   Settings
-   Help & Support
-   Sign-in / Account flows
-   Light and dark themes
-   Responsive UI
-   Typing performance metrics
-   Error feedback and review

## 2.2 Optional / unconfirmed features

These must not be treated as committed requirements until approved: -
Guest mode - Subscription or upgrade plans - Public leaderboards or
competition - Social sharing - Multiple languages and keyboard layouts -
Cloud sync across devices - Audio cues - Admin/content-management
portal - AI-generated passages or personalized coaching - Offline mode /
installable PWA - Data export - Passwordless or third-party
authentication

------------------------------------------------------------------------

# 3. User Roles and Access

## 3.1 Visitor

Can access public pages and possibly try practice without an account, if
guest mode is approved.

## 3.2 Registered user

Potential capabilities: - Complete practice, lessons, and tests. - View
personal results and reports. - Manage profile and preferences. - Track
achievements. - Access saved history across sessions.

## 3.3 Administrator / content manager --- proposed

If the product needs managed lesson content, an admin role may be
introduced to create and update passages, lessons, and test
configurations. This role is not part of the confirmed initial UI scope.

------------------------------------------------------------------------

# 4. Feature Specification

## 4.1 Dashboard

### Purpose

Provide a quick overview and a clear starting point.

### Features

-   Greeting or welcome message.
-   Primary action to start practice.
-   Summary cards for agreed metrics, potentially:
    -   Recent or current WPM
    -   Accuracy
    -   Total practice time
    -   Completed sessions
-   Recent sessions list.
-   Progress trend visualization, when sufficient history exists.
-   Quick navigation to Practice, Lessons, Tests, and Reports.
-   First-use empty state.
-   Loading, error, and no-data states.

### Behavior

-   Selecting Start Practice opens the practice flow.
-   Selecting a session opens its result details.
-   Report links navigate to the appropriate analytics view.
-   Do not display demo values as real user statistics.

## 4.2 Practice

### Purpose

Offer flexible practice without the pressure of a formal test.

### Features

-   Display a passage or typing prompt.
-   Typing input/workspace.
-   Monospace passage typography.
-   Character-level visual feedback:
    -   Not yet typed
    -   Correct
    -   Incorrect
    -   Current position
-   Live metrics, when supported:
    -   WPM
    -   Accuracy
    -   Elapsed time
    -   Progress
-   Start, restart, and finish actions.
-   Optional hint action, only after hint behavior is defined.
-   Completion confirmation and results screen.
-   Responsive layout and keyboard accessibility.

### Session states

1.  Ready
2.  Active
3.  Completed
4.  Error
5.  Paused --- only if pause/resume is approved

### Required rules to define

-   When timing begins.
-   Whether backspace is allowed.
-   Whether paste is allowed.
-   Case and punctuation sensitivity.
-   How corrected mistakes affect accuracy.
-   Whether users can continue after finishing the prompt.
-   Whether incomplete sessions are saved.
-   How restart handles existing session data.

## 4.3 Lessons

### Purpose

Guide users through progressive typing exercises.

### Features

-   Lesson catalogue.
-   Lesson title, description, level, estimated duration, and status.
-   Lesson detail/instruction view.
-   Start lesson action.
-   Typing task.
-   Completion feedback.
-   Retry or next-lesson action, based on curriculum rules.
-   Progress or completion indicators.
-   Loading, empty, and error states.

### Curriculum decisions

-   Beginner/intermediate/advanced structure.
-   Key-focused lessons versus words, punctuation, numbers, and
    passages.
-   Linear progression or free selection.
-   Passing thresholds.
-   Unlocking rules.
-   Whether lesson progress can be resumed.

## 4.4 Typing Tests

### Purpose

Measure performance against a defined time or passage objective.

### Features

-   Test setup screen.
-   Select duration, if time-based.
-   Select passage/difficulty, if supported.
-   Start test action.
-   Timer or elapsed-time indicator.
-   Live metrics.
-   End-of-time behavior.
-   Completion screen.
-   Results page.
-   Retry and return actions.

### Decisions required

-   Exact test durations.
-   Time-based, passage-based, or both.
-   Difficulty options.
-   Language and keyboard-layout options.
-   Pause/resume policy.
-   Paste policy.
-   Guest versus registered-user saving behavior.

## 4.5 Results

### Purpose

Explain the outcome of a session.

### Features

-   Session type and completion timestamp.
-   WPM and accuracy according to approved formulas.
-   Duration and completion status.
-   Error summary and error review, if captured.
-   Comparison with prior results only when comparable.
-   Retry action.
-   Continue practice action.
-   Link to reports.

### Rules

-   Distinguish missing values from zero.
-   Do not fabricate personal bests or comparisons.
-   Explain comparisons across different session types.
-   Show a fallback if character-level error data is unavailable.

## 4.6 Reports / Progress Analytics

### Purpose

Show progress across multiple sessions.

### Features

-   Session history.
-   Date-range filtering, if supported.
-   Filter by session type, if supported.
-   WPM and accuracy trends.
-   Practice time and session count summaries.
-   Error patterns, if error data is collected.
-   Responsive charts.
-   Text/table equivalent for key chart information.
-   Empty, loading, and error states.
-   Export only if implemented and approved.

### Analytics rules

-   Show the date range and metric definition.
-   Do not treat missing days as zero performance.
-   Avoid misleading chart scales.
-   Ensure charts remain legible in dark mode and on mobile.

## 4.7 Achievements

### Purpose

Recognize milestones and encourage continued practice.

### Features

-   Achievement list/grid.
-   Locked and unlocked states.
-   Name and description.
-   Unlock criteria.
-   Unlock date, if available.
-   Empty state.

### Decisions required

-   Achievement definitions and thresholds.
-   Streak definition and day-boundary rules.
-   Account-wide or device-local progress.
-   Whether celebration animations or notifications are desired.

## 4.8 Profile

### Purpose

Display and manage basic user information.

### Features

-   User display name and relevant account details.
-   Edit form, if supported.
-   Save and cancel.
-   Validation and error messages.
-   Save success/failure state.
-   Sign-out action for authenticated users.

### Privacy rules

-   Display only necessary account information.
-   Do not claim changes saved until confirmed.
-   Define guest profile behavior if guest mode exists.

## 4.9 Settings

### Candidate settings

-   Theme: light, dark, or system.
-   Typing display preferences.
-   Sound effects, if included.
-   Language or keyboard layout, if included.
-   Practice/test preferences.
-   Account and privacy options, where applicable.

### Requirements

-   Show current values.
-   Use accessible controls.
-   Define immediate-save versus explicit-save behavior.
-   Define whether settings are device-local or account-synced.
-   Provide reset only if product behavior is specified.

## 4.10 Help & Support

### Features

-   FAQ or help topics.
-   Explanation of typing metrics after formulas are finalized.
-   Keyboard/input troubleshooting.
-   Contact/support link only if a real support channel exists.
-   Clear unavailable state when support content is not configured.

## 4.11 Authentication and account flows

**Scope:** Authentication has been discussed as a page/flow, but
provider and rules remain unconfirmed.

### Candidate screens

-   Sign in
-   Sign up
-   Forgot password
-   Password reset confirmation
-   Session expired
-   Authentication error
-   Sign out

### Requirements

-   Typed and validated forms.
-   Secure password handling through the chosen authentication design.
-   Clear, non-sensitive error messages.
-   No false success state.
-   Define session expiry, refresh, logout, and guest conversion
    behavior.

## 4.12 Optional subscription / upgrade

Not confirmed for the initial release. If added, specify plan
boundaries, price, billing period, cancellation, payment failure, and
restore-purchase behavior. Do not implement placeholder billing actions
as if they were real.

------------------------------------------------------------------------

# 5. User Flows

## 5.1 Main practice flow

``` text
Open application
  → Dashboard
  → Start Practice
  → Load/select passage
  → Ready state
  → User starts typing
  → Live feedback and metrics
  → Finish or complete
  → Results
  → Retry / continue / reports
```

## 5.2 Test flow

``` text
Dashboard or Tests
  → Configure test
  → Start test
  → Timed typing session
  → Time expires or objective completes
  → Results
  → Retry / exit
```

## 5.3 Lesson flow

``` text
Lessons
  → Browse catalogue
  → Open lesson
  → Read instructions
  → Start lesson
  → Complete exercise
  → Completion feedback
  → Next lesson or retry
```

## 5.4 Account flow

``` text
Sign in / Sign up
  → Validate input
  → Authenticate
  → Dashboard
  → Profile / Settings / Sign out
```

Actual routes and authentication behavior must match the selected
implementation.

------------------------------------------------------------------------

# 6. Technology Stack

## 6.1 Confirmed / discussed direction

  ----------------------------------------------------------------------------
  Layer             Technology        Purpose                Status
  ----------------- ----------------- ---------------------- -----------------
  Frontend          React             Component-based UI     Discussed

  Frontend language TypeScript        Type safety            Discussed

  Styling           Tailwind CSS v4   Utility-first styling  Discussed
                                      and responsive UI      

  Backend           Java              Server-side            Discussed
                                      application            

  Backend framework Spring Boot       REST API and           Discussed
                                      application services   

  Database          PostgreSQL        Persistent             Discussed
                                      user/session/content   
                                      data                   

  Design            Figma             UI/UX design source    Used in project
                                                             workflow

  AI coding support GitHub Copilot    Code generation        Used in workflow
                                      assistance             
  ----------------------------------------------------------------------------

## 6.2 Proposed architecture

A conventional web application architecture is proposed:

``` text
Browser
  → React + TypeScript UI
  → HTTP API client
  → Spring Boot REST API
  → Application / domain services
  → PostgreSQL
```

Optional additions (confirm before adopting): - Redis for caching or
short-lived session data. - Spring Security for
authentication/authorization. - Flyway or Liquibase for database
migrations. - Docker for reproducible local development/deployment. - CI
pipeline for build, tests, lint, and deployment.

These additions are recommendations, not confirmed dependencies for this
app.

------------------------------------------------------------------------

# 7. Dependency Inventory

## 7.1 Dependency status convention

-   **Core candidate:** directly supports the discussed stack.
-   **Recommended:** useful if the corresponding feature is selected.
-   **Optional:** do not install without a requirement.
-   **Verify:** inspect the actual repository before adding; avoid
    duplicates.

## 7.2 Frontend runtime dependencies

  -----------------------------------------------------------------------
  Package                 Purpose                 Status
  ----------------------- ----------------------- -----------------------
  `react`                 UI component runtime    Core candidate

  `react-dom`             Browser rendering       Core candidate

  `react-router-dom`      Client-side routes      Recommended if using
                                                  React Router

  `lucide-react`          Icons                   Optional; use if
                                                  selected by design

  `recharts`              Charts for              Optional; use if chart
                          reports/dashboard       library is selected

  `clsx`                  Conditional class       Optional
                          composition             

  `tailwind-merge`        Resolve conflicting     Optional, useful with
                          Tailwind classes        reusable components
  -----------------------------------------------------------------------

Do not install a package simply because it appears in this table. Check
`package.json` first.

## 7.3 Frontend development dependencies

  -------------------------------------------------------------------------------
  Package                         Purpose                 Status
  ------------------------------- ----------------------- -----------------------
  `typescript`                    Type checking           Core candidate

  `vite`                          Development server and  Recommended if using
                                  bundler                 Vite

  `@vitejs/plugin-react`          React integration for   Recommended with Vite
                                  Vite                    

  `tailwindcss`                   Tailwind CSS v4         Core candidate

  `@tailwindcss/vite`             Tailwind v4 Vite plugin Recommended with Vite

  `eslint`                        Static linting          Recommended

  `typescript-eslint`             TypeScript lint rules   Recommended

  `prettier`                      Code formatting         Optional

  `prettier-plugin-tailwindcss`   Tailwind class sorting  Optional

  `vitest`                        Unit/component tests    Recommended if using
                                                          Vite

  `@testing-library/react`        Component testing       Recommended

  `@testing-library/user-event`   Simulated user          Recommended
                                  interaction             

  `@testing-library/jest-dom`     DOM assertions          Recommended

  `playwright`                    Browser end-to-end      Optional/recommended
                                  tests                   for critical flows
  -------------------------------------------------------------------------------

Exact versions must be selected and locked in the project package
manager lockfile.

## 7.4 Backend dependencies --- proposed Spring Boot

The following are common candidates; inspect the actual backend build
file before adding.

  -----------------------------------------------------------------------
  Dependency / starter    Purpose                 Status
  ----------------------- ----------------------- -----------------------
  Spring Boot Web         REST controllers and    Core candidate
                          HTTP APIs               

  Spring Boot Validation  Request validation      Recommended

  Spring Data JPA         ORM/repository access   Recommended if using
                                                  JPA

  PostgreSQL JDBC driver  PostgreSQL connectivity Core candidate for
                                                  PostgreSQL

  Spring Security         Authentication and      Recommended if accounts
                          authorization           are implemented

  Spring Boot Actuator    Health and operational  Recommended
                          endpoints               

  Flyway or Liquibase     Versioned database      Choose one if
                          migrations              migrations are needed

  Spring Boot Test        Backend test support    Recommended

  Testcontainers          Integration tests with  Optional/recommended
  PostgreSQL              PostgreSQL              

  springdoc-openapi       OpenAPI/Swagger API     Optional
                          documentation           
  -----------------------------------------------------------------------

Do not add both Flyway and Liquibase unless there is a specific
migration strategy requiring both.

## 7.5 Backend build and runtime

-   Java: use the Java version selected for the backend; Java 21 is a
    reasonable proposed baseline, but verify the repository and Spring
    Boot compatibility.
-   Build tool: Maven or Gradle; choose based on the existing project.
-   Dependency versions: manage through the Spring Boot dependency
    management/BOM where appropriate.
-   Configuration: use environment variables or deployment secrets for
    credentials; do not commit secrets.

## 7.6 Database

-   PostgreSQL is the discussed database direction.
-   Use migrations for schema changes if the project adopts Flyway or
    Liquibase.
-   Use constraints and indexes for data integrity and common queries.
-   Define retention/deletion rules for session history.
-   Do not store plaintext passwords.
-   Decide whether guests have persistent records.

## 7.7 Optional infrastructure

  Technology                Possible use                     Status
  ------------------------- -------------------------------- --------------
  Docker / Docker Compose   Local reproducible services      Optional
  Redis                     Cache or ephemeral state         Optional
  GitHub Actions            CI workflow                      Optional
  Cloud hosting             Deployment                       Not selected
  Object storage            Exported files/assets            Not selected
  Email provider            Password reset / notifications   Not selected

------------------------------------------------------------------------

# 8. Tailwind CSS v4 Standards

## 8.1 Setup direction

For Vite, the typical Tailwind v4 setup uses `tailwindcss` and
`@tailwindcss/vite`, with:

``` css
@import "tailwindcss";
```

Do not apply Tailwind v3 initialization commands to a v4 project without
confirming compatibility.

## 8.2 Design system

Use the approved Indigo + Teal direction, slate neutrals, and semantic
states. The Figma design and existing project tokens are the source of
truth.

## 8.3 UI rules

-   Mobile-first responsive classes.
-   Reusable components for buttons, cards, badges, forms, modals,
    alerts, and progress indicators.
-   Consistent spacing, typography, borders, radii, and shadows.
-   Light and dark theme support.
-   Visible keyboard focus.
-   Avoid dynamic class construction that Tailwind cannot detect.
-   Avoid arbitrary values when a design token exists.
-   Use semantic HTML and accessible names.

## 8.4 Suggested breakpoints

Tailwind defaults commonly include `sm`, `md`, `lg`, `xl`, and `2xl`.
Verify the project's configuration if customized.

------------------------------------------------------------------------

# 9. Frontend Architecture

## 9.1 Suggested structure

``` text
src/
├── app/
│   ├── App.tsx
│   └── routes.tsx
├── components/
│   ├── layout/
│   ├── ui/
│   └── typing/
├── features/
│   ├── dashboard/
│   ├── practice/
│   ├── lessons/
│   ├── tests/
│   ├── results/
│   ├── reports/
│   ├── achievements/
│   ├── profile/
│   ├── settings/
│   └── help/
├── lib/
├── services/
├── types/
├── index.css
└── main.tsx
```

## 9.2 Shared components

-   AppShell
-   Sidebar / MobileNav
-   Topbar
-   PageHeader
-   Button
-   Card
-   MetricCard
-   Badge
-   ProgressBar
-   Input / Textarea
-   Modal / Dialog
-   Toast / Alert
-   EmptyState
-   LoadingState
-   ErrorState
-   TypingPrompt
-   TypingStats
-   SessionHistory
-   ChartPanel

## 9.3 State boundaries

-   UI state: open/closed panels, selected filters, modal visibility.
-   Session state: prompt, typed characters, timer, metrics, completion
    status.
-   Server state: profile, saved sessions, lesson catalogue, reports.
-   Persist only according to explicit product rules.

Do not place all application state in one global store by default.
Choose state management only when the actual complexity requires it.

------------------------------------------------------------------------

# 10. Backend Architecture

## 10.1 Suggested logical layers

``` text
REST Controller
  → Application Service
  → Domain / calculation logic
  → Repository
  → PostgreSQL
```

## 10.2 Candidate modules

-   Authentication/account
-   User profile/preferences
-   Passage/content catalogue
-   Lessons and lesson progress
-   Practice sessions
-   Typing tests
-   Results and metrics
-   Reports/analytics
-   Achievements

These are logical boundaries; they do not require separate
microservices. A modular monolith is a reasonable initial approach
unless scale or team needs justify distributed services.

## 10.3 Backend requirements

-   Validate incoming requests.
-   Return consistent error responses.
-   Use DTOs rather than exposing persistence entities directly.
-   Define authorization for user-owned data.
-   Prevent duplicate session completion submissions.
-   Use pagination for growing session history.
-   Add database indexes based on actual query patterns.
-   Keep metric calculations testable and versioned.
-   Never trust client-submitted metrics without validation or an
    explicit trust model.

------------------------------------------------------------------------

# 11. Proposed Data Model

This is a conceptual starting point, not a finalized database schema.

## 11.1 User

Potential fields: - `id` - `display_name` - `email` (if account
authentication uses email) - `created_at` - `updated_at` - account
status fields as required

Password storage should be delegated to a secure authentication
implementation; never store plaintext passwords.

## 11.2 UserPreference

Potential fields: - `user_id` - `theme` - `typing_preferences` -
`language` - `updated_at`

## 11.3 Passage

Potential fields: - `id` - `title` - `content` - `difficulty` -
`language` - `is_active` - `created_at`

## 11.4 Lesson

Potential fields: - `id` - `title` - `description` - `level` -
`sequence` - `passage_id` - completion criteria

## 11.5 TypingSession

Potential fields: - `id` - `user_id` (nullable only if guest sessions
are supported) - `session_type` (practice, lesson, test) -
`passage_id` - `started_at` - `completed_at` - `duration_ms` - `wpm` -
`accuracy` - `correct_characters` - `incorrect_characters` -
`completion_status` - metric algorithm/version identifier

## 11.6 SessionError / CharacterTrace --- optional

Store detailed character-level data only if needed for error analysis.
Consider privacy, storage volume, and retention before persisting full
keystroke traces.

## 11.7 Achievement / UserAchievement

Potential fields: - Achievement definition and criteria. - User unlock
record and timestamp.

## 11.8 Important data decisions

-   Whether incomplete sessions are stored.
-   Whether guests can save history.
-   Whether detailed keystrokes are persisted.
-   Data retention and deletion.
-   Metric versioning and recalculation policy.
-   Whether lesson progress is user-specific.

------------------------------------------------------------------------

# 12. Typing Metrics and Domain Rules

## 12.1 Metrics to display

Potential metrics: - WPM (words per minute) - Accuracy percentage -
Elapsed time - Correct/incorrect character counts - Completion
percentage - Personal best, only if valid historical data exists

## 12.2 Formula decisions

Before implementation, specify: - What counts as a word for WPM
(commonly a standardized character count, but the app must choose). -
Gross versus net WPM. - Accuracy numerator and denominator. - Whether
corrected errors count. - Whether spaces and punctuation count. -
Rounding precision. - Minimum elapsed time handling. - How incomplete
tests are scored.

## 12.3 Testing

Create unit tests for: - Empty input. - Exact match. - Incorrect
characters. - Corrected mistakes. - Backspaces. - Punctuation and
whitespace. - Very short sessions. - Timer expiration. - Duplicate
finish events. - Restart behavior.

Do not finalize user-facing metric explanations until the formulas are
approved.

------------------------------------------------------------------------

# 13. API Design --- Proposed, Not Final

No API routes are confirmed in this document. Do not implement invented
endpoints without agreeing on the API contract.

Potential resource areas: - Authentication - User profile and
preferences - Passages - Lessons - Practice sessions - Typing tests -
Results - Reports - Achievements

For every endpoint, document: - HTTP method and path. - Request/response
DTO. - Authentication and ownership requirements. - Validation rules. -
Pagination and filters. - Error response format. - Idempotency behavior
for session completion. - Tests.

Suggested API versioning convention, if adopted: `/api/v1/`.

------------------------------------------------------------------------

# 14. Security, Privacy, and Reliability

-   Use HTTPS in deployed environments.
-   Keep secrets out of source control.
-   Validate and authorize all user-owned resources on the server.
-   Use secure authentication/session practices appropriate to the
    chosen provider.
-   Protect account and personal data.
-   Avoid collecting raw keystroke traces unless required.
-   Define retention and deletion policies.
-   Rate-limit sensitive endpoints where appropriate.
-   Use safe error messages.
-   Ensure session submissions are idempotent.
-   Back up production data according to an approved operational plan.

------------------------------------------------------------------------

# 15. Testing Strategy

## Frontend

-   Unit tests for metric helpers and state transitions.
-   Component tests for forms, navigation, and typing states.
-   Accessibility checks.
-   Responsive checks at mobile/tablet/desktop sizes.
-   Theme checks in light and dark modes.
-   End-to-end tests for practice, test, results, and sign-in flows if
    authentication exists.

## Backend

-   Unit tests for domain rules and metric calculations.
-   Controller/request validation tests.
-   Repository integration tests.
-   PostgreSQL integration tests if practical.
-   Authorization tests for user-owned records.
-   Idempotency tests for session submission.

## Quality gates

-   TypeScript type-check.
-   ESLint.
-   Frontend build.
-   Backend test suite.
-   Backend package/build.
-   E2E tests for critical user journeys where configured.

------------------------------------------------------------------------

# 16. Accessibility and UX Requirements

-   Semantic HTML.
-   Keyboard-operable controls.
-   Visible focus.
-   Labels and associated validation messages.
-   Dialog focus management.
-   Do not rely on color alone.
-   Readable contrast in both themes.
-   Reduced-motion support.
-   Chart summaries or data-table equivalents.
-   Preserve user input after recoverable failures.
-   Distinguish empty data from loading and error states.

------------------------------------------------------------------------

# 17. Build and Delivery Plan

## Phase 1 --- Foundation

-   Inspect existing repository and dependencies.
-   Confirm package manager, Java version, build tool, and versions.
-   Configure Tailwind CSS v4.
-   Establish tokens, theme handling, app shell, routing, and shared UI.

## Phase 2 --- Core user journey

-   Dashboard.
-   Practice UI and metric engine.
-   Typing test setup and execution.
-   Results.

## Phase 3 --- Learning and analytics

-   Lessons catalogue and flow.
-   Session history.
-   Reports and error review.

## Phase 4 --- Account and supporting areas

-   Profile.
-   Settings.
-   Achievements.
-   Help.
-   Authentication, if approved.

## Phase 5 --- Hardening

-   Accessibility.
-   Responsive and theme QA.
-   API integration.
-   Security review.
-   Automated tests and builds.
-   Deployment readiness.

------------------------------------------------------------------------

# 18. Repository Inspection Checklist

Before adding dependencies or generating files: - \[ \] Inspect frontend
`package.json`. - \[ \] Identify npm, pnpm, or yarn and preserve the
existing lockfile. - \[ \] Check installed React and TypeScript
versions. - \[ \] Confirm Vite or other build tooling. - \[ \] Confirm
Tailwind version and integration. - \[ \] Check existing router and
icon/chart libraries. - \[ \] Inspect backend `pom.xml` or
`build.gradle`. - \[ \] Confirm Java and Spring Boot versions. - \[ \]
Confirm PostgreSQL driver and persistence approach. - \[ \] Check
whether migrations are already configured. - \[ \] Reuse existing
dependencies rather than adding duplicates. - \[ \] Record actual
dependencies and versions in this document after inspection.

------------------------------------------------------------------------

# 19. Copilot Master Prompt

> You are implementing the Typing Master & Spell Corrector application.
> Use the existing repository as the source of truth for installed
> dependencies, versions, routes, API contracts, and architecture. The
> discussed frontend direction is React + TypeScript + Tailwind CSS v4;
> the backend direction is Java + Spring Boot with PostgreSQL. First
> inspect the repository before changing configuration or adding
> packages. Implement only the requested feature, follow the approved
> Figma/prototype and product specification, reuse existing components
> and design tokens, and preserve unrelated behavior. Do not invent API
> endpoints, metric formulas, lesson rules, authentication behavior, or
> optional product features. Use accessible semantic markup, responsive
> mobile-first layouts, light/dark theme support, and relevant
> loading/empty/error/success states. Keep typing calculations testable
> and separate from presentation. Add or update tests for behavior
> changes. Run available type-check, lint, test, and build commands;
> report files changed, dependencies added, assumptions, and validation
> results.

------------------------------------------------------------------------

# 20. Definition of Done

A feature is complete when: - Its user flow works as specified. - UI
matches approved design references. - It is responsive across supported
viewport sizes. - Light/dark themes are correct. - Loading, empty,
error, success, and disabled states are handled as relevant. - Keyboard
and accessibility behavior is checked. - Business rules and metric
formulas are approved and tested. - API contracts are agreed and
correctly integrated. - User data is protected and correctly scoped. -
No fake data or inactive controls are presented as production
functionality. - Type-check, lint, tests, and build pass, or failures
are explicitly documented.

------------------------------------------------------------------------

# 21. Open Decisions Register

  -----------------------------------------------------------------------
  Decision                            Why it matters
  ----------------------------------- -----------------------------------
  Guest mode                          Determines auth requirements and
                                      session ownership

  Authentication provider             Determines backend and frontend
                                      integration

  Exact lesson curriculum             Drives content model and
                                      progression

  WPM and accuracy formulas           Determines live stats, results, and
                                      reports

  Paste/backspace policy              Changes typing behavior and scoring

  Pause/resume                        Affects timer and session state

  Test durations and modes            Drives test configuration and
                                      scoring

  Languages/layouts                   Affects passage content and
                                      keyboard handling

  Session persistence                 Determines API and data model

  Achievement thresholds              Determines achievement engine

  Reports/export                      Determines analytics and export
                                      implementation

  Subscription                        Determines billing scope and
                                      compliance needs

  Admin content tools                 Determines content-management scope

  Offline mode                        Affects caching and synchronization
                                      architecture

  Exact dependency versions           Must be verified from repository
                                      and compatibility constraints
  -----------------------------------------------------------------------

------------------------------------------------------------------------

# 22. Source-of-Truth and Change Control

Use these materials together: 1. Approved Figma design and design
tokens. 2. Existing HTML UI prototype. 3. This feature and technical
specification. 4. Actual repository configuration and lockfiles. 5.
Agreed API contracts and product decisions.

If these conflict: - Do not silently overwrite existing working
behavior. - Identify the conflict. - Ask for clarification when it
changes product behavior or architecture. - Update this document after a
decision is made.

**End of specification.**
