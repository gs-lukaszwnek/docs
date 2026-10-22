---
url: https://developer-portal.gainsight.com/docs/sdk.md
description: >-
  The SDK in one import — connectors and web from @gainsight-hub/sdk for widgets
  and sitewide scripts, plus the per-widget runtime on init(sdk); how the import
  resolves and how to set up your project
---

# SDK

Widgets and sitewide scripts get the SDK from one import:

```js
import { connectors, web } from "@gainsight-hub/sdk";
```

Import only what you use. The per-widget runtime is not part of the import: it stays on the `sdk` object the platform passes to your widget's `init(sdk)` function.

| Part | Access | Purpose |
|------|--------|---------|
| **Connectors** | `connectors` from the import | Call [connectors](/connectors/) to reach external APIs without exposing credentials in the browser — see [`connectors`](/sdk/reference#connectors) |
| **Web** | `web` from the import | Search content, look up users, and manage subscriptions on community pages — see [`web`](/sdk/reference#web) |
| **Runtime** | `whenReady`, `getProps`, `on`, `getContainer`, `shadowRoot`, `getDesignTokens`, and events on `init(sdk)` | The widget's shadow root, props, design tokens, and events — see [`init(sdk)` runtime](/sdk/reference#init-sdk-runtime) |

The runtime is per widget instance: each widget gets its own `sdk` object, the same way React hands a component its `props`. `connectors` and `web` are page-wide: every widget and sitewide script on the page shares them. A widget can use all three at once:

```js
import { connectors, web } from "@gainsight-hub/sdk";

export async function init(sdk) {
  await sdk.whenReady();
  const { permalink } = sdk.getProps();
  const orders = await connectors.execute({ permalink, method: "GET" });
  const user = await web.Context.User();
  sdk.getContainer().textContent = `${user.username}: ${JSON.stringify(orders)}`;
}
```

## How the import resolves

The community page provides an import map that maps `@gainsight-hub/sdk` to the SDK. The community, not your widget, controls the version: one update reaches every widget and script on the page, and nothing you ship pins an old copy.

`web` uses the community's Web SDK and throws on pages without it. For a visitor who isn't signed in, `web.Context.User()` resolves a guest user whose `userId` is `null`:

```js
const user = await web.Context.User();
const signedIn = user.userId !== null;
```

## Widgets: keep it external

Never bundle `@gainsight-hub/sdk` into a widget. A bundled copy is pinned in your widget and bypasses the version the community provides.

`gsds` 1.9.0 and later scaffold the package as a devDependency, for types only, and keep it external in `gsds build`. In a project set up by hand, install it and mark it external in your bundler:

```bash
npm install -D @gainsight-hub/sdk
```

```js
// vite.config.js
export default {
  build: {
    rollupOptions: {
      external: ["@gainsight-hub/sdk"],
    },
  },
};
```

`gsds preview` loads the installed package in the local dev server; calls still go to the community's Web SDK and the shared connectors client.

## Sitewide scripts

A bare module specifier only resolves in a module script, so a sitewide script that imports the SDK must be registered with `"attributes": {"type": "module"}`. `gsds script new` does this for you.

```json
{
  "scripts": [
    {
      "name": "greeter",
      "path": "scripts/greeter/script.js",
      "placement": "head",
      "attributes": { "type": "module" }
    }
  ]
}
```

## Where to go next

| You want to... | Go to |
|----------------|-------|
| Build your first widget | [Your First Widget](/custom-widgets/v2/build-first-widget) |
| Call an external API from a widget | [Calling from Widget Code](/connectors/calling-from-widgets) |
| Look up a method, option, or event | [SDK Reference](/sdk/reference) |
| Copy a working snippet | [SDK Examples](/sdk/examples) |
| Read the current viewer or other users | [User Context](/sdk/user-context) |
| Understand the widget lifecycle | [Widget Runtime](/custom-widgets/v2/core-concepts) |
