---
title: Contacts (legacy)
description: A library that provides access to the phone's system contacts.
sourceCodeUrl: 'https://github.com/expo/expo/tree/sdk-57/packages/expo-contacts/src/legacy'
packageName: 'expo-contacts'
iconUrl: '/static/images/packages/expo-contacts.png'
platforms: ['android', 'ios', 'expo-go']
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/versions/latest/sdk/contacts-legacy/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/versions/latest/sdk/contacts-legacy/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, use llms.txt to find the relevant page as Markdown (.md) instead of guessing.

You are here: Reference (v57.0.0) > Expo SDK (86 pages in this section)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Expo Contacts (legacy)

A library that provides access to the phone's system contacts.
Android, iOS, Included in Expo Go

> The `legacy` version of Contacts API included on `expo-contacts` library. It can be used alongside class-based `expo-contacts` API, which is exposed from root. To use the legacy API, import it from `expo-contacts/legacy`.

`expo-contacts` provides access to the device's system contacts, allowing you to get contact information as well as adding, editing, or removing contacts.

On iOS, contacts have a multi-layered grouping system that you can also access through this API.

## Installation

```sh
# npm
npx expo install expo-contacts

# yarn
yarn expo install expo-contacts

# pnpm
pnpm expo install expo-contacts

# bun
bun expo install expo-contacts
```

If you are installing this in an [existing React Native app](/bare/overview.md), make sure to [install `expo`](/bare/installing-expo-modules.md) in your project.

## Configuration in app config

You can configure `expo-contacts` using its built-in [config plugin](/config-plugins/introduction.md) if you use config plugins in your project ([Continuous Native Generation (CNG)](/workflow/continuous-native-generation.md)). The plugin allows you to configure various properties that cannot be set at runtime and require building a new app binary to take effect. If your app does **not** use CNG, then you'll need to manually configure the library.

### Example app.json with config plugin

```json
{
  "expo": {
    "plugins": [
      [
        "expo-contacts",
        {
          "contactsPermission": "Allow $(PRODUCT_NAME) to access your contacts."
        }
      ]
    ]
  }
}
```

### Configurable properties

| Name | Default | Description |
| --- | --- | --- |
| `contactsPermission` | `"Allow $(PRODUCT_NAME) to access your contacts"` | Only for: iOS. A string to set the [`NSContactsUsageDescription`](#ios) permission message. |

#### Are you using this library in an existing React Native app?

If you're not using Continuous Native Generation ([CNG](/workflow/continuous-native-generation.md)) (you're using native **android** and **ios** projects manually), then you need to configure following permissions in your native projects:

-   For Android, add `android.permission.READ_CONTACTS` and `android.permission.WRITE_CONTACTS` permissions to your project's **android/app/src/main/AndroidManifest.xml**:
    
    ```xml
    <uses-permission android:name="android.permission.READ_CONTACTS" />
    <uses-permission android:name="android.permission.WRITE_CONTACTS" />
    ```
    
-   For iOS, add the `NSContactsUsageDescription` key to your project's **ios/[app]/Info.plist**:
    
    ```xml
    <key>NSContactsUsageDescription</key>
    <string>Allow $(PRODUCT_NAME) to access your contacts</string>
    ```
    

## Usage

```jsx
import { useEffect } from 'react';
import { StyleSheet, View, Text } from 'react-native';
import * as Contacts from 'expo-contacts/legacy';

export default function App() {
  useEffect(() => {
    (async () => {
      const { status } = await Contacts.requestPermissionsAsync();
      if (status === 'granted') {
        const { data } = await Contacts.getContactsAsync({
          fields: [Contacts.Fields.Emails],
        });

        if (data.length > 0) {
          const contact = data[0];
          console.log(contact);
        }
      }
    })();
  }, []);

  return (
    <View style={styles.container}>
      <Text>Contacts Module Example</Text>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#fff',
    alignItems: 'center',
    justifyContent: 'center',
  },
});
```

## API

```js
import * as Contacts from 'expo-contacts/legacy';
```

## Component

### `ContactAccessButton`

Supported platforms: iOS 18.0+.

Type: React.[PureComponent](https://react.dev/reference/react/PureComponent)<[ContactAccessButtonProps](#contactaccessbuttonprops)\>

Creates a contact access button to quickly add contacts under limited-access authorization.

For more details, you can read the Apple docs about the underlying [`ContactAccessButton`](https://developer.apple.com/documentation/contactsui/contactaccessbutton) SwiftUI view.

ContactAccessButtonProps

### `backgroundColor`

Supported platforms: iOS 18.0+.

Optional • Type: [ColorValue](https://reactnative.dev/docs/colors)

A color of the button's background. Provided color should not be transparent, otherwise it may not satisfy platform requirements for button legibility.

### `caption`

Supported platforms: iOS 18.0+.

Optional • Literal type: `string`

When the query produces a single result, the contact access button shows the caption under the matching contact name. It can be nothing (default), email address or phone number.

Acceptable values are: `'default'` | `'email'` | `'phone'`

### `ignoredEmails`

Supported platforms: iOS 18.0+.

Optional • Type: `string[]`

An array of email addresses. The search omits contacts matching query that also match any email address in this array.

### `ignoredPhoneNumbers`

Supported platforms: iOS 18.0+.

Optional • Type: `string[]`

An array of phone numbers. The search omits contacts matching query that also match any phone number in this set.

### `query`

Supported platforms: iOS 18.0+.

Optional • Type: `string`

A string to match against contacts not yet exposed to the app. You typically get this value from a search UI that your app presents, like a text field.

### `textColor`

Supported platforms: iOS 18.0+.

Optional • Type: [ColorValue](https://reactnative.dev/docs/colors)

A color of the button's title. Slightly dimmed version of this color is used for the caption text. Make sure there is a good contrast between the text and the background, otherwise platform requirements for button legibility may not be satisfied.

### `tintColor`

Supported platforms: iOS 18.0+.

Optional • Type: [ColorValue](https://reactnative.dev/docs/colors)

A tint color of the button and the modal that is presented when there is more than one match.

#### Inherited props

-   [ViewProps](https://reactnative.dev/docs/view#props)

## Static methods

### `isAvailable()`

Supported platforms: Android, iOS.

Returns a boolean whether the `ContactAccessButton` is available on the platform. This is `true` only on iOS 18.0 and newer.

Returns: `boolean`

## Constants

### `Contacts (legacy).onContactsChangeEventName`

Supported platforms: Android, iOS.

Type: `'onContactsChange'`

## Methods

### `Contacts (legacy).addContactAsync(contact, containerId)`

Supported platforms: Android, iOS.

| Parameter | Type |
| --- | --- |
| `contact` | [Contact](#contact) |
| `containerId`(optional) | `string` |

  

Returns: `Promise<string>`

### `Contacts (legacy).addExistingContactToGroupAsync(contactId, groupId)`

Supported platforms: Android, iOS.

| Parameter | Type |
| --- | --- |
| `contactId` | `string` |
| `groupId` | `string` |

  

Returns: `Promise<any>`

### `Contacts (legacy).addExistingGroupToContainerAsync(groupId, containerId)`

Supported platforms: Android, iOS.

| Parameter | Type |
| --- | --- |
| `groupId` | `string` |
| `containerId` | `string` |

  

Returns: `Promise<any>`

### `Contacts (legacy).createGroupAsync(name, containerId)`

Supported platforms: Android, iOS.

| Parameter | Type |
| --- | --- |
| `name`(optional) | `string` |
| `containerId`(optional) | `string` |

  

Returns: `Promise<string>`

### `Contacts (legacy).getContactByIdAsync(id, fields)`

Supported platforms: Android, iOS.

| Parameter | Type |
| --- | --- |
| `id` | `string` |
| `fields`(optional) | [FieldType[]](#fieldtype) |

  

Returns: `Promise<existingcontact>`

### `Contacts (legacy).getContactsAsync(contactQuery)`

Supported platforms: Android, iOS.

| Parameter | Type |
| --- | --- |
| `contactQuery`(optional) | [ContactQuery](#contactquery) |

  

Returns: `Promise<contactresponse>`

### `Contacts (legacy).getContainersAsync(containerQuery)`

Supported platforms: Android, iOS.

| Parameter | Type |
| --- | --- |
| `containerQuery` | [ContainerQuery](#containerquery) |

  

Returns: `Promise<container[]>`

### `Contacts (legacy).getDefaultContainerIdAsync()`

Supported platforms: Android, iOS.

Returns: `Promise<string>`

### `Contacts (legacy).getGroupsAsync(groupQuery)`

Supported platforms: Android, iOS.

| Parameter | Type |
| --- | --- |
| `groupQuery` | [GroupQuery](#groupquery) |

  

Returns: `Promise<group[]>`

### `Contacts (legacy).getPagedContactsAsync(contactQuery)`

Supported platforms: Android, iOS.

| Parameter | Type |
| --- | --- |
| `contactQuery`(optional) | [ContactQuery](#contactquery) |

  

Returns: `Promise<contactresponse>`

### `Contacts (legacy).getPermissionsAsync()`

Supported platforms: Android, iOS.

Returns: `Promise<contactspermissionresponse>`

### `Contacts (legacy).hasContactsAsync()`

Supported platforms: Android, iOS.

Returns: `Promise<boolean>`

### `Contacts (legacy).isAvailableAsync()`

Supported platforms: Android, iOS.

Returns: `Promise<boolean>`

### `Contacts (legacy).presentAccessPickerAsync()`

Supported platforms: Android, iOS.

Returns: `Promise<string[]>`

### `Contacts (legacy).presentContactPickerAsync()`

Supported platforms: Android, iOS.

Returns: `Promise<existingcontact>`

### `Contacts (legacy).presentFormAsync(contactId, contact, formOptions)`

Supported platforms: Android, iOS.

| Parameter | Type |
| --- | --- |
| `contactId`(optional) | `string | null` |
| `contact`(optional) | [Contact](#contact) | null |
| `formOptions`(optional) | [FormOptions](#formoptions) |

  

Returns: `Promise<any>`

### `Contacts (legacy).removeContactAsync(contactId)`

Supported platforms: Android, iOS.

| Parameter | Type |
| --- | --- |
| `contactId` | `string` |

  

Returns: `Promise<any>`

### `Contacts (legacy).removeContactFromGroupAsync(contactId, groupId)`

Supported platforms: Android, iOS.

| Parameter | Type |
| --- | --- |
| `contactId` | `string` |
| `groupId` | `string` |

  

Returns: `Promise<any>`

### `Contacts (legacy).removeGroupAsync(groupId)`

Supported platforms: Android, iOS.

| Parameter | Type |
| --- | --- |
| `groupId` | `string` |

  

Returns: `Promise<any>`

### `Contacts (legacy).requestPermissionsAsync()`

Supported platforms: Android, iOS.

Returns: `Promise<contactspermissionresponse>`

### `Contacts (legacy).shareContactAsync(contactId, message, shareOptions)`

Supported platforms: Android, iOS.

| Parameter | Type |
| --- | --- |
| `contactId` | `string` |
| `message` | `string` |
| `shareOptions`(optional) | [ShareOptions](https://reactnative.dev/docs/share#share) |

  

Returns: `Promise<any>`

### `Contacts (legacy).updateContactAsync(contact)`

Supported platforms: Android, iOS.

| Parameter | Type |
| --- | --- |
| `contact` | { id: string } & [Partial](https://www.typescriptlang.org/docs/handbook/utility-types.html#partialtype)<[ExistingContact](#existingcontact)\> |

  

Returns: `Promise<string>`

### `Contacts (legacy).updateGroupNameAsync(groupName, groupId)`

Supported platforms: Android, iOS.

| Parameter | Type |
| --- | --- |
| `groupName` | `string` |
| `groupId` | `string` |

  

Returns: `Promise<any>`

### `Contacts (legacy).writeContactToFileAsync(contactQuery)`

Supported platforms: Android, iOS.

| Parameter | Type |
| --- | --- |
| `contactQuery`(optional) | [ContactQuery](#contactquery) |

  

Returns: `Promise<string>`

## Event subscriptions

### `Contacts (legacy).addContactsChangeListener(listener)`

Supported platforms: Android, iOS.

| Parameter | Type |
| --- | --- |
| `listener` | `() => void` |

  

Returns: `EventSubscription`

## Types

### `Address`

Supported platforms: Android, iOS.

| Property | Type | Description |
| --- | --- | --- |
| city(optional) | `string` | City name. |
| country(optional) | `string` | Country name |
| id(optional) | `string` | Unique ID. This value will be generated by the OS. |
| isoCountryCode(optional) | `string` | [Standard country code](https://www.iso.org/iso-3166-country-codes.html). |
| label | `string` | Localized display name. |
| neighborhood(optional) | `string` | Neighborhood name. |
| poBox(optional) | `string` | P.O. Box. |
| postalCode(optional) | `string` | Local post code. |
| region(optional) | `string` | Region or state name. |
| street(optional) | `string` | Street name. |

### `CalendarFormatType`

Supported platforms: Android, iOS.

Literal type: `union`

Acceptable values are: [CalendarFormats](#calendarformats) | `{CalendarFormats}`

### `Contact`

Supported platforms: Android, iOS.

| Property | Type | Description |
| --- | --- | --- |
| addresses(optional) | [Address[]](#address) | Locations. |
| birthday(optional) | [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) | Birthday information in Gregorian format. |
| company(optional) | `string` | Organization the entity belongs to. |
| contactType | [ContactType](#contacttype) | Denoting a person or company. |
| dates(optional) | [Date[]](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) | A labeled list of other relevant user dates in Gregorian format. |
| department(optional) | `string` | Job department. |
| emails(optional) | [Email[]](#email) | Email addresses. |
| firstName(optional) | `string` | Given name. |
| image(optional) | [Image](#image) | Thumbnail image. On iOS it size is set to 320×320px, on Android it may vary. |
| imageAvailable(optional) | `boolean` | Used for efficient retrieval of images. |
| instantMessageAddresses(optional) | [InstantMessageAddress[]](#instantmessageaddress) | Instant messaging connections. |
| isFavorite(optional) | `boolean` | Supported platforms: Android. Whether the contact is starred. |
| jobTitle(optional) | `string` | Job description. |
| lastName(optional) | `string` | Last name. |
| maidenName(optional) | `string` | Maiden name. |
| middleName(optional) | `string` | Middle name |
| name | `string` | Full name with proper format. |
| namePrefix(optional) | `string` | Dr., Mr., Mrs., and so on. |
| nameSuffix(optional) | `string` | Jr., Sr., and so on. |
| nickname(optional) | `string` | An alias to the proper name. |
| nonGregorianBirthday(optional) | [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date) | Supported platforms: iOS. Birthday that doesn't conform to the Gregorian calendar format, interpreted based on the [calendar `format`](#date) setting. |
| note(optional) | `string` | Additional information. The note field requires your app to request additional entitlements. The Expo Go app does not contain those entitlements, so in order to test this feature you will need to request the entitlement from Apple, set the ios.accessesContactNotes field in app config to true, and create your development build. |
| phoneNumbers(optional) | [PhoneNumber[]](#phonenumber) | Phone numbers. |
| phoneticFirstName(optional) | `string` | Pronunciation of the first name. |
| phoneticLastName(optional) | `string` | Pronunciation of the last name. |
| phoneticMiddleName(optional) | `string` | Pronunciation of the middle name. |
| rawImage(optional) | [Image](#image) | Raw image without cropping, usually large. |
| relationships(optional) | [Relationship[]](#relationship) | Names of other relevant user connections. |
| socialProfiles(optional) | [SocialProfile[]](#socialprofile) | Supported platforms: iOS. Social networks. |
| urlAddresses(optional) | [UrlAddress[]](#urladdress) | Associated web URLs. |

### `ContactQuery`

Supported platforms: Android, iOS.

| Property | Type | Description |
| --- | --- | --- |
| containerId(optional) | `string` | Supported platforms: iOS. Get all contacts that belong to the container matching this ID. |
| fields(optional) | [FieldType[]](#fieldtype) | If specified, the defined fields will be returned. If skipped, all fields will be returned. |
| groupId(optional) | `string` | Supported platforms: iOS. Get all contacts that belong to the group matching this ID. |
| id(optional) | `string | string[]` | Get contacts with a matching ID or array of IDs. |
| name(optional) | `string` | Get all contacts whose name contains the provided string (not case-sensitive). |
| pageOffset(optional) | `number` | The number of contacts to skip before gathering contacts. |
| pageSize(optional) | `number` | The max number of contacts to return. If skipped or set to `0` all contacts will be returned. |
| rawContacts(optional) | `boolean` | Supported platforms: iOS. Prevent unification of contacts when gathering. Default: `false` |
| sort(optional) | [ContactSort](#contactsort) | Sort method used when gathering contacts. |

### `ContactResponse`

Supported platforms: Android, iOS.

| Property | Type | Description |
| --- | --- | --- |
| data | [ExistingContact[]](#existingcontact) | An array of contacts that match a particular query. |
| hasNextPage | `boolean` | This will be `true` if there are more contacts to retrieve beyond what is returned. |
| hasPreviousPage | `boolean` | This will be `true` if there are previous contacts that weren't retrieved due to `pageOffset` limit. |

### `ContactSort`

Supported platforms: Android, iOS.

String union of [SortTypes](#sorttypes) values.

### `ContactsPermissionResponse`

Supported platforms: Android, iOS.

Type: `PermissionResponse` extended by:

| Property | Type | Description |
| --- | --- | --- |
| accessPrivileges(optional) | `'all' | 'limited' | 'none'` | Indicates if your app has access to the whole or only part of the contact library. Possible values are:
-   `'all'` if the user granted your app access to the whole contact library
-   `'limited'` if the user granted your app access only to selected contacts (only available on iOS 18+)
-   `'none'`

 |

### `ContactType`

Supported platforms: Android, iOS.

Literal type: `union`

Acceptable values are: [ContactTypes](#contacttypes) | `{ContactTypes}`

### `Container`

Supported platforms: Android, iOS.

| Property | Type | Description |
| --- | --- | --- |
| id | `string` | - |
| name | `string` | - |
| type | `ContainerType` | - |

### `ContainerQuery`

Supported platforms: Android, iOS.

| Property | Type | Description |
| --- | --- | --- |
| contactId(optional) | `string` | Query all the containers that parent a contact. |
| containerId(optional) | `string | string[]` | Query all the containers that matches ID or an array od IDs. |
| groupId(optional) | `string` | Query all the containers that parent a group. |

### `ContainerType`

Supported platforms: Android, iOS.

Literal type: `union`

Acceptable values are: [ContainerTypes](#containertypes) | `{ContainerTypes}`

### `Date`

Supported platforms: Android, iOS.

| Property | Type | Description |
| --- | --- | --- |
| day | `number` | Day. |
| format(optional) | [CalendarFormatType](#calendarformattype) | Format for the date. This is provided by the OS, do not set this manually. |
| id(optional) | `string` | Unique ID. This value will be generated by the OS. |
| label(optional) | `string` | Localized display name. |
| month | `number` | Month - adjusted for JavaScript `Date` which starts at `0`. |
| year(optional) | `number` | Year. |

### `Email`

Supported platforms: Android, iOS.

| Property | Type | Description |
| --- | --- | --- |
| email(optional) | `string` | Email address. |
| id(optional) | `string` | Unique ID. This value will be generated by the OS. |
| isPrimary(optional) | `boolean` | Flag signifying if it is a primary email address. |
| label | `string` | Localized display name. |

### `ExistingContact`

Supported platforms: Android, iOS.

Type: [Contact](#contact) extended by:

| Property | Type | Description |
| --- | --- | --- |
| id | `string` | Immutable identifier used for querying and indexing. This value will be generated by the OS when the contact is created. |

### `FieldType`

Supported platforms: Android, iOS.

Literal type: `union`

Acceptable values are: [Fields](#fields) | `{Fields}`

### `FormOptions`

Supported platforms: Android, iOS.

| Property | Type | Description |
| --- | --- | --- |
| allowsActions(optional) | `boolean` | Actions like share, add, create. |
| allowsEditing(optional) | `boolean` | Allows for contact mutation. |
| alternateName(optional) | `string` | Used if contact doesn't have a name defined. |
| cancelButtonTitle(optional) | `string` | The name of the left bar button. Only applies when editing an existing contact. |
| displayedPropertyKeys(optional) | [FieldType[]](#fieldtype) | The properties that will be displayed when viewing a contact. |
| groupId(optional) | `string` | The parent group for a new contact. |
| isNew(optional) | `boolean` | Present the new contact controller. If set to `false` the unknown controller will be shown. |
| message(optional) | `string` | The message displayed under the name of the contact. Only applies when editing an existing contact. |
| preventAnimation(optional) | `boolean` | Prevents the controller from animating in. |
| shouldShowLinkedContacts(optional) | `boolean` | Show or hide the similar contacts. |

### `Group`

Supported platforms: Android, iOS.

| Property | Type | Description |
| --- | --- | --- |
| id(optional) | `string` | The editable name of a group. |
| name(optional) | `string` | Immutable id representing the group. |

### `GroupQuery`

Supported platforms: Android, iOS.

| Property | Type | Description |
| --- | --- | --- |
| containerId(optional) | `string` | Query all groups that belong to a certain container. |
| groupId(optional) | `string` | Query the group with a matching ID. |
| groupName(optional) | `string` | Query all groups matching a name. |

### `Image`

Supported platforms: Android, iOS.

| Property | Type | Description |
| --- | --- | --- |
| base64(optional) | `string` | Image as Base64 string. |
| height(optional) | `number` | Supported platforms: iOS. Image height |
| uri(optional) | `string` | A local image URI. Note: If you have a remote URI, download it first using FileSystem.downloadAsync. |
| width(optional) | `number` | Supported platforms: iOS. Image width. |

### `InstantMessageAddress`

Supported platforms: Android, iOS.

| Property | Type | Description |
| --- | --- | --- |
| id(optional) | `string` | Unique ID. This value will be generated by the OS. |
| label | `string` | Localized display name. |
| localizedService(optional) | `string` | Localized name of app. |
| service(optional) | `string` | Name of instant messaging app. |
| username(optional) | `string` | Username in IM app. |

### `PermissionExpiration`

Supported platforms: Android, iOS.

Literal type: `union`

Permission expiration time. Currently, all permissions are granted permanently.

Acceptable values are: `'never'` | `number`

### `PermissionResponse`

Supported platforms: Android, iOS.

An object obtained by permissions get and request functions.

| Property | Type | Description |
| --- | --- | --- |
| canAskAgain | `boolean` | Indicates if user can be asked again for specific permission. If not, one should be directed to the Settings app in order to enable/disable the permission. |
| expires | `PermissionExpiration` | Determines time when the permission expires. |
| granted | `boolean` | A convenience boolean that indicates if the permission is granted. |
| status | [PermissionStatus](/versions/latest/sdk/expo.md#permissionstatus) | Determines the status of the permission. |

### `PhoneNumber`

Supported platforms: Android, iOS.

| Property | Type | Description |
| --- | --- | --- |
| countryCode(optional) | `string` | Country code. . Example. `us` |
| digits(optional) | `string` | Phone number without format. . Example. `8674305` |
| id(optional) | `string` | Unique ID. This value will be generated by the OS. |
| isPrimary(optional) | `boolean` | Flag signifying if it is a primary phone number. |
| label | `string` | Localized display name. |
| number(optional) | `string` | Phone number. |

### `Relationship`

Supported platforms: Android, iOS.

| Property | Type | Description |
| --- | --- | --- |
| id(optional) | `string` | Unique ID. This value will be generated by the OS. |
| label | `string` | Localized display name. |
| name(optional) | `string` | Name of related contact. |

### `SocialProfile`

Supported platforms: Android, iOS.

| Property | Type | Description |
| --- | --- | --- |
| id(optional) | `string` | Unique ID. This value will be generated by the OS. |
| label | `string` | Localized display name. |
| localizedProfile(optional) | `string` | Localized profile name. |
| service(optional) | `string` | Name of social app. |
| url(optional) | `string` | Web URL. |
| userId(optional) | `string` | Username ID in social app. |
| username(optional) | `string` | Username in social app. |

### `UrlAddress`

Supported platforms: Android, iOS.

| Property | Type | Description |
| --- | --- | --- |
| id(optional) | `string` | Unique ID. This value will be generated by the OS. |
| label | `string` | Localized display name. |
| url(optional) | `string` | Web URL. |

## Enums

### `CalendarFormats`

Supported platforms: Android, iOS.

#### `Buddhist`

Supported platforms: iOS.

`CalendarFormats.Buddhist = "buddhist"`

#### `Chinese`

Supported platforms: iOS.

`CalendarFormats.Chinese = "chinese"`

#### `Coptic`

Supported platforms: iOS.

`CalendarFormats.Coptic = "coptic"`

#### `EthiopicAmeteAlem`

Supported platforms: iOS.

`CalendarFormats.EthiopicAmeteAlem = "ethiopicAmeteAlem"`

#### `EthiopicAmeteMihret`

Supported platforms: iOS.

`CalendarFormats.EthiopicAmeteMihret = "ethiopicAmeteMihret"`

#### `Gregorian`

`CalendarFormats.Gregorian = "gregorian"`

#### `Hebrew`

Supported platforms: iOS.

`CalendarFormats.Hebrew = "hebrew"`

#### `Indian`

Supported platforms: iOS.

`CalendarFormats.Indian = "indian"`

#### `Islamic`

Supported platforms: iOS.

`CalendarFormats.Islamic = "islamic"`

#### `IslamicCivil`

Supported platforms: iOS.

`CalendarFormats.IslamicCivil = "islamicCivil"`

#### `IslamicTabular`

Supported platforms: iOS.

`CalendarFormats.IslamicTabular = "islamicTabular"`

#### `IslamicUmmAlQura`

Supported platforms: iOS.

`CalendarFormats.IslamicUmmAlQura = "islamicUmmAlQura"`

#### `ISO8601`

Supported platforms: iOS.

`CalendarFormats.ISO8601 = "iso8601"`

#### `Japanese`

Supported platforms: iOS.

`CalendarFormats.Japanese = "japanese"`

#### `Persian`

Supported platforms: iOS.

`CalendarFormats.Persian = "persian"`

#### `RepublicOfChina`

Supported platforms: iOS.

`CalendarFormats.RepublicOfChina = "republicOfChina"`

### `ContactTypes`

Supported platforms: Android, iOS.

#### `Company`

`ContactTypes.Company = "company"`

Contact is group or company.

#### `Person`

`ContactTypes.Person = "person"`

Contact is a human.

### `ContainerTypes`

Supported platforms: Android, iOS.

#### `CardDAV`

`ContainerTypes.CardDAV = "cardDAV"`

With cardDAV protocol used for sharing.

#### `Exchange`

`ContainerTypes.Exchange = "exchange"`

In association with email server.

#### `Local`

`ContainerTypes.Local = "local"`

A local non-iCloud container.

#### `Unassigned`

`ContainerTypes.Unassigned = "unassigned"`

Unknown container.

### `Fields`

Supported platforms: Android, iOS.

#### `Addresses`

`Fields.Addresses = "addresses"`

#### `Birthday`

`Fields.Birthday = "birthday"`

#### `Company`

`Fields.Company = "company"`

#### `ContactType`

`Fields.ContactType = "contactType"`

#### `Dates`

`Fields.Dates = "dates"`

#### `Department`

`Fields.Department = "department"`

#### `Emails`

`Fields.Emails = "emails"`

#### `ExtraNames`

`Fields.ExtraNames = "extraNames"`

#### `FirstName`

`Fields.FirstName = "firstName"`

#### `ID`

`Fields.ID = "id"`

#### `Image`

`Fields.Image = "image"`

#### `ImageAvailable`

`Fields.ImageAvailable = "imageAvailable"`

#### `InstantMessageAddresses`

`Fields.InstantMessageAddresses = "instantMessageAddresses"`

#### `IsFavorite`

Supported platforms: Android.

`Fields.IsFavorite = "isFavorite"`

#### `JobTitle`

`Fields.JobTitle = "jobTitle"`

#### `LastName`

`Fields.LastName = "lastName"`

#### `MaidenName`

`Fields.MaidenName = "maidenName"`

#### `MiddleName`

`Fields.MiddleName = "middleName"`

#### `Name`

`Fields.Name = "name"`

#### `NamePrefix`

`Fields.NamePrefix = "namePrefix"`

#### `NameSuffix`

`Fields.NameSuffix = "nameSuffix"`

#### `Nickname`

`Fields.Nickname = "nickname"`

#### `NonGregorianBirthday`

Supported platforms: iOS.

`Fields.NonGregorianBirthday = "nonGregorianBirthday"`

#### `Note`

`Fields.Note = "note"`

#### `PhoneNumbers`

`Fields.PhoneNumbers = "phoneNumbers"`

#### `PhoneticFirstName`

`Fields.PhoneticFirstName = "phoneticFirstName"`

#### `PhoneticLastName`

`Fields.PhoneticLastName = "phoneticLastName"`

#### `PhoneticMiddleName`

`Fields.PhoneticMiddleName = "phoneticMiddleName"`

#### `RawImage`

`Fields.RawImage = "rawImage"`

#### `Relationships`

`Fields.Relationships = "relationships"`

#### `SocialProfiles`

Supported platforms: iOS.

`Fields.SocialProfiles = "socialProfiles"`

#### `UrlAddresses`

`Fields.UrlAddresses = "urlAddresses"`

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

### `SortTypes`

Supported platforms: Android, iOS.

#### `FirstName`

`SortTypes.FirstName = "firstName"`

Sort by first name in ascending order.

#### `LastName`

`SortTypes.LastName = "lastName"`

Sort by last name in ascending order.

#### `None`

`SortTypes.None = "none"`

No sorting should be applied.

#### `UserDefault`

Supported platforms: Android.

`SortTypes.UserDefault = "userDefault"`

The user default method of sorting.

## Permissions

### Android

This library automatically adds `READ_CONTACTS` and `WRITE_CONTACTS` permissions to your app:

| Android permission | Description |
| --- | --- |
| `READ_CONTACTS` | Allows an application to read the user's contacts data. |
| `WRITE_CONTACTS` | Allows an application to write the user's contacts data. |

### iOS

The following usage description keys are used by this library:

| Info.plist key | Description |
| --- | --- |
| `NSContactsUsageDescription` | A message that tells the user why the app is requesting access to the user’s contacts. |
