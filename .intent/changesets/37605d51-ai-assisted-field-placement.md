# AI-Assisted Field Placement

## Summary

Introduces an AI-assisted field placement capability to the document editor. Senders who upload a PDF can trigger an AI analysis that automatically proposes which fields to place, where to place them, and which recipient each field belongs to. Proposed fields appear as ghost overlays on the canvas; the sender reviews, adjusts, and accepts or rejects each one individually or in bulk before they become real fields.

The changeset is split into two specs:
1. **Suggestion Generation** — how the PDF is analysed (page-to-image rendering, AI vision model invocation via Google Vertex AI, response validation, transient suggestion storage).
2. **Suggestion Review** — how the sender inspects and acts on suggestions inside the editor (ghost-field canvas overlay, review panel, individual and batch accept/reject, drag/resize/reassign before accepting, undo support).

## Testing Notes

1. Upload a multi-page, multi-recipient document with visible signature blocks and form fields → trigger AI suggestions → verify that ghost fields appear on the correct pages with the correct field types and recipient assignments.
2. Accept a single ghost field → confirm it converts to a solid-bordered real field and is saved via autosave → verify it behaves identically to a manually placed field.
3. Drag a ghost field to a new position before accepting → accept → confirm the field is saved at the adjusted position, not the original AI-suggested position.
4. Click "Accept all" on a set of suggestions → confirm all ghosts convert to real fields in a single batch and a success toast is shown.
5. Reject all remaining suggestions via "Dismiss suggestions" with ≥ 5 pending → confirm a confirmation prompt appears before they are discarded.
6. Replace the PDF after suggestions have been generated → confirm the suggestion set is cleared and the AI button resets.
7. Remove a recipient that has pending suggestions → confirm the affected suggestions are discarded.
8. Trigger analysis on a plan that does not include AI features → confirm the upgrade prompt is shown instead.
9. Simulate an AI API failure → confirm the error message is shown and the sender can retry without data loss.
10. Accept a suggestion, then undo (Ctrl+Z) → confirm the field is removed and the ghost reappears.

## Key Decisions

- **Suggestions are never auto-applied.** Every field requires an explicit accept action from the sender. This preserves the sender's control and avoids silently placing incorrect fields.
- **Ghost fields live in a separate suggestion state**, not in the main field list, until accepted. This keeps the autosave and field-management logic clean — accepted suggestions enter the existing field creation path unchanged.
- **Google Vertex AI (Vision)** is used as the AI provider, consistent with the existing `@ai-sdk/google-vertex` integration already present in the codebase. Provider selection follows the established swappable-provider pattern.
- **Suggestions are stored server-side** against the document so they survive page refreshes and tab switches during review.
- **Confidence indicators** are derived from the model's structured output. They are informational only — they do not gate any accept/reject action.


## Specs

- [AI Field Placement — Suggestion Generation](../specs/cbc78e2e.md)
- [AI Field Placement — Suggestion Review](../specs/8477b79c.md)

---

[View in Intent](https://app.onintent.build/?project=552b1495-0e82-4b09-af19-678c73ecf992&changeset=37605d51-048a-4e09-af2b-363186d37534&tab=detail)