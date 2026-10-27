---
name: meeting-prep
description: Brief the user before a meeting, call, or conversation, using their Runa meeting history, calendar, and connected apps. Use when they ask to prep or be briefed for a meeting ("prep me for my 2pm", "what should I know before my call with Dana", "brief me on Acme before tomorrow"), or ask what to raise with someone they're about to meet.
compatibility: Needs the Runa MCP server (https://mcp.joinruna.com/mcp) connected in Hermes as `runa`, signed in to a Runa account.
---

# Meeting prep

Give the user what they need for the meeting on one screen: who they're meeting, where things stand, what's owed in each direction, and what's worth raising. Everything comes from the Runa tools (`mcp__runa__*`). If they aren't available, or a Runa tool says to reconnect, tell the user how to connect Runa and stop: the first time, `hermes mcp add runa --url https://mcp.joinruna.com/mcp --auth oauth --connect-timeout 300`; after that, `hermes mcp login runa`. Either way, a new session then loads the tools.

## 1. Find the meeting

- For "my next meeting", a time ("my 2pm"), or a title, call `mcp__runa__meetings_upcoming` and pick the match. If more than one fits, list them one per line and ask which.
- If the user names a person or company and no meeting, prep for that person or company and skip the calendar.
- When `selection_method.source` is `calendar_sync`, the list is the desktop app's last sync rather than a live calendar read. Say so in one line, with the time from `as_of`.

## 2. Gather

The user is the attendee with `is_self: true`. Attendees whose email domain differs from the user's are external.

For each external attendee, call `mcp__runa__runa_lookup_person` with their email or name: role, company, known facts, and recent meetings.

For each external company, keyed by email domain (skip personal domains such as gmail.com and outlook.com):

1. `mcp__runa__runa_get_company_memory` with the domain: dated facts such as deal stage, plus the relationship dossier with open commitments, open questions, and the meeting timeline.
2. `mcp__runa__meetings_get_last_matching` with the domain as `participant_query`: the last conversation, its decisions, and its action items.
3. `mcp__runa__runa_query_organizational_memory` with a `question` about open commitments, `intent: "commitments"`, and the company as `entity`: what each side committed to and hasn't closed.
4. `mcp__runa__runa_search_everything` with the company name and `limit_per_source: 3`: recent email, CRM records, chat, and documents. Use what bears on the meeting and leave out sources that returned nothing.

For an internal meeting, work from the topic instead: `mcp__runa__meetings_get_last_matching` with the meeting's subject as `topic_query`, `mcp__runa__runa_query_organizational_memory` with a `question` about decisions and blockers and the project or topic as `entity`, and `mcp__runa__runa_list_action_items` with each attendee as `owner`.

## 3. Write the brief

Write it as Markdown in this shape, not inside a code block:

```
[Meeting title] · [day and time] · [join link]

Who
- [Name], [title] at [company]: [one line on the relationship]

Where it stands
[Two or three sentences.]

Open between you
- You owe: [task], due [date] ([source meeting])
- They owe: [task] ([source meeting])

Last time: [meeting title], [date]
- [Decision or outcome]

Worth raising
- [Suggestion]
```

## Rules

- Each fact comes from Runa or a connected app and links to its source: meetings, people, and companies carry a `url`. Your own suggestions go under "Worth raising" and read as suggestions.
- List every open commitment between the two sides. Walking in unaware of one is the failure that matters most.
- If Runa has no history with these people, say so in one line and brief from the calendar entry and connected apps alone. Don't fill gaps with general knowledge about the company.
- Keep it to one screen. Cut anything the user wouldn't use in the room.
