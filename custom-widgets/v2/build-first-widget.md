---
url: >-
  https://developer-portal.gainsight.com/docs/custom-widgets/v2/build-first-widget.md
description: >-
  A hands-on tutorial that walks you through creating, configuring, and
  publishing a custom widget from scratch
---

# Your First Widget

In this tutorial, you will create a custom widget from scratch, configure it with user-editable settings, and publish it to your community. By the end, you will have a working widget in the No-Code Builder that responds to live configuration changes.

## Before You Start

* You have a GitHub account.
* You have access to Sources in your platform (**Integrations** → **Developer Studio** → **Sources**).
* You have a connected GitHub organization — see [Connect Your GitHub Account](connect-github) if you haven't connected one yet.
* You have a repository from the [template](https://github.com/gainsight-hub/widgets-repository-template), enabled in Sources with `main` as the watched branch — see [Repository Layout](project-setup) and [Repository & Branch Settings](repository-settings).

## Step 1: Create the Widget File

In your repository, create the directory `widgets/greeting/` and inside it create `index.html` with this content:

```html
<style>
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
</style>

<div class="greeting-card">
  <h2 class="title">Hello, Community!</h2>
  <p class="subtitle">This is your first custom widget.</p>
</div>
```

Notice that the styles reference design tokens with `var()` — the platform injects your community's branding values for colors and fonts automatically. The second argument to each `var()` is a fallback so the widget looks reasonable outside a community context.

You now have the widget's HTML file. It is a fragment — no `<html>`, `<head>`, or `<body>` tags.

## Step 2: Register the Widget

Open `extensions_registry.json` at the root of your repository and add this entry to the `widgets` array:

```json
{
  "widgets": [
    {
      "title": "Greeting Card",
      "type": "my_first_widget_greeting",
      "description": "A simple greeting card widget",
      "category": "custom",
      "source": {
        "path": "widgets/greeting",
        "entry": "index.html"
      }
    }
  ]
}
```

The `type` field is the widget's unique identifier across your entire community. The value `my_first_widget_greeting` is namespaced to avoid conflicts with other widgets — keep this pattern when you build real widgets.

The `source` block tells the platform where your widget files live in the repository. `path` is the directory; `entry` is the HTML file inside it.

These are the required fields. There are more options you can use to customize the widget — see [Widget Definition Reference](widget-schema).

## Step 3: Push and Publish

Commit and push your changes:

```bash
git add extensions_registry.json widgets/greeting/index.html
git commit -m "Add greeting widget"
git push
```

Go back to **Integrations** → **Developer Studio** → **Sources** and watch the build status for your repository. After a few seconds it will change to **Completed**.

Your widget now appears in the No-Code Builder's widget library.

If the status shows **Failed**, open the build details to see the error message. The most common cause at this stage is a JSON syntax error in `extensions_registry.json` — paste the file into [jsonlint.com](https://jsonlint.com) to check it.

## Step 4: Add It to a Page

1. Open the No-Code Builder on any page in your community.
2. Click the widget library icon or the **Add widget** button.
3. Find **Greeting Card** in the list.
4. Drag it onto the page or click **Add**.

You should see your greeting card rendered on the page. The heading color matches your community's brand color, and the background and border reflect your community's design tokens — that is the design tokens at work.

If you do not see the widget in the library, verify that the build status is **Completed**.

## Step 5: Add Configuration

The widget currently shows fixed text. In this step you will add three configuration fields so an admin can change the heading text, the subtext, and the card's background color.

Update the widget entry in `extensions_registry.json` so it looks like this:

```json
{
  "widgets": [
    {
      "title": "Greeting Card",
      "type": "my_first_widget_greeting",
      "description": "A simple greeting card widget",
      "category": "custom",
      "source": {
        "path": "widgets/greeting",
        "entry": "index.html"
      },
      "configuration": {
        "properties": [
          {
            "name": "greeting_title",
            "type": "text",
            "label": "Heading",
            "description": "The main heading shown on the card.",
            "defaultValue": "Hello, Community!",
            "rules": { "required": true, "maxLength": 80 }
          },
          {
            "name": "greeting_subtitle",
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
        "greeting_title": "Hello, Community!",
        "greeting_subtitle": "This is your first custom widget.",
        "card_background": "#ffffff"
      }
    }
  ]
}
```

The `configuration.properties` array defines the three fields that appear in the No-Code Builder form, and `defaultConfig` sets their starting values.

Your registry now defines three configurable properties.

## Step 6: Read Configuration in Your Widget

The configuration values are available at runtime via `sdk.getProps()`. You need two changes: a JavaScript module file and a script tag in your HTML.

Create `widgets/greeting/app.js`:

```javascript
export async function init(sdk) {
  await sdk.whenReady()

  const title = sdk.$('.title')
  const subtitle = sdk.$('.subtitle')
  const card = sdk.$('.greeting-card')

  function applyProps(props) {
    if (title) title.textContent = props.greeting_title || 'Hello, Community!'
    if (subtitle) subtitle.textContent = props.greeting_subtitle || 'This is your first custom widget.'
    if (card) card.style.background = props.card_background || ''
  }

  applyProps(sdk.getProps())

  sdk.on('propsChanged', (newProps) => {
    applyProps(newProps)
  })
}
```

The `export` keyword makes this an ES module — the platform discovers the `init` function and calls it with the `sdk` object. The `sdk` object provides `$()` as a shorthand for querying elements inside the widget's Shadow DOM.

Now update `widgets/greeting/index.html` to load the module:

```html
<style>
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
</style>

<div class="greeting-card">
  <h2 class="title">Hello, Community!</h2>
  <p class="subtitle">This is your first custom widget.</p>
</div>

<script type="module" src="app.js"></script>
```

The `init(sdk)` function is the entry point the platform calls after loading your widget. `await sdk.whenReady()` waits until the `sdk` object is fully initialized. `sdk.$()` queries elements inside your widget's Shadow DOM — it is scoped to your widget only.

`sdk.getProps()` returns the current configuration values. `sdk.on('propsChanged', ...)` fires every time an admin updates the configuration in the No-Code Builder, letting your widget update without a page reload.

## Step 7: Publish and Test Configuration

Commit and push the updated files:

```bash
git add extensions_registry.json widgets/greeting/index.html
git commit -m "Add configurable props to greeting widget"
git push
```

Wait for the build status to show **Completed** in Sources.

Open the No-Code Builder, find your Greeting Card widget on the page, and click the configure icon (or right-click → **Configure**). You should see a form with three fields: Heading, Subtext, and Card Background.

Change the heading text. The widget updates in real time as you type. Change the background color — the card background changes immediately. This is the `propsChanged` event in action.

If the configuration panel does not appear, verify the build completed successfully after your last push.

## What You Built

You created a widget file, registered it in the platform, pushed it to production, and added it to a page. You then defined three configuration fields and connected them to the widget's runtime using the `init(sdk)` contract. The widget now reads its initial configuration and responds to live updates from the No-Code Builder.

## What's Next

* [Widget Runtime](core-concepts) — understand how widgets run, the Shadow DOM model, and the full runtime lifecycle
* [Configurable Widgets](configurable-widgets) — all field types and validation rules for configuration properties
* [Widget Runtime Reference](sdk-api-reference) — every method and event on the `sdk` object
* [Using React](using-react) — build widgets with React instead of vanilla JavaScript
* [Rendering & DOM](rendering-and-dom) — Shadow DOM patterns and element querying in depth
