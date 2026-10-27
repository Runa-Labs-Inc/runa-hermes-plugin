# Runa

Runa brings your meetings into Hermes Agent. Prepare for what's next, recap what was decided and who owes what, draft the follow-up, and find exactly what was said, with every answer linked back to its meeting in Runa.

## What you can ask

- "Prep me for my next meeting."
- "What did we decide on the Acme call, and what do I owe them?"
- "Write the follow-up for this morning's meeting."
- "Did Riley actually agree to the October 1 date?"
- "What did I miss this week?"

## What's included

**Skills**

- **Meeting prep**: a one-screen brief before a meeting, covering who's coming, where things stand, what's owed in each direction, and what to raise.
- **Meeting recap**: decisions, action items, and open questions for one meeting, a person, a topic, or a stretch of time.
- **Follow-up email**: a draft built from the meeting's decisions and action items. Runa never sends it.
- **Find what was said**: the verbatim quote, speaker, and timestamp behind a claim, and whether a decision was later changed.

**The Runa MCP server** (`https://mcp.joinruna.com/mcp`) searches and reads your meetings, transcripts, summaries, action items, and documents in Runa, the people and companies you meet with, your upcoming meetings, and the apps you've connected to Runa: HubSpot, Salesforce, Gmail, Outlook, Google Drive, Slack, Linear, Notion, Basecamp, and Asana.

## Get started

1. Install the plugin:

   ```
   hermes plugins install runa --enable
   ```

2. Connect your Runa account:

   ```
   hermes mcp add runa --url https://mcp.joinruna.com/mcp --auth oauth --connect-timeout 300
   ```

   Hermes opens Runa's sign-in in your browser. Sign in with Google or Microsoft; your first sign-in creates your Runa account. When Hermes lists Runa's tools, keep them all.

3. Start a new session and ask about a meeting. Runa records meetings with its desktop app, available at [downloads.joinruna.com](https://downloads.joinruna.com), so a new account has nothing to search until its first recording.

If a Runa tool later asks you to reconnect, run `hermes mcp login runa`. Scheduled and unattended runs can't open a browser, so they report it instead.

## Data and privacy

The plugin is read-only. Hermes reads your Runa data through the Runa MCP server, signed in as you, and sees only what you can already open in Runa, within your organization's policy for AI assistants. It can't record or join meetings, send messages, or change anything in Runa or in your connected apps. Audio recordings and voice profiles are never shared.

What Hermes reads from Runa joins your conversation, so it reaches the model provider your Hermes profile uses. Hermes keeps the Runa sign-in in your profile and refreshes it. The plugin has no code of its own, only these four skills and the server address, and it stores nothing.

To disconnect, switch off Hermes Agent in Runa under Settings → Connectors → AI assistants, which ends its access. `hermes mcp remove runa` and `hermes plugins disable runa` only remove it from Hermes.

[Privacy policy](https://docs.joinruna.com/legal/privacy) · [Terms](https://docs.joinruna.com/legal/terms) · Help: [admin@joinruna.com](mailto:admin@joinruna.com)
