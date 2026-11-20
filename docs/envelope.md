# Event envelope (draft)

Every message between BotBoot participants uses this envelope. The format is a draft and will change.

```json
{
  "id": "evt_01J...",
  "type": "item.result",
  "goal": "goal_42",
  "item": "item_7",
  "sender": "executor-a",
  "role": "executor",
  "issued_at": "2026-11-20T20:00:00Z",
  "expires_at": "2026-11-20T20:10:00Z",
  "body": { },
  "signature": "v1,<base64>"
}
```

## Rules

- `signature` covers every other field in canonical form (sorted keys, no whitespace), with a key that belongs to the sender.
- A receiver rejects an envelope when the signature fails, the sender is unknown, the role is not allowed for the `type`, or `expires_at` has passed.
- `id` is unique. Receivers keep recent ids and drop repeats, which also protects against replay.
- Keys are never placed inside an envelope, an item or a log.

## Types (initial)

| Type | Sent by | Meaning |
|---|---|---|
| `goal.created` | owner | A new goal exists |
| `item.queued` | coordinator | Work is available |
| `item.claimed` | executor | Lease taken |
| `item.result` | executor | Result and artifact links |
| `review.comment` | reviewer | Comment on an artifact |
| `gate.requested` | any | A human decision is needed |
| `gate.decided` | owner | Approved or refused, with reason |
| `cycle.stopped` | any | Done, budget exhausted, or refused |
