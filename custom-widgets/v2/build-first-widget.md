---
url: >-
  https://developer-portal.gainsight.com/docs/custom-widgets/v2/build-first-widget.md
description: >-
  A hands-on tutorial that walks you through creating, configuring, and
  publishing a React custom widget from scratch
---

# Your First Widget

In this tutorial, you will scaffold a React custom widget with the Developer Studio CLI, give it user-editable settings, preview it in your community, and publish it. By the end, you will have a working widget in the No-Code Builder that responds to live configuration changes.

## Before You Start

* You have a GitHub account.
* You have access to Sources in your platform (**Integrations** → **Developer Studio** → **Sources**).
* You have a connected GitHub organization — see [Connect Your GitHub Account](connect-github) if you haven't connected one yet.
* You have the Developer Studio CLI installed and a session paired with your community — see [Get Started with the CLI](/cli/getting-started).
* You have a project created with `gsds init`, pushed to GitHub, and enabled in Sources with `main` as the watched branch — see [Repository Layout](project-setup#quick-start-create-a-project-with-the-cli) and [Repository & Branch Settings](repository-settings).

## Step 1: Scaffold the Widget

From the root of your project, run:

```sh
gsds create
```

`gsds create` prompts for the widget name, framework, and category. Use these answers:

| Prompt | Answer |
|--------|--------|
| **Widget name** | `greeting` |
| **Framework** | **React** |
| **Category** | `custom` |

This tutorial uses React. For your own widgets, the framework can also be Vue, Angular, vanilla JavaScript, or plain HTML.

In a non-interactive shell, such as an AI agent, pass the same answers as flags: `gsds create --name greeting --framework react --category custom`.

`gsds create` makes the directory `widgets/greeting/` with React, TypeScript, and Vite already set up, and adds an entry for the widget to `extensions_registry.json`. You edit three of the files it created:

| File | Purpose |
|------|---------|
| `src/App.tsx` | The React component your widget renders |
| `src/widget.css` | The widget's styles |
| `src/types.ts` | The types for the `sdk` object and your widget's configuration |

The fourth file to know about is `src/main.tsx`. It exports the `init(sdk)` function the platform calls when it loads your widget: it waits for `sdk.whenReady()`, adds `widget.css` to the widget's Shadow DOM, and mounts `App` with the `sdk` object. You do not need to change it.

## Step 2: Style the Card

Replace the content of `src/widget.css` with:

```css
.greeting-card {
  font-family: var(--font-family-sans, Roboto, sans-serif);
  padding: 24px;
  border-radius: 8px;
  background: var(--color-surface-default, #FFFFFF);
  border: 1px solid var(--color-line-default, #DEDFE2);
  text-align: center;
}

.greeting-card h2 {
  color: var(--color-action-primary-default, #9254D9);
  margin: 0 0 8px;
}

.greeting-card p {
  color: var(--color-content-default, #2B3346);
  margin: 0;
}
```

Notice that the styles reference design tokens with `var()` — your community's branding values for colors and fonts are inherited from the page automatically. The second argument to each `var()` is a fallback so the widget looks reasonable outside a community context.

## Step 3: Render the Card

Replace the content of `src/App.tsx` with:

```tsx
import { useState, useEffect } from "react";
import type { WidgetSDK, WidgetProps } from "./types";

export function App({ sdk }: { sdk: WidgetSDK }) {
  const [props, setProps] = useState<WidgetProps>(sdk.getProps());

  useEffect(() => sdk.on("propsChanged", setProps), [sdk]);

  return (
    <div className="greeting-card" style={{ background: props.card_background }}>
      <h2>{props.title}</h2>
      {props.description && <p>{props.description}</p>}
    </div>
  );
}
```

`sdk.getProps()` returns the widget's current configuration values. `sdk.on("propsChanged", ...)` fires every time an admin changes the configuration in the No-Code Builder, so the component stores the props in state and re-renders with the new values — no page reload. `sdk.on()` returns a function that removes the listener, and returning it from `useEffect` cleans up when the widget unmounts.

The component reads a `card_background` value that the scaffold does not know about yet. Open `src/types.ts` and add it to the `WidgetProps` interface:

```ts
export interface WidgetProps {
  title?: string;
  description?: string;
  card_background?: string;
  [key: string]: unknown;
}
```

## Step 4: Configure the Registry Entry

Open `extensions_registry.json` at the root of your project. `gsds create` added an entry for your widget to the `widgets` array, with two configuration fields — `title` and `description` — that `App` already reads. Update the entry so it looks like this:

```json
{
  "widgets": [
    {
      "type": "greeting",
      "version": "1.0.0",
      "title": "Greeting Card",
      "description": "A simple greeting card widget",
      "category": "custom",
      "imageSrc": "widgets/greeting/thumbnail.png",
      "source": {
        "path": "widgets/greeting/dist",
        "entry": "index.html"
      },
      "configuration": {
        "properties": [
          {
            "name": "title",
            "type": "text",
            "label": "Heading",
            "description": "The main heading shown on the card.",
            "defaultValue": "Hello, Community!",
            "rules": { "required": true, "maxLength": 80 }
          },
          {
            "name": "description",
            "type": "text",
            "label": "Subtext",
            "description": "The smaller text below the heading.",
            "defaultValue": "This is your first custom widget.",
            "rules": { "maxLength": 160 }
          },
          {
            "name": "card_background",
            "type": "color",
            "label": "Card Background",
            "description": "Override the card background color.",
            "defaultValue": "#ffffff"
          }
        ]
      },
      "defaultConfig": {
        "title": "Hello, Community!",
        "description": "This is your first custom widget.",
        "card_background": "#ffffff"
      }
    }
  ]
}
```

The `type` field is the widget's unique identifier across your entire community. `gsds create` sets it to the widget name. The platform uses `type` to match widgets already placed on pages to their source, so do not change it after you publish the widget.

The `source` block tells the platform where your widget files live in the repository. `path` is the build output directory, `dist/`, and `entry` is the HTML file inside it.

The `configuration.properties` array defines the three fields that appear in the No-Code Builder form, and `defaultConfig` sets their starting values. Each property's `name` is the key `App` reads from `sdk.getProps()`. The top-level `title` and `description` are what the widget picker shows; the `title` and `description` properties are what an admin edits.

There are more options you can use to customize the widget — see [Widget Definition Reference](widget-schema).

## Step 5: Preview It in Your Community

Before you publish, check the widget in your community. Start the local preview:

```sh
gsds preview
```

Open the No-Code Builder on any page in your community and open the widget picker. **Greeting Card** appears in the list while `gsds preview` is running. Add it to the page to see it rendered with your community's design tokens. Nothing is published yet.

If your browser asks for permission to connect to devices on your local network, choose **Allow** — see [Allow the browser to reach your local server](/cli/getting-started#allow-the-browser-to-reach-your-local-server). If the widget does not appear, see [Widget missing from the picker](/cli/reference/troubleshooting#widget-missing-from-the-picker).

Stop the preview with `Ctrl-C`.

## Step 6: Build and Publish

Run `gsds build` to bundle the widget into `dist/` and validate the registry, then commit and push your changes:

```bash
gsds build
git add extensions_registry.json widgets/greeting
git commit -m "Add greeting widget"
git push
```

Commit the `dist/` directory with the rest of the widget — the platform publishes the files in `source.path`, so `dist/` must be in your repository.

Go back to **Integrations** → **Developer Studio** → **Sources** and watch the build status for your repository. After a few seconds it will change to **Completed**.

If the status shows **Failed**, open the build details to see the error message.

## Step 7: Add It to a Page and Configure It

1. Open the No-Code Builder on any page in your community.
2. Click the widget library icon or the **Add widget** button.
3. Find **Greeting Card** in the list.
4. Drag it onto the page or click **Add**.

You should see your greeting card rendered on the page. The heading color matches your community's brand color, and the border and text reflect your community's design tokens.

Click the configure icon on the widget (or right-click → **Configure**). You should see a form with three fields: Heading, Subtext, and Card Background.

Change the heading text. The widget updates in real time as you type. Change the background color — the card background changes immediately. This is the `propsChanged` event in action.

If you do not see the widget in the library, or the configuration panel does not appear, verify that the build status is **Completed** after your last push.

## What You Built

You scaffolded a React widget with the CLI, styled it with your community's design tokens, and connected three configuration fields to it through the `sdk` object. You previewed it in your community, published it, and added it to a page. The widget reads its initial configuration and responds to live updates from the No-Code Builder.

## What's Next

* [Using React](using-react) — more React patterns: custom hooks, error boundaries, and bundling tips
* [Widget Runtime](core-concepts) — understand how widgets run, the Shadow DOM model, and the full runtime lifecycle
* [Configurable Widgets](configurable-widgets) — all field types and validation rules for configuration properties
* [Widget Runtime Reference](/sdk/runtime-reference) — every method and event on the `sdk` object
* [Rendering & DOM](rendering-and-dom) — Shadow DOM patterns and element querying in depth
