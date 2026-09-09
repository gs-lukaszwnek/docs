---
url: https://developer-portal.gainsight.com/docs/webhooks/user-events.md
---
# User Events

## Available user events

* [IdentityAccess.UserRegistered](#identityaccess-userregistered)
* [IdentityAccess.UserLoggedIn](#identityaccess-userloggedin)
* [integration.UserProfileUpdated](#integration-userprofileupdated)

## Event payloads

### IdentityAccess.UserRegistered

```json
{
    "userId": 12,
    "date": "2019-10-10T16:07:21+02:00"
}
```

* `userId` identifies the user who has registered
* `date` tells exact date and time when registration happened

### IdentityAccess.UserLoggedIn

```json
{
    "userId": 12,
    "date": "2019-10-10T16:07:21+02:00"
}
```

* `userId` identifies the user who has visited community
* `date` tells exact date and time when user logged in via login form or Remember me cookie

### integration.UserProfileUpdated

```json
{
    "userId": "2000",
    "username": "John Doe",
    "profileFieldId": "123",
    "profileField": "Country",
    "value": "Netherlands",
    "oldValue": "UK"
}
```
