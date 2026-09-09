---
url: https://developer-portal.gainsight.com/docs/webhooks/question-events.md
---
# Question Events

## Available question events

* [question.Asked](#question-asked)
* [question.Replied](#question-replied)
* [question.PostContentChanged](#question-postcontentchanged)
* [question.TitleChanged](#question-titlechanged)
* [question.ReplyPostContentChanged](#question-replypostcontentchanged)
* [question.Answered](#question-answered)
* [question.AnswerRemoved](#question-answerremoved)
* [question.ModeratorTagsChanged](#question-moderatortagschanged)
* [question.Liked](#question-liked)
* [question.Unliked](#question-unliked)
* [question.ReplyLiked](#question-replyliked)
* [question.ConvertedToConversation](#question-convertedtoconversation)
* [question.ConvertedToIdea](#question-convertedtoidea)
* [question.ConvertedToReply](#question-convertedtoreply)
* [question.Reported](#question-reported)
* [question.ReplyReported](#question-replyreported)
* [question.ReplyHighlightChanged](#question-replyhighlightchanged)
* [question.Trashed](#question-trashed)
* [question.Restored](#question-restored)
* [question.PermanentlyDeleted](#question-permanentlydeleted)
* [question.Moved](#question-moved)
* [question.ReplyTrashed](#question-replytrashed)
* [question.ReplyRestored](#question-replyrestored)
* [question.ReplyPermanentlyDeleted](#question-replypermanentlydeleted)

## Event payloads

### question.Asked

The `question.Asked` event gets triggered whenever a user asks a question on the community.

```json
{
    "id": "129",
    "publicId": "456",
    "authorId": "340",
    "authorUsername": "daniel.boon",
    "authorAvatar": "https://d1uyvls174j03l.cloudfront.net/inspired-en/icon/90x90/bc4ac606-f7df-429e-8e8e-c4c107a9011f.png",
    "categoryId": "43",
    "title": "i have a question",
    "content": "what time is it?",
    "ipAddress": "92.111.227.154",
    "askedAt": "2018-11-27T08:27:34+00:00",
    "tags": [
        "tag1",
        "tag2"
    ],
    "sticky": false,
    "closed": false,
    "moderatorTags": [
        "tag1",
        "tag2"
    ],
    "poll": null,
    "visibility": "visible",
    "topic_permalink": "https://community.example.com/questions-43/i-have-a-question-456"
}
```

* `id` identifies the question that was just asked on the community
* `publicId` identifies the question platform wide
* `authorId` identifies the user asking the question
* `authorUsername` contains the username of the user at the time of asking
* `authorAvatar` contains an absolute URL or an empty string for the avatar of the user
* `categoryId` identifies the category in which the question was asked
* `tite` contains the title of the question
* `content` contains the question content
* `ipAddress` contains the IP address of the user asking the question
* `askedAt` contains the time at which the question was asked
* `tags` contains a list of tags as strings
* `sticky` indicates whether or not the question was made sticky on the community
* `closed` indicates if the question is closed for further replies
* `moderatorTags` contains a list of tags added by a moderator (not shown on the community)
* `poll` contains poll information if a poll was created, null otherwise

### question.Replied

The `question.Replied` event gets triggered whenever a user replies to a question on the community.

```json
{
    "id": "539",
    "publicReplyId": "123",
    "questionId": "64",
    "publicId": "456",
    "authorId": "321",
    "authorUsername": "SuperAdmin",
    "authorAvatar": "https://ddns8imsduk7o.cloudfront.net/default/user/icons/1384787968_1.jpg",
    "content": "test question reply",
    "ipAddress": "92.111.227.154",
    "highlighted": false,
    "repliedAt": "2019-05-24T15:19:20+00:00",
    "replyCount": 2,
    "categoryId": "43",
    "topicAuthorId": "42",
    "reply_permalink": "https://community.example.com/questions-43/topic-456?postid=123#post123"
}
```

* `id` identifies the reply that was just added
* `publicReplyId` identifies the reply platform wide
* `questionId` identifies the question that the reply was added to
* `publicId` identifies the question platform wide
* `authorId` identifies the user which added the reply
* `authorUsername` contains the username of the user at the time of replying
* `authorAvatar` contains the user avatar at the time of replying as absolute URL or empty string
* `content` contains the content of the reply
* `ipAddress` contains the IP address of the user at the time of replying
* `highlighed` indicates whether or not a reply was highlighted on the frontend
* `repliedAt` contains the date at which the reply was created
* `replyCount` contains the number of replies to the question after the reply was created
* `categoryId` identifies the category in which the question was asked
* `topicAuthorId` identifies the user who asked the question

### question.PostContentChanged

The `question.PostContentChanged` event gets triggered whenever the content of a question on the community gets edited.

```json
{
    "questionId": "1",
    "publicId": "456",
    "content": "test content",
    "changedBy": "18",
    "categoryId": "43",
    "changedAt": "2024-01-15T10:30:00+00:00",
    "topic_permalink": "https://community.example.com/questions-43/topic-456"
}
```

* `questionId` identifies the question which had it's contents changed
* `publicId` identifies the question platform wide
* `content` contains the updated question content
* `changedBy` identifies the user which performed the change to the content
* `categoryId` identifies the category in which the question was asked
* `changedAt` contains the time at which the question content was changed (ISO 8601)

### question.TitleChanged

```json
{
    "questionId": "1",
    "publicId": "456",
    "categoryId": "43",
    "title": "Updated question title",
    "changedBy": "18",
    "changedAt": "2024-01-15T10:30:00+00:00",
    "topic_permalink": "https://community.example.com/ask-your-question-114/updated-question-title-456"
}
```

* `questionId` identifies the question which had its title changed
* `publicId` identifies the question platform wide
* `categoryId` identifies the category the question belongs to
* `title` contains the updated question title
* `changedBy` identifies the user who changed the question title
* `changedAt` contains the time at which the question title was changed (ISO 8601)

### question.ReplyPostContentChanged

```json
{
    "replyId": "1600",
    "publicReplyId": "7259",
    "changedBy": "5473",
    "content": "Test content",
    "publicId": "4391",
    "questionId": "2474",
    "categoryId": "43",
    "changedAt": "2024-01-15T10:30:00+00:00",
    "reply_permalink": "https://community.example.com/questions-43/topic-4391?postid=7259#post7259"
}
```

* `replyId` identifies the reply which has been changed
* `publicReplyId` identifies the reply platform wide
* `changedBy` identifies the user changing the reply
* `content` contains the reply content
* `publicId` identifies the question platform wide
* `questionId` identifies the question
* `categoryId` identifies the category in which the question was asked
* `changedAt` contains the time at which the reply content was changed (ISO 8601)

### question.Answered

```json
{
    "questionId": "2611",
    "publicId": "4686",
    "replyId": "1852",
    "replyPublicId": "123",
    "replyAuthorId": "321",
    "markedAsAnswerBy": "2008",
    "previousAnswerId": "",
    "previousAnswerAuthorId": "",
    "categoryId": "43",
    "content": "<p>This is the accepted answer content</p>",
    "answeredAt": "2026-04-14T10:30:00+00:00",
    "topic_permalink": "https://community.example.com/questions-43/topic-4686",
    "reply_permalink": "https://community.example.com/questions-43/topic-4686?postid=123#post123"
}
```

* `questionId` identifies the question
* `publicId` identifies the question platform wide
* `replyId` identifies the reply which has been marked as best answer
* `replyPublicId` identifies the reply platform wide
* `replyAuthorId` identifies the user which added the reply marked as best answer
* `markedAsAnswerBy` identifies the user that marked the reply
* `previousAnswerId` indetifies the reply which was previously marked as answered
* `previousAnswerAuthorId` identifies the user which added the reply previously marked as best answer
* `categoryId` identifies the category in which the question was asked
* `content` contains the HTML content of the accepted answer
* `answeredAt` is the ISO 8601 timestamp of when the reply was marked as the best answer

### question.AnswerRemoved

```json
{
    "questionId": "2611",
    "publicId": "4686",
    "replyId": "1852",
    "publicReplyId": "123",
    "authorId": "321",
    "removedBy": "2008",
    "categoryId": "43",
    "topic_permalink": "https://community.example.com/questions-43/topic-4686",
    "reply_permalink": "https://community.example.com/questions-43/topic-4686?postid=123#post123"
}
```

* `questionId` identifies the question from which the answer was removed
* `publicId` identifies the question platform wide
* `replyId` identifies the reply which had been marked as best answer
* `publicReplyId` identifies the reply platform wide
* `authorId` identifies the user which added the reply marked as best answer
* `removedBy` identifies the user that unmarked the reply as best answer
* `categoryId` identifies the category in which the question was asked

### question.ModeratorTagsChanged

```json
{
    "questionId": "5464",
    "publicId": "456",
    "changedBy": "4782",
    "moderatorTags": [
        "tag1",
        "tag2"
    ],
    "categoryId": "43",
    "topic_permalink": "https://community.example.com/questions-43/topic-456"
}
```

* `questionId` identifies the question which had it's moderator tags changed
* `publicId` identifies the question platform wide
* `changedBy` identifies the user which performed the change to the moderator tags
* `moderatorTags` contains up-to-date list of moderator tags
* `categoryId` identifies the category in which the question was asked

### question.Liked

```json
{
    "questionId": "86",
    "publicId": "456",
    "authorId": "173",
    "likedBy": "950",
    "likeSet": [
        "950"
    ],
    "categoryId": "43",
    "topic_permalink": "https://community.example.com/questions-43/topic-456"
}
```

* `questionId` identifies the question which has been liked
* `publicId` identifies the question platform wide
* `authorId` identifies the user who created the question
* `likedBy` identifies the user who liked the question
* `likeSet` contains list of IDs of users who liked the question
* `categoryId` identifies the category in which the question was asked

### question.Unliked

```json
{
    "questionId": "86",
    "publicId": "456",
    "authorId": "173",
    "unlikedBy": "950",
    "likeSet": [
        "950"
    ],
    "categoryId": "43",
    "topic_permalink": "https://community.example.com/questions-43/topic-456"
}
```

* `questionId` identifies the question which has been unliked
* `publicId` identifies the question platform wide
* `authorId` identifies the user who created the question
* `unlikedBy` identifies the user who unliked the question
* `likeSet` contains list of IDs of users who liked the question
* `categoryId` identifies the category in which the question was asked

### question.ReplyLiked

```json
{
    "replyId": "544",
    "publicReplyId": "123",
    "questionId": "86",
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
* `questionId` identifies the question which includes the reply that has been liked
* `publicId` identifies the question platform wide
* `authorId` identifies the author of reply liked
* `likedBy` identifies the user who liked the reply
* `likeSet` contains list of IDs of users who liked the reply

### question.ConvertedToConversation

```json
{
    "questionId": "129",
    "conversationId": "86",
    "convertedReplies": {
        "539": "544"
    },
    "publicId": "176",
    "convertedBy": "950",
    "categoryId": "43",
    "source_permalink": "https://community.example.com/questions-43/topic-176",
    "destination_permalink": "https://community.example.com/questions-43/topic-176"
}
```

* `questionId` identifies the converted question which has been converted to a conversation
* `conversationId` identifies the conversation
* `convertedReplies` contains list of private IDs of replies which have been converted to conversation replies
* `publicId` identifies the question platform wide
* `convertedBy` identifies the user who performed the conversion
* `categoryId` identifies the category in which the question was asked

### question.ConvertedToIdea

```json
{
    "questionId": "129",
    "ideaId": "86",
    "convertedReplies": {
        "539": "544"
    },
    "publicId": "176",
    "convertedBy": "950",
    "categoryId": "43",
    "source_permalink": "https://community.example.com/questions-43/topic-176",
    "destination_permalink": "https://community.example.com/ideas/topic-176"
}
```

* `questionId` identifies the converted question which has been converted to an idea
* `ideaId` identifies the idea
* `convertedReplies` contains list of private IDs of replies which have been converted to idea replies
* `publicId` identifies the question platform wide
* `convertedBy` identifies the user who performed the conversion
* `categoryId` identifies the category in which the question was asked

### question.ConvertedToReply

```json
{
    "questionId": "86",
    "publicId": "456",
    "destination": {
        "topicId": "129",
        "topicType": "conversation",
        "publicId": "654"
    },
    "convertedBy": "950",
    "categoryId": "43",
    "source_permalink": "https://community.example.com/questions-43/topic-456",
    "destination_permalink": "https://community.example.com/questions-43/topic-654"
}
```

* `questionId` identifies the question which has been converted to a reply
* `publicId` identifies the question platform wide
* `destination` identifies the topic and content type that the new reply belongs to
* `destination.topicId` the private ID of the destination topic
* `destination.topicType` the content type of the destination topic
* `destination.publicId` the public ID of the destination topic
* `convertedBy` identifies the user who converted the question to a reply
* `categoryId` identifies the category in which the question was asked

### question.Reported

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
    "categoryId": "43",
    "topic_permalink": "https://community.example.com/questions-43/topic-669"
}
```

* `id` identifies the question which has been reported
* `publicId` identifies platform wide the question
* `authorId` identifies the author who asked question
* `authorAvatar` identifies the avatar of the author who asked question
* `authorUsername` identifies the username of the author who asked question
* `reportedByUserId` identifies the author of the question reported
* `reportedByAvatar` identifies the avatar of the reporter
* `reportedByUsername` identifies the username of the reporter
* `reportedAt` identifies the date of the question reported
* `reason` identifies the reason for the report
* `notificationRecipients` contains the email addresses set for the notification for each report
* `categoryId` identifies the category in which the question was asked

### question.ReplyReported

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
    "categoryId": "43",
    "reply_permalink": "https://community.example.com/questions-43/topic-360?postid=669#post669"
}
```

* `id` identifies the reply which has been reported
* `publicId` identifies platform wide the reply
* `topicId` identifies the question which includes the reply that has been reported
* `topicPublicId` identifies the question platform wide
* `authorId` identifies the author who submitted idea
* `authorAvatar` identifies the avatar of the author who submitted idea
* `authorUsername` identifies the username of the author who submitted idea
* `reportedByUserId` identifies the author of reply reported
* `reportedByAvatar` identifies the avatar of the reporter
* `reportedByUsername` identifies the username of the reporter
* `reason` identifies the reason for the report
* `notificationRecipients` contains the email addresses set for the notification for each report
* `categoryId` identifies the category in which the question was asked

### question.ReplyHighlightChanged

```json
{
    "replyId": "544",
    "publicReplyId": "123",
    "questionId": "86",
    "publicId": "456",
    "highlighted": true,
    "changedBy": "950",
    "reply_permalink": "https://community.example.com/ask-your-question-114/topic-456?postid=123#post123"
}
```

* `replyId` identifies the reply whose highlight status has changed
* `publicReplyId` identifies the reply platform wide
* `questionId` identifies the question which includes the reply
* `publicId` identifies the question platform wide
* `highlighted` indicates the new highlight state of the reply (`true` for highlighted, `false` for unhighlighted)
* `changedBy` identifies the user who changed the highlight status

### question.Trashed

```json
{
    "questionId": "771",
    "publicId": "1538",
    "trashedBy": "3279",
    "categoryId": "43",
    "trashedAt": "2024-01-15T10:30:00+00:00",
    "topic_permalink": "https://community.example.com/questions-43/topic-1538"
}
```

* `questionId` identifies the question which has been trashed
* `publicId` identifies the question platform wide
* `trashedBy` identifies the user who trashed the question
* `categoryId` identifies the category in which the question was asked
* `trashedAt` contains the time at which the question was trashed (ISO 8601)

### question.Restored

```json
{
    "questionId": "771",
    "publicId": "1538",
    "categoryId": "43",
    "restoredBy": "3279",
    "restoredAt": "2024-01-15T10:30:00+00:00",
    "topic_permalink": "https://community.example.com/questions-43/topic-1538"
}
```

* `questionId` identifies the restored question
* `publicId` identifies the question platform wide
* `categoryId` identifies the category in which the question was asked
* `restoredBy` identifies the user that restored the question
* `restoredAt` contains the time at which the question was restored (ISO 8601)

### question.PermanentlyDeleted

```json
{
    "id": "771",
    "publicId": "1538",
    "authorId": "321",
    "authorUsername": "daniel.boon",
    "authorAvatar": "https://example.com/avatar.jpg",
    "categoryId": "43",
    "title": "Question title",
    "content": "Question content",
    "askedAt": "2024-01-15T10:30:00+00:00",
    "sticky": false,
    "closed": false,
    "tags": [
        "tag1"
    ],
    "moderatorTags": [],
    "deletedBy": "3279",
    "deletedAt": "2024-01-15T10:30:00+00:00",
    "topic_permalink": "https://community.example.com/questions-43/question-title-1538"
}
```

* `id` identifies the permanently deleted question
* `publicId` identifies the question platform wide
* `authorId` identifies the author of the question
* `authorUsername` the username of the author
* `authorAvatar` the avatar URL of the author
* `categoryId` identifies the category in which the question was asked
* `title` the title of the deleted question
* `content` the content of the deleted question
* `askedAt` the creation timestamp of the question
* `sticky` indicates whether the question was sticky
* `closed` indicates whether the question was closed
* `tags` the public tags of the question
* `moderatorTags` the moderator tags of the question
* `deletedBy` identifies the user that permanently deleted the question
* `deletedAt` contains the time at which the question was permanently deleted (ISO 8601)

### question.Moved

```json
{
    "questionId": "771",
    "publicId": "1538",
    "categoryId": "50",
    "categoryName": "New Category",
    "movedBy": "3279",
    "oldCategoryId": "43",
    "visible": true,
    "movedAt": "2024-01-15T10:30:00+00:00",
    "source_permalink": "https://community.example.com/questions-43/topic-1538",
    "destination_permalink": "https://community.example.com/new-category-50/topic-1538"
}
```

* `questionId` identifies the moved question
* `publicId` identifies the question platform wide
* `categoryId` identifies the new category
* `categoryName` the name of the new category
* `movedBy` identifies the user that moved the question
* `oldCategoryId` identifies the previous category
* `visible` indicates whether the question is visible
* `movedAt` contains the time at which the question was moved (ISO 8601)

### question.ReplyTrashed

```json
{
    "replyId": "544",
    "publicReplyId": "1540",
    "questionId": "771",
    "publicId": "1538",
    "replyCount": 5,
    "trashedBy": "3279",
    "trashedAt": "2024-01-15T10:30:00+00:00",
    "reply_permalink": "https://community.example.com/ask-your-question-114/topic-1538?postid=1540#post1540"
}
```

* `replyId` identifies the trashed reply
* `publicReplyId` identifies the reply platform wide
* `questionId` identifies the question which includes the reply
* `publicId` identifies the question platform wide
* `replyCount` the visible reply count of the question before the action was performed
* `trashedBy` identifies the user that trashed the reply
* `trashedAt` contains the time at which the reply was trashed (ISO 8601)

### question.ReplyRestored

```json
{
    "replyId": "544",
    "publicReplyId": "1540",
    "questionId": "771",
    "publicId": "1538",
    "replyCount": 6,
    "restoredBy": "3279",
    "restoredAt": "2024-01-15T10:30:00+00:00",
    "reply_permalink": "https://community.example.com/ask-your-question-114/topic-1538?postid=1540#post1540"
}
```

* `replyId` identifies the restored reply
* `publicReplyId` identifies the reply platform wide
* `questionId` identifies the question which includes the reply
* `publicId` identifies the question platform wide
* `replyCount` the visible reply count of the question before the action was performed
* `restoredBy` identifies the user that restored the reply
* `restoredAt` contains the time at which the reply was restored (ISO 8601)

### question.ReplyPermanentlyDeleted

```json
{
    "topicId": "771",
    "topicPublicId": "1538",
    "contentType": "question",
    "id": "544",
    "publicId": "1540",
    "deletedBy": "3279",
    "deletedAt": "2024-01-15T10:30:00+00:00",
    "reply_permalink": "https://community.example.com/ask-your-question-114/topic-1538?postid=1540#post1540"
}
```

* `topicId` identifies the question which included the reply
* `topicPublicId` identifies the question platform wide
* `contentType` the content type of the parent topic
* `id` identifies the permanently deleted reply
* `publicId` identifies the reply platform wide
* `deletedBy` identifies the user that permanently deleted the reply
* `deletedAt` contains the time at which the reply was permanently deleted (ISO 8601)
