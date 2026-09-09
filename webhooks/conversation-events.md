---
url: https://developer-portal.gainsight.com/docs/webhooks/conversation-events.md
---
# Conversation Events

## Available conversation events

* [conversation.Started](#conversation-started)
* [conversation.Replied](#conversation-replied)
* [conversation.PostContentChanged](#conversation-postcontentchanged)
* [conversation.TitleChanged](#conversation-titlechanged)
* [conversation.ReplyPostContentChanged](#conversation-replypostcontentchanged)
* [conversation.ModeratorTagsChanged](#conversation-moderatortagschanged)
* [conversation.Liked](#conversation-liked)
* [conversation.Unliked](#conversation-unliked)
* [conversation.ReplyLiked](#conversation-replyliked)
* [conversation.ConvertedToIdea](#conversation-convertedtoidea)
* [conversation.ConvertedToQuestion](#conversation-convertedtoquestion)
* [conversation.ConvertedToArticle](#conversation-convertedtoarticle)
* [conversation.ConvertedToReply](#conversation-convertedtoreply)
* [conversation.Reported](#conversation-reported)
* [conversation.ReplyReported](#conversation-replyreported)
* [conversation.ReplyPinned](#conversation-replypinned)
* [conversation.ReplyHighlightChanged](#conversation-replyhighlightchanged)
* [conversation.Trashed](#conversation-trashed)
* [conversation.Restored](#conversation-restored)
* [conversation.PermanentlyDeleted](#conversation-permanentlydeleted)
* [conversation.Moved](#conversation-moved)
* [conversation.ReplyTrashed](#conversation-replytrashed)
* [conversation.ReplyRestored](#conversation-replyrestored)
* [conversation.ReplyPermanentlyDeleted](#conversation-replypermanentlydeleted)

## Event payloads

### conversation.Started

```json
{
    "id": "1052",
    "publicId": "456",
    "authorId": "321",
    "authorUsername": "SuperAdmin",
    "authorAvatar": "https://ddns8imsduk7o.cloudfront.net/default/user/icons/1384787968_1.jpg",
    "categoryId": "14",
    "title": "test conversation title",
    "content": "test conversation [b]content[/b]",
    "ipAddress": "92.111.227.154",
    "startedAt": "2019-05-24T15:14:44+00:00",
    "tags": [
        "testtag"
    ],
    "sticky": false,
    "closed": false,
    "moderatorTags": [],
    "poll": {
        "title": "test poll question",
        "options": [
            "test answer a",
            "test answer b"
        ]
    },
    "topic_permalink": "https://community.example.com/category-14/test-conversation-title-456"
}
```

### conversation.Replied

```json
{
    "id": "2128",
    "publicReplyId": "123",
    "conversationId": "1052",
    "publicId": "456",
    "authorId": "321",
    "categoryId": "14",
    "authorUsername": "SuperAdmin",
    "authorAvatar": "https://ddns8imsduk7o.cloudfront.net/default/user/icons/1384787968_1.jpg",
    "content": "test conversation reply",
    "ipAddress": "92.111.227.154",
    "highlighted": false,
    "repliedAt": "2019-05-24T15:17:53+00:00",
    "replyCount": 0,
    "visibility": "visible",
    "topicAuthorId": "42",
    "reply_permalink": "https://community.example.com/category-14/topic-456?postid=123#post123"
}
```

### conversation.PostContentChanged

```json
{
    "conversationId": "990",
    "publicId": "5882",
    "content": "Test content",
    "changedBy": "2004",
    "categoryId": "14",
    "changedAt": "2024-01-15T10:30:00+00:00",
    "topic_permalink": "https://community.example.com/category-14/topic-5882"
}
```

* `conversationId` identifies the conversation
* `publicId` identifies the conversation platform wide
* `content` contains the conversation content
* `changedBy` identifies the user changed the conversation content
* `categoryId` identifies the category in which the conversation was started
* `changedAt` contains the time at which the conversation content was changed (ISO 8601)

### conversation.TitleChanged

```json
{
    "conversationId": "990",
    "publicId": "5882",
    "categoryId": "14",
    "title": "Updated conversation title",
    "changedBy": "2004",
    "changedAt": "2024-01-15T10:30:00+00:00",
    "topic_permalink": "https://community.example.com/ask-your-question-114/updated-conversation-title-5882"
}
```

* `conversationId` identifies the conversation which had its title changed
* `publicId` identifies the conversation platform wide
* `categoryId` identifies the category the conversation belongs to
* `title` contains the updated conversation title
* `changedBy` identifies the user who changed the conversation title
* `changedAt` contains the time at which the conversation title was changed (ISO 8601)

### conversation.ReplyPostContentChanged

```json
{
    "replyId": "397",
    "publicReplyId": "5540",
    "conversationId": "548",
    "categoryId": "14",
    "publicId": "3252",
    "content": "Test content",
    "changedBy": "2004",
    "changedAt": "2024-01-15T10:30:00+00:00",
    "reply_permalink": "https://community.example.com/category-14/topic-3252?postid=5540#post5540"
}
```

* `replyId` identifies the reply
* `publicReplyId` identifies platform wide the reply
* `conversationId` identifies the conversation
* `publicId` identifies the article platform wide
* `content` contains the reply content
* `changedBy` identifies the user changed the reply
* `categoryId` identifies the category in which the conversation was started
* `changedAt` contains the time at which the reply content was changed (ISO 8601)

### conversation.ModeratorTagsChanged

```json
{
    "conversationId": "5464",
    "publicId": "456",
    "changedBy": "4782",
    "moderatorTags": [
        "tag1",
        "tag2"
    ],
    "categoryId": "14",
    "topic_permalink": "https://community.example.com/category-14/topic-456"
}
```

* `conversationId` identifies the conversation which had its moderator tags changed
* `publicId` identifies the conversation platform wide
* `changedBy` identifies the user which performed the change to the moderator tags
* `moderatorTags` contains up-to-date list of moderator tags
* `categoryId` identifies the category in which the conversation was started

### conversation.Liked

```json
{
    "conversationId": "86",
    "publicId": "456",
    "authorId": "173",
    "likedBy": "950",
    "likeSet": [
        "950"
    ],
    "categoryId": "14",
    "topic_permalink": "https://community.example.com/category-14/topic-456"
}
```

* `conversationId` identifies the conversation which has been liked
* `publicId` identifies the conversation platform wide
* `authorId` identifies the user who created the conversation
* `likedBy` identifies the user who liked the conversation
* `likeSet` contains list of IDs of users who liked the conversation
* `categoryId` identifies the category in which the conversation was started

### conversation.Unliked

```json
{
    "conversationId": "86",
    "publicId": "456",
    "authorId": "173",
    "unlikedBy": "950",
    "likeSet": [
        "950"
    ],
    "categoryId": "14",
    "topic_permalink": "https://community.example.com/category-14/topic-456"
}
```

* `conversationId` identifies the conversation which has been unliked
* `publicId` identifies the conversation platform wide
* `authorId` identifies the user who created the conversation
* `unlikedBy` identifies the user who unliked the conversation
* `likeSet` contains list of IDs of users who liked the conversation
* `categoryId` identifies the category in which the conversation was started

### conversation.ReplyLiked

```json
{
    "replyId": "544",
    "publicReplyId": "123",
    "conversationId": "86",
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
* `publicReplyId` identifies the reply platform wide
* `conversationId` identifies the conversation which includes the reply that has been liked
* `publicId` identifies platform wide the conversation
* `authorId` identifies the author of reply liked
* `likedBy` identifies the user who liked the reply
* `likeSet` contains list of IDs of users who liked the reply

### conversation.ConvertedToIdea

```json
{
    "ideaId": "86",
    "conversationId": "129",
    "convertedReplies": {
        "539": "544"
    },
    "publicId": "176",
    "convertedBy": "950",
    "categoryId": "14",
    "source_permalink": "https://community.example.com/category-14/topic-176",
    "destination_permalink": "https://community.example.com/ideas/topic-176"
}
```

* `ideaId` identifies the idea
* `conversationId` identifies the converted conversation which has been converted to an idea
* `convertedReplies` contains list of private IDs of replies which have been converted to idea replies
* `publicId` identifies the conversation platform wide
* `convertedBy` identifies the user who performed the conversion
* `categoryId` identifies the category in which the conversation was started

### conversation.ConvertedToQuestion

```json
{
    "conversationId": "86",
    "questionId": "129",
    "convertedReplies": {
        "544": "539"
    },
    "publicId": "95",
    "convertedBy": "950",
    "categoryId": "14",
    "source_permalink": "https://community.example.com/category-14/topic-95",
    "destination_permalink": "https://community.example.com/category-14/topic-95"
}
```

* `conversationId` identifies the conversation which has been converted to a question
* `questionId` identifies the converted question
* `convertedReplies` contains list of private IDs of replies which have been converted to question replies
* `publicId` identifies the conversation platform wide
* `convertedBy` identifies the user who performed the conversion
* `categoryId` identifies the category in which the conversation was started

### conversation.ConvertedToArticle

```json
{
    "conversationId": "86",
    "articleId": "129",
    "convertedReplies": {
        "544": "539"
    },
    "publicId": "95",
    "convertedBy": "950",
    "categoryId": "14",
    "source_permalink": "https://community.example.com/category-14/topic-95",
    "destination_permalink": "https://community.example.com/category-14/topic-95"
}
```

* `conversationId` identifies the conversation which has been converted to an article
* `articleId` identifies the converted article
* `convertedReplies` contains list of private IDs of replies which have been converted to article replies
* `publicId` identifies the conversation platform wide
* `convertedBy` identifies the user who performed the conversion
* `categoryId` identifies the category in which the conversation was started

### conversation.ConvertedToReply

```json
{
    "conversationId": "86",
    "publicId": "456",
    "destination": {
        "topicId": "129",
        "topicType": "question",
        "publicId": "654"
    },
    "convertedBy": "950",
    "categoryId": "14",
    "source_permalink": "https://community.example.com/category-14/topic-456",
    "destination_permalink": "https://community.example.com/category-14/topic-654"
}
```

* `conversationId` identifies the conversation which has been converted to a reply
* `publicId` identifies the conversation platform wide
* `destination` identifies the topic and content type that the new reply belongs to
* `destination.topicId` the private ID of the destination topic
* `destination.topicType` the content type of the destination topic
* `destination.publicId` the public ID of the destination topic
* `convertedBy` identifies the user who converted the conversation to a reply
* `categoryId` identifies the category in which the conversation was started

### conversation.Reported

```json
{
    "id": "31",
    "publicId": "669",
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
    "categoryId": "14",
    "topic_permalink": "https://community.example.com/category-14/topic-669"
}
```

* `id` identifies the conversation which has been reported
* `publicId` identifies platform wide the conversation
* `authorId` identifies the author who started conversation
* `authorAvatar` identifies the avatar of the author who started conversation
* `authorUsername` identifies the username of the author who started conversation
* `reportedByUserId` identifies the author of the conversation reported
* `reportedByAvatar` identifies the avatar of the reporter
* `reportedByUsername` identifies the username of the reporter
* `reportedAt` identifies the date of the conversation reported
* `reason` identifies the reason for the report
* `notificationRecipients` contains the email addresses set for the notification for each report
* `categoryId` identifies the category in which the conversation was started

### conversation.ReplyReported

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
    "categoryId": "14",
    "reply_permalink": "https://community.example.com/category-14/topic-360?postid=669#post669"
}
```

* `id` identifies the reply which has been reported
* `publicId` identifies platform wide the reply
* `topicId` identifies the conversation which includes the reply that has been reported
* `topicPublicId` identifies the conversation platform wide
* `authorId` identifies the author who replied
* `authorAvatar` identifies the avatar of the author who replied
* `authorUsername` identifies the username of the author who replied
* `reportedByUserId` identifies the author of the reply reported
* `reportedByAvatar` identifies the avatar of the reporter
* `reportedByUsername` identifies the username of the reporter
* `reason` identifies the reason for the report
* `notificationRecipients` contains the email addresses set for the notification for each report
* `categoryId` identifies the category in which the conversation was started

### conversation.ReplyPinned

```json
{
    "conversationId": "995",
    "replyId": "626",
    "pinnedBy": "7707",
    "publicId": "5902",
    "publicReplyId": "9782",
    "categoryId": "14",
    "reply_permalink": "https://community.example.com/category-14/topic-5902?postid=9782#post9782"
}
```

* `conversationId` identifies the conversation which includes the reply that has been pinned
* `replyId` identifies the reply which has been pinned
* `pinnedBy` identifies the user who pinned the reply
* `publicId` identifies the conversation platform wide
* `publicReplyId` identifies the reply that has been pinned platform wide
* `categoryId` identifies the category in which the conversation was started

### conversation.ReplyHighlightChanged

```json
{
    "replyId": "626",
    "publicReplyId": "9782",
    "conversationId": "995",
    "publicId": "5902",
    "highlighted": true,
    "changedBy": "7707",
    "reply_permalink": "https://community.example.com/ask-your-question-114/topic-5902?postid=9782#post9782"
}
```

* `replyId` identifies the reply whose highlight status has changed
* `publicReplyId` identifies the reply platform wide
* `conversationId` identifies the conversation which includes the reply
* `publicId` identifies the conversation platform wide
* `highlighted` indicates the new highlight state of the reply (`true` for highlighted, `false` for unhighlighted)
* `changedBy` identifies the user who changed the highlight status

### conversation.Trashed

```json
{
    "conversationId": "765",
    "publicId": "4510",
    "trashedBy": "2006",
    "categoryId": "14",
    "trashedAt": "2024-01-15T10:30:00+00:00",
    "topic_permalink": "https://community.example.com/category-14/topic-4510"
}
```

* `conversationId` identifies the conversation which has been trashed
* `publicId` identifies the conversation platform wide
* `trashedBy` identifies the user who trashed the conversation
* `categoryId` identifies the category in which the conversation was started
* `trashedAt` contains the time at which the conversation was trashed (ISO 8601)

### conversation.Restored

```json
{
    "conversationId": "765",
    "publicId": "4510",
    "categoryId": "14",
    "restoredBy": "2006",
    "restoredAt": "2024-01-15T10:30:00+00:00",
    "topic_permalink": "https://community.example.com/category-14/topic-4510"
}
```

* `conversationId` identifies the restored conversation
* `publicId` identifies the conversation platform wide
* `categoryId` identifies the category in which the conversation was started
* `restoredBy` identifies the user that restored the conversation
* `restoredAt` contains the time at which the conversation was restored (ISO 8601)

### conversation.PermanentlyDeleted

```json
{
    "id": "765",
    "publicId": "4510",
    "authorId": "321",
    "authorUsername": "SuperAdmin",
    "authorAvatar": "https://example.com/avatar.jpg",
    "categoryId": "14",
    "categoryName": "General",
    "title": "Conversation title",
    "content": "Conversation content",
    "ipAddress": "92.111.227.154",
    "startedAt": "2024-01-15T10:30:00+00:00",
    "tags": [
        "tag1"
    ],
    "sticky": false,
    "closed": false,
    "trashed": true,
    "isSpam": false,
    "moderatorTags": [],
    "poll": null,
    "visibility": "trashed",
    "topicId": "4510",
    "pendingSince": null,
    "isPending": false,
    "deletedBy": "2006",
    "deletedAt": "2024-01-15T10:30:00+00:00",
    "topic_permalink": "https://community.example.com/general-14/conversation-title-4510"
}
```

* `id` identifies the permanently deleted conversation
* `publicId` identifies the conversation platform wide
* `authorId` identifies the author of the conversation
* `authorUsername` the username of the author
* `authorAvatar` the avatar URL of the author
* `categoryId` identifies the category in which the conversation was started
* `categoryName` the name of the category
* `title` the title of the deleted conversation
* `content` the content of the deleted conversation
* `startedAt` the creation timestamp of the conversation
* `tags` the public tags of the conversation
* `sticky` indicates whether the conversation was sticky
* `closed` indicates whether the conversation was closed
* `moderatorTags` the moderator tags of the conversation
* `poll` the poll data if the conversation had a poll, `null` otherwise
* `topicId` identifies the conversation platform wide (deprecated, use `publicId`)
* `deletedBy` identifies the user that permanently deleted the conversation
* `deletedAt` contains the time at which the conversation was permanently deleted (ISO 8601)

### conversation.Moved

```json
{
    "conversationId": "765",
    "publicId": "4510",
    "categoryId": "20",
    "categoryName": "New Category",
    "movedBy": "2006",
    "oldCategoryId": "14",
    "visible": true,
    "movedAt": "2024-01-15T10:30:00+00:00",
    "source_permalink": "https://community.example.com/category-14/topic-4510",
    "destination_permalink": "https://community.example.com/new-category-20/topic-4510"
}
```

* `conversationId` identifies the moved conversation
* `publicId` identifies the conversation platform wide
* `categoryId` identifies the new category
* `categoryName` the name of the new category
* `movedBy` identifies the user that moved the conversation
* `oldCategoryId` identifies the previous category
* `visible` indicates whether the conversation is visible
* `movedAt` contains the time at which the conversation was moved (ISO 8601)

### conversation.ReplyTrashed

```json
{
    "replyId": "544",
    "publicReplyId": "4512",
    "conversationId": "765",
    "publicId": "4510",
    "replyCount": 5,
    "trashedBy": "2006",
    "trashedAt": "2024-01-15T10:30:00+00:00",
    "reply_permalink": "https://community.example.com/ask-your-question-114/topic-4510?postid=4512#post4512"
}
```

* `replyId` identifies the trashed reply
* `publicReplyId` identifies the reply platform wide
* `conversationId` identifies the conversation which includes the reply
* `publicId` identifies the conversation platform wide
* `replyCount` the visible reply count of the conversation before the action was performed
* `trashedBy` identifies the user that trashed the reply
* `trashedAt` contains the time at which the reply was trashed (ISO 8601)

### conversation.ReplyRestored

```json
{
    "replyId": "544",
    "publicReplyId": "4512",
    "conversationId": "765",
    "publicId": "4510",
    "replyCount": 6,
    "restoredBy": "2006",
    "restoredAt": "2024-01-15T10:30:00+00:00",
    "reply_permalink": "https://community.example.com/ask-your-question-114/topic-4510?postid=4512#post4512"
}
```

* `replyId` identifies the restored reply
* `publicReplyId` identifies the reply platform wide
* `conversationId` identifies the conversation which includes the reply
* `publicId` identifies the conversation platform wide
* `replyCount` the visible reply count of the conversation before the action was performed
* `restoredBy` identifies the user that restored the reply
* `restoredAt` contains the time at which the reply was restored (ISO 8601)

### conversation.ReplyPermanentlyDeleted

```json
{
    "topicId": "765",
    "topicPublicId": "4510",
    "contentType": "conversation",
    "id": "544",
    "publicId": "4512",
    "deletedBy": "2006",
    "deletedAt": "2024-01-15T10:30:00+00:00",
    "reply_permalink": "https://community.example.com/ask-your-question-114/topic-4510?postid=4512#post4512"
}
```

* `topicId` identifies the conversation which included the reply
* `topicPublicId` identifies the conversation platform wide
* `contentType` the content type of the parent topic
* `id` identifies the permanently deleted reply
* `publicId` identifies the reply platform wide
* `deletedAt` contains the time at which the reply was permanently deleted (ISO 8601)
* `deletedBy` identifies the user that permanently deleted the reply
