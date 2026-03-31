# Mantine 9.0.0 New Features

Comprehensive reference for all new features introduced in Mantine 9.x.

## New Components

### FloatingWindow

Draggable floating element with viewport constraints.

```tsx
import { FloatingWindow } from '@mantine/core';

<FloatingWindow>
  <FloatingWindow.Header>Drag me</FloatingWindow.Header>
  <FloatingWindow.Body>Content</FloatingWindow.Body>
</FloatingWindow>
```

### OverflowList

Displays items and collapses overflow into a single element.

```tsx
import { OverflowList } from '@mantine/core';

<OverflowList
  items={items}
  renderItem={(item) => <Badge key={item}>{item}</Badge>}
  renderOverflow={(overflowCount) => <Badge>+{overflowCount}</Badge>}
/>
```

### Marquee

Continuous scrolling animation.

```tsx
import { Marquee } from '@mantine/core';

<Marquee speed={40} pauseOnHover>
  <span>Scrolling content</span>
</Marquee>
```

### Scroller

Horizontally scrollable content with navigation controls. Supports trackpad, shift+wheel, touch, and mouse drag.

```tsx
import { Scroller } from '@mantine/core';

<Scroller>
  {items.map((item) => (
    <div key={item}>{item}</div>
  ))}
</Scroller>
```

## New Hooks

### useCollapse / useHorizontalCollapse

Hook versions of Collapse for animating height (or width) from 0 to auto.

```tsx
import { useCollapse, useHorizontalCollapse } from '@mantine/core';

function Demo() {
  const { ref, toggle, opened } = useCollapse();

  return (
    <>
      <button onClick={toggle}>Toggle</button>
      <div ref={ref}>Collapsible content</div>
    </>
  );
}
```

### useFloatingWindow

Creates draggable floating elements with viewport constraints. Powers the FloatingWindow component.

```tsx
import { useFloatingWindow } from '@mantine/core';
```

### useScroller

Logic for horizontally scrollable containers with navigation controls.

```tsx
import { useScroller } from '@mantine/core';
```

### useScrollDirection

Detects scroll direction (up/down).

```tsx
import { useScrollDirection } from '@mantine/hooks';

function Demo() {
  const direction = useScrollDirection(); // 'up' | 'down'
  return <div>Scrolling {direction}</div>;
}
```

### useVirtualizedCombobox

Combobox virtualization without external library dependencies. Recommended with `@tanstack/react-virtual`.

```tsx
import { useVirtualizedCombobox } from '@mantine/core';
```

## Component Enhancements

### Collapse

| Prop | Type | Description |
| --- | --- | --- |
| `orientation` | `'vertical' \| 'horizontal'` | Animate height or width |
| `keepMounted` | `boolean` | Set `false` to unmount collapsed content |

```tsx
<Collapse in={opened} orientation="horizontal">
  <div>Horizontal collapse</div>
</Collapse>

<Collapse in={opened} keepMounted={false}>
  <div>Unmounted when closed</div>
</Collapse>
```

### Card

```tsx
// Horizontal card layout
<Card orientation="horizontal">
  <Card.Section>
    <Image src="photo.jpg" h="100%" />
  </Card.Section>
  <div>Card content beside image</div>
</Card>
```

### Input Loading

All inputs now support `loading` and `loadingPosition` props.

```tsx
<TextInput loading loadingPosition="right" />
<Select loading data={[]} />
```

### Generic Value Types

Select, MultiSelect, SegmentedControl, Chip.Group, Switch.Group, Checkbox.Group, and Radio.Group support primitive values (numbers, booleans, strings, bigint).

```tsx
<Select<number>
  data={[
    { value: 1, label: 'One' },
    { value: 2, label: 'Two' },
  ]}
  value={1}
  onChange={(val) => console.log(typeof val)} // "number"
/>

<Checkbox.Group<number> value={[1, 2]} onChange={setValues}>
  <Checkbox value={1} label="One" />
  <Checkbox value={2} label="Two" />
</Checkbox.Group>
```

### MultiSelect & TagsInput

| Prop | Type | Description |
| --- | --- | --- |
| `renderPill` | `(item) => ReactNode` | Custom pill rendering |
| `onMaxValues` / `onMaxTags` | `() => void` | Callback when max reached |

```tsx
<MultiSelect
  data={data}
  maxValues={3}
  onMaxValues={() => console.log('Max reached')}
  renderPill={(item) => <CustomPill>{item.label}</CustomPill>}
/>
```

### Checkbox.Group & Switch.Group

```tsx
<Checkbox.Group maxSelectedValues={3} value={value} onChange={setValue}>
  <Checkbox value="a" label="A" />
  <Checkbox value="b" label="B" />
  <Checkbox value="c" label="C" />
  <Checkbox value="d" label="D" />
</Checkbox.Group>
```

### SimpleGrid

| Prop | Type | Description |
| --- | --- | --- |
| `minColWidth` | `number \| string` | Min column width via CSS Grid auto-fill/auto-fit |
| `autoFlow` | `'auto-fill' \| 'auto-fit'` | Switch between auto-fill and auto-fit |
| `autoRows` | `string` | Implicit grid row sizing |

```tsx
<SimpleGrid minColWidth={200}>
  {items}
</SimpleGrid>

<SimpleGrid minColWidth={200} autoFlow="auto-fit">
  {items}
</SimpleGrid>
```

### Grid.Col

```tsx
// Vertical alignment within a column
<Grid>
  <Grid.Col span={6} align="center">Vertically centered</Grid.Col>
  <Grid.Col span={6}>Taller content here</Grid.Col>
</Grid>
```

### Slider & RangeSlider

```tsx
// Vertical orientation
<Slider orientation="vertical" h={200} />

// Hidden marks for value snapping without visual indicators
<Slider
  marks={[
    { value: 0 },
    { value: 25, hidden: true },
    { value: 50, label: '50%' },
    { value: 75, hidden: true },
    { value: 100 },
  ]}
/>
```

### Highlight

```tsx
// Per-term colors
<Highlight
  highlight={[
    { text: 'important', color: 'red' },
    { text: 'note', color: 'blue' },
  ]}
>
  This is an important note about the feature.
</Highlight>

// Whole word matching
<Highlight highlight="the" wholeWord>
  The cat sat on the mat — other is not highlighted.
</Highlight>
```

### Pagination

```tsx
// Start from page 0 (or any number)
<Pagination total={10} startValue={0} />
```

### NumberInput

| Prop | Type | Description |
| --- | --- | --- |
| `onMinReached` | `() => void` | Callback when min value reached |
| `onMaxReached` | `() => void` | Callback when max value reached |
| `selectAllOnFocus` | `boolean` | Select all text on focus |

```tsx
<NumberInput
  min={0}
  max={100}
  onMinReached={() => console.log('Min reached')}
  onMaxReached={() => console.log('Max reached')}
  selectAllOnFocus
/>

// bigint support
<NumberInput value={BigInt(9007199254740991)} />
```

### RingProgress

```tsx
<RingProgress
  sections={[{ value: 40, color: 'blue' }]}
  sectionGap={4}
  startAngle={-90}
/>
```

### ScrollArea

```tsx
<ScrollArea
  startScrollPosition={{ x: 100, y: 0 }}
  onLeftReached={() => console.log('Scrolled to left edge')}
  onRightReached={() => console.log('Scrolled to right edge')}
/>
```

### Indicator

```tsx
// Max value display: shows "99+"
<Indicator label={150} maxValue={99}>
  <Avatar />
</Indicator>

// Show zero
<Indicator label={0} showZero>
  <Avatar />
</Indicator>

// Offset with x/y
<Indicator offset={{ x: 5, y: -3 }}>
  <Avatar />
</Indicator>
```

### AppShell

```tsx
// Static mode: CSS Grid-based, normal document flow (no fixed/sticky)
<AppShell mode="static" header={{ height: 60 }} navbar={{ width: 300 }}>
  <AppShell.Header>Header</AppShell.Header>
  <AppShell.Navbar>Navbar</AppShell.Navbar>
  <AppShell.Main>Main</AppShell.Main>
</AppShell>
```

### Tree

```tsx
// Controlled state
<Tree
  data={treeData}
  expandedState={[expanded, setExpanded]}
  selectedState={[selected, setSelected]}
  checkedState={[checked, setChecked]}
  keepMounted
/>
```

### Rating

```tsx
// Allow clearing by clicking same value again
<Rating allowClear />
```

### JsonInput

```tsx
// Custom indentation
<JsonInput indentSpaces={4} />
```

### List

```tsx
// HTML5 ordered list attributes
<List type="ordered" start={5} reversed>
  <List.Item value={10}>Custom value</List.Item>
  <List.Item>Next item</List.Item>
</List>
```

### Spoiler

```tsx
<Spoiler
  maxHeight={100}
  showAriaLabel="Show full content"
  hideAriaLabel="Hide content"
  showLabel="Show more"
  hideLabel="Hide"
>
  Long content here
</Spoiler>
```

### ColorPicker

```tsx
// Form submission support
<ColorPicker name="color" hiddenInputProps={{ form: 'my-form' }} />
```

### FloatingIndicator

```tsx
<FloatingIndicator
  target={activeRef}
  parent={parentRef}
  onTransitionStart={() => console.log('Moving')}
  onTransitionEnd={() => console.log('Settled')}
/>
```

### LoadingOverlay

```tsx
<LoadingOverlay
  visible={loading}
  onEnter={() => console.log('Enter start')}
  onEntered={() => console.log('Enter complete')}
  onExit={() => console.log('Exit start')}
  onExited={() => console.log('Exit complete')}
/>
```

### Checkbox

```tsx
// Read-only and error styles
<Checkbox readOnly checked />
<Checkbox withErrorStyles error="Required" />
```

### useDisclosure

```tsx
import { useDisclosure } from '@mantine/hooks';

const [opened, { open, close, toggle, set }] = useDisclosure(false);

// New: set handler for explicit boolean control
set(true);  // same as open()
set(false); // same as close()
```

## Theme & Styling

### fontWeights

New theme property for controlling font weight CSS variables.

```tsx
import { createTheme } from '@mantine/core';

const theme = createTheme({
  fontWeights: {
    regular: 400,
    medium: 600,  // changed from 500 in 8.x
    bold: 700,
  },
});
```

| CSS Variable | Default |
| --- | --- |
| `--mantine-font-weight-regular` | 400 |
| `--mantine-font-weight-medium` | 600 |
| `--mantine-font-weight-bold` | 700 |

### Default Radius

Changed from `sm` (4px) to `md` (8px) in 9.x.

### Logical Style Props

New margin/padding props for RTL-aware layouts.

| Prop | CSS Property |
| --- | --- |
| `mis` | `margin-inline-start` |
| `mie` | `margin-inline-end` |
| `pis` | `padding-inline-start` |
| `pie` | `padding-inline-end` |

```tsx
<Box mis="md" pie="lg">RTL-aware spacing</Box>
```

### mod Prop

The `mod` prop now converts camelCase to kebab-case for data attributes.

```tsx
// 9.x: camelCase is auto-converted
<Box mod={{ isActive: true }} />
// Renders: <div data-is-active />
```

## Form Enhancements

### Async Validation

Validation rules can return Promises. Includes debounce support and abort signals.

```tsx
import { useForm, isNotEmpty } from '@mantine/form';

const form = useForm({
  mode: 'uncontrolled',
  validateDebounce: 500,
  validate: {
    username: async (value, _values, signal) => {
      const res = await fetch(`/api/check?name=${value}`, { signal });
      const data = await res.json();
      return data.available ? null : 'Username taken';
    },
    email: isNotEmpty('Required'),
  },
});

// Check validation state
form.validating;           // boolean — true while any async rule is running
form.isValidating('username'); // check specific field
```

### New Validators

```tsx
import { isUrl, isOneOf } from '@mantine/form';

const form = useForm({
  validate: {
    website: isUrl({ protocols: ['https'], allowLocalhost: false }, 'Invalid URL'),
    role: isOneOf(['admin', 'editor', 'viewer'], 'Invalid role'),
  },
});
```

### Radio Support in getInputProps

```tsx
<Radio {...form.getInputProps('color', { type: 'radio', value: 'red' })} />
// Returns checked/defaultChecked instead of value for radio inputs
```

### useForm Type Arguments

The second generic is now the TransformedValues type directly (not the transform function type).

```tsx
const form = useForm<FormValues, TransformedValues>({ ... });
```

## Utility Changes

### polymorphic

`createPolymorphicComponent` renamed to `polymorphic`.

```tsx
// 8.x
import { createPolymorphicComponent } from '@mantine/core';
const MyButton = createPolymorphicComponent<'button', Props>(_MyButton);

// 9.x
import { polymorphic } from '@mantine/core';
const MyButton = polymorphic<'button', Props>(_MyButton);
```

### useHeadroom

Now returns an object instead of a boolean. New `scrollDistance` option.

```tsx
import { useHeadroom } from '@mantine/hooks';

// 8.x: const pinned = useHeadroom({ fixedAt: 120 });
// 9.x:
const { pinned, scrollProgress } = useHeadroom({
  fixedAt: 120,
  scrollDistance: 200, // new option
});
```
