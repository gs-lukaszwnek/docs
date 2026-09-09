---
url: >-
  https://developer-portal.gainsight.com/docs/custom-widgets/v2/sdk-api-reference.md
description: >-
  Properties, methods, and events on the sdk object passed to your widget's
  init(sdk) function — a different object from window.WidgetServiceSDK; see SDK
  Concepts for how the two relate
---

# Widget Runtime Reference

This page documents the `sdk` instance the platform passes to your widget's `init(sdk)` function — a different object from `window.WidgetServiceSDK` (see [SDK Concepts](/sdk/concepts) for how the two relate).

::: warning This `sdk` instance has no `connectors` property
`sdk.connectors.execute(...)` throws `Cannot read properties of undefined (reading 'execute')`. See [Calling from Widget Code](/connectors/calling-from-widgets) for the correct pattern.
:::

## Properties

| Property      | Type         | Description                             |
| ------------- | ------------ | --------------------------------------- |
| `root`        | `ShadowRoot` | The widget's shadow root (mount target) |
| `shadowRoot`  | `ShadowRoot` | Alias for `root` — the same shadow root reference |
| `document`    | `Document`   | Global document reference               |
| `version`     | `string`     | SDK version (e.g. `"1.5.2"`)           |

## Methods

| Method               | Returns          | Description                                    |
| -------------------- | ---------------- | ---------------------------------------------- |
| `getContainer()`     | `ShadowRoot`     | Returns the widget's shadow root — same reference as `root`/`shadowRoot`, recommended mount target for framework apps (React, Vue, etc.) |
| `getProps<T>()`      | `T`              | Current widget props (from configuration)      |
| `setProps(props)`    | `void`           | Update props (emits `propsChanged`)            |
| `getVersionInfo()`   | `VersionInfo`    | `{ version, gitSha, buildTime }`               |
| `whenReady()`        | `Promise<SDK>`   | Resolves once initialization completes         |
| `$(selector)`        | `Element \| null` | Shorthand for `shadowRoot.querySelector(selector)` |
| `$$(selector)`       | `Element[]`      | Shorthand for `shadowRoot.querySelectorAll(selector)` — returns an array, not a NodeList |
| `on(event, handler)` | `() => void`     | Subscribe to an event (returns unsubscribe fn) |
| `off(event, handler)`| `void`           | Unsubscribe from an event                      |
| `emit(event, data?)` | `void`           | Emit a custom event                            |

## Built-in Events

| Event                 | Payload                              | Trigger                              |
| --------------------- | ------------------------------------ | ------------------------------------ |
| `propsChanged`        | `Record<string, unknown>`            | Widget configuration changes         |
| `destroy`             | `undefined`                          | Widget removed from DOM              |
| `error`               | `{ message: string, error?: Error }` | Module load or initialization error  |

## Next Steps

* [Widget Runtime](core-concepts) — Runtime model, lifecycle, props, and design tokens
* [Configurable Widgets](configurable-widgets) — Let editors customize your widget via a form in the **No-Code Builder**
* [Using React](using-react) — Build widgets with React
