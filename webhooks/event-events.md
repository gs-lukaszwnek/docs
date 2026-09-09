---
url: https://developer-portal.gainsight.com/docs/webhooks/event-events.md
---
# Event Events

> **Note:** If a customer wants to subscribe to `event.Published`, they must also subscribe to `event.Created`, as both events are required to receive complete lifecycle notifications.

## Available event events

* [event.Created](#event-created)
* [event.Published](#event-published)
* [event.SignedUp](#event-signedup)
* [event.SignUpCancelled](#event-signupcancelled)

## Event payloads

### event.Created

```json
{
    "id": "152",
    "authorId": "9",
    "authorUsername": "Customer Communities Admin",
    "authorAvatar": "https://uploads-eu-west-1.almostinsided.com/engagement-en/icon/200x200/54b4078e-b95b-4048-8082-b6a24bc7ebf6.png",
    "title": "test event 123",
    "content": "<p>test event 123 desc</p>",
    "startAt": "2026-02-03T00:00:00+00:00",
    "endAt": "2026-02-03T01:00:00+00:00",
    "timezone": "Africa/Abidjan",
    "location": "",
    "url": "",
    "type": "",
    "status": "published",
    "image": "",
    "createdAt": "2026-02-02T07:06:02+00:00",
    "featuredTopics": [],
    "userGroupId": "",
    "registrationUrl": ""
}
```

* `id` identifies the event
* `authorId` identifies the user who created the event
* `authorUsername` is the display name of the event author
* `authorAvatar` is the URL of the author's avatar image
* `title` is the title of the event
* `content` contains the event description in HTML format
* `startAt` indicates when the event starts
* `endAt` indicates when the event ends
* `timezone` specifies the event's local timezone
* `location` represents the physical or virtual location of the event
* `url` is an optional external link associated with the event
* `type` represents the event type
* `status` indicates the current state of the event (e.g., published)
* `image` is an optional event image URL
* `createdAt` is the timestamp when the event was created
* `featuredTopics` lists any topics highlighted for this event
* `userGroupId` identifies the associated user group (if applicable)
* `registrationUrl` is an optional URL for event registration

### event.Published

```json
{
    "id": "1",
    "publishedAt": "2025-01-01T00:00:00+00:00",
    "publishedBy": "10"
}
```

* `id` identifies the event
* `publishedAt` the time at which the event was published
* `publishedBy` identifies the user who published the event

### event.SignedUp

```json
{
    "eventId": "1",
    "userId": "15",
    "eventTitle": "Annual Conference",
    "eventTimezone": "Europe/London",
    "eventStartDate": "2025-01-01T00:00:00+00:00"
}
```

* `eventId` identifies the event
* `userId` identifies the user who signed up for the event
* `eventTitle` the title of the event
* `eventTimezone` the timezone of the event
* `eventStartDate` the start date of the event

### event.SignUpCancelled

```json
{
    "eventId": "1",
    "userId": "15"
}
```

* `eventId` identifies the event
* `userId` identifies the user who cancelled their sign-up for the event
