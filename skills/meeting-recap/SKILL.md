---
name: meeting-recap
description: Recap meetings recorded in Runa, covering what was decided, who owes what, and what's still open. Use when the user asks what happened in a meeting ("recap my last meeting", "what did we decide with Acme"), wants to catch up on a stretch of time ("what did I miss this week"), or asks about commitments ("what do I owe", "what did Maya commit to").
compatibility: Needs the Runa MCP server (https://mcp.joinruna.com/mcp) connected in Hermes as `runa`, signed in to a Runa account.
---

# Meeting recap

Everything comes from the Runa tools (`mcp__runa__*`). If they aren't available, or a Runa tool says to reconnect, tell the user how to connect Runa and stop: the first time, `hermes mcp add runa --url https://mcp.joinruna.com/mcp --auth oauth --connect-timeout 300`; after that, `hermes mcp login runa`. Either way, a new session then loads the tools.

## 1. Choose the tool by the question

| The user asks about | Call |
|---|---|
| Their last meeting, or the call they just had | `mcp__runa__meetings_get_last` |
| A meeting with a person or company, or about a topic | `mcp__runa__meetings_get_last_matching` with `participant_query`, `topic_query`, or both |
| A stretch of time ("today", "this week", "while I was out") | `mcp__runa__meetings_list` with `date_start`, `date_end`, and the user's IANA `timezone`, following `next_cursor` until it's empty |
| Commitments ("what do I owe", "what did Maya commit to") | `mcp__runa__runa_list_action_items` with `owner`, plus `mcp__runa__runa_query_organizational_memory` with the user's `question` and `intent: "commitments"` |
| A decision across meetings ("what did we decide about pricing", "what changed") | `mcp__runa__runa_query_organizational_memory` with the user's `question`, `intent: "decisions"` or `"changes"`, and the subject as `entity` |

`mcp__runa__meetings_get_last`, `mcp__runa__meetings_get_last_matching`, and `mcp__runa__meetings_list` pick meetings by date. `mcp__runa__search` ranks by relevance, so it can't answer "last" or "latest".

For the user's own commitments, `owner` is their name as it appears in meetings: take it from a participant with `is_self: true`. For a stretch of time, work from each meeting's `summary` in the list, and fetch a meeting only when its summary is missing or the user wants more.

## 2. Write the recap

Write it as Markdown, not inside a code block. For one meeting:

```
[Meeting title], [date]

Decided
- [Decision]

Action items
- [Owner]: [task], due [date]

Open questions
- [Question]
```

For several meetings, write one short block per meeting, newest first, then one combined list of open action items.

For commitments, write a ledger in three groups, "You owe", "Owed to you", and "No clear owner", with each item's due date and source meeting.

## Rules

- A decision is what was agreed, not what was discussed. If nothing was decided, say so.
- Use owners and due dates exactly as Runa has them. Write "no due date" rather than guessing one.
- Link each meeting title to its `url`.
- The summary is generated and the transcript is verbatim. When exact wording matters, quote the transcript. If it was cut (`truncated`), read the rest with `mcp__runa__runa_fetch_resource_page` before relying on it.
- If the account has no recorded meetings yet, say so: Runa records meetings with its desktop app, and there is nothing to recap until the first one.
