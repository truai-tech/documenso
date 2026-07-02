# WhatsApp Notifications via Meta Cloud API

## Summary

Adds WhatsApp as a parallel notification channel alongside email. Users can opt in from their profile settings by providing and verifying a phone number. Once enabled, all nine existing document event notifications (signing request, recipient signed, document completed, etc.) are also sent as WhatsApp messages via the Meta Cloud API. Email delivery is unaffected.

## Acceptance Criteria

- A phone number field and enable/disable toggle appear in Profile settings under a "WhatsApp Notifications" section
- Users must verify their phone number via a WhatsApp confirmation code before notifications go live
- All nine document notification events send a WhatsApp message to users who have a verified number and notifications enabled
- WhatsApp and email are dispatched in parallel — a WhatsApp failure does not affect email delivery
- Meta Cloud API credentials (phone number ID, access token) are configurable via server-side environment variables
- WhatsApp messages include the document title, event description, and a direct link

## Testing Notes

- Test the full phone verification flow including invalid/expired codes
- Verify that disabling the toggle immediately stops WhatsApp dispatch without affecting email
- Confirm all nine events trigger a WhatsApp message when a verified user is involved
- Test failure handling: Meta API errors must be logged but must not cause the parent job to fail
- Test with an unverified or missing phone number — no WhatsApp message should be sent
- Confirm messages are delivered in the user's language where a Meta-approved template exists, with English as fallback


## Specs

- [WhatsApp Notifications via Meta Cloud API](../specs/1146f9ed.md)

---

[View in Intent](https://app.onintent.build/?project=552b1495-0e82-4b09-af19-678c73ecf992&changeset=ac5db507-fd31-4f7c-b3df-9a9bc896fdd5&tab=detail)