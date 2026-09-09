---
url: https://developer-portal.gainsight.com/docs/webhooks/product-update-events.md
---
# Product Update Events

## Available product update events

* [productUpdate.Created](#productupdate-created)
* [productUpdate.Published](#productupdate-published)
* [productUpdate.PostContentChanged](#productupdate-postcontentchanged)
* [productUpdate.TitleChanged](#productupdate-titlechanged)
* [productUpdate.ModeratorTagsChanged](#productupdate-moderatortagschanged)
* [productUpdate.Liked](#productupdate-liked)
* [productUpdate.Unliked](#productupdate-unliked)
* [productUpdate.Replied](#productupdate-replied)
* [productUpdate.ReplyPostContentChanged](#productupdate-replypostcontentchanged)
* [productUpdate.ReplyReported](#productupdate-replyreported)
* [productUpdate.ReplyLiked](#productupdate-replyliked)
* [productUpdate.ReplyHighlightChanged](#productupdate-replyhighlightchanged)
* [productUpdate.Trashed](#productupdate-trashed)
* [productUpdate.Restored](#productupdate-restored)
* [productUpdate.PermanentlyDeleted](#productupdate-permanentlydeleted)
* [productUpdate.Moved](#productupdate-moved)
* [productUpdate.ReplyTrashed](#productupdate-replytrashed)
* [productUpdate.ReplyRestored](#productupdate-replyrestored)
* [productUpdate.ReplyPermanentlyDeleted](#productupdate-replypermanentlydeleted)

## Event payloads

### productUpdate.Created

```json
{
    "id": "81",
    "publicId": "1569",
    "authorId": "2008",
    "authorUsername": "Frank",
    "authorAvatar": "",
    "categoryId": "114",
    "categoryName": "Ask your question",
    "title": "Test topic",
    "featuredImage": "",
    "publicLabel": "",
    "content": "Test content",
    "ipAddress": "172.18.123.141",
    "createdAt": "2024-01-15T12:42:59+00:00",
    "tags": [],
    "sticky": false,
    "closed": false,
    "moderatorTags": [],
    "poll": null,
    "publishedAt": null,
    "status": "draft",
    "productAreas": [],
    "topic_permalink": "https://community.example.com/ask-your-question-114/test-topic-1569"
}
```

* `id` identifies the product update that was just created on the community
* `publicId` identifies the product update platform wide
* `authorId` identifies the user creating the product update
* `authorUsername` contains the username of the user at the time of creating
* `authorAvatar` contains an absolute URL or an empty string for the avatar of the user
* `title` contains the title of the product update
* `featuredImage` contains the URL of product update's featured image
* `publicLabel` contains product update's public label
* `content` contains the product update content
* `ipAddress` contains the IP address of the user creating the product update
* `createdAt` contains the time at which the product update was created
* `tags` contains a list of tags as strings
* `sticky` indicates whether the product update was made sticky
* `closed` indicates if the product update is closed for further replies
* `moderatorTags` contains a list of tags added by a moderator (not shown on the community)
* `poll` contains poll information if a poll was created, null otherwise
* `publishedAt` contains the time at which the product update was published
* `status` contains product update status
* `productAreas` contains product update's product areas

### productUpdate.Published

```json
{
    "id": "88",
    "publicId": "1612",
    "categoryId": "114",
    "authorId": "2008",
    "authorUsername": "Frank",
    "authorAvatar": "",
    "title": "Test topic",
    "featuredImage": "",
    "publicLabel": "",
    "content": "Test content",
    "ipAddress": "172.18.123.77",
    "createdAt": "2024-01-15T13:06:31+00:00",
    "tags": [],
    "sticky": false,
    "closed": false,
    "moderatorTags": [],
    "poll": null,
    "publishedAt": null,
    "status": "draft",
    "productAreas": [],
    "topic_permalink": "https://community.example.com/product-updates/test-topic-1612"
}
```

* `id` identifies the product update
* `publicId` identifies the product update platform wide
* `categoryId` identifies the category the product update belongs to
* `authorId` identifies the user creating the product update
* `authorUsername` contains the username of the user at the time of creating
* `authorAvatar` contains an absolute URL or an empty string for the avatar of the user
* `title` contains the title of the product update
* `featuredImage` contains the URL of product update's featured image
* `publicLabel` contains product update's public label
* `content` contains the product update content
* `ipAddress` contains the IP address of the user creating the product update
* `createdAt` contains the time at which the product update was created
* `tags` contains a list of tags as strings
* `sticky` indicates whether the product update was made sticky
* `closed` indicates if the product update is closed for further replies
* `moderatorTags` contains a list of tags added by a moderator (not shown on the community)
* `poll` contains poll information if a poll was created, null otherwise
* `publishedBy` identifies the user published the product update
* `publishedAt` contains the time at which the product update was published
* `status` contains product update status

### productUpdate.PostContentChanged

```json
{
    "productUpdateId": "206",
    "publicId": "1417",
    "categoryId": "114",
    "content": "Test content",
    "changedBy": "3185",
    "changedAt": "2024-01-15T10:30:00+00:00",
    "topic_permalink": "https://community.example.com/product-updates/topic-1417"
}
```

* `productUpdateId` identifies the product update that was just created on the community
* `publicId` identifies the product update platform wide
* `categoryId` identifies the category the product update belongs to
* `content` contains the product update content
* `changedBy` identifies the user changed the product update content
* `changedAt` contains the time at which the product update content was changed (ISO 8601)

### productUpdate.TitleChanged

```json
{
    "productUpdateId": "206",
    "publicId": "1417",
    "categoryId": "114",
    "title": "Updated product update title",
    "changedBy": "3185",
    "isPublished": true,
    "changedAt": "2024-01-15T10:30:00+00:00",
    "topic_permalink": "https://community.example.com/product-updates/updated-product-update-title-1417"
}
```

* `productUpdateId` identifies the product update which had its title changed
* `publicId` identifies the product update platform wide
* `categoryId` identifies the category the product update belongs to
* `title` contains the updated product update title
* `changedBy` identifies the user who changed the product update title
* `isPublished` indicates whether the product update is published
* `changedAt` contains the time at which the product update title was changed (ISO 8601)

### productUpdate.ModeratorTagsChanged

```json
{
    "productUpdateId": "5464",
    "publicId": "456",
    "categoryId": "114",
    "changedBy": "4782",
    "moderatorTags": [
        "tag1",
        "tag2"
    ],
    "topic_permalink": "https://community.example.com/product-updates/topic-456"
}
```

* `productUpdateId` identifies the product update which had its moderator tags changed
* `publicId` identifies the product update platform wide
* `categoryId` identifies the category the product update belongs to
* `changedBy` identifies the user which performed the change to the moderator tags
* `moderatorTags` contains up-to-date list of moderator tags

### productUpdate.Liked

```json
{
    "productUpdateId": "86",
    "publicId": "456",
    "categoryId": "114",
    "authorId": "173",
    "likedBy": "950",
    "likeSet": [
        "950"
    ],
    "topic_permalink": "https://community.example.com/product-updates/topic-456"
}
```

* `productUpdateId` identifies the product update which has been liked
* `publicId` identifies the product update platform wide
* `categoryId` identifies the category the product update belongs to
* `authorId` identifies the user who created the product update
* `likedBy` identifies the user who liked the product update
* `likeSet` contains list of IDs of users who liked the product update

### productUpdate.Unliked

```json
{
    "productUpdateId": "86",
    "publicId": "241",
    "categoryId": "114",
    "authorId": "9",
    "unlikedBy": "9",
    "likeSet": [],
    "topic_permalink": "https://community.example.com/product-updates/topic-241"
}
```

* `productUpdateId` identifies the product update which has been liked
* `publicId` identifies the product update platform wide
* `categoryId` identifies the category the product update belongs to
* `authorId` identifies the user who created the product update
* `unlikedBy` identifies the user who unliked the product update
* `likeSet` contains list of IDs of users who liked the product update

### productUpdate.Replied

```json
{
    "id": "24",
    "publicReplyId": "1734",
    "authorId": "3323",
    "authorUsername": "Frank",
    "authorAvatar": "",
    "productUpdateId": "81",
    "publicId": "1569",
    "content": "My topic reply",
    "ipAddress": "172.18.123.141",
    "highlighted": false,
    "repliedAt": "2024-01-15T12:43:03+00:00",
    "topicAuthorId": "42",
    "reply_permalink": "https://community.example.com/product-updates/topic-1569?postid=1734#post1734"
}
```

* `id` identifies the reply which has been liked
* `publicReplyId` identifies the reply platform wide
* `authorId` identifies the user creating the product update
* `authorUsername` contains the username of the user at the time of creating
* `authorAvatar` contains an absolute URL or an empty string for the avatar of the user
* `productUpdateId` identifies the product update that was just created on the community
* `publicId` identifies the product update platform wide
* `content` contains the reply content
* `ipAddress` contains the IP address of the user replying the product update
* `highlighed` indicates whether a reply was highlighted
* `repliedAt` contains the time at which the reply was created
* `topicAuthorId` identifies the user who created the product update

### productUpdate.ReplyPostContentChanged

```json
{
    "replyId": "620",
    "publicReplyId": "9044",
    "productUpdateId": "875",
    "publicId": "5458",
    "content": "Test content",
    "changedBy": "2004",
    "changedAt": "2024-01-15T10:30:00+00:00",
    "reply_permalink": "https://community.example.com/product-updates/topic-5458?postid=9044#post9044"
}
```

* `replyId` identifies the reply which has been liked
* `publicReplyId` identifies the reply platform wide
* `productUpdateId` identifies the product update
* `publicId` identifies the product update platform wide
* `content` contains the reply content
* `changedBy` identifies the user changing the reply
* `changedAt` contains the time at which the reply content was changed (ISO 8601)

### productUpdate.ReplyReported

```json
{
    "id": "31",
    "publicId": "669",
    "topicId": "66",
    "topicPublicId": "360",
    "authorId": "97",
    "authorAvatar": "https://d1uyvls174j03l.cloudfront.net/inspired-en/icon/90x90/340d164b-8457-49c8-85ba-6e3cf85e39a9.png",
    "authorUsername": "Frank",
    "reportedByUserId": "2004",
    "reportedByAvatar": "https://uploads-eu-west-1.almostinsided.com/insidedhq-nl/icon/200x200/0d2aaee3-56c0-46cf-a090-c7f2ccb50874.png",
    "reportedByUsername": "regUser01",
    "reportedAt": "2023-05-16T02:46:06+00:00",
    "reason": "some reason",
    "notificationRecipients": [
        "email@community.com"
    ],
    "reply_permalink": "https://community.example.com/product-updates/topic-360?postid=669#post669"
}
```

* `id` identifies the reply which has been reported
* `publicId` identifies platform wide the reply
* `topicId` identifies the product update which includes the reply that has been reported
* `topicPublicId` identifies the product update platform wide
* `authorId` identifies the author who replied
* `authorAvatar` identifies the avatar of the author who replied
* `authorUsername` identifies the username of the author who replied
* `reportedByUserId` identifies the author of the reply reported
* `reportedByAvatar` identifies the avatar of the reporter
* `reportedByUsername` identifies the username of the reporter
* `reason` identifies the reason for the report
* `notificationRecipients` contains the email addresses set for the notification for each report

### productUpdate.ReplyLiked

```json
{
    "replyId": "544",
    "publicReplyId": "123",
    "productUpdateId": "86",
    "publicId": "456",
    "authorId": "173",
    "likedBy": "950",
    "likeSet": [
        "950"
    ],
    "reply_permalink": "https://community.example.com/product-updates/topic-456?postid=123#post123"
}
```

* `replyId` identifies the reply which has been liked
* `publicReplyId` identifies the reply platform wide
* `productUpdateId` identifies the product update which includes the reply that has been liked
* `publicId` identifies the product update platform wide
* `authorId` identifies the author of reply liked
* `likedBy` identifies the user who liked the reply
* `likeSet` contains list of IDs of users who liked the reply

### productUpdate.ReplyHighlightChanged

```json
{
    "replyId": "544",
    "publicReplyId": "123",
    "productUpdateId": "86",
    "publicId": "456",
    "highlighted": true,
    "changedBy": "950",
    "reply_permalink": "https://community.example.com/product-updates/topic-456?postid=123#post123"
}
```

* `replyId` identifies the reply whose highlight status has changed
* `publicReplyId` identifies the reply platform wide
* `productUpdateId` identifies the product update which includes the reply
* `publicId` identifies the product update platform wide
* `highlighted` indicates the new highlight state of the reply (`true` for highlighted, `false` for unhighlighted)
* `changedBy` identifies the user who changed the highlight status

### productUpdate.Trashed

```json
{
    "productUpdateId": "187",
    "publicId": "1298",
    "categoryId": "114",
    "trashedBy": "2006",
    "trashedAt": "2024-01-15T10:30:00+00:00",
    "topic_permalink": "https://community.example.com/product-updates/topic-1298"
}
```

* `productUpdateId` identifies the trashed product update
* `publicId` identifies the product update platform wide
* `categoryId` identifies the category the product update belongs to
* `trashedBy` identifies the user that trashed the product update
* `trashedAt` contains the time at which the product update was trashed (ISO 8601)

### productUpdate.Restored

```json
{
    "productUpdateId": "187",
    "publicId": "1298",
    "categoryId": "114",
    "restoredBy": "2006",
    "restoredAt": "2024-01-15T10:30:00+00:00",
    "topic_permalink": "https://community.example.com/ask-your-question-114/topic-1298"
}
```

* `productUpdateId` identifies the restored product update
* `publicId` identifies the product update platform wide
* `categoryId` identifies the category in which the product update was created
* `restoredBy` identifies the user that restored the product update
* `restoredAt` contains the time at which the product update was restored (ISO 8601)

### productUpdate.PermanentlyDeleted

```json
{
    "id": "187",
    "publicId": "1298",
    "categoryId": "114",
    "authorId": "321",
    "authorUsername": "SuperAdmin",
    "authorAvatar": "https://example.com/avatar.jpg",
    "title": "Product update title",
    "content": "Product update content",
    "startedAt": "2024-01-15T10:30:00+00:00",
    "sticky": false,
    "closed": false,
    "tags": [
        "tag1"
    ],
    "moderatorTags": [],
    "deletedBy": "2006",
    "deletedAt": "2024-01-15T10:30:00+00:00",
    "topic_permalink": "https://community.example.com/product-updates/product-update-title-1298"
}
```

* `id` identifies the permanently deleted product update
* `publicId` identifies the product update platform wide
* `categoryId` identifies the category the product update belongs to
* `authorId` identifies the author of the product update
* `authorUsername` the username of the author
* `authorAvatar` the avatar URL of the author
* `title` the title of the deleted product update
* `content` the content of the deleted product update
* `startedAt` the creation timestamp of the product update
* `sticky` indicates whether the product update was sticky
* `closed` indicates whether the product update was closed
* `tags` the public tags of the product update
* `moderatorTags` the moderator tags of the product update
* `deletedBy` identifies the user that permanently deleted the product update
* `deletedAt` contains the time at which the product update was permanently deleted (ISO 8601)

### productUpdate.Moved

```json
{
    "productUpdateId": "187",
    "publicId": "1298",
    "categoryId": "120",
    "categoryName": "New Category",
    "movedBy": "2006",
    "movedAt": "2024-01-15T10:30:00+00:00",
    "source_permalink": "https://community.example.com/category-120/topic-1298",
    "destination_permalink": "https://community.example.com/new-category-120/topic-1298"
}
```

* `productUpdateId` identifies the moved product update
* `publicId` identifies the product update platform wide
* `categoryId` identifies the new category
* `categoryName` the name of the new category
* `movedBy` identifies the user that moved the product update
* `movedAt` contains the time at which the product update was moved (ISO 8601)

### productUpdate.ReplyTrashed

```json
{
    "replyId": "544",
    "publicReplyId": "1300",
    "productUpdateId": "187",
    "publicId": "1298",
    "replyCount": 5,
    "trashedBy": "2006",
    "trashedAt": "2024-01-15T10:30:00+00:00",
    "reply_permalink": "https://community.example.com/product-updates/topic-1298?postid=1300#post1300"
}
```

* `replyId` identifies the trashed reply
* `publicReplyId` identifies the reply platform wide
* `productUpdateId` identifies the product update which includes the reply
* `publicId` identifies the product update platform wide
* `replyCount` the visible reply count of the product update before the action was performed
* `trashedBy` identifies the user that trashed the reply
* `trashedAt` contains the time at which the reply was trashed (ISO 8601)

### productUpdate.ReplyRestored

```json
{
    "replyId": "544",
    "publicReplyId": "1300",
    "productUpdateId": "187",
    "publicId": "1298",
    "replyCount": 6,
    "restoredBy": "2006",
    "restoredAt": "2024-01-15T10:30:00+00:00",
    "reply_permalink": "https://community.example.com/product-updates/topic-1298?postid=1300#post1300"
}
```

* `replyId` identifies the restored reply
* `publicReplyId` identifies the reply platform wide
* `productUpdateId` identifies the product update which includes the reply
* `publicId` identifies the product update platform wide
* `replyCount` the visible reply count of the product update before the action was performed
* `restoredBy` identifies the user that restored the reply
* `restoredAt` contains the time at which the reply was restored (ISO 8601)

### productUpdate.ReplyPermanentlyDeleted

```json
{
    "topicId": "187",
    "topicPublicId": "1298",
    "contentType": "productUpdate",
    "id": "544",
    "publicId": "1300",
    "deletedBy": "2006",
    "deletedAt": "2024-01-15T10:30:00+00:00",
    "reply_permalink": "https://community.example.com/product-updates/topic-1298?postid=1300#post1300"
}
```

* `topicId` identifies the product update which included the reply
* `topicPublicId` identifies the product update platform wide
* `contentType` the content type of the parent topic
* `id` identifies the permanently deleted reply
* `publicId` identifies the reply platform wide
* `deletedBy` identifies the user that permanently deleted the reply
* `deletedAt` contains the time at which the reply was permanently deleted (ISO 8601)
