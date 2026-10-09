---
url: https://developer-portal.gainsight.com/docs/sdk/runtime-reference.md
description: >-
  Properties, methods, and events on the sdk object passed to your widget's
  init(sdk) function, including sdk.connectors and sdk.web
---

# Widget Runtime Reference

This page documents the `sdk` object the platform passes to your widget's `init(sdk)` function. It is the single SDK entry point for widget code: it carries the widget's runtime API plus the connectors client (`sdk.connectors`) and the community Web SDK (`sdk.web`).

## Properties

| Property      | Type         | Description                             |
| ------------- | ------------ | --------------------------------------- |
| `root`        | `ShadowRoot` | The widget's shadow root (mount target) |
| `shadowRoot`  | `ShadowRoot` | Alias for `root` — the same shadow root reference |
| `document`    | `Document`   | Global document reference               |
| `version`     | `string`     | SDK version (e.g. `"1.5.2"`)           |
| `connectors`  | Connectors client | The connectors client, shared by every widget on the page (`configure()` affects all of them). See [Connectors SDK](/sdk/connectors-sdk/overview) |
| `web`         | Web SDK | `undefined` | The community Web SDK. `undefined` where the host page has none (embedded widgets on external sites, Skilljar). See [Web SDK](/sdk/web-sdk/overview) |

## Methods

| Method               | Returns          | Description                                    |
| -------------------- | ---------------- | ---------------------------------------------- |
| `getContainer()`     | `ShadowRoot`     | Returns the widget's shadow root — same reference as `root`/`shadowRoot`, recommended mount target for framework apps (React, Vue, etc.) |
| `getProps<T>()`      | `T`              | Current widget props (from configuration)      |
| `setProps(props)`    | `void`           | Merges `props` into the current props (keys are never removed) and emits `propsChanged`. Local to this widget instance; not persisted.|
| `getVersionInfo()`   | `VersionInfo`    | `{ version, gitSha, buildTime }`               |
| `getDesignTokens()`  | `Record<string, string \| null>` | Current theme design tokens, keyed by token name without the leading `--` (e.g. `config--main-color-brand` for `--config--main-color-brand`). Returns a copy |
| `whenReady()`        | `Promise<SDK>`   | Resolves when the widget is ready to use. Await it as the first statement of `init`.|
| `$(selector)`        | `Element \| null` | Shorthand for `shadowRoot.querySelector(selector)` |
| `$$(selector)`       | `Element[]`      | Shorthand for `shadowRoot.querySelectorAll(selector)` — returns an array, not a NodeList |
| `on(event, handler)` | `() => void`     | Subscribe to an event (returns unsubscribe fn) |
| `off(event, handler)`| `void`           | Unsubscribe from an event                      |
| `emit(event, data?)` | `void`           | Emit an event to this widget's own `on()` listeners|

## Built-in Events

| Event                 | Payload                              | Trigger                              |
| --------------------- | ------------------------------------ | ------------------------------------ |
| `propsChanged`        | `Record<string, unknown>`            | Widget configuration changes         |
| `destroy`             | `undefined`                          | Widget removed from DOM              |
| `error`               | `{ message: string, error?: Error }` | Module load or initialization error  |
| `designTokensChanged` | `Record<string, string \| null>`     | Theme design tokens change while the page is open, e.g. live edits in the theme configurator. Payload is the full token map, same shape as `getDesignTokens()` |

## Next Steps

* [Widget Runtime](/custom-widgets/v2/core-concepts) — Runtime model, lifecycle, props, and design tokens
* [Configurable Widgets](/custom-widgets/v2/configurable-widgets) — Let editors customize your widget via a form in the **No-Code Builder**
* [Using React](/custom-widgets/v2/using-react) — Build widgets with React
* [SDK Concepts](/sdk/concepts) — How the runtime, `sdk.connectors` and `sdk.web` relate
