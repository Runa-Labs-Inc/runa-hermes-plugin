---
name: follow-up-email
description: Draft the follow-up email after a meeting recorded in Runa, from its decisions and action items. Use when the user asks for a follow-up, recap email, or thank-you note after a meeting ("write the follow-up for my Acme call", "draft a recap to the team from this morning's meeting").
compatibility: Needs the Runa MCP server (https://mcp.joinruna.com/mcp) connected in Hermes as `runa`, signed in to a Runa account.
---

# Follow-up email

Runa is read-only, so this skill writes the draft. Show it to the user first, and send it only once they approve the draft and its recipients and you have a tool for it. Never say an email was sent when it wasn't. Everything comes from the Runa tools (`mcp__runa__*`). If they aren't available, or a Runa tool says to reconnect, tell the user how to connect Runa and stop: the first time, `hermes mcp add runa --url https://mcp.joinruna.com/mcp --auth oauth --connect-timeout 300`; after that, `hermes mcp login runa`. Either way, a new session then loads the tools.

## 1. Read the meeting

Find the meeting the way the user named it: `mcp__runa__meetings_get_last` for the one they just had, or `mcp__runa__meetings_get_last_matching` for one with a person, company, or topic. Read the summary and the action items, and check the transcript for names, numbers, and dates said aloud.

## 2. Decide who it's for

The recipients are the other attendees unless the user names someone else. When attendees outside the user's email domain were there, write to them. When everyone was internal, write a team recap. If it's mixed and the user didn't say, ask once.

## 3. Draft

- Subject: specific to the meeting, such as "Acme renewal: next steps".
- First line: what the meeting was, in one sentence.
- What was agreed: the decisions, in the meeting's own terms.
- Next steps: each action item with its owner and date, on both sides.
- Open questions, if any.
- Sign off with the user's first name.

Keep a customer email under 200 words unless the meeting covered a lot. Use plain sentences, no headings inside the email, and no filler such as "I hope this finds you well". Include nothing that wasn't said in the meeting: no new promises, prices, or dates.

After the draft, note anything you left out on purpose, such as internal-only remarks, so the user can decide whether it belongs.
