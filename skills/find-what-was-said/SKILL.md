---
name: find-what-was-said
description: Find exactly what was said or decided in the user's Runa meetings, with the verbatim quote, speaker, and timestamp. Use when the user asks where something was discussed ("where did we talk about the three-minute setup target"), whether someone agreed to something ("did Riley actually agree to October 1?"), who said something, or for the exact words behind a claim.
compatibility: Needs the Runa MCP server (https://mcp.joinruna.com/mcp) connected in Hermes as `runa`, signed in to a Runa account.
---

# Find what was said

Everything comes from the Runa tools (`mcp__runa__*`). If they aren't available, or a Runa tool says to reconnect, tell the user how to connect Runa and stop: the first time, `hermes mcp add runa --url https://mcp.joinruna.com/mcp --auth oauth --connect-timeout 300`; after that, `hermes mcp login runa`. Either way, a new session then loads the tools.

## 1. Find the meeting

- For a topic, call `mcp__runa__search`, or `mcp__runa__runa_search_knowledge` to narrow by date or to meetings only. Both rank by relevance, not date.
- For a topic with a person, call `mcp__runa__meetings_get_last_matching` with both `topic_query` and `participant_query`.
- If it may have come up outside a meeting, in an email, CRM note, chat thread, or document, call `mcp__runa__runa_search_everything`.

## 2. Get the exact words

Call `mcp__runa__meetings_find_quotes` with the meeting's `runa://meeting/…` id and the key phrase. If nothing matches, try a shorter phrase or a different word for the same thing, because people rarely say a sentence the way it's later summarized.

## 3. Check that it still holds

For a decision or commitment, call `mcp__runa__runa_query_organizational_memory` with the claim as `question`, the subject as `entity`, and `state_filter: "history"` to see whether a later meeting changed or reversed it.

## 4. Answer

Lead with the direct answer (yes, no, or what was said), then the evidence as a quote:

> "[verbatim words]"
> [Speaker], [meeting title](url), [date] at [mm:ss]

If it was later changed, say so and cite the later meeting the same way.

## Rules

- Quote only transcript text, with the speaker label exactly as Runa gives it. A summary line is not a quote: label it as Runa's summary if that's all there is.
- If the connection can't read transcripts, `mcp__runa__meetings_find_quotes` says so. Answer from the summary and say the exact words couldn't be checked.
- If nothing turns up, say "I couldn't find that in your Runa meetings" and what you searched. Don't answer from general knowledge.
