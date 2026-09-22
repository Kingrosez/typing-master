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

  -----------------------------------------------------------------------
  Purpose                             Token / intent
  ----------------------------------- -----------------------------------
  Primary actions, active navigation  Brand / indigo

  Positive progress, selected success Accent / teal or semantic success
  states                              

  Page background                     Page

  Cards and panels                    Surface

  Main text                           Text

  Supporting text                     Muted

  Dividers and input borders          Border

  Errors                              Red semantic utility

  Warnings                            Amber semantic utility
  -----------------------------------------------------------------------

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
