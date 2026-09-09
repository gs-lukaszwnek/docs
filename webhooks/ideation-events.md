---
url: https://developer-portal.gainsight.com/docs/webhooks/ideation-events.md
---
# Ideation Events

## Available ideation events

* [ideation.StatusChanged](#ideation-statuschanged)

## Event payloads

### ideation.StatusChanged

```json
{
    "topicId": "123",
    "topicType": "conversation",
    "categoryId": "15",
    "oldStatusId": "1",
    "oldStatusName": "In Progress",
    "newStatusId": "2",
    "newStatusName": "Accepted",
    "changedBy": "2001"
}
```

* `topicId` identifies the topic by ID
* `topicType` identifies the type of the topic (article/conversation/question)
* `categoryId` identifies the category the ideation topic is posted in
* `oldStatusId` identifies the previous status by ID
* `oldStatusName` is the textual representation of the *previous* status
* `newStatusId` identifies the current status by ID
* `newStatusName` is the textual representation of the *current* status
* `changedBy` identifies the user that changed the ideation status
