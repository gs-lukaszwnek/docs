---
url: >-
  https://developer-portal.gainsight.com/docs/webhooks/content-moderation-events.md
---
# Content Moderation Events

For more information about The Digital Services Act Required Moderation, see [Notify authors of moderation actions](https://communities.gainsight.com/gdpr-dsa-privacy-29/notify-authors-of-moderation-actions-27704).

## Available content moderation events

* [contentModeration.authorNotificationSubmitted](#contentmoderation-authornotificationsubmitted)

## Event payloads

### contentModeration.authorNotificationSubmitted

Triggered when a topic or reply is modified by a moderator and the "Notify Author" action is taken. This event enables user notifications in compliance with the Digital Services Act (DSA), ensuring transparency about content moderation decisions.

```json
{
    "moderationReason": "Privacy Violation",
    "moderationDescription": "Privacy violation occurs when an individual's personal information is accessed, used, or disclosed without consent, leading to potential harm, embarrassment, or unauthorized surveillance. It breaches legal and ethical boundaries.<br/><br/>The moderation team.",
    "notifiedAt": "2025-06-18T13:14:42+00:00",
    "id": 5,
    "publicId": 33,
    "createdAt": "2025-06-18T09:00:00+00:00",
    "contentType": "conversation",
    "title": "Test topic",
    "url": "https://insided.com/topic/show?tid=33",
    "replyId": 2,
    "replyPublicId": 46,
    "repliedAt": "2025-06-18T11:00:00+00:00",
    "authorId": 2016,
    "authorUsername": "BitterSteel",
    "authorAvatar": "https://uploads-eu-west-1.almostinsided.com/sud-en/icon/200x200/8dfcfece-f020-4769-9fd7-8aa8eb76a26d.png",
    "notificationRecipients": [
        "BitterSteel@gmail.com"
    ],
    "changedBy": "1",
    "changedByUsername": "ModeratorUser",
    "contentAfterChange": "<p>Please contact me. My phone number is 123123123</p>",
    "contentBeforeChange": "<p>Please contact me. Phone number has been removed due to a privacy violation</p>",
    "reply_permalink": "https://community.example.com/product-updates/test-topic-33?postid=46#post46"
}
```

* `moderationReason` Reason why the content was moderated
* `moderationDescription` A detailed explanation of the moderation reason
* `notifiedAt` When the notification was sent
* `id` Private Id of the topic
* `publicId` Public Id of the topic
* `createdAt` When the topic was created.
* `contentType` Content Type
* `title` Title of the topic
* `url` Link to the moderated content
* `replyId` Private Id of the reply (if moderated content is a reply)
* `replyPublicId` Public Id of the reply (if moderated content is a reply)
* `repliedAt` When the reply was created (if moderated content is a reply)
* `authorId` Id of the user who posted the original content
* `authorUsername` Username of the author
* `authorAvatar` URL to the author's avatar image
* `notificationRecipients` List of email addresses that were notified about the moderation
* `changedBy` User Id of the moderator who performed the action
* `changedByUsername` Username of the moderator who performed the action
* `contentAfterChange` The content after moderation
* `contentBeforeChange` The original content before it was moderated
