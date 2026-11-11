---
name: connect-help
description: Help the user connect a mailbox to MCPEmails, or fix a failed connection. Use when the user asks how to connect Gmail, Google Workspace, Outlook, Microsoft 365, iCloud, Yahoo, Fastmail, Zoho, Yandex, GMX, a web-host mailbox on their own domain or any IMAP account, asks for an app password, or when an MCPEmails tool reports that no inbox is connected or a login was refused.
---

# Connect help

Goal: the user's mailbox connected in the MCPEmails dashboard, with the provider's own app-password steps given exactly, and the right guide page linked.

## Steps

1. **Check first.** Call `inbox_list`. If it returns inboxes, the connector is working; ask which mailbox the user wants to add. If the call fails because the user is not signed in, tell them to connect the MCPEmails connector (Claude: the plugin's Connectors tab; Cursor: approve the `mcpemails` MCP server) and complete the browser sign-in at mcpemails.com, then try again.
2. **Identify the provider** from the address or what the user says, and give the matching steps below. Mailboxes are connected in the MCPEmails dashboard after signing in at https://mcpemails.com; the chat cannot connect a mailbox or take a password. Never ask the user to paste a password or app password into the chat.
3. **Link the guide.** Every provider has a page at `https://mcpemails.com/connect/<slug>` with screenshots and the exact server settings. Slugs: `gmail`, `google-workspace`, `outlook`, `office365`, `icloud`, `yahoo`, `fastmail`, `zoho`, `yandex`, `gmx`, `web-de`, `proton`, `hey`, `aol`, `mail-ru`, `t-online`, `ionos`, `one-com`, `hostinger`, `godaddy`, `namecheap`, `siteground`, `cpanel`, `plesk`, `imap` (anything else). The full list is at https://mcpemails.com/connect, and the provider matrix at https://mcpemails.com/docs/providers.
4. **After connecting**, call `inbox_list` again to confirm the mailbox is there, then suggest a first task ("want me to triage it?").

## Provider steps

**Gmail and Google Workspace** (app password over IMAP; MCPEmails does not use Sign in with Google for Gmail)
- 2-Step Verification must be on for the Google account. Then open https://myaccount.google.com/apppasswords, create an app password (any name), and copy the 16 characters.
- In the MCPEmails dashboard choose Gmail, enter the Gmail address and the app password.
- Workspace: if the app-password page is missing, the admin has to allow 2-Step Verification and less secure app access is not involved; the guide at `/connect/google-workspace` lists the admin settings.
- Guide: https://mcpemails.com/connect/gmail

**Outlook.com, Hotmail, Live, MSN** (Sign in with Microsoft)
- In the dashboard choose Outlook and sign in with the Microsoft account; no password is typed into MCPEmails. Microsoft shows an account picker: pick the mailbox you mean.
- Guide: https://mcpemails.com/connect/outlook

**Microsoft 365 work or school** (Sign in with Microsoft)
- Same flow. If Microsoft shows "Need admin approval", an IT admin has to approve the app once for the organization: https://mcpemails.com/docs/microsoft-365-admin-consent
- A Microsoft account without an Exchange Online mailbox cannot connect.
- Guide: https://mcpemails.com/connect/office365

**iCloud Mail** (Apple app-specific password)
- Two-factor authentication must be on. At https://account.apple.com go to Sign-In and Security, then App-Specific Passwords, and create one.
- In the dashboard choose iCloud, enter the iCloud address (the full `@icloud.com`, `@me.com` or `@mac.com` address) and that password. A wrong-address refusal usually means a different Apple ID email was typed in than the iCloud Mail address.
- Guide: https://mcpemails.com/connect/icloud

**Yahoo Mail** (app password)
- At https://login.yahoo.com/account/security choose Generate app password, name it, copy it.
- In the dashboard choose Yahoo, enter the Yahoo address and the app password.
- Guide: https://mcpemails.com/connect/yahoo

**Fastmail** (app password only; Fastmail has no OAuth connection in MCPEmails)
- In Fastmail: Settings, Privacy & Security, Integrations, New app password, access Mail (IMAP/SMTP).
- In the dashboard choose Fastmail, enter the address and the app password.
- Guide: https://mcpemails.com/connect/fastmail

**Zoho Mail** (IMAP access plus app password)
- In Zoho Mail settings, Mail Accounts, enable IMAP Access. Then at https://accounts.zoho.com under Security, App Passwords, generate one.
- Guide: https://mcpemails.com/connect/zoho

**Yandex Mail** (IMAP plus app password)
- Enable IMAP in Yandex Mail settings (Email clients). At https://id.yandex.com/security create an app password for Mail.
- Guide: https://mcpemails.com/connect/yandex

**Any other IMAP/SMTP mailbox** (own domain, web host, ISP)
- Find the IMAP and SMTP host names, ports and security with the free lookup at https://mcpemails.com/tools/email-settings-lookup, or from the host's control panel (cPanel and Plesk show them under Email Accounts, Connect Devices).
- In the dashboard choose IMAP, enter the address, the mailbox password (or an app password if the host offers one), the IMAP host and port (usually 993 with SSL) and the SMTP host and port (usually 465 with SSL or 587 with STARTTLS).
- Guide: https://mcpemails.com/connect/imap, and the host-specific pages above.

## Fixing a refused login

- "Login refused" with a correct app password: check the username form. Most hosts want the full address; Apple documents the name part, and some web hosts want the full address even when the control panel shows a short name.
- Gmail without an app-password page: 2-Step Verification is off, or a Workspace admin blocks app passwords. There is no other way to connect Gmail by password.
- Some providers refuse connections from cloud addresses. MCPEmails routes those through its own relay automatically; nothing for the user to change. If it still fails, email support@mcpemails.com with the provider name and the exact error text, never the password.
- Help center: https://mcpemails.com/help

## Rules

- Never request, accept or repeat a password or app password in the chat. The dashboard is the only place to enter one.
- Fastmail connects with an app password, not OAuth. Gmail connects with a Google app password. Do not claim otherwise.
- Do not promise features of a plan or quote prices; link https://mcpemails.com/pricing if asked.
