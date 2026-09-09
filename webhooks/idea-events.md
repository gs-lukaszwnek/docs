---
url: https://developer-portal.gainsight.com/docs/webhooks/idea-events.md
---
# Idea Events

## Available idea events

* [idea.Submitted](#idea-submitted)
* [idea.Voted](#idea-voted)
* [idea.Replied](#idea-replied)
* [idea.PostContentChanged](#idea-postcontentchanged)
* [idea.TitleChanged](#idea-titlechanged)
* [idea.ReplyPostContentChanged](#idea-replypostcontentchanged)
* [idea.VotesMerged](#idea-votesmerged)
* [idea.Unvoted](#idea-unvoted)
* [idea.StatusAssigned](#idea-statusassigned)
* [idea.Trashed](#idea-trashed)
* [idea.ModeratorTagsChanged](#idea-moderatortagschanged)
* [idea.ConvertedToQuestion](#idea-convertedtoquestion)
* [idea.ConvertedToConversation](#idea-convertedtoconversation)
* [idea.Reported](#idea-reported)
* [idea.ReplyReported](#idea-replyreported)
* [idea.ReplyPinned](#idea-replypinned)
* [idea.ReplyLiked](#idea-replyliked)
* [idea.ReplyHighlightChanged](#idea-replyhighlightchanged)
* [idea.Restored](#idea-restored)
* [idea.PermanentlyDeleted](#idea-permanentlydeleted)
* [idea.Moved](#idea-moved)
* [idea.ReplyTrashed](#idea-replytrashed)
* [idea.ReplyRestored](#idea-replyrestored)
* [idea.ReplyPermanentlyDeleted](#idea-replypermanentlydeleted)

## Event payloads

### idea.Submitted

```json
{
    "id": "128",
    "publicId": "456",
    "categoryId": "14",
    "authorAvatar": "https://upload.wikimedia.org/wikipedia/commons/6/61/Rubiks_cube_solved.jpg",
    "authorId": "7",
    "authorUsername": "Frank",
    "closed": false,
    "content": "One smart idea that will save the world",
    "ideaStatus": "New",
    "ideaStatusBgColor": "5d7680",
    "ideaStatusColor": "ffffff",
    "ipAddress": "192.168.0.1",
    "moderatorTags": [
        "new",
        "important"
    ],
    "sticky": false,
    "submittedAt": "2020-10-16T14:42:21+00:00",
    "tags": [
        "nature"
    ],
    "title": "Beautiful",
    "visibility": "visible",
    "topic_permalink": "https://community.example.com/ideas/beautiful-456"
}
```

* `id` identifies the idea
* `publicId` identifies the idea platform wide
* `categoryId` identifies the category the idea belongs to
* `authorAvatar` contains an absolute URL or an empty string for the avatar of the user
* `authorId` identifies the user who submitted the idea
* `authorUsername` contains the username of the user
* `closed` indicates if the idea is closed
* `content` contains the idea content
* `ideaStatusName` contains the text name of the idea status
* `ideaStatusBgColor` idea status background color
* `ideaStatusColor` idea status text color
* `ipAddress` contains the IP address of the user submitting the idea
* `moderatorTags` contains a list of tags added by a moderator (not shown on the community)
* `sticky` indicates whether or not the idea was made sticky on the community
* `submittedAt` contains the time at which the idea was submitted
* `tags` contains a list of tags as strings
* `title` contains the title of the question
* `visibility` contains the idea visibility status text name

### idea.Voted

```json
{
    "ideaId": "1",
    "publicId": "456",
    "categoryId": "14",
    "authorId": "10",
    "votedBy": "15",
    "voteSet": [
        "1",
        "2",
        "3"
    ],
    "topic_permalink": "https://community.example.com/ideas/topic-456"
}
```

* `ideaId` identifies the idea
* `publicId` identifies the idea platform wide
* `categoryId` identifies the category the idea belongs to
* `authorId` identifies the user who submitted the idea
* `votedBy` identifies the user who voted for the idea
* `voteSet` contains a list of IDs of all users who voted for the idea

### idea.Replied

```json
{
    "id": "5",
    "publicReplyId": "123",
    "ideaId": "10",
    "publicId": "456",
    "authorId": "10",
    "authorUsername": "Frank",
    "authorAvatar": "https:\\/\\/upload.wikimedia.org\\/wikipedia\\/commons\\/6\\/61\\/Rubiks_cube_solved.jpg",
    "content": "Beautiful content",
    "ipAddress": "192.168.0.1",
    "highlighted": false,
    "repliedAt": "2020-10-16T16:38:30+00:00",
    "replyCount": 0,
    "visibility": "visible",
    "topicAuthorId": "42",
    "reply_permalink": "https://community.example.com/ideas/topic-456?postid=123#post123"
}
```

* `id` identifies the reply
* `publicReplyId` identifies the reply platform wide
* `ideaId` identifies the idea
* `publicId` identifies the idea platform wide
* `authorId` identifies the user who submitted the reply
* `authorUsername` contains the username of the user
* `authorAvatar` contains an absolute URL or an empty string for the avatar of the user
* `content` contains the reply content
* `ipAddress` contains the IP address of the user replying the idea
* `highlighted` indicates if the reply is highlighted
* `repliedAt` contains the time at which the idea was replied
* `replyCount` contains the number of replies to the idea after the reply was created
* `visibility` contains the idea visibility status text name
* `topicAuthorId` identifies the user who submitted the idea

### idea.PostContentChanged

```json
{
    "ideaId": "382",
    "publicId": "3905",
    "categoryId": "14",
    "content": "Test content",
    "changedBy": "5078",
    "changedAt": "2024-01-15T10:30:00+00:00",
    "topic_permalink": "https://community.example.com/ideas/topic-3905"
}
```

* `ideaId` identifies the idea which had it's contents changed
* `publicId` identifies the idea platform wide
* `categoryId` identifies the category the idea belongs to
* `content` contains the updated idea content
* `changedBy` identifies the user which performed the change to the content
* `changedAt` contains the time at which the idea content was changed (ISO 8601)

### idea.TitleChanged

```json
{
    "ideaId": "382",
    "publicId": "3905",
    "categoryId": "14",
    "title": "Updated idea title",
    "changedBy": "5078",
    "changedAt": "2024-01-15T10:30:00+00:00",
    "topic_permalink": "https://community.example.com/ideas/updated-idea-title-3905"
}
```

* `ideaId` identifies the idea which had its title changed
* `publicId` identifies the idea platform wide
* `categoryId` identifies the category the idea belongs to
* `title` contains the updated idea title
* `changedBy` identifies the user who changed the idea title
* `changedAt` contains the time at which the idea title was changed (ISO 8601)

### idea.ReplyPostContentChanged

```json
{
    "replyId": "360",
    "publicReplyId": "7851",
    "ideaId": "463",
    "publicId": "4645",
    "content": "Test content",
    "changedBy": "2008",
    "changedAt": "2024-01-15T10:30:00+00:00",
    "reply_permalink": "https://community.example.com/ideas/topic-4645?postid=7851#post7851"
}
```

* `replyId` identifies the reply which has been changed
* `publicReplyId` identifies the reply platform wide
* `ideaId` identifies the idea
* `publicId` identifies the idea platform wide
* `content` contains the reply content
* `changedBy` identifies the user changing the reply
* `changedAt` contains the time at which the reply content was changed (ISO 8601)

### idea.VotesMerged

```json
{
    "ideaId": "1",
    "publicId": "456",
    "categoryId": "14",
    "sourceIdeaId": "2",
    "sourcePublicId": "654",
    "sourceCategoryId": "14",
    "authorId": "10",
    "mergedVotesBy": "20",
    "voteSet": [
        "55",
        "56"
    ],
    "topic_permalink": "https://community.example.com/ideas/topic-456"
}
```

* `ideaId` identifies the destination idea
* `publicId` identifies the idea platform wide
* `categoryId` identifies the category the idea belongs to
* `sourceIdeaId` identifies the source idea
* `sourcePublicId` identifies the source idea platform wide
* `sourceCategoryId` identifies the category of the source idea
* `authorId` identifies the user who submitted the idea
* `mergedVotesBy` identifies the user who merged two ideas
* `voteSet` contains a list of IDs of all users who voted for the idea

### idea.Unvoted

```json
{
    "ideaId": "1",
    "publicId": "456",
    "categoryId": "14",
    "authorId": "10",
    "unvotedBy": "5",
    "voteSet": [
        "7",
        "8"
    ],
    "topic_permalink": "https://community.example.com/ideas/topic-456"
}
```

* `ideaId` identifies the idea
* `publicId` identifies the idea platform wide
* `categoryId` identifies the category the idea belongs to
* `authorId` identifies the user who submitted the idea
* `unvotedBy` identifies the user who unvoted the idea
* `voteSet` contains a list of IDs of all users who voted for the idea

### idea.StatusAssigned

```json
{
    "ideaId": "1",
    "publicId": "456",
    "categoryId": "14",
    "ideaStatusId": "10",
    "ideaStatusName": "Pending",
    "ideaStatusColor": "ffffff",
    "ideaStatusBgColor": "f800a4",
    "assignedBy": "5",
    "topic_permalink": "https://community.example.com/ideas/topic-456"
}
```

* `ideaId` identifies the idea
* `publicId` identifies the idea platform wide
* `categoryId` identifies the category the idea belongs to
* `ideaStatusId` identifies the idea status
* `ideaStatusName` contains the text name of the idea status
* `ideaStatusBgColor` idea status background color
* `ideaStatusColor` idea status text color
* `assignedBy` identifies the user who assigned the status to the idea

### idea.Trashed

```json
{
    "ideaId": "1",
    "publicId": "456",
    "categoryId": "14",
    "trashedBy": "10",
    "trashedAt": "2024-01-15T10:30:00+00:00",
    "topic_permalink": "https://community.example.com/ideas/topic-456"
}
```

* `ideaId` identifies the idea
* `publicId` identifies the idea platform wide
* `categoryId` identifies the category the idea belongs to
* `trashedBy` identifies the user who trashed the idea
* `trashedAt` contains the time at which the idea was trashed (ISO 8601)

### idea.ModeratorTagsChanged

```json
{
    "ideaId": "1",
    "publicId": "456",
    "categoryId": "14",
    "changedBy": "10",
    "moderatorTags": [
        "urgent"
    ],
    "topic_permalink": "https://community.example.com/ideas/topic-456"
}
```

* `ideaId` identifies the idea
* `publicId` identifies the idea platform wide
* `categoryId` identifies the category the idea belongs to
* `changedBy` identifies the user who changed tags
* `moderatorTags` contains a list of moderator tags text names after moderator tags changed on idea

### idea.ConvertedToConversation

```json
{
    "ideaId": "86",
    "conversationId": "129",
    "convertedReplies": {
        "539": "544"
    },
    "publicId": "135",
    "categoryId": "14",
    "convertedBy": "950",
    "source_permalink": "https://community.example.com/category-14/topic-135",
    "destination_permalink": "https://community.example.com/category-14/topic-135"
}
```

* `ideaId` identifies the converted idea which has been converted to a conversation
* `conversationId` identifies the conversation
* `convertedReplies` contains list of private IDs of replies which have been converted to conversation replies
* `publicId` identifies the idea platform wide
* `categoryId` identifies the category in which the idea was submitted
* `convertedBy` identifies the user who performed the conversion

### idea.ConvertedToQuestion

```json
{
    "ideaId": "86",
    "questionId": "129",
    "convertedReplies": {
        "539": "544"
    },
    "publicId": "135",
    "categoryId": "14",
    "convertedBy": "950",
    "source_permalink": "https://community.example.com/category-14/topic-135",
    "destination_permalink": "https://community.example.com/category-14/topic-135"
}
```

* `ideaId` identifies the converted idea which has been converted to a question
* `questionId` identifies the question
* `convertedReplies` contains list of private IDs of replies which have been converted to question replies
* `publicId` identifies the idea platform wide
* `categoryId` identifies the category in which the idea was submitted
* `convertedBy` identifies the user who performed the conversion

### idea.Reported

```json
{
    "id": "31",
    "publicId": "669",
    "categoryId": "14",
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
    "topic_permalink": "https://community.example.com/ideas/topic-669"
}
```

* `id` identifies the idea which has been reported
* `publicId` identifies platform wide the idea
* `categoryId` identifies the category the idea belongs to
* `authorId` identifies the author who submitted idea
* `authorAvatar` identifies the avatar of the author who has idea
* `authorUsername` identifies the username of the author who submitted idea
* `reportedByUserId` identifies the author of the idea reported
* `reportedByAvatar` identifies the avatar of the reporter
* `reportedByUsername` identifies the username of the reporter
* `reportedAt` identifies the date of the idea reported
* `reason` identifies the reason for the report
* `notificationRecipients` contains the email addresses set for the notification for each report

### idea.ReplyReported

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
    "reply_permalink": "https://community.example.com/ideas/topic-360?postid=669#post669"
}
```

* `id` identifies the reply which has been reported
* `publicId` identifies platform wide the reply
* `topicId` identifies the idea which includes the reply that has been reported
* `topicPublicId` identifies the idea platform wide
* `authorId` identifies the author who replied
* `authorAvatar` identifies the avatar of the author who replied
* `authorUsername` identifies the username of the author who replied
* `reportedByUserId` identifies the author of the reply reported
* `reportedByAvatar` identifies the avatar of the reporter
* `reportedByUsername` identifies the username of the reporter
* `reason` identifies the reason for the report
* `notificationRecipients` contains the email addresses set for the notification for each report

### idea.ReplyPinned

```json
{
    "ideaId": "53",
    "replyId": "187",
    "pinnedBy": "1",
    "publicId": "277",
    "publicReplyId": "1613",
    "reply_permalink": "https://community.example.com/ideas/topic-277?postid=1613#post1613"
}
```

* `ideaId` identifies the idea which includes the reply that has been pinned
* `replyId` identifies the reply which has been pinned
* `pinnedBy` identifies the user who pinned the reply
* `publicId` identifies the idea platform wide
* `publicReplyId` identifies the reply that has been pinned platform wide

### idea.ReplyLiked

```json
{
    "replyId": "3124",
    "publicReplyId": "7438",
    "authorId": "5364",
    "ideaId": "789",
    "publicId": "1466",
    "likedBy": "10187",
    "likeSet": [
        "10187"
    ],
    "reply_permalink": "https://community.example.com/ideas/topic-1466?postid=7438#post7438"
}
```

* `replyId` identifies the reply which has been liked
* `publicReplyId` identifies the reply platform wide
* `ideaId` identifies the idea which includes the reply that has been liked
* `publicId` identifies the idea platform wide
* `authorId` identifies the author of reply liked
* `likedBy` identifies the user who liked the reply
* `likeSet` contains list of IDs of users who liked the reply

### idea.ReplyHighlightChanged

```json
{
    "replyId": "3124",
    "publicReplyId": "7438",
    "ideaId": "789",
    "publicId": "1466",
    "highlighted": true,
    "changedBy": "10187",
    "reply_permalink": "https://community.example.com/ideas/topic-1466?postid=7438#post7438"
}
```

* `replyId` identifies the reply whose highlight status has changed
* `publicReplyId` identifies the reply platform wide
* `ideaId` identifies the idea which includes the reply
* `publicId` identifies the idea platform wide
* `highlighted` indicates the new highlight state of the reply (`true` for highlighted, `false` for unhighlighted)
* `changedBy` identifies the user who changed the highlight status

### idea.Restored

```json
{
    "ideaId": "1",
    "publicId": "456",
    "categoryId": "14",
    "restoredBy": "10",
    "restoredAt": "2024-01-15T10:30:00+00:00",
    "topic_permalink": "https://community.example.com/category-14/topic-456"
}
```

* `ideaId` identifies the restored idea
* `publicId` identifies the idea platform wide
* `categoryId` identifies the category in which the idea was submitted
* `restoredBy` identifies the user that restored the idea
* `restoredAt` contains the time at which the idea was restored (ISO 8601)

### idea.PermanentlyDeleted

```json
{
    "id": "1",
    "publicId": "456",
    "categoryId": "14",
    "authorId": "321",
    "authorUsername": "daniel.boon",
    "authorAvatar": "https://example.com/avatar.jpg",
    "title": "Idea title",
    "content": "Idea content",
    "submittedAt": "2024-01-15T10:30:00+00:00",
    "sticky": false,
    "closed": false,
    "tags": [
        "tag1"
    ],
    "moderatorTags": [],
    "deletedBy": "10",
    "deletedAt": "2024-01-15T10:30:00+00:00",
    "topic_permalink": "https://community.example.com/ideas/idea-title-456"
}
```

* `id` identifies the permanently deleted idea
* `publicId` identifies the idea platform wide
* `categoryId` identifies the category the idea belongs to
* `authorId` identifies the author of the idea
* `authorUsername` the username of the author
* `authorAvatar` the avatar URL of the author
* `title` the title of the deleted idea
* `content` the content of the deleted idea
* `submittedAt` the creation timestamp of the idea
* `sticky` indicates whether the idea was sticky
* `closed` indicates whether the idea was closed
* `tags` the public tags of the idea
* `moderatorTags` the moderator tags of the idea
* `deletedBy` identifies the user that permanently deleted the idea
* `deletedAt` contains the time at which the idea was permanently deleted (ISO 8601)

### idea.Moved

```json
{
    "ideaId": "1",
    "publicId": "456",
    "categoryId": "20",
    "categoryName": "New Category",
    "movedBy": "10",
    "movedAt": "2024-01-15T10:30:00+00:00",
    "source_permalink": "https://community.example.com/category-20/topic-456",
    "destination_permalink": "https://community.example.com/new-category-20/topic-456"
}
```

* `ideaId` identifies the moved idea
* `publicId` identifies the idea platform wide
* `categoryId` identifies the new category
* `categoryName` the name of the new category
* `movedBy` identifies the user that moved the idea
* `movedAt` contains the time at which the idea was moved (ISO 8601)

### idea.ReplyTrashed

```json
{
    "replyId": "544",
    "publicReplyId": "458",
    "ideaId": "1",
    "publicId": "456",
    "replyCount": 5,
    "trashedBy": "10",
    "trashedAt": "2024-01-15T10:30:00+00:00",
    "reply_permalink": "https://community.example.com/ideas/topic-456?postid=458#post458"
}
```

* `replyId` identifies the trashed reply
* `publicReplyId` identifies the reply platform wide
* `ideaId` identifies the idea which includes the reply
* `publicId` identifies the idea platform wide
* `replyCount` the visible reply count of the idea before the action was performed
* `trashedBy` identifies the user that trashed the reply
* `trashedAt` contains the time at which the reply was trashed (ISO 8601)

### idea.ReplyRestored

```json
{
    "replyId": "544",
    "publicReplyId": "458",
    "ideaId": "1",
    "publicId": "456",
    "replyCount": 6,
    "restoredBy": "10",
    "restoredAt": "2024-01-15T10:30:00+00:00",
    "reply_permalink": "https://community.example.com/ideas/topic-456?postid=458#post458"
}
```

* `replyId` identifies the restored reply
* `publicReplyId` identifies the reply platform wide
* `ideaId` identifies the idea which includes the reply
* `publicId` identifies the idea platform wide
* `replyCount` the visible reply count of the idea before the action was performed
* `restoredBy` identifies the user that restored the reply
* `restoredAt` contains the time at which the reply was restored (ISO 8601)

### idea.ReplyPermanentlyDeleted

```json
{
    "topicId": "1",
    "topicPublicId": "456",
    "contentType": "idea",
    "id": "544",
    "publicId": "458",
    "deletedBy": "10",
    "deletedAt": "2024-01-15T10:30:00+00:00",
    "reply_permalink": "https://community.example.com/ideas/topic-456?postid=458#post458"
}
```

* `topicId` identifies the idea which included the reply
* `topicPublicId` identifies the idea platform wide
* `contentType` the content type of the parent topic
* `id` identifies the permanently deleted reply
* `publicId` identifies the reply platform wide
* `deletedBy` identifies the user that permanently deleted the reply
* `deletedAt` contains the time at which the reply was permanently deleted (ISO 8601)
