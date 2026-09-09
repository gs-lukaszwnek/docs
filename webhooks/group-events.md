---
url: https://developer-portal.gainsight.com/docs/webhooks/group-events.md
---
# Group Events

## Available group events

* [group.MemberJoined](#group-memberjoined)
* [group.MemberRequest](#group-memberrequest)
* [group.MemberLeft](#group-memberleft)

## Event payloads

### group.MemberJoined

```json
{
    "memberId": 2008,
    "groupId": 777,
    "groupTitle": "Group title"
}
```

* `memberId` identifies the member who joined the group
* `groupId` identifies the group that the member has joined
* `groupTitle` title of the group that member has joined

### group.MemberRequest

```json
{
    "memberId": 2008,
    "groupId": 777,
    "groupTitle": "Group title"
}
```

### group.MemberLeft

```json
{
    "memberId": 2008,
    "groupId": 777,
    "groupTitle": "Group title"
}
```
