# WhatsApp Notifications

## Summary

Introduce WhatsApp as a second notification delivery channel alongside email. Document senders can opt in for themselves and set a per-recipient phone number + channel preference when setting up a document. All document workflow notifications (signing invitations, reminders, completion, rejection, cancellation, and more) are supported via WhatsApp. Delivery uses the Meta Cloud API, with automatic fallback to email if WhatsApp delivery fails. Auth and security emails are unaffected and always use email.

## Acceptance Criteria

- A user can add and verify a phone number under Settings → Notifications and set their channel preference to Email only, WhatsApp only, or Both.
- A user can remove their WhatsApp number at any time, reverting to email delivery.
- When adding recipients to a document, the sender can optionally enter a phone number and select WhatsApp as the delivery channel for that recipient.
- If no phone number is given for a recipient, delivery defaults to email with no change in behaviour.
- Signing invitations, reminders, completion notifications, cancellation notifications, and all other document workflow events are delivered via WhatsApp when the channel is set to WhatsApp.
- Owner-side notifications (recipient signed, document completed, bulk send done, org/team member changes) respect the account holder's channel preference.
- WhatsApp messages include the sender name, document title, required action, and a direct link. No PDF is attached.
- If WhatsApp delivery fails, the system falls back to email and records the failed attempt.
- The Meta Cloud API credentials (Business Account ID, Phone Number ID, access token) are configurable via environment settings; the feature is disabled until credentials are set.
- Account and security emails (email confirmation, password reset, admin account creation) always use email regardless of any WhatsApp preference.

## Testing Notes

- Test WhatsApp delivery with a verified Meta Cloud API sandbox number before going live.
- Verify fallback behaviour by attempting delivery to an invalid or non-WhatsApp number.
- Confirm that opting into WhatsApp does not suppress email for account/security flows.
- Test channel preference changes mid-document (e.g. changing preference after a signing invitation has already been sent).
- Verify that removing a WhatsApp number immediately reverts delivery to email for subsequent notifications.

## Key Decisions

- **Meta Cloud API only** — no other WhatsApp provider (Twilio, 360dialog) is in scope for this release.
- **Single system-level Meta credentials** — organisations cannot configure their own WhatsApp Business accounts; one set of credentials covers the whole installation.
- **No PDF attachments via WhatsApp** — documents are accessed through the signing link, keeping messages lightweight and within Meta template constraints.
- **Recipient phone number is per-document only** — it is not saved to a reusable address book, keeping the data model simple.
- **Pre-approved Meta templates** — a single set of message templates is used across all organisations; per-organisation branded templates are out of scope.


## Specs

- [WhatsApp Notifications](../specs/6f91af87.md)

---

[View in Intent](https://app.onintent.build/?project=552b1495-0e82-4b09-af19-678c73ecf992&changeset=189d8a7c-bf30-46ab-adef-129c020e55f2&tab=detail)