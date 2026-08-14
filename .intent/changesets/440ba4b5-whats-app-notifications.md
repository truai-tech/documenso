# WhatsApp Notifications

## Summary

Add WhatsApp as a supported notification channel alongside email. Users with a Documenso account can choose in their account settings whether they receive all notifications via email, WhatsApp, or both. For external recipients (without an account), senders can enter a phone number per recipient in the document editor — Documenso then sends a WhatsApp message in addition to the standard email. All existing notification events are supported over WhatsApp. A WhatsApp Business API provider (e.g. Meta Cloud API or Twilio) must be configured by the instance administrator.

## Acceptance Criteria

- A "Notifications" section appears in user account settings with a channel choice: Email only, WhatsApp only, Email and WhatsApp
- When WhatsApp is selected, a phone number field becomes visible and is required to activate WhatsApp delivery
- Users with "WhatsApp only" receive no email notifications; those with "Email and WhatsApp" receive both
- If WhatsApp delivery fails or no phone number is saved, email is used as a fallback
- A phone number field appears on each recipient row in the document editor (optional)
- If a phone number is entered for an external recipient, a WhatsApp message is sent in addition to the standard email
- All current notification events (signing invitation, reminder, signed confirmation, bulk send complete, org member joined/left, document completed/cancelled) are delivered via WhatsApp when applicable
- Signing links in WhatsApp messages are the same unique links as in emails
- When no WhatsApp Business API provider is configured, the WhatsApp channel option is hidden from all user settings
- The instance administrator can configure WhatsApp provider credentials in the platform's environment settings

## Testing Notes

- Test each notification event with channel set to Email only, WhatsApp only, and Both — verify correct delivery in each case
- Test fallback to email when phone number is missing or WhatsApp delivery fails
- Test that an external recipient with a phone number entered by the sender receives both email and WhatsApp
- Test that an account-holder recipient's own preference overrides the phone number entered by the sender
- Test that WhatsApp channel option is not visible in settings when no provider is configured
- Test international phone numbers with various country codes
- Test reminder delivery via WhatsApp on the same schedule as email reminders

## Key Decisions

- WhatsApp is positioned as an additional channel, not a replacement — email addresses remain required for all recipients
- External recipients (no account) always receive both email and WhatsApp if a phone number is provided; only account holders can opt for WhatsApp-only
- Phone numbers for external recipients are stored per document, not globally reused
- Provider configuration is an admin-level task; no WhatsApp messages are sent until a valid provider is in place


## Specs

- [WhatsApp Notifications](../specs/7a251250.md)

---

[View in Intent](https://app.onintent.build/?project=552b1495-0e82-4b09-af19-678c73ecf992&changeset=440ba4b5-fde8-439e-bdf0-8365b147f58d&tab=detail)