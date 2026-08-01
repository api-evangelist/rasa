---
name: Train a Rasa model and load it on the server
description: Train a Rasa model from training data, then load it onto a running Rasa server via the HTTP API.
api: openapi/rasa-http-api-openapi.yml
operations: [trainModel, replaceModel, getStatus]
generated: '2026-07-20'
method: generated
---

# Train a Rasa model and load it on the server

Use the Rasa HTTP API to train a model and put it into service. The server is
self-hosted (default `http://localhost:5005`) and started with
`rasa run --enable-api`.

## Authentication
Every endpoint requires either the `token` query parameter (server started with
`--auth-token`) or a JWT bearer token (`--jwt-secret`, HS256, `role: admin`).
See `authentication/rasa-authentication.yml`.

## Steps
1. **Train a model** — `POST /model/train` (`trainModel`). Send the domain,
   config, NLU data and stories/flows as `application/yaml` or `application/json`.
   The response streams back the trained model as `application/x-tar`; save it to
   a models directory.
2. **Load the model** — `PUT /model` (`replaceModel`) with the path/name of the
   trained model to make it the active model on the server.
3. **Confirm** — `GET /status` (`getStatus`) and check the reported model file
   and number of active training jobs; `GET /version` reports the Rasa version.

## Error handling
Errors use Rasa's custom envelope `{ version, status, reason, message, code }`
(not RFC 9457). Watch for `409 Conflict` when a model operation conflicts with
the currently loaded model, and `401/403` for auth issues. See
`errors/rasa-problem-types.yml`.
