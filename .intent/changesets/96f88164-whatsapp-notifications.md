# WhatsApp Notifications

## Summary
Allow registered Documenso users to opt in to WhatsApp as their notification channel instead of email. Users add and verify a phone number in their account settings, after which all document workflow notifications are delivered via WhatsApp. Recipients without a Documenso account continue to receive email only. Account security emails (password reset, 2FA, etc.) are always sent by email regardless of preference.

## Testing Notes
1. Go to account settings → Notifications, select WhatsApp, enter a valid phone number, and verify the one-time code — the preference should save and a confirmation should appear.
2. With WhatsApp enabled, trigger a signing request as a registered recipient and confirm the notification arrives on WhatsApp, not email.
3. With WhatsApp enabled, complete a document as the owner and confirm the "document completed" message arrives via WhatsApp.
4. Trigger a password reset for an account with WhatsApp enabled — the reset link should still arrive by email.
5. Switch preference back to email and confirm subsequent notifications revert to email delivery.
6. Add a non-registered recipient to a document with a registered WhatsApp-enabled sender — confirm the non-registered recipient still receives email.

## Key Decisions
- WhatsApp is a full channel replacement per user, not a per-notification toggle — simplifies the preference UI and delivery logic.
- Phone number verification via WhatsApp one-time code is required before the preference is saved, to prevent misconfigured numbers from silently dropping notifications.
- Security/auth emails remain email-only to ensure account recovery is never blocked by a messaging app dependency.


## Specs

- [WhatsApp Notifications](../specs/b1da0ff4.md)

---

[View in Intent](https://app.onintent.build/?project=552b1495-0e82-4b09-af19-678c73ecf992&changeset=96f88164-800b-4c62-b970-4edc21876324&tab=detail)