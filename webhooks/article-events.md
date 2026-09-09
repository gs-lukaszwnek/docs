---
url: https://developer-portal.gainsight.com/docs/webhooks/article-events.md
---
# Article Events

## Available article events

* [article.Created](#article-created)
* [article.Published](#article-published)
* [article.PostContentChanged](#article-postcontentchanged)
* [article.TitleChanged](#article-titlechanged)
* [article.Replied](#article-replied)
* [article.ReplyPostContentChanged](#article-replypostcontentchanged)
* [article.ModeratorTagsChanged](#article-moderatortagschanged)
* [article.Liked](#article-liked)
* [article.Unliked](#article-unliked)
* [article.ReplyLiked](#article-replyliked)
* [article.ReplyReported](#article-replyreported)
* [article.ReplyHighlightChanged](#article-replyhighlightchanged)
* [article.Trashed](#article-trashed)
* [article.Restored](#article-restored)
* [article.PermanentlyDeleted](#article-permanentlydeleted)
* [article.Moved](#article-moved)
* [article.ConvertedToProductUpdate](#article-convertedtoproductupdate)
* [article.ConvertedToConversation](#article-convertedtoconversation)
* [article.ReplyTrashed](#article-replytrashed)
* [article.ReplyRestored](#article-replyrestored)
* [article.ReplyPermanentlyDeleted](#article-replypermanentlydeleted)

## Event payloads

### article.Created

```json
{
    "id": "90",
    "publicId": "678",
    "authorId": "2012",
    "authorUsername": "Frank",
    "authorAvatar": "https://d1uyvls174j03l.cloudfront.net/inspired-en/icon/90x90/340d164b-8457-49c8-85ba-6e3cf85e39a9.png",
    "categoryId": "114",
    "categoryName": "Ask your question",
    "title": "Test topic",
    "featuredImage": "",
    "publicLabel": "",
    "content": "Test content",
    "ipAddress": "172.18.123.165",
    "createdAt": "2024-01-11T11:59:13+00:00",
    "tags": [],
    "sticky": false,
    "closed": false,
    "moderatorTags": [],
    "poll": null,
    "publishedAt": null,
    "status": "draft",
    "topic_permalink": "https://community.example.com/ask-your-question-114/test-topic-678"
}
```

* `id` identifies the article that was just created on the community
* `publicId` identifies the article platform wide
* `authorId` identifies the user creating the article
* `authorUsername` contains the username of the user at the time of creating
* `authorAvatar` contains an absolute URL or an empty string for the avatar of the user
* `categoryId` identifies the category in which the article was created
* `categoryName` contains the category name in which the article was created
* `title` contains the title of the article
* `featuredImage` contains the URL of article's featured image
* `publicLabel` contains article's public label
* `content` contains the article content
* `ipAddress` contains the IP address of the user asking the article
* `createdAt` contains the time at which the article was created
* `tags` contains a list of tags as strings
* `sticky` indicates whether the article was made sticky on the community
* `closed` indicates if the article is closed for further replies
* `moderatorTags` contains a list of tags added by a moderator (not shown on the community)
* `poll` contains poll information if a poll was created, null otherwise
* `publishedAt` contains the time at which the article was published
* `status` contains article status

### article.Published

```json
{
    "articleId": "96",
    "publicId": "729",
    "authorId": "2012",
    "authorUsername": "Frank",
    "authorAvatar": "",
    "categoryId": "114",
    "title": "Test topic",
    "featuredImage": "",
    "publicLabel": "",
    "content": "Test content",
    "ipAddress": "172.18.123.36",
    "createdAt": "2024-01-11T12:11:36+00:00",
    "tags": [],
    "sticky": false,
    "closed": false,
    "moderatorTags": [],
    "poll": null,
    "publishedBy": "2012",
    "publishedAt": "2024-01-11T12:11:37+00:00",
    "status": "published",
    "topic_permalink": "https://community.example.com/ask-your-question-114/test-topic-729"
}
```

* `articleId` identifies the article that was just created on the community
* `publicId` identifies the article platform wide
* `authorId` identifies the user creating the article
* `authorUsername` contains the username of the user at the time of creating
* `authorAvatar` contains an absolute URL or an empty string for the avatar of the user
* `categoryId` identifies the category in which the article was created
* `title` contains the title of the article
* `featuredImage` contains the URL of article's featured image
* `publicLabel` contains article's public label
* `content` contains the article content
* `ipAddress` contains the IP address of the user creating the article
* `createdAt` contains the time at which the article was created
* `tags` contains a list of tags as strings
* `sticky` indicates whether the article was made sticky on the community
* `closed` indicates if the article is closed for further replies
* `moderatorTags` contains a list of tags added by a moderator (not shown on the community)
* `poll` contains poll information if a poll was created, null otherwise
* `publishedBy` identifies the user published the article
* `publishedAt` contains the time at which the article was published
* `status` contains article status

### article.PostContentChanged

```json
{
    "articleId": "206",
    "publicId": "1417",
    "content": "Test content",
    "changedBy": "3185",
    "categoryId": "114",
    "changedAt": "2024-01-15T10:30:00+00:00",
    "topic_permalink": "https://community.example.com/ask-your-question-114/topic-1417"
}
```

* `articleId` identifies the article that was just created on the community
* `publicId` identifies the article platform wide
* `content` contains the article content
* `changedBy` identifies the user changed the article content
* `categoryId` identifies the category in which the article was created
* `changedAt` contains the time at which the article content was changed (ISO 8601)

### article.TitleChanged

```json
{
    "articleId": "206",
    "publicId": "1417",
    "categoryId": "114",
    "title": "Updated article title",
    "changedBy": "3185",
    "isPublished": true,
    "changedAt": "2024-01-15T10:30:00+00:00",
    "topic_permalink": "https://community.example.com/ask-your-question-114/updated-article-title-1417"
}
```

* `articleId` identifies the article which had its title changed
* `publicId` identifies the article platform wide
* `categoryId` identifies the category the article belongs to
* `title` contains the updated article title
* `changedBy` identifies the user who changed the article title
* `isPublished` indicates whether the article is published
* `changedAt` contains the time at which the article title was changed (ISO 8601)

### article.Replied

```json
{
    "id": "3",
    "publicReplyId": "123",
    "articleId": "8",
    "categoryId": "114",
    "publicId": "456",
    "authorId": "97",
    "authorUsername": "Frank",
    "authorAvatar": "https://d1uyvls174j03l.cloudfront.net/inspired-en/icon/90x90/340d164b-8457-49c8-85ba-6e3cf85e39a9.png",
    "content": "hello",
    "ipAddress": "92.111.227.154",
    "highlighted": false,
    "repliedAt": "2019-05-23T14:46:04+00:00",
    "topicAuthorId": "42",
    "reply_permalink": "https://community.example.com/ask-your-question-114/topic-456?postid=123#post123"
}
```

* `id` identifies the reply which has been liked
* `publicReplyId` identifies platform wide the reply
* `articleId` identifies the article that was just created on the community
* `publicId` identifies the article platform wide
* `authorId` identifies the user creating the article
* `authorUsername` contains the username of the user at the time of creating
* `authorAvatar` contains an absolute URL or an empty string for the avatar of the user
* `content` contains the reply content
* `ipAddress` contains the IP address of the user replying the article
* `highlighed` indicates whether a reply was highlighted
* `repliedAt` contains the time at which the reply was created
* `categoryId` identifies the category in which the article was created
* `topicAuthorId` identifies the user who created the article

### article.ReplyPostContentChanged

```json
{
    "replyId": "620",
    "publicReplyId": "9044",
    "articleId": "875",
    "publicId": "5458",
    "categoryId": "114",
    "content": "Test content",
    "changedBy": "2004",
    "changedAt": "2024-01-15T10:30:00+00:00",
    "reply_permalink": "https://community.example.com/ask-your-question-114/topic-5458?postid=9044#post9044"
}
```

* `replyId` identifies the reply which has been liked
* `publicReplyId` identifies platform wide the reply
* `articleId` identifies the article that was just created on the community
* `publicId` identifies the article platform wide
* `content` contains the reply content
* `changedBy` identifies the user changing the reply
* `categoryId` identifies the category in which the article was created
* `changedAt` contains the time at which the reply content was changed (ISO 8601)

### article.ModeratorTagsChanged

```json
{
    "articleId": "5464",
    "publicId": "456",
    "changedBy": "4782",
    "moderatorTags": [
        "tag1",
        "tag2"
    ],
    "categoryId": "114",
    "topic_permalink": "https://community.example.com/ask-your-question-114/topic-456"
}
```

* `articleId` identifies the article which had its moderator tags changed
* `publicId` identifies the article platform wide
* `changedBy` identifies the user which performed the change to the moderator tags
* `moderatorTags` contains up-to-date list of moderator tags
* `categoryId` identifies the category in which the article was created

### article.Liked

```json
{
    "articleId": "86",
    "publicId": "456",
    "authorId": "173",
    "likedBy": "950",
    "likeSet": [
        "950"
    ],
    "categoryId": "114",
    "topic_permalink": "https://community.example.com/ask-your-question-114/topic-456"
}
```

* `articleId` identifies the article which has been liked
* `publicId` identifies the article platform wide
* `authorId` identifies the user who created the article
* `likedBy` identifies the user who liked the article
* `likeSet` contains list of IDs of users who liked the article
* `categoryId` identifies the category in which the article was created

### article.Unliked

```json
{
    "articleId": "86",
    "publicId": "241",
    "authorId": "9",
    "unlikedBy": "9",
    "likeSet": [],
    "categoryId": "114",
    "topic_permalink": "https://community.example.com/ask-your-question-114/topic-241"
}
```

* `articleId` identifies the article which has been liked
* `publicId` identifies the article platform wide
* `authorId` identifies the user who created the article
* `unlikedBy` identifies the user who unliked the article
* `likeSet` contains list of IDs of users who liked the article
* `categoryId` identifies the category in which the article was created

### article.ReplyLiked

```json
{
    "replyId": "544",
    "publicReplyId": "123",
    "articleId": "86",
    "publicId": "456",
    "authorId": "173",
    "likedBy": "950",
    "likeSet": [
        "950"
    ],
    "reply_permalink": "https://community.example.com/ask-your-question-114/topic-456?postid=123#post123"
}
```

* `replyId` identifies the reply which has been liked
* `publicReplyId` identifies platform wide the reply
* `articleId` identifies the article which includes the reply that has been liked
* `publicId` identifies the article platform wide
* `authorId` identifies the author of reply liked
* `likedBy` identifies the user who liked the reply
* `likeSet` contains list of IDs of users who liked the reply

### article.ReplyReported

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
    "categoryId": "114",
    "reply_permalink": "https://community.example.com/ask-your-question-114/topic-360?postid=669#post669"
}
```

* `id` identifies the reply which has been reported
* `publicId` identifies platform wide the reply
* `topicId` identifies the article which includes the reply that has been reported
* `topicPublicId` identifies the article platform wide
* `authorId` identifies the author who replied
* `authorAvatar` identifies the avatar of the author who replied
* `authorUsername` identifies the username of the author who replied
* `reportedByUserId` identifies the author of the reply reported
* `reportedByAvatar` identifies the avatar of the reporter
* `reportedByUsername` identifies the username of the reporter
* `reportedAt` identifies the date of the question reported
* `reason` identifies the reason for the report
* `notificationRecipients` contains the email addresses set for the notification for each report
* `categoryId` identifies the category in which the article was created

### article.ReplyHighlightChanged

```json
{
    "replyId": "544",
    "publicReplyId": "123",
    "articleId": "86",
    "publicId": "456",
    "highlighted": true,
    "changedBy": "950",
    "reply_permalink": "https://community.example.com/ask-your-question-114/topic-456?postid=123#post123"
}
```

* `replyId` identifies the reply whose highlight status has changed
* `publicReplyId` identifies the reply platform wide
* `articleId` identifies the article which includes the reply
* `publicId` identifies the article platform wide
* `highlighted` indicates the new highlight state of the reply (`true` for highlighted, `false` for unhighlighted)
* `changedBy` identifies the user who changed the highlight status

### article.Trashed

```json
{
    "articleId": "187",
    "publicId": "1298",
    "trashedBy": "2006",
    "categoryId": "114",
    "trashedAt": "2024-01-15T10:30:00+00:00",
    "topic_permalink": "https://community.example.com/ask-your-question-114/topic-1298"
}
```

* `articleId` identifies the trashed article
* `publicId` identifies the article platform wide
* `trashedBy` identifies the user that trashed the article
* `categoryId` identifies the category in which the article was created
* `trashedAt` contains the time at which the article was trashed (ISO 8601)

### article.Restored

```json
{
    "articleId": "187",
    "publicId": "1298",
    "categoryId": "114",
    "restoredBy": "2006",
    "restoredAt": "2024-01-15T10:30:00+00:00",
    "topic_permalink": "https://community.example.com/ask-your-question-114/topic-1298"
}
```

* `articleId` identifies the restored article
* `publicId` identifies the article platform wide
* `categoryId` identifies the category in which the article was created
* `restoredBy` identifies the user that restored the article
* `restoredAt` contains the time at which the article was restored (ISO 8601)

### article.PermanentlyDeleted

```json
{
    "id": "187",
    "publicId": "1298",
    "authorId": "321",
    "authorUsername": "SuperAdmin",
    "authorAvatar": "https://example.com/avatar.jpg",
    "categoryId": "114",
    "title": "Article title",
    "content": "Article content",
    "startedAt": "2024-01-15T10:30:00+00:00",
    "sticky": false,
    "closed": false,
    "tags": [
        "tag1"
    ],
    "moderatorTags": [],
    "deletedBy": "2006",
    "deletedAt": "2024-01-15T10:30:00+00:00",
    "topic_permalink": "https://community.example.com/ask-your-question-114/article-title-1298"
}
```

* `id` identifies the permanently deleted article
* `publicId` identifies the article platform wide
* `authorId` identifies the author of the article
* `authorUsername` the username of the author
* `authorAvatar` the avatar URL of the author
* `categoryId` identifies the category in which the article was created
* `title` the title of the deleted article
* `content` the content of the deleted article
* `startedAt` the creation timestamp of the article
* `sticky` indicates whether the article was sticky
* `closed` indicates whether the article was closed
* `tags` the public tags of the article
* `moderatorTags` the moderator tags of the article
* `deletedBy` identifies the user that permanently deleted the article
* `deletedAt` contains the time at which the article was permanently deleted (ISO 8601)

### article.Moved

```json
{
    "articleId": "187",
    "publicId": "1298",
    "categoryId": "120",
    "categoryName": "New Category",
    "movedBy": "2006",
    "isPublished": true,
    "oldCategoryId": "114",
    "visible": true,
    "movedAt": "2024-01-15T10:30:00+00:00",
    "source_permalink": "https://community.example.com/ask-your-question-114/topic-1298",
    "destination_permalink": "https://community.example.com/new-category-120/topic-1298"
}
```

* `articleId` identifies the moved article
* `publicId` identifies the article platform wide
* `categoryId` identifies the new category
* `categoryName` the name of the new category
* `movedBy` identifies the user that moved the article
* `isPublished` indicates whether the article is published
* `oldCategoryId` identifies the previous category
* `visible` indicates whether the article is visible
* `movedAt` contains the time at which the article was moved (ISO 8601)

### article.ConvertedToProductUpdate

```json
{
    "articleId": "38",
    "productUpdateId": "98",
    "convertedReplies": {
        "41": "264",
        "42": "265",
        "43": "266"
    },
    "publicId": "81",
    "categoryId": "114",
    "convertedBy": "9",
    "clonedReplies": {
        "41": "264",
        "42": "265",
        "43": "266"
    },
    "source_permalink": "https://community.example.com/ask-your-question-114/topic-81",
    "destination_permalink": "https://community.example.com/product-updates/topic-81"
}
```

* `articleId` identifies the article which has been converted to a product update
* `productUpdateId` identifies the converted product update
* `convertedReplies` contains list of private IDs of replies which have been converted to product update replies
* `publicId` identifies the article platform wide
* `categoryId` identifies the category the article belongs to
* `convertedBy` identifies the user who performed the conversion
* `clonedReplies` same as `convertedReplies` (kept for backwards compatibility)

### article.ConvertedToConversation

```json
{
    "articleId": "38",
    "conversationId": "95",
    "convertedReplies": {
        "41": "264",
        "42": "265"
    },
    "publicId": "81",
    "categoryId": "114",
    "convertedBy": "9",
    "source_permalink": "https://community.example.com/ask-your-question-114/topic-81",
    "destination_permalink": "https://community.example.com/ask-your-question-114/topic-81"
}
```

* `articleId` identifies the article which has been converted to a conversation
* `conversationId` identifies the converted conversation
* `convertedReplies` contains list of private IDs of replies which have been converted to conversation replies
* `publicId` identifies the article platform wide
* `categoryId` identifies the category the article belongs to
* `convertedBy` identifies the user who performed the conversion

### article.ReplyTrashed

```json
{
    "replyId": "544",
    "publicReplyId": "1300",
    "articleId": "187",
    "publicId": "1298",
    "replyCount": 5,
    "trashedBy": "2006",
    "trashedAt": "2024-01-15T10:30:00+00:00",
    "reply_permalink": "https://community.example.com/ask-your-question-114/topic-1298?postid=1300#post1300"
}
```

* `replyId` identifies the trashed reply
* `publicReplyId` identifies the reply platform wide
* `articleId` identifies the article which includes the reply
* `publicId` identifies the article platform wide
* `replyCount` the visible reply count of the article before the action was performed
* `trashedBy` identifies the user that trashed the reply
* `trashedAt` contains the time at which the reply was trashed (ISO 8601)

### article.ReplyRestored

```json
{
    "replyId": "544",
    "publicReplyId": "1300",
    "articleId": "187",
    "publicId": "1298",
    "replyCount": 6,
    "totalReplyCount": 8,
    "restoredBy": "2006",
    "restoredAt": "2024-01-15T10:30:00+00:00",
    "reply_permalink": "https://community.example.com/ask-your-question-114/topic-1298?postid=1300#post1300"
}
```

* `replyId` identifies the restored reply
* `publicReplyId` identifies the reply platform wide
* `articleId` identifies the article which includes the reply
* `publicId` identifies the article platform wide
* `replyCount` the visible reply count of the article before the action was performed
* `totalReplyCount` the total reply count including non-visible replies
* `restoredBy` identifies the user that restored the reply
* `restoredAt` contains the time at which the reply was restored (ISO 8601)

### article.ReplyPermanentlyDeleted

```json
{
    "topicId": "187",
    "topicPublicId": "1298",
    "contentType": "article",
    "id": "544",
    "publicId": "1300",
    "deletedBy": "2006",
    "deletedAt": "2024-01-15T10:30:00+00:00",
    "reply_permalink": "https://community.example.com/ask-your-question-114/topic-1298?postid=1300#post1300"
}
```

* `topicId` identifies the article which included the reply
* `topicPublicId` identifies the article platform wide
* `contentType` the content type of the parent topic
* `id` identifies the permanently deleted reply
* `publicId` identifies the reply platform wide
* `deletedBy` identifies the user that permanently deleted the reply
* `deletedAt` contains the time at which the reply was permanently deleted (ISO 8601)
