# Tailwind CSS v4 --- Project UI Implementation Guide

**Project:** Typing Master & Spell Corrector\
**Frontend:** React + TypeScript\
**Styling:** Tailwind CSS v4\
**Purpose:** A practical reference for implementing the approved UI
consistently with GitHub Copilot.

------------------------------------------------------------------------

## 1. Goals

Use Tailwind CSS to build a responsive, accessible, maintainable
interface for the typing practice application.

-   Follow the approved UI prototype and design specification.
-   Use reusable React components rather than duplicating long class
    strings.
-   Keep visual styling consistent through shared design tokens.
-   Support desktop, tablet, and mobile layouts.
-   Support light and dark themes.
-   Avoid introducing a second styling framework without approval.

## 2. Tailwind CSS v4 setup

For a Vite + React project, install Tailwind and its Vite plugin:

``` bash
npm install tailwindcss @tailwindcss/vite
```

Configure the Vite plugin in `vite.config.ts`:

``` ts
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";
import tailwindcss from "@tailwindcss/vite";

export default defineConfig({
  plugins: [react(), tailwindcss()],
});
```

In the main stylesheet, for example `src/index.css`:

``` css
@import "tailwindcss";
```

Import that stylesheet from the application entry point:

``` ts
import "./index.css";
```

Run the app using the scripts defined in `package.json`, commonly:

``` bash
npm run dev
```

> Tailwind CSS v4 uses CSS-first configuration for many customizations.
> Do not copy v3 setup instructions such as `npx tailwindcss init -p`
> into a v4 project unless the project deliberately uses a compatible
> legacy setup.

## 3. Suggested frontend structure

``` text
src/
├── app/
│   ├── App.tsx
│   └── routes.tsx
├── components/
│   ├── layout/
│   │   ├── AppShell.tsx
│   │   ├── Sidebar.tsx
│   │   ├── Topbar.tsx
│   │   └── PageContainer.tsx
│   ├── ui/
│   │   ├── Button.tsx
│   │   ├── Card.tsx
│   │   ├── Badge.tsx
│   │   ├── Input.tsx
│   │   ├── ProgressBar.tsx
│   │   └── Modal.tsx
│   └── typing/
│       ├── TypingPrompt.tsx
│       ├── TypingInput.tsx
│       └── TypingStats.tsx
├── features/
│   ├── dashboard/
│   ├── practice/
│   ├── lessons/
│   ├── tests/
│   ├── results/
│   ├── reports/
│   ├── achievements/
│   ├── profile/
│   └── settings/
├── lib/
├── types/
├── index.css
└── main.tsx
```

Keep page-specific logic in its feature folder. Put shared, generic UI
elements in `components/ui`.

## 4. Design tokens

Use the existing approved design system as the source of truth. The
values below are a practical starting point; adjust them to match the
final Figma/prototype values rather than creating a competing palette.

### CSS-first theme tokens

Add or adapt tokens in `src/index.css`:

``` css
@import "tailwindcss";

@theme {
  --font-sans: Inter, ui-sans-serif, system-ui, sans-serif;

  --color-brand-50: #eef2ff;
  --color-brand-100: #e0e7ff;
  --color-brand-500: #6366f1;
  --color-brand-600: #4f46e5;
  --color-brand-700: #4338ca;

  --color-accent-50: #f0fdfa;
  --color-accent-500: #14b8a6;
  --color-accent-600: #0d9488;

  --color-surface: #ffffff;
  --color-page: #f8fafc;
  --color-text: #0f172a;
  --color-muted: #64748b;
  --color-border: #e2e8f0;

  --radius-card: 1rem;
}
```

These `@theme` variables expose matching utilities such as
`bg-brand-600`, `text-muted`, and `rounded-card`.

### Semantic color usage

  ---------------------------------------------------------------------
  Purpose                            Token / intent
  ---------------------------------- ----------------------------------
  Primary actions, active navigation Brand / indigo

  Positive progress, selected        Accent / teal or semantic success
  success states                     

  Page background                    Page

  Cards and panels                   Surface

  Main text                          Text

  Supporting text                    Muted

  Dividers and input borders         Border

  Errors                             Red semantic utility

  Warnings                           Amber semantic utility
  ---------------------------------------------------------------------

Use semantic meaning consistently. Do not use success green for errors
or rely on color alone to communicate state.

## 5. Responsive design

Use mobile-first classes. Unprefixed utilities apply to the base layout;
breakpoint prefixes apply from that breakpoint upward.

Common default breakpoints:

  Prefix     Minimum width
  -------- ---------------
  `sm:`              640px
  `md:`              768px
  `lg:`             1024px
  `xl:`             1280px
  `2xl:`            1536px

Example:

``` tsx
<div className="grid grid-cols-1 gap-4 md:grid-cols-2 xl:grid-cols-4">
  {/* statistic cards */}
</div>
```

Responsive layout guidance:

-   Mobile: one-column content, compact navigation, full-width primary
    actions.
-   Tablet: use two-column grids where content remains readable.
-   Desktop: persistent sidebar and multi-column dashboard layouts.
-   Avoid fixed widths that cause horizontal scrolling.
-   Test long text, empty states, loading states, and keyboard focus at
    every breakpoint.

## 6. Common utility patterns

### Page container

``` tsx
<main className="mx-auto w-full max-w-7xl px-4 py-6 sm:px-6 lg:px-8">
  {/* page content */}
</main>
```

### Card

``` tsx
<section className="rounded-card border border-border bg-surface p-5 shadow-sm">
  {/* card content */}
</section>
```

### Heading and supporting text

``` tsx
<header className="space-y-1">
  <h1 className="text-2xl font-semibold tracking-tight text-text sm:text-3xl">
    Dashboard
  </h1>
  <p className="text-sm text-muted">
    Review your typing progress and continue practicing.
  </p>
</header>
```

### Responsive action row

``` tsx
<div className="flex flex-col gap-3 sm:flex-row sm:items-center">
  <button className="w-full rounded-lg bg-brand-600 px-4 py-2.5 text-sm font-medium text-white hover:bg-brand-700 focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-brand-600 sm:w-auto">
    Start practice
  </button>
  <button className="w-full rounded-lg border border-border px-4 py-2.5 text-sm font-medium text-text hover:bg-page focus-visible:outline-2 focus-visible:outline-offset-2 sm:w-auto">
    View reports
  </button>
</div>
```

## 7. Reusable component conventions

Create typed React components for repeated UI patterns.

``` tsx
import type { ButtonHTMLAttributes, ReactNode } from "react";

type ButtonProps = ButtonHTMLAttributes<HTMLButtonElement> & {
  variant?: "primary" | "secondary" | "ghost" | "danger";
  children: ReactNode;
};

export function Button({
  variant = "primary",
  className = "",
  children,
  ...props
}: ButtonProps) {
  const base =
    "inline-flex items-center justify-center gap-2 rounded-lg px-4 py-2.5 text-sm font-medium transition-colors focus-visible:outline-2 focus-visible:outline-offset-2 disabled:pointer-events-none disabled:opacity-50";

  const variants = {
    primary: "bg-brand-600 text-white hover:bg-brand-700",
    secondary: "border border-border bg-surface text-text hover:bg-page",
    ghost: "text-text hover:bg-page",
    danger: "bg-red-600 text-white hover:bg-red-700",
  };

  return (
    <button
      className={`${base} ${variants[variant]} ${className}`}
      {...props}
    >
      {children}
    </button>
  );
}
```

Guidelines:

-   Keep component APIs small and typed.
-   Use explicit variants for recurring visual treatments.
-   Allow `className` for carefully scoped composition.
-   Avoid building a giant component with many unrelated boolean props.
-   Keep business logic out of purely presentational components.

## 8. Typing-practice UI requirements

The typing screen has interaction-specific visual states. Implement
these deliberately.

### Required states

-   Ready / not started
-   Active typing
-   Correct character
-   Incorrect character
-   Completed
-   Paused, if pause/resume is part of the approved product scope
-   Loading or unavailable prompt
-   Error state

### Styling rules

-   Use a readable monospace font for the typing passage.
-   Maintain adequate line height and comfortable passage width.
-   Make the active caret/current character clearly visible.
-   Distinguish correct and incorrect characters using both color and an
    additional cue where appropriate.
-   Keep live WPM, accuracy, elapsed time, and progress legible without
    distracting from typing.
-   Ensure focus is visible and the typing area works with a keyboard.
-   Do not calculate typing metrics solely in CSS; keep the typing
    engine and metric calculations in application logic.

Example passage container:

``` tsx
<div className="rounded-card border border-border bg-surface p-4 sm:p-6">
  <p className="font-mono text-base leading-8 text-text sm:text-lg">
    {/* Render prompt characters with state-specific classes. */}
  </p>
</div>
```

## 9. Light and dark themes

Use the app's established theme strategy consistently. If the project
uses a class-based dark mode, configure Tailwind v4 accordingly and
toggle the agreed class at the application root.

Example semantic CSS variables:

``` css
:root {
  color-scheme: light;
  --app-page: #f8fafc;
  --app-surface: #ffffff;
  --app-text: #0f172a;
  --app-muted: #64748b;
  --app-border: #e2e8f0;
}

.dark {
  color-scheme: dark;
  --app-page: #0f172a;
  --app-surface: #1e293b;
  --app-text: #f8fafc;
  --app-muted: #94a3b8;
  --app-border: #334155;
}
```

If using CSS variables directly, expose them through the project's theme
tokens or use them consistently in component styles. Do not mix multiple
theme-toggle mechanisms.

Check contrast, charts, borders, focus rings, disabled controls, and
modal overlays in both themes.

## 10. Accessibility checklist

-   Use semantic elements (`main`, `nav`, `header`, `section`, `button`,
    `form`).
-   Use real buttons for actions and links for navigation.
-   Provide visible keyboard focus.
-   Associate labels with form controls.
-   Do not communicate state through color alone.
-   Ensure dialogs manage focus and support keyboard dismissal when
    appropriate.
-   Use accessible names for icon-only buttons.
-   Respect reduced-motion preferences for nonessential animation.
-   Check text and control contrast in light and dark modes.
-   Keep keyboard navigation predictable, especially in the typing
    interface.

## 11. Performance and maintainability

-   Prefer standard Tailwind utilities over custom CSS for ordinary
    layout and styling.
-   Use custom CSS for design tokens, complex states, and cases
    utilities do not express cleanly.
-   Avoid excessive arbitrary values when a shared token is appropriate.
-   Do not dynamically construct Tailwind class names such as
    `` `bg-${color}-500` ``. Use complete class strings or a mapping so
    the build can detect them.
-   Avoid unnecessary dependencies for simple UI behavior.
-   Keep chart rendering, typing calculations, routing, and API calls
    separate from presentation.
-   Use stable, meaningful component names and avoid duplicated page
    markup.

## 12. GitHub Copilot implementation rules

When generating or modifying frontend code, Copilot should:

1.  Use React + TypeScript and the project's installed Tailwind CSS v4
    setup.
2.  Follow the approved prototype, Figma design, and this guide; do not
    redesign screens without instruction.
3.  Reuse existing tokens and shared components before adding new ones.
4.  Use mobile-first responsive utilities.
5.  Preserve existing routes, behavior, types, and API contracts unless
    explicitly asked to change them.
6.  Keep UI components separate from domain logic and data fetching.
7.  Include loading, empty, error, disabled, and success states where
    relevant.
8.  Implement accessible labels, keyboard support, and focus states.
9.  Avoid arbitrary colors and one-off spacing where a design token
    exists.
10. Do not claim backend integration is complete unless the API is
    actually connected and tested.
11. Do not add placeholder buttons that appear functional but have no
    action; clearly mark intentional prototype-only controls.
12. Run the project's lint, type-check, and build commands when
    available, and report any failures.

### Suggested Copilot prompt

> Implement the requested page using React, TypeScript, and the existing
> Tailwind CSS v4 configuration. Follow the approved Typing Master &
> Spell Corrector UI specification and design tokens. Reuse shared
> components, use mobile-first responsive classes, support light and
> dark themes, and include accessible focus, loading, empty, error, and
> disabled states where applicable. Preserve existing routes and API
> contracts. Do not invent backend endpoints or silently redesign the
> page. Before finishing, summarize files changed and report validation
> results.

## 13. Page implementation order

Implement and validate in this order:

1.  App shell, navigation, page container, theme handling
2.  Dashboard
3.  Practice screen and typing interaction states
4.  Typing test flow
5.  Results screen
6.  Reports and charts
7.  Lessons
8.  Achievements
9.  Profile and settings
10. Help, sign-in, and remaining account flows

After each page, check desktop, tablet, mobile, keyboard navigation, and
both themes.

## 14. Definition of done

A page is ready for review when:

-   It matches the approved design and uses shared tokens.
-   It responds correctly at mobile, tablet, and desktop widths.
-   Light and dark themes are consistent.
-   Interactive controls work and have clear states.
-   Forms and controls are keyboard accessible.
-   No unintended horizontal overflow appears.
-   TypeScript, lint, and build checks pass, or known failures are
    documented.
-   Any sample data or unconnected backend behavior is clearly
    identified.

------------------------------------------------------------------------

**Important:** This file is an implementation guide, not a replacement
for the approved page-by-page product specification or Figma designs.
When a detail conflicts, confirm the intended product behavior before
changing the design.

# 15. Product feature specification --- Typing Master & Spell Corrector

This section describes the product features and the UI responsibilities
for each area. It is intended to guide implementation, not to claim that
every feature is already connected to a backend.

## 15.1 Product purpose

Typing Master & Spell Corrector helps users improve typing speed,
accuracy, consistency, and confidence through guided practice, timed
tests, progress tracking, and feedback.

### Core objectives

-   Let users start typing practice quickly.
-   Offer structured lessons and configurable typing tests.
-   Show meaningful performance metrics after a session.
-   Help users identify recurring typing errors.
-   Track progress over time.
-   Provide a clear, accessible experience across desktop, tablet, and
    mobile.
-   Keep the interface consistent with the approved Indigo + Teal design
    direction.

### Primary user journey

``` text
Open app
  → Sign in or continue as guest (guest mode requires product confirmation)
  → Dashboard
  → Choose Practice, Lesson, or Typing Test
  → Configure session (when applicable)
  → Type passage
  → Finish or reach time limit
  → Results summary
  → Review errors and performance
  → Reports / continue practice
```

## 15.2 Application navigation

Primary navigation areas represented in the UI:

1.  Dashboard
2.  Practice
3.  Lessons
4.  Typing Tests
5.  Results
6.  Reports
7.  Achievements
8.  Profile
9.  Settings
10. Help & Support
11. Sign in / Account

Navigation requirements:

-   Show the active destination clearly.
-   Keep navigation usable on narrow screens; use a compact or
    collapsible pattern.
-   Preserve unsaved or active-session state when navigating only if the
    product explicitly supports it.
-   Provide clear page titles and contextual actions.
-   Use actual route links for navigation rather than clickable
    non-semantic containers.

## 15.3 Dashboard

### Purpose

Give users a quick overview of recent activity and a direct route into
practice.

### Feature requirements

-   Welcome area with user name or neutral greeting.
-   Primary "Start practice" action.
-   Summary metric cards, such as current or recent WPM, accuracy,
    practice time, and completed sessions. Exact metrics and calculation
    definitions must be finalized.
-   Recent session list with date, activity type, and available
    performance summary.
-   Progress visualization for a meaningful time range, if data is
    available.
-   Quick links to lessons, tests, and reports.
-   First-use empty state that explains how to begin.
-   Loading and error states for dashboard data.

### Interactions

-   Start practice opens the practice flow.
-   Recent session opens its result details.
-   Report link opens reports.
-   Metric cards may link to the relevant detailed report when that
    behavior is defined.

### Acceptance checks

-   Dashboard remains readable at mobile widths.
-   Missing history does not produce broken charts or empty cards
    without explanation.
-   Sample metrics are identified as demo data until connected to real
    records.

## 15.4 Practice

### Purpose

Provide a low-friction, repeatable typing workspace.

### Feature requirements

-   Display a practice passage or prompt.
-   Render passage text in a readable monospace style.
-   Provide a focused typing input area.
-   Show live session statistics, such as elapsed time, WPM, accuracy,
    and progress, where supported.
-   Clearly distinguish typed-correct and typed-incorrect characters.
-   Provide a visible current typing position.
-   Include start, restart, and finish actions.
-   Provide contextual hints only when the product defines what a hint
    reveals.
-   Confirm destructive restart/exit actions if they would discard
    meaningful progress.
-   Show completion feedback and route to results.

### Interaction states

-   Ready: passage loaded; session has not started.
-   Active: input accepted and live metrics update.
-   Incorrect input: error styling and error count update.
-   Completed: passage or configured objective is complete.
-   Paused: only if pause/resume is approved and implemented.
-   Error: passage could not load or session could not be saved.

### Typing-engine requirements

-   Keep timing and metric calculations in application logic, not CSS.
-   Define whether WPM uses five characters per word, how elapsed time
    is measured, and whether gross or net WPM is displayed.
-   Define accuracy formula and treatment of corrected mistakes.
-   Define backspace behavior, paste policy, punctuation/case
    sensitivity, and whether users can continue after the prompt ends.
-   Avoid starting the timer merely because the page rendered unless
    that is the explicitly approved behavior.
-   Prevent duplicate completion submissions.
-   Preserve a session identifier where backend persistence is
    implemented.

### Acceptance checks

-   Keyboard focus is obvious and predictable.
-   Live values do not cause disruptive layout shifts.
-   Restart resets timer, passage progress, and session counters
    consistently.
-   Results reflect the agreed metric definitions.

## 15.5 Lessons

### Purpose

Provide structured learning content that progresses from basic to more
advanced typing skills.

### Feature requirements

-   Lesson catalogue with title, description, level, estimated duration,
    and completion status when available.
-   Categories or progression groups if defined by the curriculum.
-   Lesson detail/start view.
-   Clear lesson instructions and expected typing task.
-   Completion state and next-step action.
-   Resume or retry behavior only if supported by product rules.
-   Empty, loading, and error states.

### Curriculum decisions to confirm

-   Lesson levels and ordering.
-   Whether lessons focus on home row, individual keys, words,
    punctuation, numbers, or full passages.
-   Whether progression is linear or users may choose any lesson.
-   Whether lessons require a minimum accuracy or score to pass.
-   Whether lesson completion unlocks subsequent content.

## 15.6 Typing tests

### Purpose

Measure typing performance under a defined test configuration.

### Feature requirements

-   Test setup screen.
-   Duration selection (exact options to be confirmed).
-   Passage or content selection if supported.
-   Start test action.
-   Countdown or elapsed-time display according to the chosen test mode.
-   Live performance metrics.
-   Clear end-of-time behavior.
-   Completion and results route.
-   Retry and return-to-dashboard actions.

### Configuration decisions to confirm

-   Available durations.
-   Whether tests are time-based, passage-based, or both.
-   Whether difficulty and language can be selected.
-   Whether the user can pause.
-   Whether paste is blocked.
-   Whether test results are saved for guests and signed-in users.

### Acceptance checks

-   Test starts only after explicit user action.
-   Time limit and completion rules are deterministic.
-   A test is recorded once, even if the finish action is clicked
    repeatedly.

## 15.7 Results

### Purpose

Explain the outcome of a completed practice session, lesson, or test.

### Feature requirements

-   Session type and completion date/time.
-   Main performance metrics (e.g., WPM and accuracy, using agreed
    formulas).
-   Duration and completion status.
-   Error summary and error review when data is available.
-   Comparison with a previous or personal-best result only when the
    comparison is valid.
-   Retry action.
-   Continue practice action.
-   Link to detailed reports.

### Data rules

-   Clearly distinguish unavailable metrics from zero.
-   Do not invent records or personal-best claims.
-   Avoid comparing unlike session types without explaining the
    difference.
-   Show a useful fallback when error-level data was not recorded.

## 15.8 Reports and progress analytics

### Purpose

Help users understand their typing development over time.

### Feature requirements

-   Date-range filter if supported.
-   Trend visualization for selected performance metrics.
-   Session history table/list.
-   Filters by activity type where available.
-   Summary cards with transparent time periods.
-   No-data state with a path to start practice.
-   Loading and error states.
-   Export action only if export is implemented; otherwise label it as
    unavailable or omit it.

### Chart requirements

-   Use accessible labels and readable axes.
-   Provide a text summary or table equivalent for important chart
    information.
-   Avoid misleading scales and unclear date ranges.
-   Handle missing days without implying zero performance.
-   Ensure charts resize correctly on mobile and in dark mode.

## 15.9 Achievements

### Purpose

Recognize milestones and encourage consistent practice without obscuring
the learning experience.

### Feature requirements

-   Achievement catalogue or grid.
-   Locked and unlocked states.
-   Name, description, and unlock criteria.
-   Unlock date when known.
-   Empty state for users who have not earned achievements.
-   Avoid presenting fabricated achievement progress.

### Product decisions to confirm

-   Achievement definitions and thresholds.
-   Whether streaks are tracked and how a day is defined.
-   Whether achievements are account-wide or device-local.
-   Whether notifications or celebratory animations are desired.

## 15.10 Profile

### Purpose

Let users view and manage their basic account information.

### Feature requirements

-   Display name and account identifier fields as appropriate.
-   Edit profile form, if supported.
-   Save and cancel actions.
-   Validation and inline error messages.
-   Loading and save-success/failure states.
-   Account-related navigation, including sign out when authenticated.

### Data and privacy

-   Do not expose sensitive account details unnecessarily.
-   Do not imply profile changes were saved before the server confirms
    success.
-   Confirm whether profile editing is supported for guest users.

## 15.11 Settings

### Purpose

Allow users to configure preferences that affect the app experience.

### Candidate settings

-   Light / dark / system appearance.
-   Sound effects, if the app includes sound.
-   Typing display preferences, if supported.
-   Language or keyboard layout, if supported.
-   Test/practice preferences, if supported.
-   Account and privacy options, where applicable.

### Requirements

-   Show current values clearly.
-   Apply changes immediately only when safe and expected; otherwise
    provide Save/Cancel.
-   Persist settings according to the agreed account/device strategy.
-   Ensure controls have accessible labels.
-   Include a reset-to-default action only if product requirements
    define it.

## 15.12 Help & Support

### Purpose

Explain how to use the application and provide a route for assistance.

### Feature requirements

-   Frequently asked questions or help topics.
-   Basic explanation of WPM and accuracy once formulas are finalized.
-   Troubleshooting guidance for keyboard/input issues.
-   Contact/support action only if a real destination exists.
-   Clear empty or unavailable state for unconfigured support channels.

## 15.13 Sign-in and account flows

### Purpose

Support authentication and account access if required by the product.

### Candidate screens and flows

-   Sign in.
-   Create account / sign up.
-   Forgot password.
-   Password reset confirmation.
-   Sign out confirmation if needed.
-   Session-expired or authentication-error state.

### Requirements

-   Use proper labels, input types, and validation.
-   Display authentication errors without exposing sensitive
    implementation details.
-   Do not claim login or account creation succeeded until confirmed.
-   Keep auth provider, password policy, and guest access behavior
    configurable according to the chosen backend.

## 15.14 Optional upgrade / subscription

An upgrade or subscription screen is **not a confirmed core
requirement**. Add it only if a monetization model is approved.

If included, specify: - Free versus paid feature boundaries. - Price,
billing period, and renewal terms. - Restore-purchase and cancellation
information where relevant. - Clear handling of payment errors. - No
misleading urgency or hidden charges.

## 16. Shared UI components

Build and reuse components where repeated patterns exist:

  Component             Responsibility
  --------------------- --------------------------------------------------
  AppShell              Global page structure and responsive layout
  Sidebar / MobileNav   Main navigation and active state
  Topbar                Page-level actions, profile, theme control
  PageHeader            Consistent title, description, and actions
  Button                Typed variants and interaction states
  Card                  Shared surface, border, radius, and spacing
  MetricCard            Label, value, supporting context, optional trend
  Badge                 Status, level, or category
  ProgressBar           Accessible progress indication
  Input / Textarea      Form controls with labels and errors
  Modal / Dialog        Confirmations and focused tasks
  Toast / Alert         Action feedback and non-blocking notices
  EmptyState            Explain missing content and next action
  LoadingState          Skeleton or progress indication
  ErrorState            Explain failure and offer retry where possible
  TypingPrompt          Passage rendering and character states
  TypingStats           Live session metrics
  SessionHistory        Reusable session list/table
  ChartPanel            Responsive chart container and summary

Do not create duplicates with slightly different styling for the same
purpose. Extend shared components through explicit variants.

## 17. Cross-cutting UX requirements

### Loading

-   Use stable skeletons or progress indicators for asynchronous
    content.
-   Avoid presenting placeholder numbers as real metrics.
-   Keep layout dimensions stable where practical.

### Empty

-   Explain why there is no content.
-   Provide a relevant next action.
-   Distinguish "no history yet" from "failed to load history."

### Error

-   Use clear, non-technical language.
-   Provide retry when retry is safe.
-   Preserve user-entered data when possible.
-   Log technical details through the app's logging mechanism, not in
    user-facing messages.

### Success

-   Confirm completed actions accurately.
-   Avoid success feedback before persistence or host confirmation.
-   Use concise toast or inline status feedback.

### Responsive behavior

-   Ensure navigation, forms, charts, tables, and typing controls work
    on narrow viewports.
-   Use horizontal scrolling for dense tables only when necessary and
    make it discoverable.
-   Do not hide essential session metrics solely on mobile.

### Accessibility

-   Use semantic HTML.
-   Provide visible focus indicators and logical tab order.
-   Associate errors and helper text with inputs.
-   Ensure dialogs manage focus.
-   Support keyboard-only use.
-   Do not use color as the only signal.
-   Respect reduced-motion preferences.

## 18. Frontend data and API integration boundaries

The UI must not invent API endpoints or assume a backend contract.

Before connecting a page, document: - Request method and route. -
Request and response types. - Authentication requirements. - Loading,
empty, success, and failure behavior. - Pagination/filtering rules for
lists. - Retry and idempotency behavior for session submission. - Which
data is demo-only versus server-backed.

Recommended separation:

``` text
Page / Feature UI
  → feature hook or service
  → typed API client
  → backend endpoint
```

Keep formatting and display concerns in UI components. Keep
calculations, validation rules, and network behavior in domain/service
layers where appropriate.

## 19. Suggested implementation phases

### Phase 1 --- Foundation

-   Tailwind v4 setup and tokens.
-   App shell, navigation, responsive layout.
-   Theme handling.
-   Shared UI primitives.

### Phase 2 --- Core typing journey

-   Dashboard.
-   Practice screen and typing engine integration.
-   Test configuration and timed test flow.
-   Results summary.

### Phase 3 --- Learning and insights

-   Lessons catalogue and lesson flow.
-   Reports and session history.
-   Error review.

### Phase 4 --- Account and supporting features

-   Profile.
-   Settings.
-   Achievements.
-   Help and support.
-   Authentication, if required.

### Phase 5 --- Hardening

-   Accessibility review.
-   Responsive and theme QA.
-   Empty/loading/error states.
-   API contract validation.
-   Type-check, lint, build, and interaction tests.

## 20. Product decisions that must be confirmed

Do not silently decide these while implementing:

-   Guest mode availability and its persistence limits.
-   Authentication provider and required account fields.
-   Lesson curriculum, levels, and unlock rules.
-   Exact WPM and accuracy formulas.
-   Whether corrected errors count toward accuracy.
-   Paste policy and backspace behavior.
-   Whether pause/resume is supported.
-   Typing test durations and completion rules.
-   Supported languages and keyboard layouts.
-   Whether practice and test sessions are saved automatically.
-   Achievement definitions and streak rules.
-   Reports date ranges and export formats.
-   Whether upgrade/subscription is in scope.
-   Profile-editing and account-deletion requirements.
-   Whether user preferences sync across devices.

## 21. Copilot master instruction for feature implementation

Use this prompt when asking GitHub Copilot to implement a feature:

> You are implementing the Typing Master & Spell Corrector frontend
> using React, TypeScript, and Tailwind CSS v4. Treat the approved Figma
> design, HTML prototype, product specification, and this guide as the
> source of truth. First inspect the existing project structure and
> reuse its components, tokens, routes, and API types. Implement only
> the requested feature and preserve unrelated behavior. Use
> mobile-first responsive utilities, support light and dark themes, and
> provide accessible loading, empty, error, success, disabled, and focus
> states as relevant. Keep typing calculations and API calls out of
> presentational components. Do not invent endpoints, metric formulas,
> lesson rules, or authentication behavior; flag missing decisions
> instead. Do not leave controls that appear functional but do nothing.
> Run available type-check, lint, and build commands and report changed
> files, assumptions, and validation results.

## 22. Definition of product completion

The product should not be considered complete merely because screens
render.

A feature is complete when: - The intended user flow works end to end. -
Its UI matches the approved design. - It is responsive and usable in
supported themes. - Its states are implemented, including failure and
empty states. - It uses agreed business rules and typed data
contracts. - Its actions have real behavior or are clearly marked as
prototype-only. - Accessibility and keyboard behavior have been
checked. - Automated checks pass, or known failures are documented. - No
sample data is presented as real user data.
