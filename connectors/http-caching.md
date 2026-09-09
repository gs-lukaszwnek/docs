---
url: https://developer-portal.gainsight.com/docs/connectors/http-caching.md
description: >-
  How the platform automatically caches connector responses using standard HTTP
  caching headers — no configuration required
---

# HTTP Caching

HTTP caching reduces widget load times and lowers the number of calls to your external APIs. When an API response includes standard caching headers, the platform automatically stores and reuses that response for subsequent requests — no additional configuration needed on your part.

## How caching works

* Responses are cached when the external API returns cacheable headers
* The system respects `Cache-Control`, `Expires`, `ETag`, and `Last-Modified`
* Caching is automatic and requires no additional configuration

## What gets cached

* **Methods**: `GET` and `HEAD`
* **Status codes**: `200`, `301`, `308`
* **Per-connector scope**: each connector has its own cache namespace

## Cacheable request example

```json
{
  "name": "Weather API",
  "url": "https://api.weather.com/current",
  "method": "GET",
  "query_parameters": [
    {
      "key": "location",
      "value": "New York",
      "overridable": true
    }
  ]
}
```

## Non-cacheable request example

```json
{
  "name": "Create Order",
  "url": "https://api.store.com/orders",
  "method": "POST",
  "request_body": "{\"item\": \"{{ (body_text | from_json).item_id }}\", \"quantity\": 1}"
}
```

## Cache validation and freshness

* **ETag validation** uses `If-None-Match`
* **Last-Modified validation** uses `If-Modified-Since`
* Expiration respects `Cache-Control: max-age` and `Expires`

## Headers that prevent caching

```http
Cache-Control: no-store
Cache-Control: no-cache
Cache-Control: private
```

## Headers that enable caching

```http
Cache-Control: public, max-age=3600
Cache-Control: max-age=86400
Expires: Thu, 01 Dec 2024 16:00:00 GMT
ETag: "123456789"
```

## Best practices for cache-friendly connectors

* Use `GET` for data retrieval
* Keep URLs and query parameters stable
* Prefer APIs that return cache headers

## Next Steps

* [Configuration](configuration) — Full list of connector fields including URL and method
* [Testing & Debugging](testing-and-debugging) — Test your connector and diagnose performance issues
