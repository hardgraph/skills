---
title: GLView
description: A library that provides GLView that acts as an OpenGL ES render target and provides GLContext. Useful for rendering 2D and 3D graphics.
sourceCodeUrl: 'https://github.com/expo/expo/tree/sdk-57/packages/expo-gl'
packageName: 'expo-gl'
iconUrl: '/static/images/packages/expo-gl.png'
platforms: ['android', 'ios', 'web', 'expo-go']
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/versions/latest/sdk/gl-view/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/versions/latest/sdk/gl-view/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, use llms.txt to find the relevant page as Markdown (.md) instead of guessing.

You are here: Reference (v57.0.0) > Expo SDK (86 pages in this section)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Expo GLView

A library that provides GLView that acts as an OpenGL ES render target and provides GLContext. Useful for rendering 2D and 3D graphics.
Android, iOS, Web, Included in Expo Go

`expo-gl` provides a `View` that acts as an OpenGL ES render target, useful for rendering 2D and 3D graphics. On mounting, an OpenGL ES context is created. Its drawing buffer is presented as the contents of the `View` every frame.

## Installation

```sh
# npm
npx expo install expo-gl

# yarn
yarn expo install expo-gl

# pnpm
pnpm expo install expo-gl

# bun
bun expo install expo-gl
```

If you are installing this in an [existing React Native app](/bare/overview.md), make sure to [install `expo`](/bare/installing-expo-modules.md) in your project.

## Usage

```jsx
import { View } from 'react-native';
import { GLView } from 'expo-gl';

export default function App() {
  return (
    <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
      <GLView style={{ width: 300, height: 300 }} onContextCreate={onContextCreate} />
    </View>
  );
}

function onContextCreate(gl) {
  gl.viewport(0, 0, gl.drawingBufferWidth, gl.drawingBufferHeight);
  gl.clearColor(0, 1, 1, 1);

  // Create vertex shader (shape & position)
  const vert = gl.createShader(gl.VERTEX_SHADER);
  gl.shaderSource(
    vert,
    `
    void main(void) {
      gl_Position = vec4(0.0, 0.0, 0.0, 1.0);
      gl_PointSize = 150.0;
    }
  `
  );
  gl.compileShader(vert);

  // Create fragment shader (color)
  const frag = gl.createShader(gl.FRAGMENT_SHADER);
  gl.shaderSource(
    frag,
    `
    void main(void) {
      gl_FragColor = vec4(0.0, 0.0, 0.0, 1.0);
    }
  `
  );
  gl.compileShader(frag);

  // Link together into a program
  const program = gl.createProgram();
  gl.attachShader(program, vert);
  gl.attachShader(program, frag);
  gl.linkProgram(program);
  gl.useProgram(program);

  gl.clear(gl.COLOR_BUFFER_BIT);
  gl.drawArrays(gl.POINTS, 0, 1);

  gl.flush();
  gl.endFrameEXP();
}
```

## High-level APIs

Since the WebGL API is quite low-level, it can be helpful to use higher-level graphics APIs rendering through a `GLView` underneath. The following libraries integrate popular graphics APIs:

-   [expo-three](https://github.com/expo/expo-three) for [three.js](https://threejs.org)
-   [expo-processing](https://github.com/expo/expo-processing) for [Processing](https://processing.org/reference)

Any WebGL-supporting library that expects a [WebGLRenderingContext](https://www.khronos.org/registry/webgl/specs/latest/1.0/#5.14) could be used. Some times such libraries assume a web JavaScript context (such as assuming `document`). Usually this is for resource loading or event handling, with the main rendering logic still only using pure WebGL. So these libraries can usually still be used with a couple workarounds. The Expo-specific integrations above include workarounds for some popular libraries.

## Integration with Reanimated worklets

To use this API inside Reanimated worklet, you need to pass the GL context ID to the worklet and recreate the GL object like in the example below.

```jsx
import { View } from 'react-native';
import { runOnUI } from 'react-native-reanimated';
import { GLView } from 'expo-gl';

function render(gl) {
  'worklet';
  // add your WebGL code here
}

function onContextCreate(gl) {
  runOnUI((contextId: number) => {
    'worklet';
    const gl = GLView.getWorkletContext(contextId);
    render(gl);
  })(gl.contextId);
}

export default function App() {
  return (
    <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
      <GLView
        style={{ width: 300, height: 300 }}
        enableExperimentalWorkletSupport
        onContextCreate={onContextCreate}
      />
    </View>
  );
}
```

For more in-depth example on how to use `expo-gl` with Reanimated and Gesture Handler you can check [this example](https://github.com/expo/expo/tree/main/apps/native-component-list/src/screens/GL/GLReanimatedExample.tsx).

### Limitations

Worklet runtime is imposing some limitations on the code that runs inside it, so if you have existing WebGL code, it'll likely require some modifications to run inside a worklet thread.

-   Third-party libraries like Pixi.js or Three.js won't work inside the worklet, you can only use functions that have `'worklet'` added at the start.
-   If you need to load some assets to pass to the WebGL code, it needs to be done on the main thread and passed via some reference to the worklet. If you are using `expo-assets` you can just pass asset object returned by `Asset.fromModule` or from hook `useAssets` to the `runOnUI` function.
-   To implement a rendering loop you need to use `requestAnimationFrame`, APIs like `setTimeout` are not supported.

Check [Reanimated documentation](https://docs.swmansion.com/react-native-reanimated/docs/guides/worklets/) to learn more.

## Remote debugging and GLView

This API does not function as intended with remote debugging enabled. The React Native debugger runs JavaScript on your computer, not the mobile device. GLView requires synchronous native calls that are not supported in Chrome.

## API

```js
import { GLView } from 'expo-gl';
```

## Component

### `GLView`

Supported platforms: Android, iOS, Web.

Type: React.[Component](https://react.dev/reference/react/Component)<[GLViewProps](#glviewprops)\>

A View that acts as an OpenGL ES render target. On mounting, an OpenGL ES context is created. Its drawing buffer is presented as the contents of the View every frame.

GLViewProps

### `enableExperimentalWorkletSupport`

Supported platforms: Android, iOS, Web.

Optional • Type: `boolean` • Default: `false`

Enables support for interacting with a `gl` object from code running on the Reanimated worklet thread.

### `msaaSamples`

Supported platforms: iOS.

Optional • Type: `number` • Default: `4`

`GLView` can enable iOS's built-in [multisampling](https://www.khronos.org/registry/OpenGL/extensions/APPLE/APPLE_framebuffer_multisample.txt). This prop specifies the number of samples to use. Setting this to `0` turns off multisampling.

### `onContextCreate`

Supported platforms: Android, iOS, Web.

Type: (gl: [ExpoWebGLRenderingContext](#expowebglrenderingcontext)) => void

A function that will be called when the OpenGL ES context is created. The function is passed a single argument `gl` that extends a [WebGLRenderingContext](https://www.khronos.org/registry/webgl/specs/latest/1.0/#5.14) interface.

#### Inherited props

-   [ViewProps](https://reactnative.dev/docs/view#props)

## Static methods

### `createContextAsync()`

Supported platforms: Android, iOS, Web.

Imperative API that creates headless context which is devoid of underlying view. It's useful for headless rendering or in case you want to keep just one context per application and share it between multiple components. It is slightly faster than usual context as it doesn't swap framebuffers and doesn't present them on the canvas, however it may require you to take a snapshot in order to present its results. Also, keep in mind that you need to set up a viewport and create your own framebuffer and texture that you will be drawing to, before you take a snapshot.

Returns: `Promise<expowebglrenderingcontext>`

A promise that resolves to WebGL context object. See [WebGL API](#webgl-api) for more details.

### `destroyContextAsync(exgl)`

Supported platforms: Android, iOS, Web.

| Parameter | Type | Description |
| --- | --- | --- |
| `exgl`(optional) | number | [ExpoWebGLRenderingContext](#expowebglrenderingcontext) | WebGL context to destroy. |

  

Destroys given context.

Returns: `Promise<boolean>`

A promise that resolves to boolean value that is `true` if given context existed and has been destroyed successfully.

### `takeSnapshotAsync(exgl, options)`

Supported platforms: Android, iOS, Web.

| Parameter | Type | Description |
| --- | --- | --- |
| `exgl`(optional) | number | [ExpoWebGLRenderingContext](#expowebglrenderingcontext) | WebGL context to take a snapshot from. |
| `options`(optional) | [SnapshotOptions](#snapshotoptions) | Default: `{}` |

  

Takes a snapshot of the framebuffer and saves it as a file to app's cache directory.

Returns: `Promise<glsnapshot>`

A promise that resolves to `GLSnapshot` object.

## Component methods

### `createCameraTextureAsync(cameraRefOrHandle)`

Supported platforms: Android, iOS, Web.

| Parameter | Type |
| --- | --- |
| `cameraRefOrHandle` | [ComponentOrHandle](#componentorhandle) |

  

Returns: `Promise<webgltexture>`

### `destroyObjectAsync(glObject)`

Supported platforms: Android, iOS, Web.

| Parameter | Type |
| --- | --- |
| `glObject` | [WebGLObject](#webglobject) |

  

Returns: `Promise<boolean>`

### `takeSnapshotAsync(options)`

Supported platforms: Android, iOS, Web.

| Parameter | Type |
| --- | --- |
| `options`(optional) | [SnapshotOptions](#snapshotoptions) |

  

Same as static [`takeSnapshotAsync()`](#takesnapshotasyncoptions), but uses WebGL context that is associated with the view on which the method is called.

Returns: `Promise<glsnapshot>`

## Methods

### `GLView.getWorkletContext(contextId)`

Supported platforms: Android, iOS, Web.

| Parameter | Type |
| --- | --- |
| `contextId` | `number` |

  

Returns: `ExpoWebGLRenderingContext | undefined`

## Interfaces

### `ExpoWebGLRenderingContext`

Supported platforms: Android, iOS, Web.

Extends: [WebGL2RenderingContext](https://developer.mozilla.org/en-US/docs/Web/API/WebGL2RenderingContext)

| Property | Type | Description |
| --- | --- | --- |
| contextId | `number` | - |

ExpoWebGLRenderingContext Methods

### `__expoSetLogging(option)`

Supported platforms: Android, iOS, Web.

| Parameter | Type |
| --- | --- |
| `option` | [GLLoggingOption](#glloggingoption) |

  

Returns: `void`

### `_expo_texImage2D(...props)`

Supported platforms: Android, iOS, Web.

| Parameter | Type |
| --- | --- |
| `. .props` | `any[]` |

  

Returns: `void`

### `_expo_texSubImage2D(...props)`

Supported platforms: Android, iOS, Web.

| Parameter | Type |
| --- | --- |
| `. .props` | `any[]` |

  

Returns: `void`

### `endFrameEXP()`

Supported platforms: Android, iOS, Web.

Returns: `void`

### `flushEXP()`

Supported platforms: Android, iOS, Web.

Returns: `void`

## Types

### `ComponentOrHandle`

Supported platforms: Android, iOS, Web.

Literal type: `union`

Acceptable values are: `null` | `number` | [Component](https://react.dev/reference/react/Component)<any, any\> | `ComponentClass<any>`

### `GLSnapshot`

Supported platforms: Android, iOS, Web.

| Property | Type | Description |
| --- | --- | --- |
| height | `number` | Height of the snapshot. |
| localUri | `string` | Synonym for `uri`. Makes snapshot object compatible with `texImage2D`. |
| uri | string | [Blob](https://developer.mozilla.org/en-US/docs/Web/API/Blob) | null | URI to the snapshot. |
| width | `number` | Width of the snapshot. |

### `SnapshotOptions`

Supported platforms: Android, iOS, Web.

| Property | Type | Description |
| --- | --- | --- |
| compress(optional) | `number` | A value in range `0` to `1.0` specifying compression level of the result image. `1.0` means no compression and `0` the highest compression. Default: `1.0` |
| flip(optional) | `boolean` | Whether to flip the snapshot vertically. Default: `false` |
| format(optional) | `'jpeg' | 'png' | 'webp'` | Specifies what type of compression should be used and what is the result file extension. PNG compression is lossless but slower, JPEG is faster but the image has visible artifacts. Note: When using WebP format, the iOS version will print a warning, and generate a 'png' file instead. It is recommended to use platform-specific code in this case. Default: `'jpeg'` |
| framebuffer(optional) | [WebGLFramebuffer](https://developer.mozilla.org/en-US/docs/Web/API/WebGLFramebuffer) | Specify the framebuffer that we will be reading from. Defaults to underlying framebuffer that is presented in the view or the current framebuffer if context is headless. |
| rect(optional) | `{ height: number, width: number, x: number, y: number }` | Rect to crop the snapshot. It's passed directly to `glReadPixels`. |

### `SurfaceCreateEvent`

Supported platforms: Android, iOS, Web.

| Property | Type | Description |
| --- | --- | --- |
| nativeEvent | `{ exglCtxId: number }` | - |

### `WebGLObject`

Supported platforms: Android, iOS, Web.

| Property | Type | Description |
| --- | --- | --- |
| id | `number` | - |

## Enums

### `GLLoggingOption`

Supported platforms: Android, iOS, Web.

#### `DISABLED`

`GLLoggingOption.DISABLED = 0`

Disables logging entirely.

#### `METHOD_CALLS`

`GLLoggingOption.METHOD_CALLS = 1`

Logs method calls, their parameters and results.

#### `GET_ERRORS`

`GLLoggingOption.GET_ERRORS = 2`

Calls `gl.getError()` after each other method call and prints an error if any is returned. This option has a significant impact on the performance as this method is blocking.

#### `RESOLVE_CONSTANTS`

`GLLoggingOption.RESOLVE_CONSTANTS = 4`

Resolves parameters of type `number` to their constant names.

#### `TRUNCATE_STRINGS`

`GLLoggingOption.TRUNCATE_STRINGS = 8`

When this option is enabled, long strings will be truncated. It's useful if your shaders are really big and logging them significantly reduces performance.

#### `ALL`

`GLLoggingOption.ALL = 15`

Enables all other options. It implies `GET_ERRORS` so be aware of the slowdown.

## WebGL API

Once the component is mounted and the OpenGL ES context has been created, the `gl` object received through the `onContextCreate` prop becomes the interface to the OpenGL ES context, providing a WebGL API. It resembles a [WebGL2RenderingContext](https://registry.khronos.org/webgl/specs/latest/2.0/#3.7) in the WebGL 2 spec.

Some older Android devices may not support WebGL2 features. To check whether the device supports WebGL2 it's recommended to use `gl instanceof WebGL2RenderingContext`.

An additional method `gl.endFrameEXP()` is present, which notifies the context that the current frame is ready to present. This is similar to a 'swap buffers' API call in other OpenGL platforms.

The following WebGL2RenderingContext methods are currently unimplemented:

-   `getFramebufferAttachmentParameter()`
-   `getRenderbufferParameter()`
-   `compressedTexImage2D()`
-   `compressedTexSubImage2D()`
-   `getTexParameter()`
-   `getUniform()`
-   `getVertexAttrib()`
-   `getVertexAttribOffset()`
-   `getBufferSubData()`
-   `getInternalformatParameter()`
-   `renderbufferStorageMultisample()`
-   `compressedTexImage3D()`
-   `compressedTexSubImage3D()`
-   `fenceSync()`
-   `isSync()`
-   `deleteSync()`
-   `clientWaitSync()`
-   `waitSync()`
-   `getSyncParameter()`
-   `getActiveUniformBlockParameter()`

The `pixels` argument of [`texImage2D()`](https://developer.mozilla.org/en-US/docs/Web/API/WebGLRenderingContext/texImage2D) must be `null`, an `ArrayBuffer` with pixel data, or an object of the form `{ localUri }` where `localUri` is the `file://` URI of an image in the device's file system. Thus, an `Asset` object is used once `.downloadAsync()` has been called on it (and completed) to fetch the resource.

For efficiency reasons, the current implementations of the methods don't perform type or bounds checking on their arguments. So, passing invalid arguments may cause a native crash. There are plans to update the API to perform argument checking in upcoming SDK versions.

Currently, the priority for error checking is low since engines generally don't rely on the OpenGL API to perform argument checking; otherwise, checks performed by the underlying OpenGL ES implementation are often sufficient.
