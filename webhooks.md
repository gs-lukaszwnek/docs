---
url: https://developer-portal.gainsight.com/docs/webhooks.md
---
# Webhooks

## How to subscribe to an event

```http request
POST /webhooks/{eventName}/subscriptions
```

All webhook events must be authenticated using an OAuth2 token in the headers: for more information consult the [OAuth2 section](#section/Authentication/OAuth2).

Add the event name you want to subscribe to in the path (e.g. `question.Asked`).

Add the following details in the body:

```json
{
    "url": "your callback url",
    "username": "your-api-token",
    "secret": "your-api-secret"
}
```

For more information about the webhook subscription endpoint, see [Subscribe URL to webhook event](/api/operations/community/subscribeWebhook).

## Available events

Currently available webhook events are:

* [Article events](./article-events.md)
* [Content Moderation events](./content-moderation-events.md)
* [Conversation events](./conversation-events.md)
* [Event events](./event-events.md)
* [Gamification events](./gamification-events.md)
* [Group events](./group-events.md)
* [Idea events](./idea-events.md)
* [Ideation events](./ideation-events.md)
* [Product update events](./product-update-events.md)
* [Public Tag events](./public-tag-events.md)
* [Question events](./question-events.md)
* [User events](./user-events.md)

We have more events available and we're expanding this officially-supported list based on customer request, so please let [Customer Communities support](mailto:support@gainsight.com) know if there's a use case you can't support with the above events.

## Private and Public Ids

The public ids identify the topic/reply platform-wide and are used in the community URLS. The public id can be found in most of the events under the following keys:

* `publicId` for topics
* `publicReplyId` for replies

The private ids identify the topic/reply internally and are used in the API URLS.The private id can be found in most of the events under the following keys:

* `articleId` for topics of content type Article
* `questionId` for topics of content type Question
* `conversationId` for topics of content type Conversation
* `ideaId` for topics of content type Idea
* `id` for topics/replies
* `replyId` for replies

## Permalink

Content webhook payloads may include absolute community URLs. Topic URLs use `{baseUrl}/{categorySlug-categoryId}/{titleSlug-publicTopicId}` (or `/ideas/` / `/product-updates/` when no category applies). Reply URLs append `?postid={publicReplyId}#post{publicReplyId}`.

* `topic_permalink` — topic-level events
* `reply_permalink` — reply-level events
* `topic_permalink` and `reply_permalink` — e.g. answered, answer removed
* `source_permalink` and `destination_permalink` — moved, converted events
