# Trade Email Trail: Design

Date: 2026-11-17
Status: draft, awaiting approval

## Intent and why

The trade team currently handles applicant email in the `trade@urbandeco.co.uk` webmail inbox. The app sends four emails (application received, team alert, approval, rejection), stores none of them, and cannot see replies. The merchant has to switch to webmail to follow a conversation and has no history inside the app next to the application.

Add an **Emails** page to the embedded Shopify app where the merchant can:

- see every applicant with their latest email,
- open an applicant and read the whole trail,
- reply from the app, sent from `trade@urbandeco.co.uk`.

Applicant replies must reach the app, and the team must still see every reply in the `trade@` webmail inbox.

## Goals

- One page listing applicants by most recent email, with an unread indicator.
- One thread view per applicant showing every email in order, latest highlighted, older ones collapsible.
- Reply form in the thread view. Replies send from `trade@`, are logged, and appear in the thread.
- Applicant replies saved automatically through a Resend inbound webhook.
- A copy of every applicant reply sent to `trade@` so webmail stays complete.
- All app-sent applicant emails (received, approval, rejection) logged from now on.
- Link from the trade application detail page to its thread.

## Non-goals (version 1)

- Attachments, inbound or outbound. Inbound attachments are noted by name only if Resend supplies it; they remain visible in the webmail copy.
- Reading the webmail mailbox (IMAP), importing past emails, or capturing replies the team writes directly in webmail.
- Rich-text or HTML composing. Replies are plain text, rendered as HTML paragraphs on send.
- Multiple team members' assignments, tags, internal notes, or search across all message bodies.
- Emails to anyone other than applicants. Team alert emails are not logged.

## Anti-goals

- Applicants must never see an odd or broken reply experience: replies sent from the app must come from `trade@`, with the subject threaded as `Re: ...`.
- The team must never lose a reply: a failure in saving or forwarding must not silently drop an inbound email.
- Inbound email HTML must never be rendered raw in the admin UI.

## Constraints

- Stack: React Router (flat routes, `@react-router/fs-routes`), Prisma + PostgreSQL, Resend, Shopify App Bridge UI (`s-*` web components) as in existing routes.
- Applicant emails are stored lowercase at intake (`app-proxy.application.jsx`), so matching is by lowercase comparison.
- Sending already works from `trade@urbandeco.co.uk` (domain verified in Resend). `EMAIL_FROM` already defaults to `Urban Deco Trade <trade@urbandeco.co.uk>`.
- Emails sent before this ships keep reply-to `trade@`, so their replies will not reach the app.
- A Resend inbound address and a publicly reachable webhook URL are required. The webhook route lives outside `/app` so it needs no Shopify admin session.
- The `trade@` webmail is on cPanel. No change is made there.

## Approaches considered

| Approach | What it is | Trade-off | Decision |
| --- | --- | --- | --- |
| A. Resend inbound + copy to webmail | Reply-to is a Resend inbound address. A webhook saves the reply, then the app sends a copy to `trade@`. | Same pattern already working in the Stoneage enquiry app. No cPanel or mailbox access. Only captures replies after switch-on and not replies typed in webmail. | **Chosen** |
| A2. Resend inbound + cPanel forwarder | Reply-to stays `trade@`; cPanel forwards a copy to Resend inbound. | Needs a cPanel forwarder and a subdomain MX record. More setup for the same result. | Rejected |
| B. IMAP | App logs in to the `trade@` mailbox and reads mail. | Captures history and webmail replies, no DNS. Needs the mailbox password in the app and port 993 access. | Kept as a later option |

## Design

### Data model

New Prisma model `EmailMessage` (migration required):

| Field | Type | Notes |
| --- | --- | --- |
| `id` | String, cuid | |
| `applicationId` | String? | Relation to `TradeApplication`. Null means unmatched. |
| `direction` | Enum `OUTBOUND` or `INBOUND` | |
| `kind` | String | `APPLICATION_RECEIVED`, `APPROVAL`, `REJECTION`, `MANUAL_REPLY`, `APPLICANT_REPLY` |
| `fromEmail` | String | |
| `toEmail` | String | |
| `subject` | String | |
| `bodyText` | String | Cleaned text for inbound; the sent text for outbound. |
| `providerMessageId` | String, unique | Resend email id. Used for de-duplication. |
| `readAt` | DateTime? | Inbound only. Set when the thread is opened. |
| `createdAt` | DateTime | Default now |

Indexes: `applicationId`, `createdAt`, `readAt`. `TradeApplication` gains `emailMessages EmailMessage[]`.

### Outbound logging

In `app/services/email.server.js`:

- After each successful send of the received, approval and rejection emails, write an `EmailMessage` row (`OUTBOUND`). A logging failure must not fail the send; it is logged to the console.
- All applicant-facing emails set `replyTo` to the `EMAIL_REPLY_TO` environment variable (the Resend inbound address). If it is unset, fall back to `trade@` so nothing breaks.
- New `sendTradeReply({ application, subject, body })`: sends from `EMAIL_FROM` to the applicant with reply-to as above, subject prefixed `Re: ` if missing, branded plain layout, then logs a `MANUAL_REPLY` row and returns the saved message.

### Inbound webhook

New route `app/routes/webhooks.resend.jsx` (public, POST only), modelled on Stoneage `routes/webhooks.js`:

1. Read the raw body and verify the signature (`svix-id`, `svix-timestamp`, `svix-signature`) against `RESEND_WEBHOOK_SECRET`. Missing or invalid signature returns 400.
2. Ignore any event other than `email.received` with a 200.
3. Fetch the full message from Resend (`resend.emails.receiving.get(email_id)`).
4. Extract the sender address, lowercase it, and find the most recent `TradeApplication` with that email.
5. If a row with the same `providerMessageId` exists, return 200 (duplicate).
6. Take the body text (fall back to a text conversion of HTML), strip the quoted chain (port of `cleanEmailReply`: "On ... wrote:", "Original Message", `>` lines, `From/Sent/To/Subject` blocks).
7. Save an `INBOUND` / `APPLICANT_REPLY` row. No match: save it with `applicationId` null so it appears under Unmatched.
8. Send a copy to `REPLY_COPY_TO` (default: the address in `EMAIL_FROM`) from `EMAIL_FROM`, with `replyTo` set to the applicant's address, a subject like `Reply from <business name>: <subject>`, the cleaned body, and a link to the thread. A forward failure is logged and does not fail the webhook. The copy never goes to the inbound address, so there is no loop.
9. Return 200. Any error before the row is saved returns 500 so Resend retries.

### Pages

Navigation: add an `Emails` link to the `<s-app-nav>` in `app/routes/app.jsx`, after Trade Applications.

**List page** (`app/routes/app.emails._index.jsx`), one row per applicant that has at least one message, ordered by latest message:

```
Emails                                          [Unmatched (2)]
Search: [______________]

 Applicant            Latest email                          When      
 ● Studio North Ltd   "Thanks, sending the portfolio now"   10:42     (inbound, unread)
   Ada Interiors      "Re: Your trade application"          Yesterday (outbound)
```

- Unread dot when the applicant has an inbound message with `readAt` null.
- Direction shown on the snippet ("You:" for outbound).
- Search matches applicant name, business name and email.
- Unmatched shows saved messages with no application, with an assign-to-application action.

**Thread page** (`app/routes/app.emails.$applicationId.jsx`):

```
Studio North Ltd · Ada Smith · ada@example.com        [View application]

 ▸ 3 earlier emails (expand)
 ┌ Latest: Applicant · 10:42 ──────────────────────┐
 │ Thanks, sending the portfolio now                │
 └──────────────────────────────────────────────────┘
 Reply
 Subject: [Re: Your trade application]
 [ textarea                                        ]
                                         [Send reply]
```

- Chronological, oldest first, latest expanded, earlier ones collapsed with an expand-all control.
- Opening the page marks that applicant's inbound messages read.
- Reply form posts to the route action, calls `sendTradeReply`, then revalidates. Validates non-empty body, shows an error on send failure, disables the button while sending.
- All message bodies render as escaped text with line breaks preserved.

Link: the trade application detail page (`app.trade-applications.$id.jsx`) gets an "Email trail" link to its thread.

### Configuration

| Variable | Purpose |
| --- | --- |
| `RESEND_API_KEY` | existing |
| `EMAIL_FROM` | existing |
| `EMAIL_REPLY_TO` | new: Resend inbound address (Resend-provided or on a `reply.` subdomain) |
| `RESEND_WEBHOOK_SECRET` | new: signing secret from the Resend webhook |
| `REPLY_COPY_TO` | new, optional: where reply copies go; defaults to the `EMAIL_FROM` address |

### Setup outside the code (user)

1. In Resend, enable receiving and note the inbound address.
2. If using a custom address, the DNS team adds the MX record on the subdomain only, never the root domain.
3. After deploy, add a Resend webhook for incoming email pointing at `<app url>/webhooks/resend` and set `RESEND_WEBHOOK_SECRET`.

The full step-by-step guide is in the Claude Docs document "Trade Email Trail: Setup Guide (Forwarding vs IMAP)".

## Testing

- Unit: the quoted-reply cleaner (Gmail, Outlook, Apple formats, `>` lines), sender matching (case, multiple applications), subject prefixing.
- Webhook: signature rejected, unknown event ignored, duplicate ignored, matched and unmatched saved, copy failure does not fail the request.
- Manual end to end: submit a test application, receive the "received" email, reply from the applicant address, confirm the reply appears in the thread and in `trade@` webmail, reply from the app, confirm delivery from `trade@`.

## Calls made by the designer (please check)

- One thread per applicant, not per subject line. Simple, and matches how the team talks about applicants.
- Match by sender email only (most recent application wins when an email applied more than once). Headers such as `In-Reply-To` are not used in version 1.
- Team alert emails are not logged, only applicant-facing ones.
- Plain-text only, no attachments in version 1.
- The copy goes to `trade@` by default; `REPLY_COPY_TO` can change this.

## Left to the builder

- Exact Prisma migration naming and the unmatched-assignment UI details.
- Polaris layout details within the existing `s-*` component conventions.
- Pagination size for the list page.
- Whether to convert inbound HTML to text with a small dependency or a simple tag stripper.
