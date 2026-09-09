---
url: >-
  https://developer-portal.gainsight.com/docs/api/operations/events/createEvent.md
---

# Create an event

Events

Moderators can create an event with a title, description, event timezone, event start time/date, and event end time/date. Events do not have a category.

Events can be created and published immediately, or can be created as draft and published later. Draft events are not publicly visible.

Optionally, moderators can provide a location for an event, as well as a URL for end users to find more information or formally register for an event. Moderators can optionally set a display label for a  URL, to provide more context for the end user about the link.

In addition, moderators can specify an event type for the event (a label highlighting the type of event, e.g. Webinar, Conference, Meetup).

Moderators can also add a custom confirmation message that is shown to end users after they press attend; for example, to highlight any actions that the end user needs to take external to the community.

Events can also optionally have a featured image with a valid url. The allowed image extensions for featured image are png, jpeg, jpg, gif.

## Endpoint

```
POST https://api2-eu-west-1.insided.com/events/events/create
```

**Required scope:** `write`

### Parameters

| Name | In | Type | Required | Description |
|------|----|------|----------|-------------|
| `moderatorId` | query | string | Yes | ID of the moderator creating the event |

### Request Body

`application/json` (required)

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `title` | string | Yes | — |
| `type` | string | No | — |
| `content` | string | Yes | — |
| `startsAt` | string | Yes | ISO format date-time representing the start date and time of the event |
| `endsAt` | string | Yes | ISO format date-time representing the end date and time of the event |
| `timezone` | string | Yes | A valid timezone name from the IANA database. For reference: [list of IANA timezone names](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) |
| `location` | string | No | — |
| `url` | string | No | — |
| `image` | string | No | — |
| `externalRegistrationUrl` | string | No | Note that event with configured external RSVP can not be configured back with community RSVP |
| `externalRegistrationUrlLabel` | string | No | — |
| `featuredTopics` | array of object | No | — |
| `confirmationMessage` | string | No | — |
| `userGroupId` | string | No | — |

### Responses

| Status | Description |
|--------|-------------|
| 201 | Event created |
| 400 | Malformed input |
| 422 | Validation error |
| 500 | Unexpected error |
