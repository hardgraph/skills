---
title: Calendar (legacy)
description: A library that provides an API for interacting with the device's system calendars, events, reminders, and associated records.
sourceCodeUrl: 'https://github.com/expo/expo/tree/sdk-57/packages/expo-calendar/src/legacy'
packageName: 'expo-calendar'
platforms: ['android', 'ios', 'expo-go']
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/versions/latest/sdk/calendar-legacy/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/versions/latest/sdk/calendar-legacy/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, use llms.txt to find the relevant page as Markdown (.md) instead of guessing.

You are here: Reference (v57.0.0) > Expo SDK (86 pages in this section)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Expo Calendar (legacy)

A library that provides an API for interacting with the device's system calendars, events, reminders, and associated records.
Android, iOS, Included in Expo Go

> The `legacy` version of Calendar API is included in the `expo-calendar` library. It can be used alongside the class-based `expo-calendar` API, which is exposed from root. To use the legacy API, import it from `expo-calendar/legacy`.

`expo-calendar` provides an API for interacting with the device's system calendars, events, reminders, and associated records.

Additionally, it provides methods to launch the [system-provided calendar UI](/versions/latest/sdk/calendar-legacy.md#launching-system-provided-calendar-dialogs) to allow user view or edit events. On Android, these methods start the system calendar app using an Intent. On iOS, they present either [`EKEventViewController`](https://developer.apple.com/documentation/eventkitui/ekeventviewcontroller) or [`EKEventEditViewController`](https://developer.apple.com/documentation/eventkitui/ekeventeditviewcontroller) as a modal.

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

#### Are you using this library in an existing React Native app?

If you're not using Continuous Native Generation ([CNG](/workflow/continuous-native-generation.md)) (you're using native **android** and **ios** projects manually), then you need to configure following permissions in your native projects:

-   For Android, add `android.permission.READ_CALENDAR` and `android.permission.WRITE_CALENDAR` permissions to your project's **android/app/src/main/AndroidManifest.xml**:
    
    ```xml
    <uses-permission android:name="android.permission.READ_CALENDAR" />
    <uses-permission android:name="android.permission.WRITE_CALENDAR" />
    ```
    
-   For iOS, add `NSCalendarsUsageDescription` and `NSRemindersUsageDescription` to your project's **ios/[app]/Info.plist**:
    
    ```xml
    <key>NSCalendarsUsageDescription</key>
    <string>Allow $(PRODUCT_NAME) to access your calendar</string>
    <key>NSRemindersUsageDescription</key>
    <string>Allow $(PRODUCT_NAME) to access your reminders</string>
    ```
    

## Usage

```jsx
import { useEffect } from 'react';
import { StyleSheet, View, Text, Button, Platform } from 'react-native';
import * as Calendar from 'expo-calendar/legacy';

export default function App() {
  useEffect(() => {
    (async () => {
      const { status } = await Calendar.requestCalendarPermissionsAsync();
      if (status === 'granted') {
        const calendars = await Calendar.getCalendarsAsync(Calendar.EntityTypes.EVENT);
        console.log('Here are all your calendars:');
        console.log({ calendars });
      }
    })();
  }, []);

  return (
    <View style={styles.container}>
      <Text>Calendar Module Example</Text>
      <Button title="Create a new calendar" onPress={createCalendar} />
    </View>
  );
}

async function getDefaultCalendarSource() {
  const defaultCalendar = await Calendar.getDefaultCalendarAsync();
  return defaultCalendar.source;
}

async function createCalendar() {
  const defaultCalendarSource =
    Platform.OS === 'ios'
      ? await getDefaultCalendarSource()
      : { isLocalAccount: true, name: 'Expo Calendar' };
  const newCalendarID = await Calendar.createCalendarAsync({
    title: 'Expo Calendar',
    color: 'blue',
    entityType: Calendar.EntityTypes.EVENT,
    sourceId: defaultCalendarSource.id,
    source: defaultCalendarSource,
    name: 'internalCalendarName',
    ownerAccount: 'personal',
    accessLevel: Calendar.CalendarAccessLevel.OWNER,
  });
  console.log(`Your new calendar ID is: ${newCalendarID}`);
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
import * as Calendar from 'expo-calendar/legacy';
```

## Launching system-provided calendar dialogs

### `createEventInCalendarAsync(eventData, presentationOptions)`

Supported platforms: Android, iOS.

| Parameter | Type | Description |
| --- | --- | --- |
| `eventData`(optional) | [Omit](https://www.typescriptlang.org/docs/handbook/utility-types.html#omittype-keys)<[Partial](https://www.typescriptlang.org/docs/handbook/utility-types.html#partialtype)<[Event](#event)\>, 'id'\> | A map of details for the event to be created. Default: `{}` |
| `presentationOptions`(optional) | [PresentationOptions](#presentationoptions) | Configuration that influences how the calendar UI is presented. |

  

Launches the calendar UI provided by the OS to create a new event.

Returns: `Promise<dialogeventresult>`

A promise which resolves with information about the dialog result.

### `editEventInCalendarAsync(params, presentationOptions)`

Supported platforms: Android, iOS.

| Parameter | Type |
| --- | --- |
| `params` | [CalendarDialogParams](#calendardialogparams) |
| `presentationOptions`(optional) | [PresentationOptions](#presentationoptions) |

  

Launches the calendar UI provided by the OS to edit or delete an event. On Android, this is the same as `openEventInCalendarAsync`.

Returns: `Promise<dialogeventresult>`

A promise which resolves with information about the dialog result.

### `openEventInCalendar(id)`

Supported platforms: Android.

| Parameter | Type | Description |
| --- | --- | --- |
| `id` | `string` | ID of the event to open. |

  

Sends an intent to open the specified event in the OS Calendar app.

Returns: `void`

### `openEventInCalendarAsync(params, presentationOptions)`

Supported platforms: Android, iOS.

| Parameter | Type |
| --- | --- |
| `params` | [CalendarDialogParams](#calendardialogparams) |
| `presentationOptions`(optional) | [OpenEventPresentationOptions](#openeventpresentationoptions) |

  

Launches the calendar UI provided by the OS to preview an event.

Returns: `Promise<openeventdialogresult>`

A promise which resolves with information about the dialog result.

## Hooks

### `useCalendarPermissions(options)`

Supported platforms: Android, iOS.

| Parameter | Type |
| --- | --- |
| `options`(optional) | `PermissionHookOptions<object>` |

  

Check or request permissions to access the calendar. This uses both `getCalendarPermissionsAsync` and `requestCalendarPermissionsAsync` to interact with the permissions.

Returns: `[PermissionResponse | null, RequestPermissionMethod<permissionresponse>, GetPermissionMethod]</permissionresponse>`

Example

```ts
const [status, requestPermission] = Calendar.useCalendarPermissions();
```

### `useRemindersPermissions(options)`

Supported platforms: Android, iOS.

| Parameter | Type |
| --- | --- |
| `options`(optional) | `PermissionHookOptions<object>` |

  

Check or request permissions to access reminders. This uses both `getRemindersPermissionsAsync` and `requestRemindersPermissionsAsync` to interact with the permissions.

Returns: `[PermissionResponse | null, RequestPermissionMethod<permissionresponse>, GetPermissionMethod]</permissionresponse>`

Example

```ts
const [status, requestPermission] = Calendar.useRemindersPermissions();
```

## Methods

### `Calendar (legacy).createAttendeeAsync(eventId, details)`

Supported platforms: Android.

| Parameter | Type | Description |
| --- | --- | --- |
| `eventId` | `string` | ID of the event to add this attendee to. |
| `details`(optional) | [Partial](https://www.typescriptlang.org/docs/handbook/utility-types.html#partialtype)<[Attendee](#attendee)\> | A map of details for the attendee to be created. Default: `{}` |

  

Creates a new attendee record and adds it to the specified event. Note that if `eventId` specifies a recurring event, this will add the attendee to every instance of the event.

Returns: `Promise<string>`

A string representing the ID of the newly created attendee record.

### `Calendar (legacy).createCalendarAsync(details)`

Supported platforms: Android, iOS.

| Parameter | Type | Description |
| --- | --- | --- |
| `details`(optional) | [Partial](https://www.typescriptlang.org/docs/handbook/utility-types.html#partialtype)<Calendar\> | A map of details for the calendar to be created. Default: `{}` |

  

Creates a new calendar on the device, allowing events to be added later and displayed in the OS Calendar app.

Returns: `Promise<string>`

A string representing the ID of the newly created calendar.

### `Calendar (legacy).createEventAsync(calendarId, eventData)`

Supported platforms: Android, iOS.

| Parameter | Type | Description |
| --- | --- | --- |
| `calendarId` | `string` | ID of the calendar to create this event in. |
| `eventData`(optional) | [Omit](https://www.typescriptlang.org/docs/handbook/utility-types.html#omittype-keys)<[Partial](https://www.typescriptlang.org/docs/handbook/utility-types.html#partialtype)<[Event](#event)\>, 'id' | 'organizer'\> | A map of details for the event to be created. Default: `{}` |

  

Creates a new event on the specified calendar.

Returns: `Promise<string>`

A promise which fulfils with a string representing the ID of the newly created event.

### `Calendar (legacy).createReminderAsync(calendarId, reminder)`

Supported platforms: iOS.

| Parameter | Type | Description |
| --- | --- | --- |
| `calendarId` | `string | null` | ID of the calendar to create this reminder in (or `null` to add the calendar to the OS-specified default calendar for reminders). |
| `reminder`(optional) | [Reminder](#reminder) | A map of details for the reminder to be created. Default: `{}` |

  

Creates a new reminder on the specified calendar.

Returns: `Promise<string>`

A promise which fulfils with a string representing the ID of the newly created reminder.

### `Calendar (legacy).deleteAttendeeAsync(id)`

Supported platforms: Android.

| Parameter | Type | Description |
| --- | --- | --- |
| `id` | `string` | ID of the attendee to delete. |

  

Deletes an existing attendee record from the device. **Use with caution.**

Returns: `Promise<void>`

### `Calendar (legacy).deleteCalendarAsync(id)`

Supported platforms: Android, iOS.

| Parameter | Type | Description |
| --- | --- | --- |
| `id` | `string` | ID of the calendar to delete. |

  

Deletes an existing calendar and all associated events/reminders/attendees from the device. **Use with caution.**

Returns: `Promise<void>`

### `Calendar (legacy).deleteEventAsync(id, recurringEventOptions)`

Supported platforms: Android, iOS.

| Parameter | Type | Description |
| --- | --- | --- |
| `id` | `string` | ID of the event to be deleted. |
| `recurringEventOptions`(optional) | [RecurringEventOptions](#recurringeventoptions) | A map of options for recurring events. Default: `{}` |

  

Deletes an existing event from the device. Use with caution.

Returns: `Promise<void>`

### `Calendar (legacy).deleteReminderAsync(id)`

Supported platforms: iOS.

| Parameter | Type | Description |
| --- | --- | --- |
| `id` | `string` | ID of the reminder to be deleted. |

  

Deletes an existing reminder from the device. **Use with caution.**

Returns: `Promise<void>`

### `Calendar (legacy).getAttendeesForEventAsync(id, recurringEventOptions)`

Supported platforms: Android, iOS.

| Parameter | Type | Description |
| --- | --- | --- |
| `id` | `string` | ID of the event to return attendees for. |
| `recurringEventOptions`(optional) | [RecurringEventOptions](#recurringeventoptions) | A map of options for recurring events. Default: `{}` |

  

Gets all attendees for a given event (or instance of a recurring event).

Returns: `Promise<attendee[]>`

A promise which fulfils with an array of [`Attendee`](#attendee) associated with the specified event.

### `Calendar (legacy).getCalendarPermissionsAsync()`

Supported platforms: Android, iOS.

Checks user's permissions for accessing user's calendars.

Returns: `Promise<permissionresponse>`

A promise that resolves to an object of type [`PermissionResponse`](#permissionresponse).

### `Calendar (legacy).getCalendarsAsync(entityType)`

Supported platforms: Android, iOS.

| Parameter | Type | Description |
| --- | --- | --- |
| `entityType`(optional) | `string` | **iOS Only.** Not required, but if defined, filters the returned calendars to a specific entity type. Possible values are `Calendar.EntityTypes.EVENT` (for calendars shown in the Calendar app) and `Calendar.EntityTypes.REMINDER` (for the Reminders app). Note: If not defined, you will need both permissions: CALENDAR and REMINDERS. |

  

Gets an array of calendar objects with details about the different calendars stored on the device.

Returns: `Promise<calendar[]>`

An array of [calendar objects](#calendar) matching the provided entity type (if provided).

### `Calendar (legacy).getDefaultCalendarAsync()`

Supported platforms: iOS.

Gets an instance of the default calendar object.

Returns: `Promise<calendar>`

A promise resolving to the [Calendar](#calendar) object that is the user's default calendar.

### `Calendar (legacy).getEventAsync(id, recurringEventOptions)`

Supported platforms: Android, iOS.

| Parameter | Type | Description |
| --- | --- | --- |
| `id` | `string` | ID of the event to return. |
| `recurringEventOptions`(optional) | [RecurringEventOptions](#recurringeventoptions) | A map of options for recurring events. Default: `{}` |

  

Returns a specific event selected by ID. If a specific instance of a recurring event is desired, the start date of this instance must also be provided, as instances of recurring events do not have their own unique and stable IDs on either iOS or Android.

Returns: `Promise<event>`

A promise which fulfils with an [`Event`](#event) object matching the provided criteria, if one exists.

### `Calendar (legacy).getEventsAsync(calendarIds, startDate, endDate)`

Supported platforms: Android, iOS.

| Parameter | Type | Description |
| --- | --- | --- |
| `calendarIds` | `string[]` | Array of IDs of calendars to search for events in. |
| `startDate` | [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) | Beginning of time period to search for events in. |
| `endDate` | [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) | End of time period to search for events in. |

  

Returns all events in a given set of calendars over a specified time period. The filtering has slightly different behavior per-platform - on iOS, all events that overlap at all with the `[startDate, endDate]` interval are returned, whereas on Android, only events that begin on or after the `startDate` and end on or before the `endDate` will be returned.

Returns: `Promise<event[]>`

A promise which fulfils with an array of [`Event`](#event) objects matching the search criteria.

### `Calendar (legacy).getReminderAsync(id)`

Supported platforms: iOS.

| Parameter | Type | Description |
| --- | --- | --- |
| `id` | `string` | ID of the reminder to return. |

  

Returns a specific reminder selected by ID.

Returns: `Promise<reminder>`

A promise which fulfils with a [`Reminder`](#reminder) matching the provided ID, if one exists.

### `Calendar (legacy).getRemindersAsync(calendarIds, status, startDate, endDate)`

Supported platforms: iOS.

| Parameter | Type | Description |
| --- | --- | --- |
| `calendarIds` | `(string | null)[]` | Array of IDs of calendars to search for reminders in. |
| `status` | [ReminderStatus](#reminderstatus) | null | One of `Calendar.ReminderStatus.COMPLETED` or `Calendar.ReminderStatus.INCOMPLETE`. |
| `startDate` | [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) | null | Beginning of time period to search for reminders in. Required if `status` is defined. |
| `endDate` | [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) | null | End of time period to search for reminders in. Required if `status` is defined. |

  

Returns a list of reminders matching the provided criteria. If `startDate` and `endDate` are defined, returns all reminders that overlap at all with the [startDate, endDate] interval - i.e. all reminders that end after the `startDate` or begin before the `endDate`.

Returns: `Promise<reminder[]>`

A promise which fulfils with an array of [`Reminder`](#reminder) objects matching the search criteria.

### `Calendar (legacy).getRemindersPermissionsAsync()`

Supported platforms: iOS.

Checks user's permissions for accessing user's reminders.

Returns: `Promise<permissionresponse>`

A promise that resolves to an object of type [`PermissionResponse`](#permissionresponse).

### `Calendar (legacy).getSourceAsync(id)`

Supported platforms: iOS.

| Parameter | Type | Description |
| --- | --- | --- |
| `id` | `string` | ID of the source to return. |

  

Returns a specific source selected by ID.

Returns: `Promise`

A promise which fulfils with an array of [`Source`](#source) object matching the provided ID, if one exists.

### `Calendar (legacy).getSourcesAsync()`

Supported platforms: iOS.

Returns: `Promise<source[]>`

A promise which fulfils with an array of [`Source`](#source) objects all sources for calendars stored on the device.

### `Calendar (legacy).isAvailableAsync()`

Supported platforms: Android, iOS.

Returns whether the Calendar API is enabled on the current device. This does not check the app permissions.

Returns: `Promise<boolean>`

Async `boolean`, indicating whether the Calendar API is available on the current device. Currently, this resolves `true` on iOS and Android only.

### `Calendar (legacy).requestCalendarPermissionsAsync()`

Supported platforms: Android, iOS.

Asks the user to grant permissions for accessing user's calendars.

Returns: `Promise<permissionresponse>`

A promise that resolves to an object of type [`PermissionResponse`](#permissionresponse).

### `Calendar (legacy).requestPermissionsAsync()`

Supported platforms: Android, iOS.

Returns: `Promise<permissionresponse>`

### `Calendar (legacy).requestRemindersPermissionsAsync()`

Supported platforms: iOS.

Asks the user to grant permissions for accessing user's reminders.

Returns: `Promise<permissionresponse>`

A promise that resolves to an object of type [`PermissionResponse`](#permissionresponse).

### `Calendar (legacy).updateAttendeeAsync(id, details)`

Supported platforms: Android.

| Parameter | Type | Description |
| --- | --- | --- |
| `id` | `string` | ID of the attendee record to be updated. |
| `details`(optional) | [Partial](https://www.typescriptlang.org/docs/handbook/utility-types.html#partialtype)<[Attendee](#attendee)\> | A map of properties to be updated. Default: `{}` |

  

Updates an existing attendee record. To remove a property, explicitly set it to `null` in `details`.

Returns: `Promise<string>`

### `Calendar (legacy).updateCalendarAsync(id, details)`

Supported platforms: Android, iOS.

| Parameter | Type | Description |
| --- | --- | --- |
| `id` | `string` | ID of the calendar to update. |
| `details`(optional) | [Partial](https://www.typescriptlang.org/docs/handbook/utility-types.html#partialtype)<Calendar\> | A map of properties to be updated. Default: `{}` |

  

Updates the provided details of an existing calendar stored on the device. To remove a property, explicitly set it to `null` in `details`.

Returns: `Promise<string>`

### `Calendar (legacy).updateEventAsync(id, details, recurringEventOptions)`

Supported platforms: Android, iOS.

| Parameter | Type | Description |
| --- | --- | --- |
| `id` | `string` | ID of the event to be updated. |
| `details`(optional) | [Omit](https://www.typescriptlang.org/docs/handbook/utility-types.html#omittype-keys)<[Partial](https://www.typescriptlang.org/docs/handbook/utility-types.html#partialtype)<[Event](#event)\>, 'id'\> | A map of properties to be updated. Default: `{}` |
| `recurringEventOptions`(optional) | [RecurringEventOptions](#recurringeventoptions) | A map of options for recurring events. Default: `{}` |

  

Updates the provided details of an existing calendar stored on the device. To remove a property, explicitly set it to `null` in `details`.

Returns: `Promise<string>`

### `Calendar (legacy).updateReminderAsync(id, details)`

Supported platforms: iOS.

| Parameter | Type | Description |
| --- | --- | --- |
| `id` | `string` | ID of the reminder to be updated. |
| `details`(optional) | [Reminder](#reminder) | A map of properties to be updated. Default: `{}` |

  

Updates the provided details of an existing reminder stored on the device. To remove a property, explicitly set it to `null` in `details`.

Returns: `Promise<string>`

## Types

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

### `Attendee`

Supported platforms: Android, iOS.

A person or entity that is associated with an event by being invited or fulfilling some other role.

| Property | Type | Description |
| --- | --- | --- |
| email(optional) | `string` | Supported platforms: Android. Email address of the attendee. |
| id(optional) | `string` | Supported platforms: Android. Internal ID that represents this attendee on the device. |
| isCurrentUser(optional) | `boolean` | Supported platforms: iOS. Indicates whether or not this attendee is the current OS user. |
| name | `string` | Displayed name of the attendee. |
| role | [AttendeeRole](#attendeerole) | Role of the attendee at the event. |
| status | [AttendeeStatus](#attendeestatus) | Status of the attendee in relation to the event. |
| type | [AttendeeType](#attendeetype) | Type of the attendee. |
| url(optional) | `string` | Supported platforms: iOS. URL for the attendee. |

### `Calendar`

Supported platforms: Android, iOS.

A calendar record upon which events (or, on iOS, reminders) can be stored. Settings here apply to the calendar as a whole and how its events are displayed in the OS calendar app.

| Property | Type | Description |
| --- | --- | --- |
| accessLevel(optional) | [CalendarAccessLevel](#calendaraccesslevel) | Supported platforms: Android. Level of access that the user has for the calendar. |
| allowedAttendeeTypes(optional) | [AttendeeType[]](#attendeetype) | Supported platforms: Android. Attendee types that this calendar supports. |
| allowedAvailabilities | [Availability[]](#availability) | Availability types that this calendar supports. |
| allowedReminders(optional) | [AlarmMethod[]](#alarmmethod) | Supported platforms: Android. Alarm methods that this calendar supports. |
| allowsModifications | `boolean` | Boolean value that determines whether this calendar can be modified. |
| color | `string` | Color used to display this calendar's events. |
| entityType(optional) | [EntityTypes](#entitytypes) | Supported platforms: iOS. Whether the calendar is used in the Calendar or Reminders OS app. |
| id | `string` | Internal ID that represents this calendar on the device. |
| isPrimary(optional) | `boolean` | Supported platforms: Android. Boolean value indicating whether this is the device's primary calendar. |
| isSynced(optional) | `boolean` | Supported platforms: Android. Indicates whether this calendar is synced and its events stored on the device. Unexpected behavior may occur if this is not set to `true`. |
| isVisible(optional) | `boolean` | Supported platforms: Android. Indicates whether the OS displays events on this calendar. |
| name(optional) | `string | null` | Supported platforms: Android. Internal system name of the calendar. |
| ownerAccount(optional) | `string` | Supported platforms: Android. Name for the account that owns this calendar. |
| source | [Source](#source) | Object representing the source to be used for the calendar. |
| sourceId(optional) | `string` | Supported platforms: iOS. ID of the source to be used for the calendar. Likely the same as the source for any other locally stored calendars. |
| timeZone(optional) | `string` | Supported platforms: Android. Time zone for the calendar. |
| title | `string` | Visible name of the calendar. |
| type(optional) | [CalendarType](#calendartype) | Supported platforms: iOS. Type of calendar this object represents. |

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

### `Event`

Supported platforms: Android, iOS.

An event record, or a single instance of a recurring event. On iOS, used in the Calendar app.

| Property | Type | Description |
| --- | --- | --- |
| accessLevel(optional) | [EventAccessLevel](#eventaccesslevel) | Supported platforms: Android. User's access level for the event. |
| alarms | [Alarm[]](#alarm) | Array of Alarm objects which control automated reminders to the user. |
| allDay | `boolean` | Whether the event is displayed as an all-day event on the calendar |
| availability | [Availability](#availability) | The availability setting for the event. |
| calendarId | `string` | ID of the calendar that contains this event. |
| creationDate(optional) | string | [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) | Supported platforms: iOS. Date when the event record was created. |
| endDate | string | [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) | Date object or string representing the time when the event ends. |
| endTimeZone(optional) | `string` | Supported platforms: Android. Time zone for the event end time. |
| guestsCanInviteOthers(optional) | `boolean` | Supported platforms: Android. Whether invited guests can invite other guests. |
| guestsCanModify(optional) | `boolean` | Supported platforms: Android. Whether invited guests can modify the details of the event. |
| guestsCanSeeGuests(optional) | `boolean` | Supported platforms: Android. Whether invited guests can see other guests. |
| id | `string` | Internal ID that represents this event on the device. |
| instanceId(optional) | `string` | Supported platforms: Android. For instances of recurring events, volatile ID representing this instance. Not guaranteed to always refer to the same instance. |
| isDetached(optional) | `boolean` | Supported platforms: iOS. Boolean value indicating whether or not the event is a detached (modified) instance of a recurring event. |
| lastModifiedDate(optional) | string | [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) | Supported platforms: iOS. Date when the event record was last modified. |
| location | `string | null` | Location field of the event. |
| notes | `string` | Description or notes saved with the event. |
| organizer(optional) | [Organizer](#organizer) | Supported platforms: iOS. Organizer of the event. This property is only available on events associated with calendars that are managed by a service ie. Google Calendar or iCloud. The organizer is read-only and cannot be set. |
| organizerEmail(optional) | `string` | Supported platforms: Android. Email address of the organizer of the event. |
| originalId(optional) | `string` | Supported platforms: Android. For detached (modified) instances of recurring events, the ID of the original recurring event. |
| originalStartDate(optional) | string | [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) | Supported platforms: iOS. For recurring events, the start date for the first (original) instance of the event. |
| recurrenceRule | [RecurrenceRule](#recurrencerule) | null | Object representing rules for recurring or repeating events. Set to `null` for one-time events. |
| startDate | string | [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) | Date object or string representing the time when the event starts. |
| status | [EventStatus](#eventstatus) | Status of the event. |
| timeZone | `string` | Time zone the event is scheduled in. |
| title | `string` | Visible name of the event. |
| url(optional) | `string` | Supported platforms: iOS. URL for the event. |

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

### `Organizer`

Supported platforms: Android, iOS.

| Property | Type | Description |
| --- | --- | --- |
| isCurrentUser | `boolean` | - |
| name(optional) | `string` | - |
| role | `string` | - |
| status | `string` | - |
| type | `string` | - |
| url(optional) | `string` | - |

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

### `Reminder`

Supported platforms: iOS.

A reminder record, used in the iOS Reminders app. No direct analog on Android.

| Property | Type | Description |
| --- | --- | --- |
| alarms(optional) | [Alarm[]](#alarm) | Array of Alarm objects which control automated alarms to the user about the task. |
| allDay(optional) | `boolean` | Supported platforms: iOS. Indicates if the reminder has a time specified for the due date. |
| calendarId(optional) | `string` | ID of the calendar that contains this reminder. |
| completed(optional) | `boolean` | Indicates whether or not the task has been completed. |
| completionDate(optional) | string | [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) | Date object or string representing the date of completion, if `completed` is `true`. Setting this property of a nonnull `Date` will automatically set the reminder's `completed` value to `true`. |
| creationDate(optional) | string | [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) | Date when the reminder record was created. |
| dueDate(optional) | string | [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) | Date object or string representing the time when the reminder task is due. |
| id(optional) | `string` | Internal ID that represents this reminder on the device. |
| lastModifiedDate(optional) | string | [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) | Date when the reminder record was last modified. |
| location(optional) | `string` | Location field of the reminder |
| notes(optional) | `string` | Description or notes saved with the reminder. |
| recurrenceRule(optional) | [RecurrenceRule](#recurrencerule) | null | Object representing rules for recurring or repeated reminders. `null` for one-time tasks. |
| startDate(optional) | string | [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) | Date object or string representing the start date of the reminder task. |
| timeZone(optional) | `string` | Time zone the reminder is scheduled in. |
| title(optional) | `string` | Visible name of the reminder. |
| url(optional) | `string` | URL for the reminder. |

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

### `PermissionStatus`

Supported platforms: Android, iOS.

#### `DENIED`

`PermissionStatus.DENIED = "denied"`

User has denied the permission.

#### `GRANTED`

`PermissionStatus.GRANTED = "granted"`

User has granted the permission.

#### `UNDETERMINED`

`PermissionStatus.UNDETERMINED = "undetermined"`

User hasn't granted or denied the permission yet.

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

If you only intend to use the [system-provided calendar UI](/versions/latest/sdk/calendar-legacy.md#launching-system-provided-calendar-dialogs), you don't need to request any permissions.

Otherwise, you must add the following permissions to your **app.json** inside the [`expo.android.permissions`](/versions/latest/config/app.md#permissions) array.

| Android permission | Description |
| --- | --- |
| `READ_CALENDAR` | Allows an application to read the user's calendar data. |
| `WRITE_CALENDAR` | Allows an application to write the user's calendar data. |

### iOS

If you only intend to create events using system-provided calendar UI with [`createEventInCalendarAsync`](/versions/latest/sdk/calendar-legacy.md#createeventincalendarasynceventdata-presentationoptions), you don't need to request permissions.

The following usage description keys are used by this library:

| Info.plist key | Description |
| --- | --- |
| `NSCalendarsUsageDescription` | A message that tells the user why the app is requesting access to the user’s calendar data. |
| `NSRemindersUsageDescription` | A message that tells the user why the app is requesting access to the user’s reminders. |
