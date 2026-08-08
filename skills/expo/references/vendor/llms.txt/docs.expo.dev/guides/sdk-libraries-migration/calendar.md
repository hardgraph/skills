---
modificationDate: May 23, 2026
title: Migrate to the new expo-calendar API
description: Migrate from the legacy expo-calendar API to the new class-based expo-calendar API with ExpoCalendar, ExpoCalendarEvent, and hooks.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/guides/sdk-libraries-migration/calendar/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/guides/sdk-libraries-migration/calendar/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > More > SDK libraries migration
Pages in this section:
- [expo-media-library/legacy](https://docs.expo.dev/guides/sdk-libraries-migration/media-library.md)
- [expo-calendar/legacy](https://docs.expo.dev/guides/sdk-libraries-migration/calendar.md) (this page)
- [expo-contacts/legacy](https://docs.expo.dev/guides/sdk-libraries-migration/contacts.md)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Migrate to the new expo-calendar API

Migrate from the legacy expo-calendar API to the new class-based expo-calendar API with ExpoCalendar, ExpoCalendarEvent, and hooks.

The new object-oriented `expo-calendar` API is now stable. The legacy API is available from `expo-calendar/legacy`. Migrate to the root `expo-calendar` import to benefit from the new API and future fixes.

The new API replaces free functions that accepted IDs with methods on class instances. Calendars, events, reminders, and attendees are now represented as class instances with their own methods. Key changes:

-   Operations on calendars, events, reminders, and attendees are now methods on the corresponding instance instead of free functions that accept an ID.
-   `createCalendar`, `createEvent`, and `createReminder` return class instances instead of string IDs.

## Installation

Install the SDK-compatible package that includes the new `expo-calendar` API:

```sh
npx expo install expo-calendar
```

## Importing the new API

Import the legacy API from `expo-calendar/legacy` while you migrate, and import the new API from `expo-calendar`:

```ts
// Before
import * as Calendar from 'expo-calendar/legacy';

// After
import { ExpoCalendar, ExpoCalendarEvent } from 'expo-calendar';
```

## Calendars

### Create a calendar

```ts
// Before
const calendarId = await Calendar.createCalendarAsync({ title: 'My Calendar', color: '#ff0000' });

// After
const calendar = await createCalendar({ title: 'My Calendar', color: '#ff0000' });
```

`createCalendar` returns an `ExpoCalendar` instance, not just an ID.

### List calendars

```ts
// Before
const calendars = await Calendar.getCalendarsAsync(Calendar.EntityTypes.EVENT);

// After
const calendars = await getCalendars(EntityTypes.EVENT);
```

### Get a calendar by ID

```ts
// Before
// No direct equivalent, had to filter from getCalendarsAsync

// After
const calendar = await ExpoCalendar.get(calendarId);
```

### Update a calendar

```ts
// Before
await Calendar.updateCalendarAsync(calendarId, { title: 'Renamed' });

// After
await calendar.update({ title: 'Renamed' });
```

### Delete a calendar

```ts
// Before
await Calendar.deleteCalendarAsync(calendarId);

// After
await calendar.delete();
```

### Get default calendar (iOS only)

```ts
// Before
const calendar = await Calendar.getDefaultCalendarAsync();

// After
const calendar = getDefaultCalendarSync();
```

### Show a calendar picker (iOS only)

```ts
// Before
// No equivalent

// After
const calendar = await presentPicker();
if (calendar) {
  // user selected a calendar
}
```

`presentPicker` returns `null` if the app user dismisses the picker without selecting a calendar.

## Events

### Create an event

```ts
// Before
const eventId = await Calendar.createEventAsync(calendarId, {
  title: 'Lunch',
  startDate,
  endDate,
});

// After
const event = await calendar.createEvent({ title: 'Lunch', startDate, endDate });
```

`createEvent` returns an `ExpoCalendarEvent` instance, not just an ID.

### List events in a calendar

```ts
// Before
const events = await Calendar.getEventsAsync([calendarId], startDate, endDate);

// After
const events = await calendar.listEvents(startDate, endDate);
```

### List events across multiple calendars

```ts
// Before
const events = await Calendar.getEventsAsync([id1, id2], startDate, endDate);

// After
const events = await listEvents([calendar1, calendar2], startDate, endDate);
```

### Get an event by ID

```ts
// Before
const event = await Calendar.getEventAsync(eventId);

// After
const event = await ExpoCalendarEvent.get(eventId);
```

### Update an event

```ts
// Before
await Calendar.updateEventAsync(eventId, { title: 'Lunch with Alex' });

// After
await event.update({ title: 'Lunch with Alex' });
```

The legacy `updateEventAsync` accepted `recurringEventOptions` (iOS only) to target a single occurrence or future occurrences of a recurring event. This is not supported in the new API — `update()` always modifies the entire recurring event series.

### Delete an event

```ts
// Before
await Calendar.deleteEventAsync(eventId);

// After
await event.delete();
```

The legacy `deleteEventAsync` accepted `recurringEventOptions` (iOS only) to target a single occurrence or future occurrences of a recurring event. This is not supported in the new API — `delete()` always deletes the entire recurring event series.

### Open event in calendar

```ts
// Before
await Calendar.openEventInCalendarAsync(params);

// After
await event.openInCalendar(params);
```

The `id` field is no longer part of params — it comes from the event instance. Presentation options (`allowsEditing`, `allowsCalendarPreview`, `startNewActivityTask`) are now passed in the same params object instead of as a separate argument.

### Edit event with native form

```ts
// Before
await Calendar.editEventInCalendarAsync(params);
// or, to create a new event with form
await Calendar.createEventInCalendarAsync({ title, startDate, endDate });

// After
await event.editInCalendar(params);
// or, to create a new event with form
await calendar.addEventWithForm({ title, startDate, endDate });
```

The `id` field is no longer part of params — it comes from the event instance. Presentation options (`startNewActivityTask`) are now passed in the same params object instead of as a separate argument.

### Get recurring event occurrence

```ts
// Before
const event = await Calendar.getEventAsync(eventId, { instanceStartDate });

// After
const event = await ExpoCalendarEvent.get(eventId);
const occurrence = event.getOccurrenceSync({ instanceStartDate });
```

## Attendees

### Get attendees of an event

```ts
// Before
const attendees = await Calendar.getAttendeesForEventAsync(eventId);

// After
const attendees = await event.getAttendees();
```

### Add an attendee

```ts
// Before
const attendeeId = await Calendar.createAttendeeAsync(eventId, {
  email: 'alex@example.com',
  name: 'Alex',
  role: Calendar.AttendeeRole.ATTENDEE,
  type: Calendar.AttendeeType.PERSON,
  status: Calendar.AttendeeStatus.ACCEPTED,
});

// After
const attendee = await event.createAttendee({ email: 'alex@example.com', name: 'Alex' });
```

### Update an attendee (Android only)

```ts
// Before
await Calendar.updateAttendeeAsync(attendeeId, { name: 'Alexander' });

// After
await attendee.update({ name: 'Alexander' });
```

### Delete an attendee (Android only)

```ts
// Before
await Calendar.deleteAttendeeAsync(attendeeId);

// After
await attendee.delete();
```

## Reminders (iOS only)

### Create a reminder

```ts
// Before
const reminderId = await Calendar.createReminderAsync(calendarId, { title: 'Buy milk' });

// After
const reminder = await calendar.createReminder({ title: 'Buy milk' });
```

`createReminder` returns an `ExpoCalendarReminder` instance, not just an ID.

### List reminders

```ts
// Before
const reminders = await Calendar.getRemindersAsync([calendarId], status, startDate, endDate);

// After
const reminders = await calendar.listReminders(startDate, endDate, status);
```

### Get a reminder by ID

```ts
// Before
const reminder = await Calendar.getReminderAsync(reminderId);

// After
const reminder = await ExpoCalendarReminder.get(reminderId);
```

### Update a reminder

```ts
// Before
await Calendar.updateReminderAsync(reminderId, { title: 'Buy oat milk' });

// After
await reminder.update({ title: 'Buy oat milk' });
```

### Delete a reminder

```ts
// Before
await Calendar.deleteReminderAsync(reminderId);

// After
await reminder.delete();
```

## Sources

```ts
// Before
const sources = await Calendar.getSourcesAsync();

// After
const sources = getSourcesSync();
```

`getSourcesAsync` is replaced by the synchronous `getSourcesSync`. Fetching a single source by ID has no direct equivalent in the new API.

## Permissions

```ts
// Before
await Calendar.requestCalendarPermissionsAsync();
await Calendar.getCalendarPermissionsAsync();
await Calendar.requestRemindersPermissionsAsync();
await Calendar.getRemindersPermissionsAsync();

// After
await requestCalendarPermissions();
await getCalendarPermissions();
await requestRemindersPermissions();
await getRemindersPermissions();
```

`useCalendarPermissions` and `useRemindersPermissions` hooks are unchanged.

## Breaking semantic changes

-   Calendars, events, reminders, and attendees are now class instances. Operations are methods on the instance instead of free functions that accept an ID. Use the corresponding `.get(id)` static method to obtain an instance if you only have an ID.
-   `createCalendar`, `createEvent`, and `createReminder` return class instances instead of string IDs.
-   The `Async` suffix is dropped. The majority of the library is asynchronous — only synchronous functions use a `Sync` suffix (for example, `getDefaultCalendarSync`, `getSourcesSync`, `getOccurrenceSync`).
-   `getSourcesAsync` is replaced by the synchronous `getSourcesSync`. Fetching a single source by ID has no direct equivalent.
-   `createEventInCalendarAsync` is renamed to `calendar.addEventWithForm`.
-   `openEventInCalendar` (the fire-and-forget sync variant) is removed. Use `event.openInCalendar()` instead.
-   Attendee operations are now instance methods: creating an attendee is done via `event.createAttendee()` on an `ExpoCalendarEvent` instance; updating and deleting are methods on the resulting `ExpoCalendarAttendee` instance.

## Reference

[Calendar](/versions/latest/sdk/calendar.md) — See the full API reference for expo-calendar.
