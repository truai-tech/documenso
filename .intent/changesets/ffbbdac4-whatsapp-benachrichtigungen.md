# WhatsApp-Benachrichtigungen

## Summary

Nutzer können in ihren Profileinstellungen eine verifizierte Handynummer hinterlegen und zwischen drei Benachrichtigungskanälen wählen: nur E-Mail (Standard), nur WhatsApp oder beides. Alle dokumentenbezogenen Benachrichtigungen (Einladungen, Erinnerungen, Abschlüsse, Ablehnungen etc.) werden dann entsprechend der persönlichen Einstellung per WhatsApp zugestellt — ergänzend oder als Ersatz für die bisherigen E-Mails. Der Versand erfolgt direkt über die Meta Cloud API (WhatsApp Business Platform).

## Testing Notes

1. Handynummer in den Profileinstellungen eintragen → Verifizierungscode per WhatsApp erhalten und eingeben → Nummer gilt als verifiziert
2. Kanalauswahl auf „Nur WhatsApp" setzen → Dokument zum Unterzeichnen versenden → Empfänger erhält WhatsApp-Nachricht, keine E-Mail
3. Kanalauswahl auf „E-Mail und WhatsApp" setzen → Dokument abschließen → Empfänger erhält sowohl E-Mail als auch WhatsApp-Nachricht
4. Kanalauswahl auf „Nur WhatsApp" ohne hinterlegte Handynummer versuchen → Auswahl ist gesperrt, Hinweis wird angezeigt
5. Verifizierte Handynummer entfernen → Kanaleinstellung fällt automatisch auf „Nur E-Mail" zurück
6. Passwort-Reset anfordern → Nachricht kommt ausschließlich per E-Mail, unabhängig von der Kanaleinstellung

## Key Decisions

- **Meta Cloud API direkt** (kein Drittanbieter wie Twilio) — geringere Kosten pro Nachricht, mehr Kontrolle, aber erfordert vollständige Meta-Unternehmensverifizierung und eigene Telefonnummer.
- Externe Unterzeichner ohne Documenso-Konto sind ausgeschlossen, da sie keine Profileinstellungen haben und keine Handynummer hinterlegen können.
- Konto- und sicherheitsbezogene E-Mails (Passwort-Reset, E-Mail-Bestätigung etc.) bleiben bewusst auf E-Mail beschränkt.
- Die Handynummer-Verifizierung per WhatsApp-Code erfüllt gleichzeitig die von Meta geforderte Opt-in-Pflicht.
- Jeder Benachrichtigungstyp benötigt eine vorab von Meta genehmigte Nachrichtenvorlage (*Utility Message Template*) — ohne Genehmigung kann der jeweilige Typ nicht gesendet werden.


## Specs

- [WhatsApp-Benachrichtigungen](../specs/99b2904b.md)

---

[View in Intent](https://app.onintent.build/?project=552b1495-0e82-4b09-af19-678c73ecf992&changeset=ffbbdac4-63ba-413d-88e3-c1ed669585b4&tab=detail)