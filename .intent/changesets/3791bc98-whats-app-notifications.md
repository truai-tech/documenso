# WhatsApp Notifications

## Summary

Users can opt into WhatsApp notifications by adding and verifying a phone number in their personal profile settings. Once verified via a one-time code sent through the WhatsApp Business API, they receive WhatsApp messages for all document events that currently trigger email notifications. The feature is additive — email notifications are not replaced.

## Acceptance Criteria

- A WhatsApp Notifications section is available on the user's profile settings page
- Users can enter an international phone number and trigger a verification code sent via WhatsApp
- Entering the correct code marks the number as verified and activates WhatsApp notifications
- Changing the phone number requires re-verification before the new number becomes active
- Users can toggle WhatsApp notifications on/off without removing their number
- WhatsApp messages are sent for all document events (signing request, signed, completed, pending, deleted, removed, expired, created from template)
- Per-document email toggle settings suppress the corresponding WhatsApp notification as well
- Only registered users with a verified number receive WhatsApp notifications (non-account recipients are excluded)

## Testing Notes

- Verify OTP flow: correct code activates, wrong code rejects, expired code prompts re-send
- Verify that disabling the toggle stops WhatsApp messages without clearing the phone number
- Verify that changing the number keeps the old number active until the new one is verified
- Verify that suppressing an email event type on a document also suppresses the WhatsApp message for that event
- Test with international phone numbers in various formats
- Test WhatsApp Business API error handling (invalid number, delivery failure)


## Specs

- [WhatsApp Notifications](../specs/0d1c5292.md)

---

[View in Intent](https://app.onintent.build/?project=552b1495-0e82-4b09-af19-678c73ecf992&changeset=3791bc98-84ed-4190-82c4-0987df41af25&tab=detail)