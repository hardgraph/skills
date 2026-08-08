---
title: Blob
description: A web standards-compliant Blob implementation for React Native.
sourceCodeUrl: 'https://github.com/expo/expo/tree/sdk-57/packages/expo-blob'
packageName: 'expo-blob'
platforms: ['android', 'ios', 'web', 'expo-go']
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/versions/latest/sdk/blob/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/versions/latest/sdk/blob/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, use llms.txt to find the relevant page as Markdown (.md) instead of guessing.

You are here: Reference (v57.0.0) > Expo SDK (86 pages in this section)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Expo Blob

A web standards-compliant Blob implementation for React Native.
Android, iOS, Web, Included in Expo Go

`expo-blob` provides a web standards-compliant Blob implementation for React Native that offers superior performance and works consistently across all platforms. It is more reliable compared to the implementation exported from `react-native`, unlike Blob from `react-native`, which has limitations with the `slice()` method and other Web API features.

## Installation

```sh
# npm
npx expo install expo-blob

# yarn
yarn expo install expo-blob

# pnpm
pnpm expo install expo-blob

# bun
bun expo install expo-blob
```

If you are installing this in an [existing React Native app](/bare/overview.md), make sure to [install `expo`](/bare/installing-expo-modules.md) in your project.

## Usage

### Basic blob creation

```typescript
import { Blob } from 'expo-blob';

// Create an empty blob
const emptyBlob = new Blob();

// Create a blob from text
const textBlob = new Blob(['Hello, World!'], { type: 'text/plain' });

// Create a blob from binary data
const binaryBlob = new Blob([new Uint8Array([1, 2, 3, 4])], {
  type: 'application/octet-stream',
});

// Create a blob from mixed content
const mixedBlob = new Blob(
  [
    'Text content',
    new Uint8Array([65, 66, 67]), // ABC in ASCII
    'More text',
  ],
  { type: 'text/plain' }
);
```

### Blob properties

```typescript
const blob = new Blob(['Hello, World!'], { type: 'text/plain' });

console.log(blob.size); // 13 (bytes)
console.log(blob.type); // "text/plain"
```

### Reading blob content

```typescript
const blob = new Blob(['Hello, World!'], { type: 'text/plain' });

// Read as text
const text = await blob.text();
console.log(text); // "Hello, World!"

// Read as bytes
const bytes = await blob.bytes();
console.log(bytes); // Uint8Array(13) [72, 101, 108, 108, 111, 44, 32, 87, 111, 114, 108, 100, 33]

// Read as ArrayBuffer
const arrayBuffer = await blob.arrayBuffer();
console.log(arrayBuffer); // ArrayBuffer(13)
```

### Slicing blobs

```typescript
const blob = new Blob(['Hello, World!'], { type: 'text/plain' });

// Slice from position 0 to 5
const slice1 = blob.slice(0, 5);
console.log(await slice1.text()); // "Hello"

// Slice from position 7 to end
const slice2 = blob.slice(7);
console.log(await slice2.text()); // "World!"

// Slice with custom type
const slice3 = blob.slice(0, 5, 'text/html');
console.log(slice3.type); // "text/html"
```

### Streaming

```typescript
const blob = new Blob(['Large content...'], { type: 'text/plain' });

// Create a readable stream
const stream = blob.stream();
const reader = stream.getReader();

// Read chunks
while (true) {
  const { done, value } = await reader.read();
  if (done) break;
  console.log('Chunk:', value);
}
```

## API

## Classes

### `Blob`

Supported platforms: Android, iOS, Web.

Blob Properties

### `size`

Supported platforms: Android, iOS, Web.

Read only • Type: `number`

The size of the `Blob` in bytes.

### `type`

Supported platforms: Android, iOS, Web.

Read only • Type: `string`

The MIME type of the `Blob`, or the empty string if the type cannot be determined.

Blob Methods

### `arrayBuffer()`

Supported platforms: Android, iOS, Web.

Returns: `Promise<arraybuffer>`

Promise resolving to the `Blob`'s binary data as an `ArrayBuffer`.

### `bytes()`

Supported platforms: Android, iOS, Web.

Returns: `Promise<uint8array</uint8array`

Promise resolving to the `Blob`'s binary data as a `Uint8Array`.

### `slice(start, end, contentType)`

Supported platforms: Android, iOS, Web.

| Parameter | Type | Description |
| --- | --- | --- |
| `start`(optional) | `number` | The starting byte index (inclusive) represented as a signed 32 bit integer (up to 2^31 - 1). |
| `end`(optional) | `number` | The ending byte index (exclusive) represented as a signed 32 bit integer (up to 2^31 - 1). |
| `contentType`(optional) | `string` | The MIME type of the new `Blob`. If not provided, defaults to an empty string. |

  

Returns: `Blob`

A new `Blob` object containing the data in the specified range of bytes of the source `Blob`.

### `stream()`

Supported platforms: Android, iOS, Web.

> **Note**: The current implementation loads the entire `Blob` into memory before streaming.

Returns: `ReadableStream`

A `ReadableStream` of the `Blob`'s data.

### `text()`

Supported platforms: Android, iOS, Web.

Returns: `Promise<string>`

Promise that resolves with the entire contents of the `Blob` as a UTF-8 string.

## Types

### `BlobPart`

Supported platforms: Android, iOS, Web.

Literal type: `union`

Represents a part of a `Blob`. Can be a `string`, `ArrayBuffer`, `ArrayBufferView`, or another `Blob`.

Acceptable values are: `string` | [ArrayBuffer](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/ArrayBuffer) | `ArrayBufferView` | [Blob](https://developer.mozilla.org/en-US/docs/Web/API/Blob)
