---
url: >-
  https://developer-portal.gainsight.com/docs/custom-widgets/v2/page-targeting.md
description: >-
  Control when global scripts and stylesheets load (not widgets) — target
  specific pages, locales, and user states via condition rules
---

# Page Targeting

Condition rules control which pages your scripts and stylesheets load on. Each rule matches against a **context object** that the platform builds automatically for each page request.

When multiple rules are specified, **all must match** (AND logic) for the asset to be included. Assets without rules (or with an empty array) load on every page.

## Rule Structure

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `field` | string | Yes | The context field to match against. One of: `page`, `pageType`, `slug`, `locale`, `authenticated`. |
| `operator` | string | Yes | Comparison operator: `eq`, `neq`, `in`, `not_in`. |
| `value` | string or array | Yes | Value(s) to compare. Use a string with `eq`/`neq`, an array with `in`/`not_in`. |

## Operators

| Operator | Description |
|----------|-------------|
| `eq` | Field equals value |
| `neq` | Field does not equal value |
| `in` | Field matches one of the values (array) |
| `not_in` | Field does not match any of the values (array) |

## Context Fields

The context object is built by the platform for each page request. Not all fields are present on every page — rules targeting an absent field will not match.

### `page`

The internal page identifier within the Customer Community. Only present on customizable and overview pages.

| Value | Customer Community Page |
|-------|------------------------|
| `homepage` | Homepage / Hub homepage |
| `community` | Community overview |
| `ideation` | Ideation overview |
| `productUpdates` | Product updates overview |
| `eventOverview` | Events overview |
| `knowledgeBase` | Knowledge base overview |
| `category/{id}` | Specific category page |
| `customPage/{id}` | Specific custom page |
| `educationHome` | Education home |
| `educationDashboard` | Education dashboard |
| `educationCatalog` | Education catalog |
| `educationLearningPaths` | Education learning paths |
| `educationTrainingEvents` | Education training events |

::: tip Page IDs with dynamic segments
For `category` and `customPage`, the `page` field always includes the ID suffix (e.g., `category/123`, `customPage/456`). To match **all** categories or custom pages regardless of ID, use the [`pageType`](#pagetype) field instead.
:::

### `pageType`

Only present for `category` and `customPage` pages in the Customer Community. Contains the page name without the ID suffix — useful for matching all pages of a given type.

| Value | Matches |
|-------|---------|
| `category` | All category pages |
| `customPage` | All custom pages |

### `slug`

Only present on custom pages in the Customer Community. Contains the URL slug of the custom page.

**Example values:** `getting-started`, `faq`, `release-notes`

### `locale`

The current Customer Community language in BCP 47 format.

**Example values:** `en-US`, `nl-NL`, `de-DE`, `fr-FR`, `es-ES`, `pt-BR`, `ja-JP`

### `authenticated`

Whether the current user is logged in to the Customer Community.

| Value | Description |
|-------|-------------|
| `"true"` | User is authenticated |
| `"false"` | User is a guest |

## Examples

### Load stylesheet only on the homepage

```json
{
  "name": "homepage-styles",
  "path": "stylesheets/homepage/style.css",
  "rules": [{ "field": "page", "operator": "eq", "value": "homepage" }]
}
```

### Load script on ideation and product updates pages

```json
{
  "name": "feedback-tracker",
  "path": "scripts/feedback/script.js",
  "rules": [
    { "field": "page", "operator": "in", "value": ["ideation", "productUpdates"] }
  ]
}
```

### Load script only for authenticated users

```json
{
  "name": "user-analytics",
  "path": "scripts/analytics/script.js",
  "rules": [{ "field": "authenticated", "operator": "eq", "value": "true" }]
}
```

### Load stylesheet for Dutch locale on the knowledge base

Multiple rules — all must match (AND logic):

```json
{
  "name": "kb-nl-styles",
  "path": "stylesheets/kb-nl/style.css",
  "rules": [
    { "field": "page", "operator": "eq", "value": "knowledgeBase" },
    { "field": "locale", "operator": "eq", "value": "nl-NL" }
  ]
}
```

### Exclude script from education pages

```json
{
  "name": "non-education-script",
  "path": "scripts/main/script.js",
  "rules": [
    {
      "field": "page",
      "operator": "not_in",
      "value": [
        "educationHome",
        "educationDashboard",
        "educationCatalog",
        "educationLearningPaths",
        "educationTrainingEvents"
      ]
    }
  ]
}
```

### Load stylesheet on a specific category page

```json
{
  "name": "category-123-styles",
  "path": "stylesheets/category-123/style.css",
  "rules": [{ "field": "page", "operator": "eq", "value": "category/123" }]
}
```

### Load stylesheet on all category pages

Use `pageType` to match regardless of the specific category ID:

```json
{
  "name": "all-categories-styles",
  "path": "stylesheets/categories/style.css",
  "rules": [{ "field": "pageType", "operator": "eq", "value": "category" }]
}
```

### Load script on a specific custom page by slug

```json
{
  "name": "faq-script",
  "path": "scripts/faq/script.js",
  "rules": [{ "field": "slug", "operator": "eq", "value": "faq" }]
}
```

### Load stylesheet on multiple custom pages by slug

```json
{
  "name": "onboarding-styles",
  "path": "stylesheets/onboarding/style.css",
  "rules": [
    {
      "field": "slug",
      "operator": "in",
      "value": ["getting-started", "faq", "release-notes"]
    }
  ]
}
```

### Load script on all custom pages

```json
{
  "name": "all-custom-pages-script",
  "path": "scripts/custom-pages/script.js",
  "rules": [{ "field": "pageType", "operator": "eq", "value": "customPage" }]
}
```

### No rules — load everywhere

Assets without a `rules` array (or with an empty array) load on every page:

```json
{
  "name": "global-styles",
  "path": "stylesheets/global/style.css"
}
```

## Next Steps

* [Script Definition Reference](scripts) — every field and HTML attribute for script entries
* [Stylesheet Definition Reference](stylesheets) — every field and HTML attribute for stylesheet entries
* [Registry Reference](registry-reference) — the root of `extensions_registry.json`
