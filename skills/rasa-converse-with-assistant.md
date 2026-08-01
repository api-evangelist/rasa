---
name: Send a message to a conversation and read the tracker
description: Drive a conversation through the Rasa HTTP API — add a user message, inspect the tracker, and predict the next action.
api: openapi/rasa-http-api-openapi.yml
operations: [addConversationMessage, getConversationTracker, predictConversationAction]
generated: '2026-07-20'
method: generated
---

# Send a message to a conversation and read the tracker

Interact with a loaded assistant over the Rasa HTTP API. Conversations are keyed
by `conversation_id` (the sender id) in the path.

## Authentication
Provide the `token` query parameter or a JWT bearer token. With a `user`-role
JWT, the token's `username` must match the `conversation_id`; an `admin` role can
access any conversation. See `authentication/rasa-authentication.yml`.

## Steps
1. **Add a user message** — `POST /conversations/{conversation_id}/tracker/events`
   is for raw events; to send an actual user utterance use
   `POST /conversations/{conversation_id}/messages` (`addConversationMessage`)
   with the message text and sender. The updated tracker is returned.
2. **Read conversation state** — `GET /conversations/{conversation_id}/tracker`
   (`getConversationTracker`) to inspect slots, latest_message, active_loop and
   the full event log.
3. **Predict the next action** —
   `POST /conversations/{conversation_id}/predict` (`predictConversationAction`)
   to get the ranked next action without executing it; use
   `.../execute` to run one.

## Conventions
Trackers return full documents (no pagination). There is no Idempotency-Key
contract; conversation state is keyed by `conversation_id`. See
`conventions/rasa-conventions.yml`.
