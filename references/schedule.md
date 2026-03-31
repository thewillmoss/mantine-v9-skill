# @mantine/schedule Reference

New package in Mantine 9.x for calendar scheduling views.

```bash
npm install @mantine/schedule
```

Import styles:

```tsx
import '@mantine/schedule/styles.css';
```

## Schedule

Unified container combining all views with built-in navigation and view switching. Supports drag-and-drop event rescheduling.

```tsx
import { Schedule } from '@mantine/schedule';

<Schedule
  events={events}
  dayViewProps={{
    startTime: '08:00:00',
    endTime: '18:00:00',
    intervalMinutes: 30,
  }}
  weekViewProps={{
    startTime: '08:00:00',
    endTime: '18:00:00',
    withWeekendDays: false,
  }}
  monthViewProps={{
    withWeekNumbers: true,
    firstDayOfWeek: 0,
  }}
  yearViewProps={{
    withWeekNumbers: true,
  }}
/>
```

## Event Data

Events must be a stable reference (use `useState` or `useMemo`):

```tsx
interface ScheduleEventData<Payload = unknown> {
  id: string | number;
  title: string;
  start: string;  // ISO date string
  end: string;    // ISO date string
  color?: string;
  allDay?: boolean;
  payload?: Payload;
}
```

## DayView

Single day display with configurable time slots and current time indicator.

```tsx
import { DayView } from '@mantine/schedule';

<DayView
  date="2026-03-31"
  events={events}
  startTime="08:00:00"
  endTime="18:00:00"
  intervalMinutes={30}
  withCurrentTimeIndicator
  withCurrentTimeBubble
  withAllDaySlot
  withDragSlotSelect
  withEventsDragAndDrop
  onTimeSlotClick={(slotStart, slotEnd, e) => {}}
  onAllDaySlotClick={(date, e) => {}}
  onEventClick={(event, e) => {}}
  onEventDrop={(eventId, newStart, newEnd) => {}}
  onSlotDragEnd={(rangeStart, rangeEnd) => {}}
  onDateChange={(date) => {}}
/>
```

| Prop | Type | Default | Description |
|---|---|---|---|
| `date` | `string \| Date` | required | Day to display (YYYY-MM-DD) |
| `events` | `ScheduleEventData[]` | — | Events to display |
| `startTime` | `string` | — | Start time (HH:mm:ss) |
| `endTime` | `string` | — | End time (HH:mm:ss) |
| `intervalMinutes` | `number` | — | Minutes per time slot |
| `slotHeight` | `number \| string` | — | Height of 1hr slot |
| `businessHours` | `[string, string]` | — | Business hours range |
| `highlightBusinessHours` | `boolean` | — | Highlight business hours |
| `withCurrentTimeIndicator` | `boolean` | — | Show current time line |
| `withCurrentTimeBubble` | `boolean` | — | Show time in bubble |
| `withAllDaySlot` | `boolean` | — | Show all-day slot |
| `withDragSlotSelect` | `boolean` | — | Enable drag-to-select |
| `withEventsDragAndDrop` | `boolean` | — | Enable event drag-and-drop |
| `withHeader` | `boolean` | — | Show header |
| `mode` | `'default' \| 'static'` | — | Static disables interactions |
| `renderEvent` | `RenderEvent` | — | Custom event rendering |
| `renderEventBody` | `RenderEventBody` | — | Custom event body |
| `canDragEvent` | `(event) => boolean` | — | Control which events are draggable |

### Callbacks

| Callback | Signature |
|---|---|
| `onTimeSlotClick` | `(slotStart, slotEnd, event) => void` |
| `onAllDaySlotClick` | `(date, event) => void` |
| `onEventClick` | `(event, mouseEvent) => void` |
| `onEventDrop` | `(eventId, newStart, newEnd) => void` |
| `onEventDragStart` | `(event) => void` |
| `onEventDragEnd` | `() => void` |
| `onSlotDragEnd` | `(rangeStart, rangeEnd) => void` |
| `onDateChange` | `(date: string) => void` |
| `onViewChange` | `(view: ScheduleViewLevel) => void` |

## WeekView

Weekly calendar grid with multi-day event spanning.

```tsx
import { WeekView } from '@mantine/schedule';

<WeekView
  date="2026-03-31"
  events={events}
  startTime="08:00:00"
  endTime="18:00:00"
  withDragSlotSelect
  withEventsDragAndDrop
  onTimeSlotClick={handleTimeSlotClick}
  onEventClick={handleEventClick}
  onSlotDragEnd={handleSlotDragEnd}
  onEventDrop={handleEventDrop}
/>
```

Supports all DayView props plus:

| Prop | Type | Description |
|---|---|---|
| `withWeekendDays` | `boolean` | Show/hide weekend columns |
| `withWeekNumbers` | `boolean` | Show week numbers |

## MonthView

Monthly grid with event overflow handling.

```tsx
import { MonthView } from '@mantine/schedule';

<MonthView
  date="2026-03-31"
  events={events}
  withWeekNumbers
  firstDayOfWeek={0}
  onEventClick={handleEventClick}
  onDateChange={setDate}
/>
```

## YearView

12-month overview organized by quarters.

```tsx
import { YearView } from '@mantine/schedule';

<YearView
  date="2026-03-31"
  events={events}
  withWeekNumbers
  onDateChange={setDate}
/>
```

## MobileMonthView

Mobile-optimized month view with event details panel.

```tsx
import { MobileMonthView } from '@mantine/schedule';

<MobileMonthView
  date="2026-03-31"
  events={events}
  onEventClick={handleEventClick}
/>
```

## Full Example: Event CRUD

```tsx
import dayjs from 'dayjs';
import { useState } from 'react';
import { ScheduleEventData, WeekView } from '@mantine/schedule';

function Demo() {
  const [date, setDate] = useState(dayjs().format('YYYY-MM-DD'));
  const [events, setEvents] = useState<ScheduleEventData[]>([]);

  const handleTimeSlotClick = (slotStart: string, slotEnd: string) => {
    setEvents((prev) => [
      ...prev,
      {
        id: crypto.randomUUID(),
        title: 'New Event',
        start: slotStart,
        end: slotEnd,
        color: 'blue',
      },
    ]);
  };

  const handleEventDrop = (eventId: string | number, newStart: string, newEnd: string) => {
    setEvents((prev) =>
      prev.map((e) => (e.id === eventId ? { ...e, start: newStart, end: newEnd } : e))
    );
  };

  return (
    <WeekView
      date={date}
      onDateChange={setDate}
      events={events}
      startTime="08:00:00"
      endTime="18:00:00"
      withDragSlotSelect
      withEventsDragAndDrop
      onTimeSlotClick={handleTimeSlotClick}
      onEventDrop={handleEventDrop}
    />
  );
}
```
