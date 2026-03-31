# 8.x -> 9.x Migration Guide

## Prerequisites

Mantine 9.x requires React 19.2 or later. If your project uses an older React version,
you need to update it before migrating to Mantine 9.x. If you cannot update React to 19.2+
yet, you can continue using Mantine 8.x until you are ready to update React and migrate to Mantine 9.x.

## Update dependencies

* Update all `@mantine/*` packages to version 9.x
* If you use `@mantine/tiptap` package, update all `@tiptap/*` packages to the latest `3.x` version
* If you use `@mantine/charts` package, update `recharts` to the latest `3.x` version

## use-form TransformValues type

The second generic type of the `useForm` hook is now the type of transformed values
instead of the transform function type. New usage example:

```tsx
import { useForm } from '@mantine/form';

interface FormValues {
  name: string;
  locationId: string;
}

interface TransformedValues {
  name: string;
  locationId: number;
}

function Demo() {
  const form = useForm<FormValues, TransformedValues>({
    mode: 'uncontrolled',
    initialValues: {
      name: '',
      locationId: '2',
    },

    transformValues: (values) => ({
      ...values,
      locationId: Number(values.locationId),
    }),
  });
}
```

## Text color prop

The `color` prop of the Text and Anchor components was removed.
Use the `c` style prop instead:

```tsx
import { Text } from '@mantine/core';

// 8.x
<Text color="red">Text</Text>

// 9.x
<Text c="red">Text</Text>
```

## Light variant color changes

In Mantine 9, the `light` variant CSS variables were changed to use solid color values
instead of transparency. If you need to keep 8.x behavior during migration,
use `v8CssVariablesResolver`:

```tsx
import {
  Button,
  MantineProvider,
  v8CssVariablesResolver,
} from '@mantine/core';

function Demo() {
  return (
    <MantineProvider cssVariablesResolver={v8CssVariablesResolver}>
      <Button variant="light" color="blue.6">
        Uses 8.x light variant colors
      </Button>
    </MantineProvider>
  );
}
```

## Form resolvers

In 9.x, `@mantine/form` has built-in support for Standard Schema (https://standardschema.dev/).
If your schema library supports Standard Schema (Zod v4, Valibot, ArkType), use the built-in
`schemaResolver` instead of a dedicated resolver package:

8.x:

```tsx
import { z } from 'zod';
import { useForm, zodResolver } from '@mantine/form';

const schema = z.object({
  email: z.string().email({ message: 'Invalid email' }),
});

const form = useForm({
  initialValues: { email: '' },
  validate: zodResolver(schema),
});
```

9.x using Standard Schema (recommended):

```tsx
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

## TypographyStylesProvider

The TypographyStylesProvider component was renamed to Typography:

```tsx
// 8.x
import { TypographyStylesProvider } from '@mantine/core';

// 9.x
import { Typography } from '@mantine/core';
```

## Popover and Tooltip positionDependencies prop

The Popover and Tooltip components no longer accept the `positionDependencies` prop; it is no longer required
-- the position is now calculated automatically.

```tsx
import { Popover } from '@mantine/core';

// 8.x
<Popover position="top" positionDependencies={[props.a, props.b]}>
  {/* ... */}
</Popover>

// 9.x
<Popover position="top">
  {/* ... */}
</Popover>
```

## use-fullscreen hook changes

The use-fullscreen hook was split into two hooks: `useFullscreenElement` and `useFullscreenDocument`.

New usage with the `document` element:

```tsx
import { useFullscreenDocument } from '@mantine/hooks';
import { Button } from '@mantine/core';

function Demo() {
  const { toggle, fullscreen } = useFullscreenDocument();

  return (
    <Button onClick={toggle} color={fullscreen ? 'red' : 'blue'}>
      {fullscreen ? 'Exit Fullscreen' : 'Enter Fullscreen'}
    </Button>
  );
}
```

New usage with a custom target element:

```tsx
import { useFullscreenElement } from '@mantine/hooks';
import { Button, Stack } from '@mantine/core';

function RefDemo() {
  const { ref, toggle, fullscreen } = useFullscreenElement();

  return (
    <Stack align="center">
      <img
        ref={ref}
        src="https://raw.githubusercontent.com/mantinedev/mantine/master/.demo/images/bg-4.png"
        alt="For demo"
        width={200}
      />
      <Button onClick={toggle} color={fullscreen ? 'red' : 'blue'}>
        {fullscreen ? 'Exit Fullscreen' : 'View Image Fullscreen'}
      </Button>
    </Stack>
  );
}
```

## use-mouse hook changes

The use-mouse hook was split into two hooks: `useMouse` and `useMousePosition`.

For document-level mouse tracking, use `useMousePosition` instead of `useMouse`:

```tsx
// 8.x
import { useMouse } from '@mantine/hooks';
const { x, y } = useMouse();

// 9.x
import { useMousePosition } from '@mantine/hooks';
const { x, y } = useMousePosition();
```

## use-mutation-observer hook changes

The use-mutation-observer hook now uses the new callback ref approach.

```tsx
// 8.x
import { useMutationObserver } from '@mantine/hooks';
useMutationObserver(
  (mutations) => console.log(mutations),
  { childList: true },
  document.getElementById('external-element')
);

// 9.x
import { useMutationObserverTarget } from '@mantine/hooks';
useMutationObserverTarget(
  (mutations) => console.log(mutations),
  { childList: true },
  document.getElementById('external-element')
);
```

## Rename hooks types

`@mantine/hooks` types were renamed for consistency:

* `UseScrollSpyReturnType` -> `UseScrollSpyReturnValue`
* `StateHistory` -> `UseStateHistoryValue`
* `OS` -> `UseOSReturnValue`

## Collapse in -> expanded

The Collapse component now uses the `expanded` prop instead of `in`:

```tsx
// 8.x
<Collapse in={false}>{/* ... */}</Collapse>

// 9.x
<Collapse expanded={false}>{/* ... */}</Collapse>
```

## Spoiler initialState -> defaultExpanded

The Spoiler component's `initialState` prop was renamed to `defaultExpanded`:

```tsx
// 8.x
<Spoiler initialState={false} showLabel="Show" hideLabel="Hide">{/* ... */}</Spoiler>

// 9.x
<Spoiler defaultExpanded={false} showLabel="Show" hideLabel="Hide">{/* ... */}</Spoiler>
```

## Grid gutter -> gap

The Grid component `gutter` prop was renamed to `gap`. New `rowGap` and `columnGap` props were also added:

```tsx
// 8.x
<Grid gutter="xl">
  <Grid.Col span={6}>1</Grid.Col>
  <Grid.Col span={6}>2</Grid.Col>
</Grid>

// 9.x
<Grid gap="xl">
  <Grid.Col span={6}>1</Grid.Col>
  <Grid.Col span={6}>2</Grid.Col>
</Grid>

// 9.x: Separate row and column gaps
<Grid rowGap="xl" columnGap="sm">
  <Grid.Col span={6}>1</Grid.Col>
  <Grid.Col span={6}>2</Grid.Col>
</Grid>
```

## Grid overflow="hidden" no longer required

Grid no longer uses negative margins. It uses native CSS `gap`, so `overflow="hidden"` can be safely removed.

## useLocalStorage and useSessionStorage return type

These hooks now correctly include `undefined` in the return type when no `defaultValue` is provided.

```tsx
// 8.x: value typed as string (incorrect)
const [value, setValue] = useLocalStorage({ key: 'my-key' });

// 9.x: value typed as string | undefined
const [value, setValue] = useLocalStorage({ key: 'my-key' });

// Provide defaultValue to keep non-undefined type
const [value, setValue] = useLocalStorage({
  key: 'my-key',
  defaultValue: '',
});
```

The same applies to `readLocalStorageValue`, `useSessionStorage`, and `readSessionStorageValue`.

## Notifications pauseResetOnHover default change

In 9.x, hovering over any notification pauses the auto close timer of all visible notifications.
To keep the 8.x per-notification behavior:

```tsx
import { Notifications } from '@mantine/notifications';

<Notifications pauseResetOnHover="notification" />
```

## createPolymorphicComponent -> polymorphic

`createPolymorphicComponent` was renamed to `polymorphic`:

```tsx
// 8.x
import { createPolymorphicComponent } from '@mantine/core';
const MyButton = createPolymorphicComponent<'button', Props>(_MyButton);

// 9.x
import { polymorphic } from '@mantine/core';
const MyButton = polymorphic<'button', Props>(_MyButton);
```

## useHeadroom return type change

`useHeadroom` now returns an object instead of a boolean:

```tsx
// 8.x
const pinned = useHeadroom({ fixedAt: 120 });

// 9.x
const { pinned, scrollProgress } = useHeadroom({
  fixedAt: 120,
  scrollDistance: 200, // new option
});
```

## Default radius change

The default `theme.defaultRadius` changed from `sm` (4px) to `md` (8px). To keep 8.x behavior:

```tsx
const theme = createTheme({ defaultRadius: 'sm' });
```

## fontWeights theme change

The `medium` font weight changed from 500 to 600. New `fontWeights` theme property:

```tsx
// To keep 8.x behavior:
const theme = createTheme({
  fontWeights: { regular: 400, medium: 500, bold: 700 },
});
```
