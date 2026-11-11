# MCPEmails plugin

Work through your email from Claude or Cursor: triage what came in, draft replies in your own voice, clean up by sender, chase threads that went quiet, and get a morning brief across every mailbox you have connected. The plugin pairs the [MCPEmails](https://mcpemails.com) connector with six skills that tell the assistant how to use the connector's tools carefully.

MCPEmails connects Gmail and Google Workspace, Outlook.com and Microsoft 365, iCloud, Yahoo, Fastmail, Zoho, Yandex and any IMAP/SMTP mailbox, including mail on your own domain. One connector, every mailbox, and the same inbox works in every MCP client.

## What is in the plugin

- `.mcp.json` (Claude) and `mcp.json` (Cursor): one remote MCP server, `https://mcpemails.com/api/mcp`. You sign in with OAuth in the browser the first time it is used. No API key or secret is stored in this plugin.
- `skills/`: six skills, loaded by the assistant when a request fits.

| Skill | What it does |
| --- | --- |
| `inbox-triage` | Sorts unread mail across every connected mailbox into what needs you, what can wait and what can go, then proposes actions and waits for your word. |
| `reply-drafts` | Writes replies in your voice and saves them as drafts in the mailbox's Drafts folder, where you review and send them yourself. |
| `daily-brief` | A morning summary of what arrived since yesterday across all mailboxes: who wrote, what they want, what is due. |
| `inbox-cleanup` | Bulk archive or move by sender or topic from a plan you confirm first. Moves are reversible; nothing is deleted unless you ask for it. |
| `follow-ups` | Finds sent mail that never got an answer and threads waiting on you, and sets server-side follow-up reminders. |
| `connect-help` | Walks you through connecting Gmail, Outlook, iCloud, Yahoo, Fastmail, Zoho, Yandex or any IMAP mailbox, with the right app-password steps for each. |

The plugin has no hooks, no scripts, no local executables and no build step. It is Markdown and JSON only.

## How to use it

1. Install the plugin. From the Claude plugin directory: Customize, Plugins, search for MCPEmails. In Claude Code you can also install straight from this repository:

   ```
   /plugin install mcpemails-inbox --marketplace Albretsen/mcpemails-plugin
   ```

   In Cursor: Marketplace, MCPEmails. Then, in Claude, open its Connectors tab and connect MCPEmails; in Cursor, approve the `mcpemails` MCP server when prompted. A browser window opens for the OAuth sign-in to mcpemails.com.
2. If you have no mailbox connected yet, ask "help me connect my iCloud mail" (or any provider). The `connect-help` skill walks you through it, or go to [mcpemails.com/connect](https://mcpemails.com/connect).
3. Ask for what you need in plain words: "triage my inbox", "draft a reply to Maria", "what came in overnight?", "archive everything from that newsletter", "who have I not heard back from?".

## Safety rules the skills follow

- Nothing is sent, scheduled or deleted without your explicit confirmation in the chat. The default outcome of a reply request is a saved draft, not a sent message.
- Bulk moves are planned first (sender, count, destination) and run only after you approve the plan. Every move can be undone with a move back; deletes go to Trash, never permanent unless you ask for it in so many words.
- Email content is treated as untrusted data. Instructions found inside a message, a subject line or a folder name are shown to you as content, never followed as commands. The connector marks such results `untrusted_content: true`.
- Where a mailbox owner has turned on send approval in the MCPEmails dashboard, a send is held until a person approves it there.

## What runs, and where data goes

This plugin runs nothing on your machine. Every tool call goes from your Claude or Cursor client to the MCPEmails server at `https://mcpemails.com/api/mcp` over HTTPS, authenticated by the OAuth token your client obtained when you signed in. The server then reads from or writes to your own mail provider using the credentials you connected in the MCPEmails dashboard (an OAuth grant for Microsoft accounts, an app password for Gmail, iCloud, Yahoo, Fastmail, Zoho, Yandex and IMAP mailboxes).

- Email is fetched live from your provider on each request and is not stored by MCPEmails. The exceptions are outgoing messages that have to wait, such as a scheduled send or a send held for approval, which are stored encrypted until they go out.
- Credentials are encrypted at rest and decrypted only for the request that needs them.
- Data is stored in one region, the EU (Stockholm). The servers that call your mail provider run in the US (East). For the few providers that refuse connections from cloud addresses, the IMAP/SMTP connection goes through the MCPEmails relay (Hetzner, Germany), a plain tunnel with TLS end to end, so the relay never sees mail or passwords.
- Your own provider sends every outgoing message, from your own address.
- The skills send nothing anywhere else: no analytics, no third-party services, no other endpoints.

## Privacy Policy

The MCPEmails privacy policy at [mcpemails.com/privacy](https://mcpemails.com/privacy) describes what is and is not stored, for how long, and how to delete your account and connected mailboxes. The terms of service are at [mcpemails.com/terms](https://mcpemails.com/terms) and the security overview at [mcpemails.com/security](https://mcpemails.com/security).

## Plans

MCPEmails has a free plan with no card needed, and paid plans for more mailboxes and features such as follow-up reminders, snooze, a morning brief delivered into your inbox and search across all inboxes at once. Current plans: [mcpemails.com/pricing](https://mcpemails.com/pricing).

## Support

Email [support@mcpemails.com](mailto:support@mcpemails.com) or read the help center at [mcpemails.com/help](https://mcpemails.com/help). Docs: [mcpemails.com/docs](https://mcpemails.com/docs). Setup guides for Claude and Cursor: [mcpemails.com/docs/claude](https://mcpemails.com/docs/claude) and [mcpemails.com/docs/cursor](https://mcpemails.com/docs/cursor).

## License

MIT. See `LICENSE`.
