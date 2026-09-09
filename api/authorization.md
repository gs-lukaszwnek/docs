---
url: https://developer-portal.gainsight.com/docs/api/authorization.md
description: >-
  How to authorize moderation actions in the Gainsight CC API using a moderator
  ID
---

# Authorization

Certain requests require moderator permissions. This includes moderation actions such as trashing a conversation or moving it to another category.

For these requests, you must also provide a `moderatorId` as a query parameter in the request URL.

## Using a Moderator ID

Add `moderatorId` as a query parameter on any request that requires moderator permissions.

For example, to delete article `123` as moderator `456`:

```
DELETE /articles/123?moderatorId=456
```

## Obtaining a Moderator ID

A moderator ID is the user ID of a user assigned to a privileged role group Moderator or above.
As long as the moderator ID passed in the call is that of a valid user with moderator privileges, the call should authorize properly.
