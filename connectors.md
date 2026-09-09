---
url: https://developer-portal.gainsight.com/docs/connectors.md
description: >-
  Overview of connectors — configure secure server-side proxies to call external
  APIs from widget code
---

# Connectors

Connectors let your widgets talk to external services — like Salesforce, weather APIs, or any HTTP endpoint — without exposing credentials in the browser. You configure a connector once, and any widget can call it securely. They are commonly used inside [Extensions](/custom-widgets/v2/) to fetch or send data from widget code via the [SDK](/sdk/).

New to connectors? Start with [Build Your First Connector](build-first-connector).

## Why use connectors

Connectors keep API keys and tokens out of the browser — requests run on the backend, so secrets stay on the server. They also let you configure a call once, such as a CRM lookup or payment integration, and reuse it across every widget in your community.

## Guides

### Connectors

| Guide | Description |
|-------|-------------|
| [How Connectors Work](how-connectors-work) | Detailed request and response pipeline |

### Setup

| Guide | Description |
|-------|-------------|
| [Build Your First Connector](build-first-connector) | Hands-on tutorial to create and call a connector |
| [Configuration](configuration) | Required and optional fields, naming, URL rules |
| [Choose an Authentication Type](choose-authentication) | Decision guide for picking the right authentication type |
| [Authentication](authentication) | API Key, OAuth Client Credentials, None |
| [Secrets and Variables](secrets/) | Store and manage credentials |

### Request & Response

| Guide | Description |
|-------|-------------|
| [Headers & Query Parameters](headers-and-query-parameters) | Headers, query params, overridable fields |
| [Dynamic URL Path Segments](dynamic-url-paths) | Caller-supplied values as path segments in the URL, with URL-safe validation |
| [Request Transformation](payload-template) | Reshape request bodies with [Jinja2](https://jinja.palletsprojects.com/) |
| [Response Transformation](response-transformation) | Transform upstream API responses with [Jinja2](https://jinja.palletsprojects.com/) |
| [Template Variables](template-variables) | Variables, filters, and functions reference |
| [HTTP Caching](http-caching) | Automatic response caching |

### Using Connectors

| Guide | Description |
|-------|-------------|
| [Calling from Widget Code](calling-from-widgets) | Call connectors from widget code |
| [Passing User Context](passing-user-context) | Securely inject user identity into connector requests |
| [Filtering Sensitive Data](filtering-sensitive-data) | Strip hidden or private records from responses before they reach the browser |
| [Testing & Debugging](testing-and-debugging) | Test connections, performance headers, troubleshooting |

### Composite Connectors

| Guide | Description |
|-------|-------------|
| [Build a Composite Connector](composite-connectors) | Chain multiple API calls into a single execution |
| [Composite Connector Reference](composite-connector-reference) | Step fields, template variables, and error types |

### Managing as Code

| Guide | Description |
|-------|-------------|
| [Code Mode](code-mode) | Define connectors using JSON |
| [Repository Registry](repository-registry) | Define connectors in your GitHub repository |
