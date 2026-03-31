---
name: mantine-v9
description: >
  Migrate a Mantine project from 8.x to 9.x and build with Mantine 9. Use this skill when:
  (1) upgrading Mantine packages to version 9, (2) fixing breaking changes after a Mantine 9
  upgrade, (3) replacing removed or renamed props (color -> c, gutter -> gap, in -> expanded,
  initialState -> defaultExpanded), (4) updating form resolvers to use schemaResolver with
  Standard Schema, (5) replacing removed hooks (useFullscreen, useMouse) with their 9.x
  equivalents, (6) renaming TypographyStylesProvider to Typography, (7) removing
  positionDependencies from Popover/Tooltip, (8) building React applications with Mantine
  components, configuring theming/dark mode, or working with Mantine hooks and forms, or
  (9) any task involving Mantine 8.x to 9.x migration.
metadata:
  version: "9.0.0"
  release_date: "2026-03-31"
---

# Mantine v9

Mantine is a fully-featured React components library with TypeScript support. It provides 100+ hooks and components with native dark mode, CSS-in-JS via CSS modules, and excellent accessibility.

## Focus

This skill focuses on:

- Migrating from Mantine 8.x to 9.x
- **Vite** + **TypeScript** setup (not Next.js or CRA)
- CSS modules with PostCSS preset
- Vitest for testing
- ESLint with eslint-config-mantine

## Prerequisites (9.x)

- React 19.2 or later is required
- Update all `@mantine/*` packages to 9.x
- If using `@mantine/tiptap`, update `@tiptap/*` to latest 3.x
- If using `@mantine/charts`, update `recharts` to latest 3.x

## Key Breaking Changes (8.x -> 9.x)

| 8.x | 9.x | Component/Hook |
|---|---|---|
| `<Text color="red">` | `<Text c="red">` | Text, Anchor |
| `<Collapse in={bool}>` | `<Collapse expanded={bool}>` | Collapse |
| `<Grid gutter="xl">` | `<Grid gap="xl">` | Grid |
| `<Spoiler initialState={bool}>` | `<Spoiler defaultExpanded={bool}>` | Spoiler |
| `<Grid overflow="hidden">` | Remove prop (no longer needed) | Grid |
| `positionDependencies={[...]}` | Remove prop (automatic now) | Popover, Tooltip |
| `TypographyStylesProvider` | `Typography` | Core |
| `zodResolver` / `yupResolver` | `schemaResolver` (Standard Schema) | Form |
| `useFullscreen` | `useFullscreenDocument` / `useFullscreenElement` | Hooks |
| `useMouse` (document) | `useMousePosition` | Hooks |
| `useMutationObserver` (with target) | `useMutationObserverTarget` | Hooks |
| `UseScrollSpyReturnType` | `UseScrollSpyReturnValue` | Hooks types |
| `StateHistory` | `UseStateHistoryValue` | Hooks types |
| `OS` | `UseOSReturnValue` | Hooks types |
| `createPolymorphicComponent` | `polymorphic` | Core utility |
| `useHeadroom` returns `boolean` | Returns `{ pinned, scrollProgress }` | Hooks |
| Default radius `sm` (4px) | Default radius `md` (8px) | Theme |
| `fontWeights.medium` = 500 | `fontWeights.medium` = 600 | Theme |

## New in 9.x

### New Packages

- `@mantine/schedule` — Calendar scheduling with DayView, WeekView, MonthView, YearView, MobileMonthView

### New Components

- `FloatingWindow` — Draggable floating elements
- `OverflowList` — Collapses overflowing items into a menu
- `Marquee` — Continuous scrolling animation
- `Scroller` — Horizontally scrollable content with navigation controls

### New Hooks

- `useCollapse` / `useHorizontalCollapse` — Hook versions of Collapse component
- `useFloatingWindow` — Draggable floating elements
- `useScroller` — Horizontal scroll logic
- `useScrollDirection` — Detect scroll direction
- `useVirtualizedCombobox` — Combobox virtualization without external deps

### Key Enhancements

- Collapse: `orientation="horizontal"` and `keepMounted` props
- Card: `orientation="horizontal"` prop
- All inputs: `loading` prop
- Generic value types for Select, MultiSelect, SegmentedControl, Chip.Group, Radio.Group, etc.
- SimpleGrid: `minColWidth`, `autoFlow`, `autoRows` props
- Grid.Col: `align` prop
- Slider / RangeSlider: `orientation="vertical"`
- Standard Schema support via `schemaResolver`
- New theme `fontWeights` property
- Default radius changed from `sm` to `md`
- New logical style props: `mis`, `mie`, `pis`, `pie`
- Async form validation with AbortSignal
- New validators: `isUrl`, `isOneOf`
- `useDisclosure`: new `set` handler
- AppShell: `mode="static"`

## Form Resolver Migration

Replace dedicated resolver packages with built-in `schemaResolver`:

```tsx
// 9.x (Standard Schema)
import { z } from 'zod/v4';
import { useForm, schemaResolver } from '@mantine/form';

const schema = z.object({
  email: z.email({ error: 'Invalid email' }),
});

const form = useForm({
  initialValues: { email: '' },
  validate: schemaResolver(schema, { sync: true }),
});
```

## Light Variant Color Changes

Light variant now uses solid colors instead of transparency. To keep 8.x behavior:

```tsx
import { MantineProvider, v8CssVariablesResolver } from '@mantine/core';

<MantineProvider cssVariablesResolver={v8CssVariablesResolver}>
  {/* app */}
</MantineProvider>
```

## Installation

See `references/getting-started.md` for Vite template setup, manual installation, and optional packages.

## PostCSS Configuration

Create `postcss.config.cjs`:

```js
module.exports = {
  plugins: {
    "postcss-preset-mantine": {},
    "postcss-simple-vars": {
      variables: {
        "mantine-breakpoint-xs": "36em",
        "mantine-breakpoint-sm": "48em",
        "mantine-breakpoint-md": "62em",
        "mantine-breakpoint-lg": "75em",
        "mantine-breakpoint-xl": "88em",
      },
    },
  },
};
```

## App Setup

```tsx
// src/App.tsx
import "@mantine/core/styles.css";
// Other style imports as needed:
// import '@mantine/dates/styles.css';
// import '@mantine/notifications/styles.css';

import { MantineProvider, createTheme } from "@mantine/core";

const theme = createTheme({
  // Theme customization here
});

function App() {
  return <MantineProvider theme={theme}>{/* Your app */}</MantineProvider>;
}
```

## Critical Prohibitions

- Do NOT skip MantineProvider wrapper — all components require it
- Do NOT forget to import `@mantine/core/styles.css` — components won't style without it
- Do NOT mix Mantine with other UI libraries (e.g., Chakra, MUI) in same project
- Do NOT use inline styles for theme values — use CSS variables or theme object
- Do NOT skip PostCSS setup — responsive mixins won't work
- Do NOT forget `key={form.key('path')}` when using uncontrolled forms

## References (Detailed Guides)

### Migration

- [migration-guide.md](references/migration-guide.md) — Full 8.x to 9.x migration guide with all breaking changes and before/after code examples

### Setup & Configuration

- [getting-started.md](references/getting-started.md) — Installation, Vite setup, project structure
- [styling.md](references/styling.md) — MantineProvider, theme, CSS modules, style props, dark mode

### Core Features

- [components.md](references/components.md) — Core UI components patterns
- [hooks.md](references/hooks.md) — @mantine/hooks utility hooks
- [forms.md](references/forms.md) — @mantine/form, useForm, validation
- [form-api.md](references/form-api.md) — Full @mantine/form API: useForm options, return value, useField, createFormContext, validators, types
- [form-patterns.md](references/form-patterns.md) — Form code examples: nested objects, arrays, async validation, form context, transformValues, useField

### New in 9.x

- [schedule.md](references/schedule.md) — @mantine/schedule calendar views and scheduling
- [new-features.md](references/new-features.md) — New components, hooks, and enhancements in 9.x

### Combobox

- [combobox-api.md](references/combobox-api.md) — Combobox API: useCombobox hook, sub-components, CSS variables, Styles API
- [combobox-patterns.md](references/combobox-patterns.md) — Combobox examples: basic select, searchable, multi-select with pills, groups, custom rendering

### Custom Components

- [custom-components-api.md](references/custom-components-api.md) — Custom component API: factory, useProps, useStyles, createVarsResolver, StylesApiProps, BoxProps, theme helpers
- [custom-components-patterns.md](references/custom-components-patterns.md) — Custom component examples: minimal, CSS variables, compound components, polymorphic, generic, theme integration

### Development

- [testing.md](references/testing.md) — Vitest setup, custom render, mocking
- [eslint.md](references/eslint.md) — eslint-config-mantine setup

## Links

- [Documentation](https://mantine.dev)
- [Releases](https://github.com/mantinedev/mantine/releases)
- [GitHub](https://github.com/mantinedev/mantine)
- [npm](https://www.npmjs.com/package/@mantine/core)
- [Vite template](https://github.com/mantinedev/vite-template)
- [LLM docs](https://mantine.dev/llms.txt)
