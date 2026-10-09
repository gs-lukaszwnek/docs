---
url: https://developer-portal.gainsight.com/docs/sdk.md
description: >-
  The sdk object passed to a widget's init(sdk) function — widget runtime,
  Connectors SDK (sdk.connectors) and Web SDK (sdk.web)
---

# SDK

The SDK is the `sdk` object the platform passes to your widget's `init(sdk)` function. It carries the widget runtime plus two clients, and nothing needs to be constructed or loaded. Choose the part you need:

| Part | Access | Purpose |
|------|--------|---------|
| **Runtime** | `sdk.getProps()`, `sdk.$()`, `sdk.on()` | The widget's shadow root, props, events, and design tokens — see the [Widget Runtime Reference](/sdk/runtime-reference). |
| **Connectors** | `sdk.connectors` | Call [connectors](/connectors/) from widget code to reach external APIs — see the [Connectors SDK](/sdk/connectors-sdk/overview). |
| **Web** | `sdk.web` | Search content, users, and subscriptions on community pages — see the [Web SDK](/sdk/web-sdk/overview). |

New to widgets? Start with [Your First Widget](/custom-widgets/v2/build-first-widget). To call an external API from a widget, see [Calling from Widget Code](/connectors/calling-from-widgets). For how the three parts relate, read [SDK Concepts](/sdk/concepts).
