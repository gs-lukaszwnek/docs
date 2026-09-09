---
url: https://developer-portal.gainsight.com/docs/webhooks/gamification-events.md
---
# Gamification Events

## Available gamification events

* [Yip.UserBadgeEarned](#yip-userbadgeearned)

## Event payloads

### Yip.UserBadgeEarned

```json
{
    "userId": "2000",
    "newBadgeName": "Badge Name",
    "newBadgeImage": "badge-image.jpg",
    "newBadgeId": "40",
    "badgeList": {
        "Badge Name": "badge-image.jpg",
        "Another Badge Name": "another-badge-image.jpg"
    },
    "badgeIdList": [
        "40",
        "41"
    ]
}
```

* `userId` identifies the user who earned a badge
* `newBadgeId` identifies the badge that was earned by the user
* `badgeList` a list of badges earned by the user including the newly earned badge
* `badgeIdList` a list of badge identifiers referencing all badges that were earned by the user
