---
url: https://developer-portal.gainsight.com/docs/getting-started.md
description: >-
  Extend your community with Extensions (widgets, scripts, stylesheets),
  Connectors, and the SDK — or the No-Code Builder if you don't write code
---

# Getting Started

Developer Studio lets you extend your community from code. You publish **Extensions** — widgets, scripts, and stylesheets — from a Git repository, wire them to external services with **Connectors**, and drive it all with the **SDK**. If you'd rather not write code, the **No-Code Builder** lets you add and configure built-in widgets visually.

Whether you're a community manager enriching your pages or a developer building custom Extensions, this page points you to the right starting point.

## What Can You Do

There are several ways to extend your community, depending on your goals and technical comfort level.

### Use the No-Code Builder

**Best for**: Community managers, content creators, anyone comfortable with drag-and-drop.

The **No-Code Builder** is a visual editor where you can add, arrange, and configure widgets on your community pages — no coding required. Many widgets are available out of the box.

### Build Extensions from Code

**Best for**: Developers who want full control over what they ship.

**Extensions** are the widgets, scripts, and stylesheets you publish from a Git repository. Declare them in `extensions_registry.json`, push to your watched branch, and the platform publishes them automatically — with version control, code review, and branch-based environments.

* [Extensions](/custom-widgets/v2/) — overview of widgets, scripts, and stylesheets
* [Your First Widget](/custom-widgets/v2/build-first-widget) — hands-on widget tutorial
* [Your First Script](/custom-widgets/v2/first-script) — hands-on script tutorial
* [Your First Stylesheet](/custom-widgets/v2/first-stylesheet) — hands-on stylesheet tutorial

### Connect to External Data

**Best for**: Teams that want to pull or push data from services like Salesforce, Statuspage, or any API.

**Connectors** act as secure bridges between your Extensions and external APIs. You configure them through a form-based interface — setting the URL, authentication, and data format — and your code can call them without exposing any credentials.

[Connectors](/connectors/)

### Call Connectors from Code

**Best for**: Developers wiring live data into a widget.

The **SDK** is the JavaScript library your widget code uses to call connectors and read community context from the browser.

[SDK](/sdk/)

## Choose Your Path

| I want to...                                | Start here                                              |
|---------------------------------------------|---------------------------------------------------------|
| Add widgets to pages without coding         | Use the **No-Code Builder** (built into your community) |
| Build a custom widget from scratch          | [Your First Widget](/custom-widgets/v2/build-first-widget) |
| Add a global script to the community        | [Your First Script](/custom-widgets/v2/first-script) |
| Add a global stylesheet to the community    | [Your First Stylesheet](/custom-widgets/v2/first-stylesheet) |
| Connect a widget to an external API         | [Build Your First Connector](/connectors/build-first-connector) |
| Understand the SDK for calling connectors   | [SDK](/sdk/)                                            |
| Learn the key terms and concepts            | [Key Concepts](getting-started/concepts)                                |

## Next Steps

* [Your First Widget](/custom-widgets/v2/build-first-widget) — hands-on widget tutorial
* [Your First Script](/custom-widgets/v2/first-script) — hands-on script tutorial
* [Your First Stylesheet](/custom-widgets/v2/first-stylesheet) — hands-on stylesheet tutorial
* [Build Your First Connector](/connectors/build-first-connector) — hands-on tutorial for external API integration
* [Extensions](/custom-widgets/v2/) — all guides and reference for widgets, scripts, and stylesheets
* [Connectors](/connectors/) — all guides and reference for connectors
* [Key Concepts](getting-started/concepts) — the terms and ideas used throughout this documentation
