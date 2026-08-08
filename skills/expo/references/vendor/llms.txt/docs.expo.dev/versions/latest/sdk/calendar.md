---
title: Calendar
description: A library that provides an API for interacting with the device's system calendars, events, reminders, and associated records.
sourceCodeUrl: 'https://github.com/expo/expo/tree/sdk-57/packages/expo-calendar/src'
packageName: 'expo-calendar'
platforms: ['ios*', 'android*']
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/versions/latest/sdk/calendar/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/versions/latest/sdk/calendar/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, use llms.txt to find the relevant page as Markdown (.md) instead of guessing.

You are here: Reference (v57.0.0) > Expo SDK (86 pages in this section)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Expo Calendar

A library that provides an API for interacting with the device's system calendars, events, reminders, and associated records.
Android (device only), iOS (device only)

> To provide quicker updates, `expo-calendar` is currently unsupported in Expo Go and Snack. To use it, create a [development build](/develop/development-builds/introduction.md#how-would-you-like-to-build-your-development-build).

`expo-calendar` provides an API for interacting with the device's system calendars, events, reminders, and associated records.

Additionally, it provides methods to launch the system-provided calendar UI to allow the user to view or edit events. On iOS, they present either [`EKEventViewController`](https://developer.apple.com/documentation/eventkitui/ekeventviewcontroller) or [`EKEventEditViewController`](https://developer.apple.com/documentation/eventkitui/ekeventeditviewcontroller) as a modal.

## Installation

```sh
# npm
npx expo install expo-calendar

# yarn
yarn expo install expo-calendar

# pnpm
pnpm expo install expo-calendar

# bun
bun expo install expo-calendar
```

If you are installing this in an [existing React Native app](/bare/overview.md), make sure to [install `expo`](/bare/installing-expo-modules.md) in your project.

## Configuration in app config

You can configure `expo-calendar` using its built-in [config plugin](/config-plugins/introduction.md) if you use config plugins in your project ([Continuous Native Generation (CNG)](/workflow/continuous-native-generation.md)). The plugin allows you to configure various properties that cannot be set at runtime and require building a new app binary to take effect. If your app does **not** use CNG, then you'll need to manually configure the library.

### Example app.json with config plugin

```json
{
  "expo": {
    "plugins": [
      [
        "expo-calendar",
        {
          "calendarPermission": "The app needs to access your calendar."
        }
      ]
    ]
  }
}
```

### Configurable properties

| Name | Default | Description |
| --- | --- | --- |
| `calendarPermission` | `"Allow $(PRODUCT_NAME) to access your calendar"` | Only for: iOS. A string to set the [`NSCalendarsUsageDescription`](#ios) permission message. |
| `remindersPermission` | `"Allow $(PRODUCT_NAME) to access your reminders"` | Only for: iOS. A string to set the [`NSRemindersUsageDescription`](#ios) permission message. |
| `writeOnlyCalendarPermission` | `"Allow $(PRODUCT_NAME) to add events to your calendars"` | Only for: iOS. A string to set the [`NSCalendarsWriteOnlyAccessUsageDescription`](#ios) permission message, shown when requesting write-only calendar access (iOS 17+). Only used when `writeOnlyAccess` is `true`. |
| `writeOnlyAccess` | `false` | Only for: iOS. When `true`, requests write-only calendar access (iOS 17+). Sets `NSCalendarsWriteOnlyAccessUsageDescription` and omits `NSCalendarsFullAccessUsageDescription`. |

To localize an iOS calendar or reminders permission message, add the matching usage description keys to the `ios` object in each [locale file](/guides/localization.md#translating-app-metadata). The `calendarPermission` property sets both `NSCalendarsUsageDescription` and `NSCalendarsFullAccessUsageDescription`, and `remindersPermission` sets both `NSRemindersUsageDescription` and `NSRemindersFullAccessUsageDescription`. Expo writes the localized values to **InfoPlist.strings** during prebuild.

#### Are you using this library in an existing React Native app?

If you're not using Continuous Native Generation ([CNG](/workflow/continuous-native-generation.md)) (you're using native **ios** project manually), then you need to configure following permissions in your native project:

-   For iOS, add `NSCalendarsUsageDescription`, `NSCalendarsFullAccessUsageDescription`, and `NSRemindersUsageDescription` to your project's **ios/[app]/Info.plist**:
    
    ```xml
    <key>NSCalendarsUsageDescription</key>
    <string>Allow $(PRODUCT_NAME) to access your calendar</string>
    <key>NSCalendarsFullAccessUsageDescription</key>
    <string>Allow $(PRODUCT_NAME) to access your calendar</string>
    <key>NSRemindersUsageDescription</key>
    <string>Allow $(PRODUCT_NAME) to access your reminders</string>
    ```
    
    When requesting write-only calendar access on iOS 17+, add `NSCalendarsWriteOnlyAccessUsageDescription` instead of `NSCalendarsFullAccessUsageDescription`:
    
    ```xml
    <key>NSCalendarsWriteOnlyAccessUsageDescription</key>
    <string>Allow $(PRODUCT_NAME) to add events to your calendars</string>
    ```
    

## Usage

```jsx
import * as Calendar from 'expo-calendar';
import { useEffect } from 'react';
import { StyleSheet, View, Text, Button } from 'react-native';

const BasicUsage = () => {
  useEffect(() => {
    (async () => {
      const { status } = await Calendar.requestCalendarPermissions();
      if (status === 'granted') {
        const calendars = Calendar.getCalendars(Calendar.EntityTypes.EVENT);
        console.log('Here are all your calendars:');
        console.log(JSON.stringify(calendars));
      }
    })();
  }, []);

  return (
    <View style={styles.container}>
      <Text>Calendar Module Example</Text>
      <Button title="Create a new calendar" onPress={createCalendar} />
    </View>
  );
};

async function createCalendar() {
  const newCalendar = await Calendar.createCalendar({
    title: 'Expo Calendar',
    color: 'blue',
    entityType: Calendar.EntityTypes.EVENT,
  });
  console.log(`Your new calendar: ${JSON.stringify(newCalendar)}`);
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    alignItems: 'center',
    justifyContent: 'space-around',
  },
});
```

## API

```js
import * as Calendar from 'expo-calendar';
```

Unless specified otherwise, all dates are returned in the ISO 8601 format.

## Hooks

### `useCalendarPermissions(options)`

Supported platforms: Android, iOS.

| Parameter | Type |
| --- | --- |
| `options`(optional) | `PermissionHookOptions<{ writeOnly: boolean }>` |

  

Check or request permissions to access the user's calendars. This uses both `getCalendarPermissions` and `requestCalendarPermissions` to interact with the permissions. On iOS, `writeOnly` requests permission to create calendar events without reading existing calendars or events. It does not grant permission to create, update, or delete calendars.

Returns: `[PermissionResponse | null, RequestPermissionMethod<permissionresponse>, GetPermissionMethod]</permissionresponse>`

Example

```ts
const [status, requestPermission] = Calendar.useCalendarPermissions();
```

### `useRemindersPermissions(options)`

Supported platforms: iOS.

| Parameter | Type |
| --- | --- |
| `options`(optional) | `PermissionHookOptions<object>` |

  

Check or request permissions to access the user's reminders. This uses both `getRemindersPermissions` and `requestRemindersPermissions` to interact with the permissions.

Returns: `[PermissionResponse | null, RequestPermissionMethod<permissionresponse>, GetPermissionMethod]</permissionresponse>`

Example

```ts
const [status, requestPermission] = Calendar.useRemindersPermissions();
```

## Classes

### `ExpoCalendar`

Supported platforms: Android, iOS.

Type: Class extends [ExpoCalendar](#expocalendar)

Represents a calendar object that can be accessed and modified using the Expo Calendar Next API.

This class provides properties and methods for interacting with a specific calendar on the device, such as retrieving its events, updating its details, and accessing its metadata.

ExpoCalendar Properties

### `accessLevel`

Supported platforms: Android.

Optional • Type: [CalendarAccessLevel](#calendaraccesslevel)

Level of access that the user has for the calendar.

### `allowedAttendeeTypes`

Supported platforms: Android.

Optional • Type: [AttendeeType[]](#attendeetype)

Attendee types that this calendar supports.

### `allowedAvailabilities`

Supported platforms: Android, iOS.

Type: [Availability[]](#availability)

Availability types that this calendar supports.

### `allowedReminders`

Supported platforms: Android.

Optional • Type: [AlarmMethod[]](#alarmmethod)

Alarm methods that this calendar supports.

### `allowsModifications`

Supported platforms: Android, iOS.

Type: `boolean`

Boolean value that determines whether this calendar can be modified.

### `color`

Supported platforms: Android, iOS.

Optional • Type: `string`

Color used to display this calendar's events.

### `entityType`

Supported platforms: iOS.

Optional • Type: [EntityTypes](#entitytypes)

Whether the calendar is used in the Calendar or Reminders OS app.

### `id`

Supported platforms: Android, iOS.

Type: `string`

Internal ID that represents this calendar on the device.

### `isPrimary`

Supported platforms: Android.

Optional • Type: `boolean`

Boolean value indicating whether this is the device's primary calendar.

### `isSynced`

Supported platforms: Android.

Optional • Type: `boolean`

Indicates whether this calendar is synced and its events stored on the device. Unexpected behavior may occur if this is not set to `true`.

### `isVisible`

Supported platforms: Android.

Optional • Type: `boolean`

Indicates whether the OS displays events on this calendar.

### `name`

Supported platforms: Android.

Optional • Literal type: `union`

Internal system name of the calendar.

Acceptable values are: `string` | `null`

### `ownerAccount`

Supported platforms: Android.

Optional • Type: `string`

Name for the account that owns this calendar.

### `source`

Supported platforms: Android, iOS.

Type: [Source](#source)

Object representing the source to be used for the calendar.

### `sourceId`

Supported platforms: iOS.

Optional • Type: `string`

ID of the source to be used for the calendar. Likely the same as the source for any other locally stored calendars.

### `timeZone`

Supported platforms: Android.

Optional • Type: `string`

Time zone for the calendar.

### `title`

Supported platforms: Android, iOS.

Type: `string`

Visible name of the calendar.

### `type`

Supported platforms: iOS.

Optional • Type: [CalendarType](#calendartype)

Type of calendar this object represents.

ExpoCalendar Methods

### `addEventWithForm(options)`

Supported platforms: Android, iOS.

| Parameter | Type |
| --- | --- |
| `options`(optional) | [AddEventWithFormOptions](#addeventwithformoptions) |

  

Presents the system-provided dialog to create a new event in this calendar, pre-filled with the provided data. Requires at minimum write-only calendar permission.

Returns: `Promise<dialogeventresult>`

### `createEvent(details)`

Supported platforms: Android, iOS.

| Parameter | Type |
| --- | --- |
| `details` | [Partial](https://www.typescriptlang.org/docs/handbook/utility-types.html#partialtype)<[Omit](https://www.typescriptlang.org/docs/handbook/utility-types.html#omittype-keys)<[Event](#event), 'creationDate' | 'lastModifiedDate' | 'originalStartDate' | 'isDetached' | 'status' | 'organizer'\>\> |

  

Creates a new event in the calendar.

Returns: `Promise<expocalendarevent>`

An instance of the created event.

### `createReminder(details)`

Supported platforms: iOS.

| Parameter | Type |
| --- | --- |
| `details` | [Partial](https://www.typescriptlang.org/docs/handbook/utility-types.html#partialtype)<[Reminder](#reminder)\> |

  

Creates a new reminder in the calendar.

Returns: `Promise<expocalendarreminder>`

An instance of the created reminder.

### `delete()`

Supported platforms: Android, iOS.

Deletes the calendar.

Returns: `Promise<void>`

### `get(calendarId)`

Supported platforms: Android, iOS.

| Parameter | Type | Description |
| --- | --- | --- |
| `calendarId` | `string` | The ID of the calendar to get. |

  

Gets a calendar by its ID. Throws an error if the calendar with the given ID does not exist.

Returns: `Promise<expocalendar>`

An [`ExpoCalendar`](#expocalendar) object representing the calendar.

### `listEvents(startDate, endDate)`

Supported platforms: Android, iOS.

| Parameter | Type |
| --- | --- |
| `startDate` | [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) |
| `endDate` | [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) |

  

Returns a calendar event list for the given date range.

Returns: `Promise<expocalendarevent[]>`

### `listReminders(startDate, endDate, status)`

Supported platforms: iOS.

| Parameter | Type | Description |
| --- | --- | --- |
| `startDate`(optional) | [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) | null | Beginning of time period to search for reminders in, or `null` for all completed reminders before `endDate`. Default: `null` |
| `endDate`(optional) | [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) | null | End of time period to search for reminders in, or `null` for all completed reminders after `startDate`. Default: `null` |
| `status`(optional) | [ReminderStatus](#reminderstatus) | null | One of `Calendar.ReminderStatus.COMPLETED` or `Calendar.ReminderStatus.INCOMPLETE`. If not defined, both completed and incomplete reminders will be returned. Default: `null` |

  

Returns a list of reminders matching the provided criteria. If `startDate` and `endDate` are defined, returns all reminders that overlap at all with the `[startDate, endDate]` interval, that is, all reminders that end after the `startDate` or begin before the `endDate`.

Returns: `Promise<expocalendarreminder[]>`

An array of [`ExpoCalendarReminder`](#expocalendarreminder) objects matching the search criteria.

### `update(details)`

Supported platforms: Android, iOS.

| Parameter | Type | Description |
| --- | --- | --- |
| `details` | [Partial](https://www.typescriptlang.org/docs/handbook/utility-types.html#partialtype)<[ModifiableCalendarProperties](#modifiablecalendarproperties)\> | A map of properties to be updated. |

  

Updates the provided details of an existing calendar stored on the device. To remove a property, explicitly set it to `null` in `details`.

Returns: `Promise<void>`

### `ExpoCalendarAttendee`

Supported platforms: Android, iOS.

Type: Class extends [ExpoCalendarAttendee](#expocalendarattendee)

Represents a calendar attendee object.

ExpoCalendarAttendee Properties

### `email`

Supported platforms: Android.

Type: `string`

Email of the attendee.

### `id`

Supported platforms: Android.

Optional • Type: `string`

Internal ID that represents this attendee on the device.

### `isCurrentUser`

Supported platforms: iOS.

Optional • Type: `boolean`

Indicates whether or not this attendee is the current OS user.

### `name`

Supported platforms: Android, iOS.

Type: `string`

Displayed name of the attendee.

### `role`

Supported platforms: Android, iOS.

Type: [AttendeeRole](#attendeerole)

Role of the attendee at the event.

### `status`

Supported platforms: Android, iOS.

Type: [AttendeeStatus](#attendeestatus)

Status of the attendee in relation to the event.

### `type`

Supported platforms: Android, iOS.

Type: [AttendeeType](#attendeetype)

Type of the attendee.

### `url`

Supported platforms: iOS.

Optional • Type: `string`

URL for the attendee.

ExpoCalendarAttendee Methods

### `delete()`

Supported platforms: Android.

Deletes the attendee.

Returns: `Promise<void>`

### `update(details)`

Supported platforms: Android.

| Parameter | Type |
| --- | --- |
| `details` | [Partial](https://www.typescriptlang.org/docs/handbook/utility-types.html#partialtype)<ModifiableAttendeeProperties\> |

  

Updates the attendee.

Returns: `Promise<void>`

### `ExpoCalendarEvent`

Supported platforms: Android, iOS.

Type: Class extends [ExpoCalendarEvent](#expocalendarevent)

Represents a calendar event object that can be accessed and modified using the Expo Calendar Next API.

ExpoCalendarEvent Properties

### `accessLevel`

Supported platforms: Android.

Optional • Type: [EventAccessLevel](#eventaccesslevel)

User's access level for the event.

### `alarms`

Supported platforms: Android, iOS.

Type: [Alarm[]](#alarm)

Array of Alarm objects which control automated reminders to the user.

### `allDay`

Supported platforms: Android, iOS.

Type: `boolean`

Whether the event is displayed as an all-day event on the calendar

### `availability`

Supported platforms: Android, iOS.

Type: [Availability](#availability)

The availability setting for the event.

### `calendarId`

Supported platforms: Android, iOS.

Type: `string`

ID of the calendar that contains this event.

### `creationDate`

Supported platforms: iOS.

Optional • Literal type: `union`

Date when the event record was created.

Acceptable values are: `string` | [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date)

### `endDate`

Supported platforms: Android, iOS.

Literal type: `union`

Date object or string representing the time when the event ends.

Acceptable values are: `string` | [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date)

### `endTimeZone`

Supported platforms: Android.

Optional • Type: `string`

Time zone for the end date of the event.

### `guestsCanInviteOthers`

Supported platforms: Android.

Optional • Type: `boolean`

Whether invited guests can invite other guests.

### `guestsCanModify`

Supported platforms: Android.

Optional • Type: `boolean`

Whether invited guests can modify the details of the event.

### `guestsCanSeeGuests`

Supported platforms: Android.

Optional • Type: `boolean`

Whether invited guests can see other guests.

### `id`

Supported platforms: Android, iOS.

Type: `string`

Internal ID that represents this event on the device.

### `instanceId`

Supported platforms: Android.

Optional • Type: `string`

For instances of recurring events, volatile ID representing this instance. Not guaranteed to always refer to the same instance.

### `isDetached`

Supported platforms: iOS.

Optional • Type: `boolean`

Boolean value indicating whether or not the event is a detached (modified) instance of a recurring event.

### `lastModifiedDate`

Supported platforms: iOS.

Optional • Literal type: `union`

Date when the event record was last modified.

Acceptable values are: `string` | [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date)

### `location`

Supported platforms: Android, iOS.

Literal type: `union`

Location field of the event.

Acceptable values are: `string` | `null`

### `notes`

Supported platforms: Android, iOS.

Type: `string`

Description or notes saved with the event.

### `organizer`

Supported platforms: iOS.

Optional • Type: [Organizer](#organizer)

Organizer of the event. This property is only available on events associated with calendars that are managed by a service such as Google Calendar or iCloud. The organizer is read-only and cannot be set.

### `organizerEmail`

Supported platforms: Android.

Optional • Type: `string`

Email address of the organizer of the event.

### `originalId`

Supported platforms: Android.

Optional • Type: `string`

For detached (modified) instances of recurring events, the ID of the original recurring event.

### `originalStartDate`

Supported platforms: iOS.

Optional • Literal type: `union`

For recurring events, the start date for the first (original) instance of the event.

Acceptable values are: `string` | [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date)

### `recurrenceRule`

Supported platforms: Android, iOS.

Literal type: `union`

Object representing rules for recurring or repeating events. Set to `null` for one-time events. It is either `endDate` or `occurrence` based.

Acceptable values are: [RecurrenceRule](#recurrencerule) | `null`

### `startDate`

Supported platforms: Android, iOS.

Literal type: `union`

Date object or string representing the time when the event starts.

Acceptable values are: `string` | [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date)

### `status`

Supported platforms: Android, iOS.

Type: [EventStatus](#eventstatus)

Status of the event.

### `timeZone`

Supported platforms: Android, iOS.

Type: `string`

Time zone the event is scheduled in. When set to `null`, the event is scheduled to the device's time zone.

### `title`

Supported platforms: Android, iOS.

Type: `string`

Visible name of the event.

### `url`

Supported platforms: iOS.

Optional • Type: `string`

URL for the event.

ExpoCalendarEvent Methods

### `createAttendee(attendee)`

Supported platforms: Android, iOS.

| Parameter | Type |
| --- | --- |
| `attendee` | [Attendee](#attendee) |

  

Creates a new attendee and adds it to this event.

Returns: `Promise<expocalendarattendee>`

### `delete()`

Supported platforms: Android, iOS.

Deletes the event.

Returns: `Promise<void>`

### `editInCalendar(params)`

Supported platforms: Android, iOS.

| Parameter | Type |
| --- | --- |
| `params`(optional) | `CalendarDialogParamsNext | null` |

  

Launches the calendar UI provided by the OS to edit or delete an event.

Returns: `Promise<dialogeventresult>`

A promise which resolves with information about the dialog result.

### `get(eventId)`

Supported platforms: Android, iOS.

| Parameter | Type | Description |
| --- | --- | --- |
| `eventId` | `string` | The ID of the event to get. |

  

Gets an event by its ID. Throws an error if the event with the given ID does not exist.

Returns: `Promise<expocalendarevent>`

An [`ExpoCalendarEvent`](#expocalendarevent) object representing the event.

### `getAttendees()`

Supported platforms: Android, iOS.

Gets all attendees for a given event (or instance of a recurring event).

Returns: `Promise<expocalendarattendee[]>`

An array of [`Attendee`](#attendee) associated with the specified event.

### `getOccurrenceSync(recurringEventOptions)`

Supported platforms: Android, iOS.

| Parameter | Type | Description |
| --- | --- | --- |
| `recurringEventOptions`(optional) | [RecurringEventOptions](#recurringeventoptions) | A map of options for recurring events. Default: `{}` |

  

Returns an event instance for a given event (or instance of a recurring event).

Returns: `ExpoCalendarEvent`

An event instance.

### `openInCalendar(params)`

Supported platforms: Android, iOS.

| Parameter | Type |
| --- | --- |
| `params`(optional) | `CalendarDialogOpenParamsNext | null` |

  

Launches the calendar UI provided by the OS to preview an event.

Returns: `Promise<openeventdialogresult>`

A promise which resolves with information about the dialog result.

### `update(details)`

Supported platforms: Android, iOS.

| Parameter | Type | Description |
| --- | --- | --- |
| `details` | [Partial](https://www.typescriptlang.org/docs/handbook/utility-types.html#partialtype)<[ModifiableEventProperties](#modifiableeventproperties)\> | A map of properties to be updated. |

  

Updates the provided details of an existing calendar stored on the device. To remove a property, explicitly set it to `null` in `details`.

Returns: `Promise<void>`

### `ExpoCalendarReminder`

Supported platforms: iOS.

Type: Class extends [ExpoCalendarReminder](#expocalendarreminder)

Represents a calendar reminder object that can be accessed and modified using the Expo Calendar Next API.

ExpoCalendarReminder Properties

### `alarms`

Supported platforms: iOS.

Optional • Type: [Alarm[]](#alarm)

Array of Alarm objects which control automated alarms to the user about the task.

### `allDay`

Supported platforms: iOS.

Optional • Type: `boolean`

Whether the reminder is an all-day reminder.

### `calendarId`

Supported platforms: iOS.

Optional • Type: `string`

ID of the calendar that contains this reminder.

### `completed`

Supported platforms: iOS.

Optional • Type: `boolean`

Indicates whether or not the task has been completed.

### `completionDate`

Supported platforms: iOS.

Optional • Literal type: `union`

Date object or string representing the date of completion, if `completed` is `true`. Setting this property of a nonnull `Date` will automatically set the reminder's `completed` value to `true`.

Acceptable values are: `string` | [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date)

### `creationDate`

Supported platforms: iOS.

Optional • Literal type: `union`

Date when the reminder record was created.

Acceptable values are: `string` | [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date)

### `dueDate`

Supported platforms: iOS.

Optional • Literal type: `union`

Date object or string representing the time when the reminder task is due.

Acceptable values are: `string` | [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date)

### `id`

Supported platforms: iOS.

Optional • Type: `string`

Internal ID that represents this reminder on the device.

### `lastModifiedDate`

Supported platforms: iOS.

Optional • Literal type: `union`

Date when the reminder record was last modified.

Acceptable values are: `string` | [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date)

### `location`

Supported platforms: iOS.

Optional • Type: `string`

Location field of the reminder

### `notes`

Supported platforms: iOS.

Optional • Type: `string`

Description or notes saved with the reminder.

### `recurrenceRule`

Supported platforms: iOS.

Optional • Literal type: `union`

Object representing rules for recurring or repeated reminders. `null` for one-time tasks.

Acceptable values are: [RecurrenceRule](#recurrencerule) | `null`

### `startDate`

Supported platforms: iOS.

Optional • Literal type: `union`

Date object or string representing the start date of the reminder task.

Acceptable values are: `string` | [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date)

### `timeZone`

Supported platforms: iOS.

Optional • Type: `string`

Time zone the reminder is scheduled in.

### `title`

Supported platforms: iOS.

Optional • Type: `string`

Visible name of the reminder.

### `url`

Supported platforms: iOS.

Optional • Type: `string`

URL for the reminder.

ExpoCalendarReminder Methods

### `delete()`

Supported platforms: iOS.

Deletes the reminder.

Returns: `Promise<void>`

### `get(reminderId)`

Supported platforms: iOS.

| Parameter | Type | Description |
| --- | --- | --- |
| `reminderId` | `string` | The ID of the reminder to get. |

  

Gets a reminder by its ID. Throws an error if the reminder with the given ID does not exist.

Returns: `Promise<expocalendarreminder>`

An [`ExpoCalendarReminder`](#expocalendarreminder) object representing the reminder.

### `update(details)`

Supported platforms: iOS.

| Parameter | Type |
| --- | --- |
| `details` | [Partial](https://www.typescriptlang.org/docs/handbook/utility-types.html#partialtype)<[ModifiableReminderProperties](#modifiablereminderproperties)\> |

  

Returns: `Promise<void>`

## Methods

> **Deprecated:** Use `event.createAttendee()` or import this method from `expo-calendar/legacy`. This method will throw in runtime.

### `Calendar.createAttendeeAsync(eventId, details)`

Supported platforms: Android, iOS.

| Parameter | Type |
| --- | --- |
| `eventId` | `string` |
| `details` | [Attendee](#attendee) |

  

Returns: `Promise<string>`

### `Calendar.createCalendar(details)`

Supported platforms: Android, iOS.

| Parameter | Type | Description |
| --- | --- | --- |
| `details`(optional) | [Partial](https://www.typescriptlang.org/docs/handbook/utility-types.html#partialtype)<Calendar\> | A map of details for the calendar to be created. Default: `{}` |

  

Creates a new calendar on the device, allowing events to be added later and displayed in the OS Calendar app.

Returns: `Promise<expocalendar>`

An [`ExpoCalendar`](#expocalendar) object representing the newly created calendar.

> **Deprecated:** Use `createCalendar()` or import this method from `expo-calendar/legacy`. This method will throw in runtime.

### `Calendar.createCalendarAsync(details)`

Supported platforms: Android, iOS.

| Parameter | Type |
| --- | --- |
| `details`(optional) | [Partial](https://www.typescriptlang.org/docs/handbook/utility-types.html#partialtype)<Calendar\> |

  

Returns: `Promise<string>`

> **Deprecated:** Use `calendar.createEvent()` or import this method from `expo-calendar/legacy`. This method will throw in runtime.

### `Calendar.createEventAsync(calendarId, details)`

Supported platforms: Android, iOS.

| Parameter | Type |
| --- | --- |
| `calendarId` | `string` |
| `details`(optional) | [Partial](https://www.typescriptlang.org/docs/handbook/utility-types.html#partialtype)<[Event](#event)\> |

  

Returns: `Promise<string>`

> **Deprecated:** Use `calendar.addEventWithForm()` or import this method from `expo-calendar/legacy`. This method will throw in runtime.

### `Calendar.createEventInCalendarAsync(eventData, presentationOptions)`

Supported platforms: Android, iOS.

| Parameter | Type |
| --- | --- |
| `eventData`(optional) | [Omit](https://www.typescriptlang.org/docs/handbook/utility-types.html#omittype-keys)<[Partial](https://www.typescriptlang.org/docs/handbook/utility-types.html#partialtype)<[Event](#event)\>, 'id'\> |
| `presentationOptions`(optional) | [PresentationOptions](#presentationoptions) |

  

Returns: `Promise<dialogeventresult>`

> **Deprecated:** Use `calendar.createReminder()` or import this method from `expo-calendar/legacy`. This method will throw in runtime.

### `Calendar.createReminderAsync(calendarId, details)`

Supported platforms: Android, iOS.

| Parameter | Type |
| --- | --- |
| `calendarId` | `string` |
| `details`(optional) | [Reminder](#reminder) |

  

Returns: `Promise<string>`

> **Deprecated:** Use `attendee.delete()` or import this method from `expo-calendar/legacy`. This method will throw in runtime.

### `Calendar.deleteAttendeeAsync(id)`

Supported platforms: Android, iOS.

| Parameter | Type |
| --- | --- |
| `id` | `string` |

  

Returns: `Promise<void>`

> **Deprecated:** Use `calendar.delete()` or import this method from `expo-calendar/legacy`. This method will throw in runtime.

### `Calendar.deleteCalendarAsync(id)`

Supported platforms: Android, iOS.

| Parameter | Type |
| --- | --- |
| `id` | `string` |

  

Returns: `Promise<void>`

> **Deprecated:** Use `event.delete()` or import this method from `expo-calendar/legacy`. This method will throw in runtime.

### `Calendar.deleteEventAsync(id, recurringEventOptions)`

Supported platforms: Android, iOS.

| Parameter | Type |
| --- | --- |
| `id` | `string` |
| `recurringEventOptions`(optional) | [RecurringEventOptions](#recurringeventoptions) |

  

Returns: `Promise<void>`

> **Deprecated:** Use `reminder.delete()` or import this method from `expo-calendar/legacy`. This method will throw in runtime.

### `Calendar.deleteReminderAsync(id)`

Supported platforms: Android, iOS.

| Parameter | Type |
| --- | --- |
| `id` | `string` |

  

Returns: `Promise<void>`

> **Deprecated:** Use `event.editInCalendar()` or import this method from `expo-calendar/legacy`. This method will throw in runtime.

### `Calendar.editEventInCalendarAsync(params, presentationOptions)`

Supported platforms: Android, iOS.

| Parameter | Type |
| --- | --- |
| `params` | [CalendarDialogParams](#calendardialogparams) |
| `presentationOptions`(optional) | [PresentationOptions](#presentationoptions) |

  

Returns: `Promise<dialogeventresult>`

> **Deprecated:** Use `event.getAttendees()` or import this method from `expo-calendar/legacy`. This method will throw in runtime.

### `Calendar.getAttendeesForEventAsync(id)`

Supported platforms: Android, iOS.

| Parameter | Type |
| --- | --- |
| `id` | `string` |

  

Returns: `Promise<attendee[]>`

### `Calendar.getCalendarPermissions(writeOnly)`

Supported platforms: Android, iOS.

| Parameter | Type | Description |
| --- | --- | --- |
| `writeOnly`(optional) | `boolean` | On iOS, whether to check write-only access, which allows creating calendar events without reading existing calendars or events. This does not grant permission to create, update, or delete calendars. |

  

Checks user's permissions for accessing user's calendars.

Returns: `Promise<permissionresponse>`

> **Deprecated:** Use `getCalendarPermissions()` or import this method from `expo-calendar/legacy`. This method will throw in runtime.

### `Calendar.getCalendarPermissionsAsync()`

Supported platforms: Android, iOS.

Returns: `Promise<permissionresponse>`

### `Calendar.getCalendars(entityType)`

Supported platforms: Android, iOS.

| Parameter | Type | Description |
| --- | --- | --- |
| `entityType`(optional) | [EntityTypes](#entitytypes) | **iOS Only.** Not required, but if defined, filters the returned calendars to a specific [entity type](#entitytypes). Possible values are `Calendar.EntityTypes.EVENT` (for calendars shown in the Calendar app) and `Calendar.EntityTypes.REMINDER` (for the Reminders app). Note: If not defined, you will need both permissions: CALENDAR and REMINDERS. |

  

Gets an array of [`ExpoCalendar`](#expocalendar) shared objects with details about the different calendars stored on the device.

Returns: `Promise<expocalendar[]>`

An array of [`ExpoCalendar`](#expocalendar) shared objects matching the provided entity type (if provided).

> **Deprecated:** Use `getCalendars()` or import this method from `expo-calendar/legacy`. This method will throw in runtime.

### `Calendar.getCalendarsAsync(entityType)`

Supported platforms: Android, iOS.

| Parameter | Type |
| --- | --- |
| `entityType`(optional) | `string` |

  

Returns: `Promise<calendar[]>`

> **Deprecated:** Use `getDefaultCalendarSync()` or import this method from `expo-calendar/legacy`. This method will throw in runtime.

### `Calendar.getDefaultCalendarAsync()`

Supported platforms: Android, iOS.

Returns: `Promise<calendar>`

### `Calendar.getDefaultCalendarSync()`

Supported platforms: iOS.

Gets an instance of the default calendar object.

> **Android:** This function is not available on Android. Android does not expose a single system-managed default calendar. Use `getCalendars()` and choose an appropriate writable calendar for your app; `isPrimary` can help identify per-account primary calendars.

Returns: `ExpoCalendar`

An [`ExpoCalendar`](#expocalendar) object that is the user's default calendar.

> **Deprecated:** Use `ExpoCalendarEvent.get()` or import this method from `expo-calendar/legacy`. This method will throw in runtime.

### `Calendar.getEventAsync(id, recurringEventOptions)`

Supported platforms: Android, iOS.

| Parameter | Type |
| --- | --- |
| `id` | `string` |
| `recurringEventOptions`(optional) | [RecurringEventOptions](#recurringeventoptions) |

  

Returns: `Promise<event>`

> **Deprecated:** Use `listEvents()` or import this method from `expo-calendar/legacy`. This method will throw in runtime.

### `Calendar.getEventsAsync(calendarIds, startDate, endDate)`

Supported platforms: Android, iOS.

| Parameter | Type |
| --- | --- |
| `calendarIds` | `string[]` |
| `startDate` | string | [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) |
| `endDate` | string | [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) |

  

Returns: `Promise<event[]>`

> **Deprecated:** Use `ExpoCalendarReminder.get()` or import this method from `expo-calendar/legacy`. This method will throw in runtime.

### `Calendar.getReminderAsync(id)`

Supported platforms: Android, iOS.

| Parameter | Type |
| --- | --- |
| `id` | `string` |

  

Returns: `Promise<reminder>`

> **Deprecated:** Use `calendar.listReminders()` or import this method from `expo-calendar/legacy`. This method will throw in runtime.

### `Calendar.getRemindersAsync(calendarIds, status, startDate, endDate)`

Supported platforms: Android, iOS.

| Parameter | Type |
| --- | --- |
| `calendarIds` | `string[]` |
| `status` | [ReminderStatus](#reminderstatus) |
| `startDate`(optional) | string | [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) |
| `endDate`(optional) | string | [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) |

  

Returns: `Promise<reminder[]>`

### `Calendar.getRemindersPermissions()`

Supported platforms: iOS.

Checks user's permissions for accessing user's reminders.

Returns: `Promise<permissionresponse>`

A promise that resolves to an object of type [`PermissionResponse`](#permissionresponse).

> **Deprecated:** Use `getRemindersPermissions()` or import this method from `expo-calendar/legacy`. This method will throw in runtime.

### `Calendar.getRemindersPermissionsAsync()`

Supported platforms: Android, iOS.

Returns: `Promise<permissionresponse>`

> **Deprecated:** Import this method from `expo-calendar/legacy`. This method will throw in runtime.

### `Calendar.getSourceAsync(id)`

Supported platforms: Android, iOS.

| Parameter | Type |
| --- | --- |
| `id` | `string` |

  

Returns: `Promise`

> **Deprecated:** Use `getSourcesSync()` or import this method from `expo-calendar/legacy`. This method will throw in runtime.

### `Calendar.getSourcesAsync()`

Supported platforms: Android, iOS.

Returns: `Promise<source[]>`

### `Calendar.getSourcesSync()`

Supported platforms: iOS.

Gets an array of Source objects with details about the different sources stored on the device.

> **Android:** This function is not available on Android. Android does not expose a first-class calendar sources API. If you need account-like source information, call `getCalendars()` and inspect each calendar's `source` field.

Returns: `Source[]`

An array of Source objects representing the sources found.

> **Deprecated:** Import this method from `expo-calendar/legacy`. This method will throw in runtime.

### `Calendar.isAvailableAsync()`

Supported platforms: Android, iOS.

Returns: `Promise<boolean>`

### `Calendar.listEvents(calendars, startDate, endDate)`

Supported platforms: Android, iOS.

| Parameter | Type | Description |
| --- | --- | --- |
| `calendars` | (string | [ExpoCalendar](#expocalendar))[] | An array of calendar IDs (`string[]`) or [`ExpoCalendar`](#expocalendar) objects to search for events. |
| `startDate` | [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) | The start date of the time range to search for events. |
| `endDate` | [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) | The end date of the time range to search for events. |

  

Lists events from the device's calendar. It can be used to search events in multiple calendars.

> **Note:** If you want to search events in a single calendar, you can use [`ExpoCalendar.listEvents`](#listeventsstartdate-enddate) instead.

Returns: `Promise<expocalendarevent[]>`

An array of [`ExpoCalendarEvent`](#expocalendarevent) objects representing the events found.

> **Deprecated:** Use `event.openInCalendar()` or import this method from `expo-calendar/legacy`. This method will throw in runtime.

### `Calendar.openEventInCalendar(id)`

Supported platforms: Android, iOS.

| Parameter | Type |
| --- | --- |
| `id` | `string` |

  

Returns: `void`

> **Deprecated:** Use `event.openInCalendar()` or import this method from `expo-calendar/legacy`. This method will throw in runtime.

### `Calendar.openEventInCalendarAsync(params, presentationOptions)`

Supported platforms: Android, iOS.

| Parameter | Type |
| --- | --- |
| `params` | [CalendarDialogParams](#calendardialogparams) |
| `presentationOptions`(optional) | [OpenEventPresentationOptions](#openeventpresentationoptions) |

  

Returns: `Promise<openeventdialogresult>`

### `Calendar.presentPicker()`

Supported platforms: iOS.

Presents the OS calendar picker and returns the selected calendar.

Returns: `Promise<expocalendar>`

An [`ExpoCalendar`](#expocalendar) object or `null` when the picker is cancelled.

### `Calendar.requestCalendarPermissions(writeOnly)`

Supported platforms: Android, iOS.

| Parameter | Type | Description |
| --- | --- | --- |
| `writeOnly`(optional) | `boolean` | On iOS, whether to request write-only access, which allows creating calendar events without reading existing calendars or events. This does not grant permission to create, update, or delete calendars. |

  

Asks the user to grant permissions for accessing user's calendars.

Returns: `Promise<permissionresponse>`

> **Deprecated:** Use `requestCalendarPermissions()` or import this method from `expo-calendar/legacy`. This method will throw in runtime.

### `Calendar.requestCalendarPermissionsAsync()`

Supported platforms: Android, iOS.

Returns: `Promise<permissionresponse>`

> **Deprecated:** Use `requestCalendarPermissions()` or import this method from `expo-calendar/legacy`. This method will throw in runtime.

### `Calendar.requestPermissionsAsync()`

Supported platforms: Android, iOS.

Returns: `Promise<permissionresponse>`

### `Calendar.requestRemindersPermissions()`

Supported platforms: iOS.

Asks the user to grant permissions for accessing user's reminders.

Returns: `Promise<permissionresponse>`

A promise that resolves to an object of type [`PermissionResponse`](#permissionresponse).

> **Deprecated:** Use `requestRemindersPermissions()` or import this method from `expo-calendar/legacy`. This method will throw in runtime.

### `Calendar.requestRemindersPermissionsAsync()`

Supported platforms: Android, iOS.

Returns: `Promise<permissionresponse>`

> **Deprecated:** Use `attendee.update()` or import this method from `expo-calendar/legacy`. This method will throw in runtime.

### `Calendar.updateAttendeeAsync(id, details)`

Supported platforms: Android, iOS.

| Parameter | Type |
| --- | --- |
| `id` | `string` |
| `details`(optional) | [Partial](https://www.typescriptlang.org/docs/handbook/utility-types.html#partialtype)<[Attendee](#attendee)\> |

  

Returns: `Promise<string>`

> **Deprecated:** Use `calendar.update()` or import this method from `expo-calendar/legacy`. This method will throw in runtime.

### `Calendar.updateCalendarAsync(id, details)`

Supported platforms: Android, iOS.

| Parameter | Type |
| --- | --- |
| `id` | `string` |
| `details`(optional) | [Partial](https://www.typescriptlang.org/docs/handbook/utility-types.html#partialtype)<Calendar\> |

  

Returns: `Promise<string>`

> **Deprecated:** Use `event.update()` or import this method from `expo-calendar/legacy`. This method will throw in runtime.

### `Calendar.updateEventAsync(id, details, recurringEventOptions)`

Supported platforms: Android, iOS.

| Parameter | Type |
| --- | --- |
| `id` | `string` |
| `details`(optional) | [Partial](https://www.typescriptlang.org/docs/handbook/utility-types.html#partialtype)<[Event](#event)\> |
| `recurringEventOptions`(optional) | [RecurringEventOptions](#recurringeventoptions) |

  

Returns: `Promise<string>`

> **Deprecated:** Use `reminder.update()` or import this method from `expo-calendar/legacy`. This method will throw in runtime.

### `Calendar.updateReminderAsync(id, details)`

Supported platforms: Android, iOS.

| Parameter | Type |
| --- | --- |
| `id` | `string` |
| `details`(optional) | [Reminder](#reminder) |

  

Returns: `Promise<string>`

## Types

### `AddEventWithFormOptions`

Supported platforms: Android, iOS.

Type: [PresentationOptions](#presentationoptions) extended by:

| Property | Type | Description |
| --- | --- | --- |
| alarms(optional) | [Alarm[]](#alarm) | Supported platforms: iOS. |
| allDay(optional) | `boolean` | - |
| endDate(optional) | [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) | string | - |
| location(optional) | `string` | - |
| notes(optional) | `string` | - |
| recurrenceRule(optional) | [RecurrenceRule](#recurrencerule) | - |
| startDate(optional) | [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) | string | - |
| title(optional) | `string` | - |
| url(optional) | `string` | Supported platforms: iOS. |

### `Alarm`

Supported platforms: Android, iOS.

A method for having the OS automatically remind the user about a calendar item.

| Property | Type | Description |
| --- | --- | --- |
| absoluteDate(optional) | `string` | Supported platforms: iOS. Date object or string representing an absolute time the alarm should occur. Overrides `relativeOffset` and `structuredLocation` if specified alongside either. |
| method(optional) | [AlarmMethod](#alarmmethod) | Supported platforms: Android. Method of alerting the user that this alarm should use. On iOS this is always a notification. |
| relativeOffset(optional) | `number` | Number of minutes from the `startDate` of the calendar item that the alarm should occur. Use negative values to have the alarm occur before the `startDate`. |
| structuredLocation(optional) | [AlarmLocation](#alarmlocation) | - |

### `AlarmLocation`

Supported platforms: Android, iOS.

| Property | Type | Description |
| --- | --- | --- |
| coords(optional) | `{ latitude: number, longitude: number }` | - |
| proximity(optional) | `string` | - |
| radius(optional) | `number` | - |
| title(optional) | `string` | Supported platforms: iOS. |

### `CalendarDialogParams`

Supported platforms: Android, iOS.

| Property | Type | Description |
| --- | --- | --- |
| id | `string` | ID of the event to be presented in the calendar UI. |
| instanceStartDate(optional) | string | [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) | Supported platforms: iOS. Date object representing the start time of the desired instance, if looking for a single instance of a recurring event. If this is not provided and **id** represents a recurring event, the first instance of that event will be returned by default. |

### `DaysOfTheWeek`

Supported platforms: iOS.

| Property | Type | Description |
| --- | --- | --- |
| dayOfTheWeek | [DayOfTheWeek](#dayoftheweek) | Sunday to Saturday - `DayOfTheWeek` enum. |
| weekNumber(optional) | `number` | `-53` to `53` (`0` ignores this field, and a negative indicates a value from the end of the range). |

### `DialogEventResult`

Supported platforms: Android, iOS.

The result of presenting a calendar dialog for creating or editing an event.

| Property | Type | Description |
| --- | --- | --- |
| action | [Extract](https://www.typescriptlang.org/docs/handbook/utility-types.html#extracttype-union)<[CalendarDialogResultActions](#calendardialogresultactions), 'done' | 'saved' | 'canceled' | 'deleted'\> | How user responded to the dialog. On Android, this is always `done` (Android doesn't provide enough information to determine the user's action - the user may have canceled the dialog, saved or deleted the event). On iOS, it can be `saved`, `canceled` or `deleted`. |
| id | `string | null` | The ID of the event that was created or edited. On Android, this is always `null`. On iOS, this is a string when permissions are granted and user confirms the creation or editing of an event. Otherwise, it's `null`. |

### `ModifiableCalendarProperties`

Supported platforms: Android, iOS.

Type: [Pick](https://www.typescriptlang.org/docs/handbook/utility-types.html#picktype-keys)<[ExpoCalendar](#expocalendar), 'color' | 'title'\>

### `ModifiableEventProperties`

Supported platforms: Android, iOS.

Type: [Pick](https://www.typescriptlang.org/docs/handbook/utility-types.html#picktype-keys)<[ExpoCalendarEvent](#expocalendarevent), 'title' | 'location' | 'timeZone' | 'url' | 'notes' | 'alarms' | 'recurrenceRule' | 'availability' | 'startDate' | 'endDate' | 'allDay'\>

### `ModifiableReminderProperties`

Supported platforms: Android, iOS.

Type: [Pick](https://www.typescriptlang.org/docs/handbook/utility-types.html#picktype-keys)<[ExpoCalendarReminder](#expocalendarreminder), 'title' | 'location' | 'timeZone' | 'url' | 'notes' | 'alarms' | 'recurrenceRule' | 'startDate' | 'dueDate' | 'completed' | 'completionDate'\>

### `OpenEventDialogResult`

Supported platforms: Android, iOS.

The result of presenting the calendar dialog for opening (viewing) an event.

| Property | Type | Description |
| --- | --- | --- |
| action | [Extract](https://www.typescriptlang.org/docs/handbook/utility-types.html#extracttype-union)<[CalendarDialogResultActions](#calendardialogresultactions), 'done' | 'canceled' | 'deleted' | 'responded'\> | Indicates how user responded to the dialog. On Android, the `action` is always `done`. On iOS, it can be `done`, `canceled`, `deleted` or `responded`. |

### `OpenEventPresentationOptions`

Supported platforms: Android, iOS.

Type: [PresentationOptions](#presentationoptions) extended by:

| Property | Type | Description |
| --- | --- | --- |
| allowsCalendarPreview(optional) | `boolean` | Supported platforms: iOS. Determines whether event can be shown in calendar day view preview. This property applies only to invitations. Default: `false` |
| allowsEditing(optional) | `boolean` | Supported platforms: iOS. Whether to allow the user to edit the previewed event. This property applies only to events in calendars created by the user. Note that if the user edits the event, the returned action is the one that user performs last. For example, when user previews the event, confirms some edits and finally dismisses the dialog, the event is edited, but response is `canceled`. Default: `false` |

### `PermissionExpiration`

Supported platforms: Android, iOS.

Literal type: `union`

Permission expiration time. Currently, all permissions are granted permanently.

Acceptable values are: `'never'` | `number`

### `PermissionHookOptions`

Supported platforms: Android, iOS.

Literal type: `union`

Acceptable values are: `PermissionHookBehavior` | `Options`

### `PermissionResponse`

Supported platforms: Android, iOS.

An object obtained by permissions get and request functions.

| Property | Type | Description |
| --- | --- | --- |
| canAskAgain | `boolean` | Indicates if user can be asked again for specific permission. If not, one should be directed to the Settings app in order to enable/disable the permission. |
| expires | `PermissionExpiration` | Determines time when the permission expires. |
| granted | `boolean` | A convenience boolean that indicates if the permission is granted. |
| status | [PermissionStatus](/versions/latest/sdk/expo.md#permissionstatus) | Determines the status of the permission. |

### `PresentationOptions`

Supported platforms: Android, iOS.

| Property | Type | Description |
| --- | --- | --- |
| startNewActivityTask(optional) | `boolean` | Supported platforms: Android. Whether to launch the Activity as a new [task](https://developer.android.com/reference/android/content/Intent#FLAG_ACTIVITY_NEW_TASK). If `true`, the promise resolves with `'done'` action immediately after opening the calendar activity. Default: `true` |

### `RecurrenceRule`

Supported platforms: Android, iOS.

A recurrence rule for events or reminders, allowing the same calendar item to recur multiple times. This type is based on [the iOS interface](https://developer.apple.com/documentation/eventkit/ekrecurrencerule/1507320-initrecurrencewithfrequency) which is in turn based on [the iCal RFC](https://tools.ietf.org/html/rfc5545#section-3.8.5.3) so you can refer to those to learn more about this potentially complex interface.

Not all the combinations make sense. For example, when frequency is `DAILY`, setting `daysOfTheMonth` makes no sense.

| Property | Type | Description |
| --- | --- | --- |
| daysOfTheMonth(optional) | `number[]` | Supported platforms: iOS. The days of the month this event occurs on. `-31` to `31` (not including `0`). Negative indicates a value from the end of the range. This field is only valid for `Calendar.Frequency.Monthly`. |
| daysOfTheWeek(optional) | [DaysOfTheWeek[]](#daysoftheweek) | Supported platforms: iOS. The days of the week the event should recur on. An array of [`DaysOfTheWeek`](#daysoftheweek) object. |
| daysOfTheYear(optional) | `number[]` | Supported platforms: iOS. The days of the year this event occurs on. `-366` to `366` (not including `0`). Negative indicates a value from the end of the range. This field is only valid for `Calendar.Frequency.Yearly`. |
| endDate(optional) | string | [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) | Date on which the calendar item should stop recurring; overrides `occurrence` if both are specified. |
| frequency | [Frequency](#frequency) | How often the calendar item should recur. |
| interval(optional) | `number` | Interval at which the calendar item should recur. For example, an `interval: 2` with `frequency: DAILY` would yield an event that recurs every other day. Default: `1` |
| monthsOfTheYear(optional) | [MonthOfTheYear[]](#monthoftheyear) | Supported platforms: iOS. The months this event occurs on. This field is only valid for `Calendar.Frequency.Yearly`. |
| occurrence(optional) | `number` | Number of times the calendar item should recur before stopping. |
| setPositions(optional) | `number[]` | Supported platforms: iOS. TAn array of numbers that filters which recurrences to include. For example, for an event that recurs every Monday, passing 2 here will make it recur every other Monday. `-366` to `366` (not including `0`). Negative indicates a value from the end of the range. This field is only valid for `Calendar.Frequency.Yearly`. |
| weeksOfTheYear(optional) | `number[]` | Supported platforms: iOS. The weeks of the year this event occurs on. `-53` to `53` (not including `0`). Negative indicates a value from the end of the range. This field is only valid for `Calendar.Frequency.Yearly`. |

### `RecurringEventOptions`

Supported platforms: iOS.

Options for specifying a particular instance of a recurring event. This type is used in various methods that operate on recurring events, such as updating or deleting a single occurrence or a set of future occurrences.

| Property | Type | Description |
| --- | --- | --- |
| futureEvents(optional) | `boolean` | Whether future events in the recurring series should also be updated. If `true`, will apply the given changes to the recurring instance specified by `instanceStartDate` and all future events in the series. If `false`, will only apply the given changes to the instance specified by `instanceStartDate`. |
| instanceStartDate(optional) | string | [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) | Date object representing the start time of the desired instance, if looking for a single instance of a recurring event. If this is not provided and **id** represents a recurring event, the first instance of that event will be returned by default. |

### `Source`

Supported platforms: Android, iOS.

A source account that owns a particular calendar. Expo apps will typically not need to interact with `Source` objects.

| Property | Type | Description |
| --- | --- | --- |
| id(optional) | `string` | Supported platforms: iOS. Internal ID that represents this source on the device. |
| isLocalAccount(optional) | `boolean` | Supported platforms: Android. Whether this source is the local phone account. Must be `true` if `type` is `undefined`. |
| name | `string` | Name for the account that owns this calendar and was used to sync the calendar to the device. |
| type | string | [SourceType](#sourcetype) | Type of the account that owns this calendar and was used to sync it to the device. If `isLocalAccount` is falsy then this must be defined, and must match an account on the device along with `name`, or the OS will delete the calendar. On iOS, one of [`SourceType`](#sourcetype)s. |

## Enums

### `AlarmMethod`

Supported platforms: Android.

#### `ALARM`

`AlarmMethod.ALARM = "alarm"`

#### `ALERT`

`AlarmMethod.ALERT = "alert"`

#### `DEFAULT`

`AlarmMethod.DEFAULT = "default"`

#### `EMAIL`

`AlarmMethod.EMAIL = "email"`

#### `SMS`

`AlarmMethod.SMS = "sms"`

### `AttendeeRole`

Supported platforms: Android, iOS.

#### `ATTENDEE`

Supported platforms: Android.

`AttendeeRole.ATTENDEE = "attendee"`

#### `CHAIR`

Supported platforms: iOS.

`AttendeeRole.CHAIR = "chair"`

#### `NONE`

Supported platforms: Android.

`AttendeeRole.NONE = "none"`

#### `NON_PARTICIPANT`

Supported platforms: iOS.

`AttendeeRole.NON_PARTICIPANT = "nonParticipant"`

#### `OPTIONAL`

Supported platforms: iOS.

`AttendeeRole.OPTIONAL = "optional"`

#### `ORGANIZER`

Supported platforms: Android.

`AttendeeRole.ORGANIZER = "organizer"`

#### `PERFORMER`

Supported platforms: Android.

`AttendeeRole.PERFORMER = "performer"`

#### `REQUIRED`

Supported platforms: iOS.

`AttendeeRole.REQUIRED = "required"`

#### `SPEAKER`

Supported platforms: Android.

`AttendeeRole.SPEAKER = "speaker"`

#### `UNKNOWN`

Supported platforms: iOS.

`AttendeeRole.UNKNOWN = "unknown"`

### `AttendeeStatus`

Supported platforms: Android, iOS.

#### `ACCEPTED`

`AttendeeStatus.ACCEPTED = "accepted"`

#### `COMPLETED`

Supported platforms: iOS.

`AttendeeStatus.COMPLETED = "completed"`

#### `DECLINED`

`AttendeeStatus.DECLINED = "declined"`

#### `DELEGATED`

Supported platforms: iOS.

`AttendeeStatus.DELEGATED = "delegated"`

#### `IN_PROCESS`

Supported platforms: iOS.

`AttendeeStatus.IN_PROCESS = "inProcess"`

#### `INVITED`

Supported platforms: Android.

`AttendeeStatus.INVITED = "invited"`

#### `NONE`

Supported platforms: Android.

`AttendeeStatus.NONE = "none"`

#### `PENDING`

Supported platforms: iOS.

`AttendeeStatus.PENDING = "pending"`

#### `TENTATIVE`

`AttendeeStatus.TENTATIVE = "tentative"`

#### `UNKNOWN`

Supported platforms: iOS.

`AttendeeStatus.UNKNOWN = "unknown"`

### `AttendeeType`

Supported platforms: Android, iOS.

#### `GROUP`

Supported platforms: iOS.

`AttendeeType.GROUP = "group"`

#### `NONE`

Supported platforms: Android.

`AttendeeType.NONE = "none"`

#### `OPTIONAL`

Supported platforms: Android.

`AttendeeType.OPTIONAL = "optional"`

#### `PERSON`

Supported platforms: iOS.

`AttendeeType.PERSON = "person"`

#### `REQUIRED`

Supported platforms: Android.

`AttendeeType.REQUIRED = "required"`

#### `RESOURCE`

`AttendeeType.RESOURCE = "resource"`

#### `ROOM`

Supported platforms: iOS.

`AttendeeType.ROOM = "room"`

#### `UNKNOWN`

Supported platforms: iOS.

`AttendeeType.UNKNOWN = "unknown"`

### `Availability`

Supported platforms: Android, iOS.

#### `BUSY`

`Availability.BUSY = "busy"`

#### `FREE`

`Availability.FREE = "free"`

#### `NOT_SUPPORTED`

Supported platforms: iOS.

`Availability.NOT_SUPPORTED = "notSupported"`

#### `TENTATIVE`

`Availability.TENTATIVE = "tentative"`

#### `UNAVAILABLE`

Supported platforms: iOS.

`Availability.UNAVAILABLE = "unavailable"`

### `CalendarAccessLevel`

Supported platforms: Android.

#### `CONTRIBUTOR`

`CalendarAccessLevel.CONTRIBUTOR = "contributor"`

#### `EDITOR`

`CalendarAccessLevel.EDITOR = "editor"`

#### `FREEBUSY`

`CalendarAccessLevel.FREEBUSY = "freebusy"`

#### `NONE`

`CalendarAccessLevel.NONE = "none"`

#### `OVERRIDE`

`CalendarAccessLevel.OVERRIDE = "override"`

#### `OWNER`

`CalendarAccessLevel.OWNER = "owner"`

#### `READ`

`CalendarAccessLevel.READ = "read"`

#### `RESPOND`

`CalendarAccessLevel.RESPOND = "respond"`

#### `ROOT`

`CalendarAccessLevel.ROOT = "root"`

### `CalendarDialogResultActions`

Supported platforms: Android, iOS.

Enum containing all possible user responses to the calendar UI dialogs. Depending on what dialog is presented, a subset of the values applies.

#### `canceled`

Supported platforms: iOS.

`CalendarDialogResultActions.canceled = "canceled"`

The user canceled or dismissed the dialog.

#### `deleted`

Supported platforms: iOS.

`CalendarDialogResultActions.deleted = "deleted"`

The user deleted the event.

#### `done`

`CalendarDialogResultActions.done = "done"`

On Android, this is the only possible result because the OS doesn't provide enough information to determine the user's action - the user may have canceled the dialog, modified the event, or deleted it.

On iOS, this means the user simply closed the dialog.

#### `responded`

Supported platforms: iOS.

`CalendarDialogResultActions.responded = "responded"`

The user responded to and saved a pending event invitation.

#### `saved`

Supported platforms: iOS.

`CalendarDialogResultActions.saved = "saved"`

The user saved a new event or modified an existing one.

### `CalendarType`

Supported platforms: iOS.

#### `BIRTHDAYS`

`CalendarType.BIRTHDAYS = "birthdays"`

#### `CALDAV`

`CalendarType.CALDAV = "caldav"`

#### `EXCHANGE`

`CalendarType.EXCHANGE = "exchange"`

#### `LOCAL`

`CalendarType.LOCAL = "local"`

#### `SUBSCRIBED`

`CalendarType.SUBSCRIBED = "subscribed"`

#### `UNKNOWN`

`CalendarType.UNKNOWN = "unknown"`

### `DayOfTheWeek`

Supported platforms: iOS.

#### `Sunday`

`DayOfTheWeek.Sunday = 1`

#### `Monday`

`DayOfTheWeek.Monday = 2`

#### `Tuesday`

`DayOfTheWeek.Tuesday = 3`

#### `Wednesday`

`DayOfTheWeek.Wednesday = 4`

#### `Thursday`

`DayOfTheWeek.Thursday = 5`

#### `Friday`

`DayOfTheWeek.Friday = 6`

#### `Saturday`

`DayOfTheWeek.Saturday = 7`

### `EntityTypes`

Supported platforms: iOS.

#### `EVENT`

`EntityTypes.EVENT = "event"`

#### `REMINDER`

`EntityTypes.REMINDER = "reminder"`

### `EventAccessLevel`

Supported platforms: Android.

#### `CONFIDENTIAL`

`EventAccessLevel.CONFIDENTIAL = "confidential"`

#### `DEFAULT`

`EventAccessLevel.DEFAULT = "default"`

#### `PRIVATE`

`EventAccessLevel.PRIVATE = "private"`

#### `PUBLIC`

`EventAccessLevel.PUBLIC = "public"`

### `EventStatus`

Supported platforms: Android, iOS.

#### `CANCELED`

`EventStatus.CANCELED = "canceled"`

#### `CONFIRMED`

`EventStatus.CONFIRMED = "confirmed"`

#### `NONE`

`EventStatus.NONE = "none"`

#### `TENTATIVE`

`EventStatus.TENTATIVE = "tentative"`

### `Frequency`

Supported platforms: Android, iOS.

#### `DAILY`

`Frequency.DAILY = "daily"`

#### `MONTHLY`

`Frequency.MONTHLY = "monthly"`

#### `WEEKLY`

`Frequency.WEEKLY = "weekly"`

#### `YEARLY`

`Frequency.YEARLY = "yearly"`

### `MonthOfTheYear`

Supported platforms: iOS.

#### `January`

`MonthOfTheYear.January = 1`

#### `February`

`MonthOfTheYear.February = 2`

#### `March`

`MonthOfTheYear.March = 3`

#### `April`

`MonthOfTheYear.April = 4`

#### `May`

`MonthOfTheYear.May = 5`

#### `June`

`MonthOfTheYear.June = 6`

#### `July`

`MonthOfTheYear.July = 7`

#### `August`

`MonthOfTheYear.August = 8`

#### `September`

`MonthOfTheYear.September = 9`

#### `October`

`MonthOfTheYear.October = 10`

#### `November`

`MonthOfTheYear.November = 11`

#### `December`

`MonthOfTheYear.December = 12`

### `ReminderStatus`

Supported platforms: iOS.

#### `COMPLETED`

`ReminderStatus.COMPLETED = "completed"`

#### `INCOMPLETE`

`ReminderStatus.INCOMPLETE = "incomplete"`

### `SourceType`

Supported platforms: iOS.

#### `BIRTHDAYS`

`SourceType.BIRTHDAYS = "birthdays"`

#### `CALDAV`

`SourceType.CALDAV = "caldav"`

#### `EXCHANGE`

`SourceType.EXCHANGE = "exchange"`

#### `LOCAL`

`SourceType.LOCAL = "local"`

#### `MOBILEME`

`SourceType.MOBILEME = "mobileme"`

#### `SUBSCRIBED`

`SourceType.SUBSCRIBED = "subscribed"`

## Permissions

### Android

If you only intend to use the system-provided calendar UI, you don't need to request any permissions.

Otherwise, you must add the following permissions to your **app.json** inside the [`expo.android.permissions`](/versions/latest/config/app.md#permissions) array.

| Android permission | Description |
| --- | --- |
| `READ_CALENDAR` | Allows an application to read the user's calendar data. |
| `WRITE_CALENDAR` | Allows an application to write the user's calendar data. |

### iOS

To only add events without reading existing calendar data, enable the `writeOnlyAccess` option in the [config plugin](/versions/latest/sdk/calendar.md#configuration-in-app-config) and request write-only access by passing `true` to [`requestCalendarPermissions`](/versions/latest/sdk/calendar.md#calendarrequestcalendarpermissionswriteonly). This is sufficient for methods like [`ExpoCalendar.createEvent`](/versions/latest/sdk/calendar.md#createeventdetails). Methods that read calendar data, such as [`getCalendars`](/versions/latest/sdk/calendar.md#calendargetcalendarsentitytype), [`listEvents`](/versions/latest/sdk/calendar.md#calendarlisteventscalendars-startdate-enddate), or [`presentPicker`](/versions/latest/sdk/calendar.md#calendarpresentpicker), require full calendar access.

The following usage description keys are used by this library:

| Info.plist key | Description |
| --- | --- |
| `NSCalendarsUsageDescription` | A message that tells the user why the app is requesting access to the user’s calendar data. |
| `NSCalendarsFullAccessUsageDescription` | A message that tells the user why the app is requesting full access to the user's calendar data. |
| `NSCalendarsWriteOnlyAccessUsageDescription` | A message that tells the user why the app is requesting write-only access to the user's calendar data. |
| `NSRemindersUsageDescription` | A message that tells the user why the app is requesting access to the user’s reminders. |
