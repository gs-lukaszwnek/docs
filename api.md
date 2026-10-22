---
url: https://developer-portal.gainsight.com/docs/api.md
description: >-
  REST API reference for Gainsight Customer Communities — endpoints for
  community content, users, events, gamification, and federated search
---

# API Reference

The Gainsight CC API gives you direct access to community data and platform capabilities. Use it to build integrations, automate workflows, or extend the platform beyond what the [SDK](/sdk/) exposes in widget code.

## Operation groups

| Group | Description |
|-------|-------------|
| Community | Create and manage articles, conversations, questions, ideas, and product updates |
| Events | Query and manage community event data |
| Gamification | Read and manage points, badges, and ranks |
| Search | Index external content for federated search |
| User | Read and manage user profiles and memberships |

## Authentication

API requests require authentication. See the [Authentication](/api/api-authentication) section for details on how to obtain and pass credentials.

## Related

* [Web SDK](/sdk/reference#web) — call community APIs from widget or script code with `web` from [`@gainsight-hub/sdk`](/sdk/), without managing auth yourself
* [Connectors SDK](/sdk/reference#connectors) — call [Connectors](/connectors/) from widget code with `connectors` from `@gainsight-hub/sdk` to reach external APIs
* [Connectors](/connectors/) — configure secure proxy endpoints to external services
