---
url: https://developer-portal.gainsight.com/docs/custom-widgets/v2/widget-schema.md
description: >-
  Reference for widget entries in the widgets array of extensions_registry.json
  — fields, types, defaults, and validation rules
---

# Widget Definition Reference

This page documents every field of a widget entry in the `widgets` array of `extensions_registry.json`. For the root of the registry file, see [Registry Reference](registry-reference).

::: tip Working example
The [template repository](https://github.com/gainsight-hub/widgets-repository-template) contains a complete [`extensions_registry.json`](https://github.com/gainsight-hub/widgets-repository-template/blob/main/extensions_registry.json) with example widgets, a global script, and a stylesheet — a useful reference alongside this documentation.
:::

## Widget Object

| Field | Type | Required | Default | Description |
|-------|------|----------|---------|-------------|
| `title` | string | Yes | — | Display name shown to users |
| `type` | string | Yes | — | Unique widget identifier — must be unique across your community |
| `category` | string | Yes | — | Free-form category label, e.g. `"engagement"`, `"analytics"` |
| `content` | object | Conditional | — | External hosting — see [Content Object](#content-object). Required if `source` is not set. |
| `source` | object | Conditional | — | Repository hosting — see [Source Block](#source-block). Required if `content` is not set. |
| `version` | string | No | `"1.0.0"` | Widget version following semver |
| `description` | string | No | `""` | Brief description of the widget |
| `containers` | array of strings | No | `["Full width"]` | Page zones where the widget can be placed. Values: `"Full width"`, `"Left container"`, `"Sidebar"` |
| `widgetsLibrary` | boolean | No | `true` | When `true`, the widget appears in the No-Code Builder library |
| `imageName` | string | No | `"banner"` | Built-in thumbnail identifier — see [Thumbnails](#thumbnails). Ignored when `imageSrc` is set. |
| `imageSrc` | string | No | — | Custom thumbnail — absolute URL or relative repository path. See [Thumbnails](#thumbnails). Mutually exclusive with `imageName`. |
| `settings` | object | No | `shared` `false`, `container` `false`, others `true` | Widget behavior settings — see [Settings Object](#settings-object) |
| `configuration` | object | No | — | Form fields for the No-Code Builder — see [Configuration Object](#configuration-object) |
| `defaultConfig` | object | No | — | Default values for configuration properties |

> **XOR Requirement:** Each widget must have exactly one of `content` or `source` — never both, never neither.

> **Important: The `type` field is the widget's unique identifier.** Use a descriptive, namespaced format like `mycompany_welcome_banner` to avoid conflicts with other widgets.

### Settings Object

| Field | Type | Required | Default | Description |
|-------|------|----------|---------|-------------|
| `configurable` | boolean | No | `true` | Whether users can configure the widget after adding it |
| `editable` | boolean | No | `true` | Whether the widget content can be edited |
| `removable` | boolean | No | `true` | Whether users can remove the widget from pages |
| `shared` | boolean | No | `false` | When `true`, one widget instance is shared across every page it appears on — edits propagate everywhere. Opt in explicitly; the default keeps the widget local to each page. |
| `movable` | boolean | No | `true` | Whether users can reposition the widget |
| `container` | boolean | No | `false` | Whether the widget acts as a container for other widgets |

An empty `settings: {}` object is valid — all fields take their defaults.

### Content Object

| Field | Type | Required | Default | Description |
|-------|------|----------|---------|-------------|
| `endpoint` | string | Yes | — | Public URL to widget content, must start with `https://` |
| `method` | string | No | `"GET"` | HTTP method — `"GET"` or `"POST"` |
| `cacheStrategy` | string | No | — | Caching behavior — `"none"`, `"no-cache"`, `"ttl"`, or `"permanent"` |
| `cacheTtlSeconds` | integer | No | — | Cache duration in seconds — use with `cacheStrategy: "ttl"` |
| `requiresAuthentication` | boolean | No | `false` | Not supported — leave `false` or omit |
| `headers` | object | No | — | Optional headers sent with the request |
| `body` | object | No | — | Optional request body, for `POST`. This declarative `body` is the platform-side request body for content fetches — distinct from the SDK `widgetServiceSdk.connectors.execute({ payload })` client option. See [Widget SDK Methods and Constructors](/sdk/widget-sdk/methods-constructors). |
| `params` | object | No | — | Optional query parameters |

### Source Block

| Field | Type | Required | Default | Description |
|-------|------|----------|---------|-------------|
| `path` | string | Yes | — | Directory path relative to repository root |
| `entry` | string | Yes | — | HTML entry point file relative to `path` |

### Configuration Object

| Field | Type | Required | Default | Description |
|-------|------|----------|---------|-------------|
| `properties` | array of objects | Yes | — | Form fields — see [ConfigField Object](#configfield-object) |
| `sections` | array of objects | No | — | Groups that organize fields into collapsible sections — see [ConfigSection Object](#configsection-object) |

#### ConfigField Object

| Field | Type | Required | Default | Description |
|-------|------|----------|---------|-------------|
| `name` | string | Yes | — | Field identifier, used as the configuration key |
| `label` | string | Yes | — | Field label shown in the No-Code Builder |
| `type` | string | Yes | — | Field type — `"text"`, `"number"`, `"color"`, `"date"`, `"boolean"`, `"select"`, or `"autocomplete"` |
| `description` | string | No | — | Help text shown under the field |
| `section` | string | No | — | `name` of the [section](#configsection-object) this field is grouped under. Presentation only — values are always stored flat, keyed by `name`. Fields without a `section` fall into the default group. |
| `defaultValue` | any | No | — | Pre-filled value |
| `rules` | object | No | — | Validation rules — see [FieldRules Object](#fieldrules-object) |
| `options` | array | No | — | Static choices for `"select"` — see [SelectOption Object](#selectoption-object). Mutually exclusive with `dynamicOptions`. |
| `dynamicOptions` | object | No | — | API-driven choices for `"select"` or `"autocomplete"` — see [Dynamic Options](#dynamic-options). Mutually exclusive with `options`. |
| `multiple` | boolean | No | `false` | Allow selecting more than one value. Only for `"select"` or `"autocomplete"`. |
| `sortable` | boolean | No | `false` | Let editors reorder selected values. Only valid for `"autocomplete"` fields with `multiple: true`; array order is preserved at runtime. |

##### Field Types

| `type` | Renders as | Notes |
|--------|-----------|-------|
| `text` | Single-line text input | — |
| `number` | Numeric input | Respects `rules.minimum` / `rules.maximum` |
| `color` | Color picker | — |
| `date` | Date picker | — |
| `boolean` | On/off toggle | — |
| `select` | Dropdown | Requires `options` OR `dynamicOptions` (see [Dynamic Options](#dynamic-options)), not both |
| `autocomplete` | Searchable field | Requires `dynamicOptions`; supports `multiple` and `sortable` |

##### FieldRules Object

| Field | Type | Required | Default | Description |
|-------|------|----------|---------|-------------|
| `required` | boolean | No | — | Whether the field must be filled in |
| `minLength` | integer | No | — | Minimum string length |
| `maxLength` | integer | No | — | Maximum string length |
| `pattern` | string | No | — | Regex the value must match |
| `minimum` | number | No | — | Minimum numeric value |
| `maximum` | number | No | — | Maximum numeric value |

##### SelectOption Object

| Field | Type | Required | Default | Description |
|-------|------|----------|---------|-------------|
| `value` | string | Yes | — | Value stored when the option is selected |
| `label` | string | Yes | — | Label shown in the dropdown |

#### ConfigSection Object

An entry in the `sections` array. A field joins a section by setting its `section` to the section's `name`. Listing a section here is optional — it sets the section's label, description, and default-open state.

| Field | Type | Required | Default | Description |
|-------|------|----------|---------|-------------|
| `name` | string | Yes | — | Section identifier, referenced by a field's `section` |
| `label` | string | No | `name` | Title shown on the section header |
| `description` | string | No | — | Help text shown under the section header |
| `expanded` | boolean | No | `false` | When `true`, this section is the one open when the form first loads |

##### Section Ordering and Rendering

* A section renders only when at least one field references it. A section listed in `sections` with no fields is not shown.
* Fields without a `section` fall into a default group, labeled **Configuration**, shown first.
* After the default group, sections appear in the order they are listed in `sections`, followed by any sections referenced only by a field — those in the order their first field appears.
* The form shows one section open at a time. On load, the first `sections` entry marked `expanded` is opened; if none is marked, the first section is opened.
* A field's `section` is presentation only. Configuration values are always stored flat, keyed by field `name`, so you can rename, reorder, or remove sections without affecting saved values.

## Dynamic Options

The `dynamicOptions` object populates a `select` or `autocomplete` field's choices from an external API instead of a static `options` list.

### `dynamicOptions` Object

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `endpoint` | object | Yes | API endpoint to fetch the list of options — see [API Endpoint Object](#api-endpoint-object) |
| `valuesEndpoint` | object | No | API endpoint to resolve stored IDs back to fresh labels — see [API Endpoint Object](#api-endpoint-object) |
| `mapping` | object | Yes | How to extract `value` and `label` from the API response — see [Options Mapping Object](#options-mapping-object) |
| `isAsyncSearch` | boolean | No | When `true`, the API is called on every keystroke. Only valid on `autocomplete` fields. |

### API Endpoint Object

:::info Naming
The field on `dynamicOptions` is also named `endpoint` — the URL string lives one level deeper inside it: `dynamicOptions.endpoint.endpoint`.
:::

Used for both `endpoint` and `valuesEndpoint`.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `endpoint` | string | Yes | HTTPS URL for the API call. Supports [URL template tokens](#url-template-tokens). Must start with `https://`. |
| `method` | string | Yes | HTTP method — `"GET"` or `"POST"` |
| `headers` | object | No | Additional HTTP headers. See [Disallowed Headers](#disallowed-headers). |
| `body` | object | No | Request body, for `POST` requests. This declarative `body` is the platform-side request body for dynamic-options fetches — distinct from the SDK `widgetServiceSdk.connectors.execute({ payload })` client option. See [Widget SDK Methods and Constructors](/sdk/widget-sdk/methods-constructors). |
| `params` | object | No | Query parameters appended to the URL after template interpolation. |

#### URL Template Tokens

Embed these tokens in `endpoint` URLs to make requests dynamic:

| Token | Replaced with | Used in |
|-------|--------------|---------|
| `{q}` | Current search query (URL-encoded) | `endpoint` on `autocomplete` fields with `isAsyncSearch: true` |
| `{value}` | Stored IDs, comma-joined; each ID is individually URL-encoded | `valuesEndpoint` |

Example: `"https://api.example.com/courses?q={q}"` sends the current search term as a query parameter.

#### Disallowed Headers

The following headers cannot be declared in `headers` (case-insensitive): `Authorization`, `Cookie`, `Set-Cookie`, `Proxy-*`, `X-Forwarded-*`. The registry is not the right place to store credentials — use a [Connector](/connectors/) for authenticated external API calls.

#### `params` Serialization

Entries in `params` are appended to the URL as a query string after template interpolation:

* Strings, numbers, and booleans are converted to strings.
* Arrays become repeated entries: `tag=a&tag=b`.
* Objects are JSON-stringified.
* `null` and `undefined` values are skipped.

Example: `{ "page": 1, "tag": ["a", "b"] }` → `?page=1&tag=a&tag=b`

### Options Mapping Object

Maps an API response array to `{ value, label }` pairs.

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `path` | string | No | Dot-path to the array within the response (e.g. `"data.items"`). Defaults to the response root. |
| `valueKey` | string | Yes | Property name to use as the stored value (e.g. `"id"`). |
| `labelKey` | string | Yes | Property name to use as the display label (e.g. `"name"`). |

`path` accepts dot-paths like `"data.items"`. A leading `$.` is stripped for JSONPath compatibility; filters and wildcards are not supported.

### Content Endpoint Wire Format

When the platform forwards saved widget configuration to your content endpoint, dynamic selection values are normalized:

| Saved shape | Sent as (GET) | Sent as (POST) |
|------------|--------------|---------------|
| Single value | `?field=id` | `{"field": "id"}` |
| Multiple values | `?field=id1,id2` | `{"field": ["id1", "id2"]}` |
| Empty selection | `?field=` | `{"field": []}` |

The `cachedLabel` is never sent to the content endpoint. If your widget needs a display name, fetch it from your API using the ID.

For a field with `multiple: true` and `sortable: true`, the multiple-value order
matches the order chosen in the No-Code Builder. The same ordered array is
available through `sdk.getProps()`.

### Limitations

* **Authentication** — `dynamicOptions` endpoints must be publicly accessible. Per-request auth headers are not supported; use a [Connector](/connectors/) for endpoints that require credentials.
* **Caching** — the platform does not cache `dynamicOptions` responses. For large datasets, prefer `isAsyncSearch: true` to limit the volume of results returned.
* **Pagination** — not supported. Your endpoint should return a usable page size or all items for bounded lists.
* **Cascading fields** — one field's selection cannot drive another field's endpoint. Each field's `endpoint` is fixed at registration time.
* **Response shape** — only flat dot-paths and `valueKey`/`labelKey` lookups are supported. Filters, computed fields, and wildcard paths are not.

## Thumbnails

A thumbnail is the preview image shown for a widget in the No-Code Builder library. You have three ways to set one, controlled by two fields on the widget object:

* **Built-in** — set `imageName` to one of the built-in identifiers.
* **External URL** — set `imageSrc` to a publicly accessible image URL.
* **Repository-hosted** — set `imageSrc` to a relative path to an image file in your repository.

`imageName` and `imageSrc` are mutually exclusive — if both are set, `imageSrc` wins. If neither is set, the default `imageName: "banner"` applies.

Custom thumbnails (external or repository-hosted) must be **512 KB or smaller**. For best display, use **420×145 pixels** — dimensions are not enforced.

### Built-in (`imageName`)

Set `imageName` to one of the identifiers below. Each preview shows the thumbnail at a reduced size.

| imageName | Preview |
|-----------|---------|
| `announcement_card` |  |
| `autopilot` |  |
| `autopilot_theloops` |  |
| `badges` |  |
| `banner` |  |
| `categories` |  |
| `community_statistics` |  |
| `container-2.1` |  |
| `create_idea` |  |
| `customer_education_widget` |  |
| `dynamic_content` |  |
| `events_calendar` |  |
| `featured_topics` |  |
| `groups` |  |
| `hero_banner` |  |
| `html_widget` |  |
| `ideation_pipeline` |  |
| `introduction_bar` |  |
| `leaderboard` |  |
| `most_liked` |  |
| `quicklinks` |  |
| `recommendations` |  |
| `solved_topics` |  |
| `statistics` |  |
| `tabs` |  |
| `tag_cloud` |  |

Example: `"imageName": "banner"` uses the built-in banner thumbnail.

### External URL (`imageSrc`)

Point to any publicly accessible HTTPS image:

```json
"imageSrc": "https://example.com/images/my-widget-preview.png"
```

### Repository-hosted (`imageSrc`)

Store the image in your repository and reference it with a relative path from the repo root:

```json
"imageSrc": "./images/preview.png"
```

Both `./images/preview.png` and `images/preview.png` are valid. During publishing, the platform fetches the image from your repository, publishes it, and rewrites the path to an absolute URL. The resulting registry always contains absolute URLs.

Path rules:

* No path traversal (`../` is not allowed)
* No absolute paths (must not start with `/`)
* The image must exist in the repository at the referenced path

## Validation Rules

The schema enforces these rules:

1. **Root object** must have a `widgets` property (required)
2. **`widgets`** must be an array (may be empty)
3. **Each widget** must include all required fields
4. **`attributes`** on scripts and stylesheets must be an object with string values

:::tip Validation errors
For a full list of validation and build errors, including causes and fixes, see [Error Codes](error-codes).
:::

## Best Practices

1. **Use globally unique `type` values**: The `type` is the widget ID and must be unique across your entire community. Use a prefix like your company or project name (e.g., `acme_welcome_banner`)
2. **Keep descriptions concise**: Brief but descriptive
3. **Set `widgetsLibrary: true`** to make widgets appear in the No-Code Builder library
4. **Use semantic versioning**: Follow semver (major.minor.patch)
5. **Validate before pushing**: Use a JSON validator to catch syntax errors

## Next Steps

* [Hosting Widgets](hosting-widgets) — how to choose between external and repository hosting
* [Configurable Widgets](configurable-widgets) — how to add form fields for the No-Code Builder
* [Registry Reference](registry-reference) — the root of `extensions_registry.json`
* [Script Definition Reference](scripts) — global script entries
* [Stylesheet Definition Reference](stylesheets) — global stylesheet entries
* [Error Codes](error-codes) — reference for validation failures

- Script/stylesheet "attributes" is an optional object of HTML attributes (e.g., {"defer": "", "crossorigin": "anonymous"}) added to the generated tag; use empty string for boolean attributes
