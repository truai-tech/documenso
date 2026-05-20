# WhatsApp Notifications

## Intent

Users currently have no choice about how they receive notifications — all alerts arrive by email. This feature gives registered users the ability to switch to WhatsApp as their preferred notification channel, so they can receive signing-related alerts in the messaging app they use most.

## Summary

Registered users can choose between **email** and **WhatsApp** as their preferred notification channel. The chosen channel applies to all document workflow notifications. Account security and authentication messages (e.g. password reset, email confirmation, 2FA codes) are always delivered by email regardless of this setting.

Recipients who do not have a Documenso account always receive notifications by email only.

## How It Works

### Notification preference setting

In the user's account settings, a **Notifications** section lets users pick their preferred channel:

- **Email** (default) — no change from current behaviour
- **WhatsApp** — requires the user to provide and verify a mobile phone number

When a user selects WhatsApp, they are prompted to enter a phone number (in international format). A one-time verification code is sent to that number via WhatsApp. The user enters the code to confirm ownership before the preference is saved. Until the number is verified, the channel stays as email.

Users can switch back to email at any time. Changing the preference takes effect immediately for all future notifications.

### Notifications delivered via WhatsApp

When a registered user's preference is set to WhatsApp, the following notifications are sent as WhatsApp messages instead of emails:

| Event | Who receives it |
|---|---|
| Document sent for signing | The registered user as a recipient |
| Signing reminder | The registered user as a recipient |
| Signing link expired | The registered user as a recipient |
| Removed from a document | The registered user as a recipient |
| Document completed | The document owner and all registered recipients |
| Document pending (waiting for others) | The document owner |
| A recipient has signed | The document owner |
| Document cancelled | The document owner and all registered recipients |
| Document rejected | The document owner and the rejecting recipient |
| Bulk send completed | The document owner |
| Team / organisation invitation | The invited registered user |
| Team / organisation deletion | Affected registered users |

WhatsApp messages contain the same essential information as their email equivalents — document name, relevant names, and a link to take action — formatted as plain text appropriate for a messaging app.

### Notifications that always use email

The following are always sent by email, regardless of the user's notification preference, because they are security-sensitive or required for account access:

- Email address confirmation
- Password reset and forgot-password flows
- Two-factor authentication codes
- Account created (admin-triggered)

## Included / Not Included

**Included**
- A notification preference setting (Email or WhatsApp) in user account settings
- Phone number entry and WhatsApp verification flow
- WhatsApp delivery of all document workflow notifications listed above for registered users who opt in
- Ability to switch back to email at any time

**Not Included**
- WhatsApp notifications for recipients who do not have a Documenso account
- The ability to send WhatsApp notifications to document recipients on a per-document basis
- Support for other messaging channels (e.g. SMS, Telegram)
- Per-notification-type channel selection (the chosen channel applies to all workflow notifications)
