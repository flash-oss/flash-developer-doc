---
description: All reply codes and GraphQL errors
---

# RFI response codes

There are two places an RFI call can report a problem: the `code` field on the mutation reply payload, and the top-level GraphQL `errors` array.

### **Reply codes**

Returned in the `code` field of `AnswerRfiQuestionReply` / `DeclineRfiReply`:

| Mutation            | success | code               | When                                                                                                 |
| ------------------- | ------- | ------------------ | ---------------------------------------------------------------------------------------------------- |
| `answerRfiQuestion` | `true`  | `RECORDED`         | The reply was recorded                                                                               |
| `declineRfi`        | `true`  | `MARKED`           | The RFI was declined and closed                                                                      |
| both                | `false` | `NOT_FOUND`        | The RFI does not exist or does not belong to you                                                     |
| both                | `false` | `INVALID_STATUS`   | The RFI is not `PENDING` — answers and declines are only accepted while it is open for your response |
| `answerRfiQuestion` | `false` | `Already provided` | The question was answered by a concurrent submission (for example, via the email form)               |

### **GraphQL errors**

Invalid input is rejected before any reply payload is produced. These are returned in the top-level `errors` array with no `data`, with `extensions.code` set to:

| extensions.code     | Operation                 | When                                                                                                                                                                                                                                                                                    |
| ------------------- | ------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `BAD_USER_INPUT`    | any RFI query or mutation | Malformed `rfiId` / `id` — IDs are 24-character hex strings                                                                                                                                                                                                                             |
| `BAD_USER_INPUT`    | `answerRfiQuestion`       | The RFI was not found, unknown `questionCode`, the question has already been answered, or a file-rule violation — files on a `NONE` question, both `text` and `files` on a `PREFERRED` question, neither provided, too many files, unsupported file type, or a file over the size limit |
| `TOO_MANY_REQUESTS` | any                       | Rate limit exceeded — see below                                                                                                                                                                                                                                                         |

The error `message` spells out the specific problem, for example `Question SENDER_ID_PROOF has already been answered.` or `File "passport.pdf" exceeds the 10 MB per-file limit.`

### **Rate limits**

RFI queries and mutations are subject to the same per-client and per-IP limits as the rest of the API. When a limit is exceeded, the response is HTTP `429` with a `Retry-After` header (seconds) and `code: TOO_MANY_REQUESTS`. See [Rate limiting](../../other/rate-limiting.md).
