---
url: >-
  https://developer-portal.gainsight.com/docs/connectors/response-transformation-recipes.md
description: >-
  Worked Jinja2 examples and common gotchas for reshaping, filtering, and
  resolving data in an upstream API response before it reaches the widget
---

# Response Transformation Recipes

Reshape, filter, or resolve linked data in an upstream API response before it reaches your widget. Use these recipes when a response template does not render the way you expect, or when you need a working pattern for a common transformation.

## Prerequisites

* A connector with a **Response Body** template field — see [Response Transformation](response-transformation)
* Familiarity with Jinja2 template syntax — see [Template Variables](template-variables)

## Response Transform Gotchas

### Parsing JSON responses

`response.body` is a raw UTF-8 string. To parse it, use the `from_json` filter:

```jinja2
{% set data = response.body | from_json %}
{{ data.user.name }}
```

If parsing fails (invalid JSON), the connector returns HTTP 422 with an error.

### Dict access: use subscripts, not attributes

When a dictionary key collides with a built-in dict method name (`items`, `keys`, `values`, `get`, `update`, …), you **must** use **bracket notation** `data['items']`. Dot notation (`data.items`) returns the Python method instead of the key's value. Bracket notation is the safe default for all key access:

```jinja2
{# Correct — subscript access #}
{{ data['items'] }}
{{ data['user']['name'] }}

{# Wrong — returns built-in dict method, not the key value #}
{{ data.items }}    {# Returns the dict.items() method itself! #}
{{ data.keys }}     {# Returns the dict.keys() method! #}
{{ data.get }}      {# Returns the dict.get() method! #}
```

This is especially common when the response has a top-level key named `items`, `keys`, `values`, or `get` (e.g., Contentful's `items` array). Dot notation on dicts returns the built-in Python method, causing errors like:

```
error: object of type 'builtin_function_or_method' has no len()
```

**Always use bracket notation:** `data['items']` instead of `data.items`.

### Accumulating across loops with namespace()

To collect values across a loop you must use `namespace()`. A plain `{% set %}` inside a `{% for %}` is scoped to the loop body and is silently discarded afterwards:

```jinja2
{# Correct — namespace persists across iterations #}
{% set ns = namespace(items=[]) %}
{% for record in data['results'] %}
  {% set ns.items = ns.items + [record['id']] %}
{% endfor %}
{{ ns.items | json_encode }}

{# Correct — accumulate only the rows you want #}
{% set ns = namespace(items=[]) %}
{% for item in data['items'] %}
  {% if item['active'] %}
    {% set ns.items = ns.items + [item] %}
  {% endif %}
{% endfor %}
{{ ns.items | json_encode }}

{# Wrong — `items` is empty after the loop (loop-body scope) #}
{% set items = [] %}
{% for record in data['results'] %}
  {% set items = items + [record['id']] %}
{% endfor %}
{{ items | json_encode }}   {# always renders [] #}
```

::: warning Loop-scoping gotcha
`{% set items = items + [...] %}` inside a `{% for %}` block does **not** update the outer `items` variable — Jinja2 loop bodies have their own scope. The assignment is silently discarded, and `items` is empty after the loop. Always use `{% set ns = namespace(items=[]) %}` and `{% set ns.items = ... %}` to accumulate across iterations.
:::

The environment is sandboxed — you can't reach Python internals such as `data.__class__`. It does **not** block calling mutation methods like `.append()` or `.update()`, but those produce no output on their own, so always build and emit results with `{% set %}` / `namespace()`.

### get\_secret() is not available

Response templates do not have access to {{ get\_secret() }}. Response output is returned to the browser, so secrets are excluded to prevent accidental leakage. If you need secret values in the request to the upstream API, put them in the authentication config, headers, or query parameters instead. See [Secrets and Variables](./secrets/) for details.

## Worked Examples

The examples below were verified by running the exact templates through `ResponseData.transform` against real input. The "Rendered output" block shows what the renderer actually returned.

### Example 1: Reshape a list

Upstream Contentful-style response with nested `sys.id` and `fields.title` — produce a flat array with only the fields the widget needs.

**Input JSON (abbreviated)**

```json
{
  "items": [
    { "sys": { "id": "entry-001" }, "fields": { "title": "Widget Docs" } },
    { "sys": { "id": "entry-002" }, "fields": { "title": "Getting Started" } }
  ]
}
```

**Template**

```jinja2
{% set data = response.body | from_json %}
{% set ns = namespace(out=[]) %}
{% for item in data['items'] %}
{% set ns.out = ns.out + [{"id": item['sys']['id'], "title": item['fields']['title']}] %}
{% endfor %}
{{ ns.out | json_encode }}
```

**Rendered output**

```json
[{"id": "entry-001", "title": "Widget Docs"}, {"id": "entry-002", "title": "Getting Started"}]
```

::: tip
`namespace()` is required here. A plain `{% set out = out + [...] %}` inside a `{% for %}` block is silently discarded — see the [loop-scoping gotcha](#accumulating-across-loops-with-namespace) above.
:::

***

### Example 2: Resolve linked assets by id (Contentful)

The response contains an `items` array where each entry references an asset by id, and a separate `includes.Asset` array holding the full asset data. For each item, attach its image URL by matching on the asset id.

**Input JSON (abbreviated)**

```json
{
  "items": [
    { "sys": { "id": "entry-001" }, "fields": { "title": "First Post", "imageId": "asset-abc" } },
    { "sys": { "id": "entry-002" }, "fields": { "title": "Second Post", "imageId": "asset-def" } }
  ],
  "includes": {
    "Asset": [
      { "id": "asset-abc", "url": "https://images.ctfassets.net/abc.jpg" },
      { "id": "asset-def", "url": "https://images.ctfassets.net/def.jpg" }
    ]
  }
}
```

**Template**

```jinja2
{% set data = response.body | from_json %}
{% set ns = namespace(out=[]) %}
{% for item in data['items'] %}
{% set matching = data['includes']['Asset'] | selectattr('id', 'equalto', item['fields']['imageId']) | list %}
{% set img_url = matching[0]['url'] if matching else '' %}
{% set ns.out = ns.out + [{"id": item['sys']['id'], "title": item['fields']['title'], "imageUrl": img_url}] %}
{% endfor %}
{{ ns.out | json_encode }}
```

**Rendered output**

```json
[{"id": "entry-001", "title": "First Post", "imageUrl": "https://images.ctfassets.net/abc.jpg"}, {"id": "entry-002", "title": "Second Post", "imageUrl": "https://images.ctfassets.net/def.jpg"}]
```

::: tip
`selectattr('id', 'equalto', ...)` is sandbox-safe and avoids building a dictionary lookup manually. The `| list` coerces the lazy iterator so you can index into `matching[0]`.
:::

::: warning
Always use `data['includes']['Asset']` (subscript), not `data.includes.Asset`. The key `items` on the top-level dict and `Asset` nested inside `includes` both require subscript access to avoid shadowing built-in dict methods.
:::

***

### Example 3: Filter a list

Return only items where a boolean field is truthy — strip unpublished records before they reach the widget.

**Input JSON (abbreviated)**

```json
{
  "items": [
    { "id": "a", "title": "Alpha", "published": true },
    { "id": "b", "title": "Beta",  "published": false },
    { "id": "c", "title": "Gamma", "published": true }
  ]
}
```

**Template**

```jinja2
{% set data = response.body | from_json %}
{% set ns = namespace(out=[]) %}
{% for item in data['items'] %}
{% if item['published'] %}
{% set ns.out = ns.out + [{"id": item['id'], "title": item['title']}] %}
{% endif %}
{% endfor %}
{{ ns.out | json_encode }}
```

**Rendered output**

```json
[{"id": "a", "title": "Alpha"}, {"id": "c", "title": "Gamma"}]
```

::: tip
Like the others, this example needs the `namespace()` accumulator (a `{% set %}` directly inside the loop would be discarded). If no items match the `{% if %}`, the result is an empty array `[]` — handle that case in your widget.
:::

## Next Steps

* [Response Transformation](response-transformation) — Reference for the response fields, variables, and error handling
* [Template Variables](template-variables) — Full reference for Jinja2 variables, filters, and functions
