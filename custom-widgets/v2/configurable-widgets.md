---
url: >-
  https://developer-portal.gainsight.com/docs/custom-widgets/v2/configurable-widgets.md
description: >-
  Let community admins customize your widget in the No-Code Builder by defining
  a configuration form in extensions_registry.json
---

# Configurable Widgets

To let community admins customize your widget when they add it in the No-Code Builder — without editing code — define a **configuration form** in `extensions_registry.json`. The fields you declare become a form in the No-Code Builder, and your widget reads the submitted values at runtime through `sdk.getProps()`.

## How It Works

1. You add a `configuration` object (with a `properties` array) and `defaultConfig` to your widget definition in `extensions_registry.json`.
2. When someone adds or edits the widget on a page, the No-Code Builder shows a form built from those properties.
3. When the widget runs, the SDK makes the current configuration values available via `sdk.getProps()`.
4. If configuration changes while the widget is active, the SDK emits a `propsChanged` event so your widget can react.

## Add a Configuration Form

Add a `configuration` object with a `properties` array. Each property needs at least `name`, `label`, and `type` — plus any optional fields that shape its behavior, such as `rules`, `options` (static choices for `select`), `dynamicOptions` (API-driven choices for `select` and `autocomplete`), `multiple`, and `sortable`. Add a `defaultConfig` object that maps each property `name` to a default value. Set **settings.configurable** to `true` so the configuration form is shown.

### Minimal Example

A configuration form is defined inside a widget entry in `extensions_registry.json`. In the complete file below, the lines in focus are the configuration-specific fields; the greyed-out lines are the standard widget definition that surrounds them:

```jsonc
{
  "widgets": [
    {
      "title": "Card Grid",
      "type": "my_company_card_grid",
      "category": "custom",
      "source": {
        "path": "widgets/card-grid",
        "entry": "index.html"
      },
      "configuration": { // [!code focus:17]
        "properties": [
          {
            "name": "cards_per_row",
            "type": "number",
            "label": "Cards Per Row",
            "defaultValue": 3,
            "rules": { "required": true, "minimum": 1, "maximum": 6 }
          }
        ]
      },
      "defaultConfig": {
        "cards_per_row": 3
      },
      "settings": {
        "configurable": true
      }
    }
  ]
}
```

For the full field schema — every `ConfigField` property, field type, and validation rule — see the [Widget Definition Reference](widget-schema#configuration-object).

## Provide Defaults

The `defaultConfig` object maps each property `name` to a default value. Keys must match the `name` of items in `configuration.properties`. These values are used when the widget is first added or when a value is not set by the editor.

**Example:**

```jsonc
{
  "widgets": [
    {
      "title": "Card Grid",
      "type": "my_company_card_grid",
      "category": "custom",
      "source": {
        "path": "widgets/card-grid",
        "entry": "index.html"
      },
      "configuration": { // [!code focus:26]
        "properties": [
          {
            "name": "cards_per_row",
            "type": "number",
            "label": "Cards Per Row",
            "defaultValue": 3,
            "rules": { "required": true, "minimum": 1, "maximum": 6 }
          },
          {
            "name": "card_style",
            "type": "select",
            "label": "Card Style",
            "defaultValue": "shadow",
            "options": [
              { "value": "flat", "label": "Flat" },
              { "value": "shadow", "label": "Shadow" },
              { "value": "bordered", "label": "Bordered" }
            ]
          }
        ]
      },
      "defaultConfig": {
        "cards_per_row": 3,
        "card_style": "shadow"
      }
    }
  ]
}
```

> **Tip**: `defaultValue` (on each property) and `defaultConfig` (on the widget) serve similar roles. When both are set for the same property, one value is used as the starting configuration — check your platform's behavior if you use both.

Set **settings.configurable** to `true` so the configuration form is shown in the No-Code Builder.

## Group Fields into Sections

By default, every field appears in a single **Configuration** group. For a larger form, group related fields into collapsible **sections**: give each field a `section`, then — optionally — list your sections in a `sections` array on the `configuration` object to set their labels, order, and which one opens first.

```jsonc
{
  "widgets": [
    {
      "title": "Card Grid",
      "type": "my_company_card_grid",
      "category": "custom",
      "source": {
        "path": "widgets/card-grid",
        "entry": "index.html"
      },
      "configuration": { // [!code focus:26]
        "sections": [
          { "name": "layout", "label": "Layout", "expanded": true },
          { "name": "appearance", "label": "Appearance", "description": "Colors and spacing" }
        ],
        "properties": [
          {
            "name": "cards_per_row",
            "type": "number",
            "label": "Cards Per Row",
            "section": "layout"
          },
          {
            "name": "card_background",
            "type": "color",
            "label": "Card Background",
            "section": "appearance"
          },
          {
            "name": "internal_note",
            "type": "text",
            "label": "Internal Note"
          }
        ]
      }
    }
  ]
}
```

Key behavior:

* A field's `section` matches a section's `name`. A section appears only when at least one field points to it.
* Fields without a `section` — like `internal_note` above — fall into the default **Configuration** group, shown first.
* Sections then appear in the order they are listed in `sections`. Mark one `expanded: true` to have it open when the form loads; otherwise the first section opens.
* Grouping is presentation only. You still read every value the same way with `sdk.getProps()`, keyed by field `name`, so you can rename or reorder sections without changing your widget code or losing saved values.

For the full section field list and ordering rules, see the [Widget Definition Reference](widget-schema#configsection-object).

## Populate a Dropdown from an API

Instead of a static `options` array, a `select` or `autocomplete` field can load its choices from an external API using `dynamicOptions`.

### Choosing `select` vs. `autocomplete`

| Type | Static options | Dynamic options | Multi-value |
|------|---------------|----------------|-------------|
| `select` | Yes — via `options` array | Yes — via `dynamicOptions` | Optional — set `multiple: true` |
| `autocomplete` | No | Required — via `dynamicOptions` | Optional — set `multiple: true` |

A `select` field declares **exactly one** of `options` (static) or `dynamicOptions` (dynamic) — never both. An `autocomplete` field always requires `dynamicOptions`.

### When to Use Each Pattern

| Pattern | Type | `isAsyncSearch` | When to use |
|---------|------|----------------|-------------|
| Fixed dropdown | `select` + `options` | — | Fixed enum: sort order, layout style |
| Load-once dropdown | `select` + `dynamicOptions` | — | Bounded list (up to ~200 items): categories, regions |
| Search-as-you-type | `autocomplete` + `dynamicOptions` | `true` | Unbounded list: 10 000+ courses, users, products |
| Single-fetch typeahead | `autocomplete` + `dynamicOptions` | `false` | Small list with chip UI |

### Allowing Multiple Selections

Add `"multiple": true` to any `select` or `autocomplete` field to allow the user to pick more than one option:

```jsonc
{
  "widgets": [
    {
      "title": "Card Grid",
      "type": "my_company_card_grid",
      "category": "custom",
      "source": {
        "path": "widgets/card-grid",
        "entry": "index.html"
      },
      "configuration": {
        "properties": [
          { // [!code focus:7]
            "name": "tags",
            "type": "autocomplete",
            "label": "Tags",
            "multiple": true,
            "dynamicOptions": { /* endpoint and mapping */ }
          }
        ]
      }
    }
  ]
}
```

### Allowing Editors to Reorder Selections

For a multi-value `autocomplete` field, add `"sortable": true` to let editors
drag selected items into the order your widget should use:

```jsonc
{
  "name": "featured_courses",
  "type": "autocomplete",
  "label": "Featured Courses",
  "multiple": true,
  "sortable": true,
  "dynamicOptions": { /* endpoint and mapping */ }
}
```

`sortable` is only valid when `type` is `"autocomplete"` and `multiple` is
`true`. The selected values are saved and returned by `sdk.getProps()` in the
editor's chosen order. For content-backed widgets, the platform also preserves
this order when it forwards the array to the content endpoint.

### Load-Once Dropdown

```jsonc
{
  "widgets": [
    {
      "title": "Card Grid",
      "type": "my_company_card_grid",
      "category": "custom",
      "source": {
        "path": "widgets/card-grid",
        "entry": "index.html"
      },
      "configuration": {
        "properties": [
          { // [!code focus:15]
            "name": "category",
            "type": "select",
            "label": "Category",
            "dynamicOptions": {
              "endpoint": {
                "endpoint": "https://api.example.com/categories",
                "method": "GET"
              },
              "mapping": {
                "valueKey": "id",
                "labelKey": "name"
              }
            }
          }
        ]
      }
    }
  ]
}
```

### Search-as-You-Type Multi-Select

```jsonc
{
  "widgets": [
    {
      "title": "Card Grid",
      "type": "my_company_card_grid",
      "category": "custom",
      "source": {
        "path": "widgets/card-grid",
        "entry": "index.html"
      },
      "configuration": {
        "properties": [
          { // [!code focus:23]
            "name": "courses",
            "type": "autocomplete",
            "label": "Courses",
            "multiple": true,
            "sortable": true,
            "rules": { "required": true },
            "dynamicOptions": {
              "endpoint": {
                "endpoint": "https://api.skilljar.com/v1/courses?q={q}",
                "method": "GET"
              },
              "valuesEndpoint": {
                "endpoint": "https://api.skilljar.com/v1/courses?ids={value}",
                "method": "GET"
              },
              "mapping": {
                "path": "results",
                "valueKey": "id",
                "labelKey": "title"
              },
              "isAsyncSearch": true
            }
          }
        ]
      }
    }
  ]
}
```

The `valuesEndpoint` is optional but recommended: when the configurator reloads, it calls `valuesEndpoint` with the stored IDs to display fresh labels. Without it, the cached label is shown until the user searches again.

For the full `dynamicOptions` field schema and runtime behavior, see the [Widget Definition Reference](widget-schema#dynamic-options).

## Read Configuration in Your Widget

Call `sdk.getProps()` to read the current configuration values. Each key in the returned object corresponds to a property `name` from `configuration.properties`.

### Reading props

```javascript
export async function init(sdk) {
  await sdk.whenReady()

  const props = sdk.getProps()
  console.log(props.cards_per_row) // 3
}
```

### Reacting to changes

When an editor updates configuration in the No-Code Builder, the SDK emits a `propsChanged` event. Listen for it to keep your widget in sync:

```javascript
export async function init(sdk) {
  await sdk.whenReady()

  const props = sdk.getProps()
  render(props)

  sdk.on('propsChanged', (newProps) => {
    render(newProps)
  })
}
```

### Applying props to styles

Use configuration values to set CSS custom properties on your widget, then reference them in your styles:

```javascript
function applyStyles(sdk, props) {
  const host = sdk.getContainer().host
  host.style.setProperty('--w-cards-per-row', props.cards_per_row)
  host.style.setProperty('--w-card-bg', props.card_background)
}
```

```css
/* --w- prefix: widget-private custom property, not a design token */
.card-grid {
  display: grid;
  grid-template-columns: repeat(var(--w-cards-per-row, 3), 1fr);
  gap: 20px;
  background: var(--w-card-bg, #ffffff);
}
```

## Example

A "Card Grid" widget defines a number (`cards_per_row`), colors (`card_background`, `card_text_color`), and a select (`show_icons` with options "yes" / "no"). In the registry you add those to `configuration.properties` and `defaultConfig`. In your widget code you read them with `sdk.getProps()` and use the values to build the grid, set colors, and conditionally show icons. When an editor changes "Cards Per Row" to 4 or picks "No" for "Show Icons", the `propsChanged` event fires and your widget updates.

## Troubleshooting

### The configuration form does not appear

* Ensure **settings.configurable** is `true` for the widget.
* Ensure the widget has a `configuration` object with a non-empty `properties` array.

### Props are empty or undefined

* Call `sdk.getProps()` after `await sdk.whenReady()` — the SDK must be initialized first.
* Ensure the property has a key in `defaultConfig` so a default is always available.
* Ensure the property names in your code match the `name` in `configuration.properties` exactly (case-sensitive).

## Next Steps

* [Widget Runtime](core-concepts#props) — SDK API reference for `getProps()` and `propsChanged`
* [Using React](using-react#working-with-props) — React-specific patterns for reactive props
* [Widget Definition Reference](widget-schema) — All field types, rules, and configuration structure
* [Repository Layout](project-setup) — Organizing your widget repository
