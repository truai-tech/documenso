# AI Field Placement — Suggestion Generation

## Intent

Senders who upload fresh or unfamiliar PDFs currently have to identify every signature block, date line, and input area by eye and then drag each field onto the document manually. This is slow and error-prone, especially for multi-page or multi-recipient documents. The AI suggestion-generation flow removes that cold-start problem by analysing the document automatically and proposing a complete set of field placements before the sender touches the editor.

---

## Summary

When a sender reaches the field-placement step of the document editor and recipients have already been added, they can trigger an AI analysis of the uploaded PDF. The system converts each page of the document to an image, sends those images together with the list of recipients to an AI vision model, and receives back a structured set of suggested fields — each with a type, position, size, and assigned recipient. These suggestions are stored transiently and passed to the review UI for the sender to inspect and act on.

---

## How It Works

### Entry point

A prominent **"Suggest fields with AI"** button appears in the field-placement step of the document editor. The button is shown when:

- At least one recipient with a signing or approving role (Signer or Approver) has been added.
- No AI analysis for this document version is already in progress.

The button is hidden or disabled if the document has no eligible recipients, or if an analysis is already running.

### Analysis trigger

Clicking the button initiates the analysis. The following happens server-side, initiated as a background operation so the sender does not block on it:

1. **PDF-to-image conversion** — each page of the stored document PDF is rendered to a JPEG image. The rendered scale produces images of sufficient resolution for the AI model to read fine print and identify form regions. Pages are rendered concurrently (up to the configured concurrency limit).

2. **Prompt construction** — a structured prompt is assembled that includes:
   - All rendered page images, in order.
   - The list of recipients by name, email, and role (e.g. "Alice Smith, alice@acme.com, Signer").
   - Instructions to identify regions that look like signature blocks, initials boxes, date fields, name fields, email fields, text inputs, checkboxes, radio groups, dropdown menus, and free-form signature areas.
   - Instructions to assign each suggested field to the most contextually appropriate recipient.
   - A requirement to return results as structured JSON using a defined schema (field type, page number, position as percentage of page width/height, width and height as percentages, recipient index).

3. **AI model invocation** — the prompt is sent to the configured AI provider (Google Vertex AI by default). The model returns a JSON array of field suggestions.

4. **Response validation and normalisation** — the raw model output is parsed against the field suggestion schema. Invalid or out-of-range entries (e.g. positions outside 0–100%) are discarded. Coordinates are expressed as percentages of page dimensions, matching the coordinate system already used for field storage.

5. **Suggestion persistence** — validated suggestions are stored transiently against the document (not yet committed as real fields). Each suggestion carries:
   - A unique suggestion ID (used for tracking individual accepts/rejections in the review UI).
   - Field type (Signature, Free Signature, Initials, Name, Email, Date, Text, Number, Checkbox, Radio, Dropdown).
   - Page number.
   - Position X and Y (percentage of page).
   - Width and Height (percentage of page).
   - Assigned recipient ID.
   - A confidence indicator (high / medium / low), derived from the model's structured output where available.
   - Status: `pending` (default on generation).

### While analysis is running

A loading state is shown in the button area. The sender can continue editing recipients or document metadata during analysis. The field canvas remains interactive. If the sender makes a change that would invalidate the analysis (e.g. removing a recipient), the in-progress or completed suggestion set is discarded and the button resets.

### Error handling

If the AI call fails or returns an unparseable response, the button resets with a brief error message ("Couldn't generate suggestions — please try again"). No partial suggestions are shown. The sender can retry at any time.

### Eligibility constraints

- The feature is only available on plans that include AI features. On ineligible plans the button shows a plan-upgrade prompt instead.
- Analysis runs on the currently stored PDF version. If the sender replaces the PDF after suggestions have been generated, the suggestion set is discarded.
- For multi-file envelopes, suggestions are generated per file; each file's suggestions appear in the review UI scoped to that file's pages.

---

## Included / Not Included

**Included**
- AI-powered analysis of the uploaded PDF using a vision model.
- Structured suggestion output covering all supported field types.
- Recipient assignment per suggestion based on document context.
- Confidence indicators on individual suggestions.
- Per-file suggestion generation for multi-file envelopes.
- Graceful error handling and retry.

**Not Included**
- Automatic acceptance of suggestions without sender review (covered by the review flow spec).
- Learning from sender corrections across documents — suggestions are stateless per document.
- Support for documents that have already been sent (analysis is restricted to draft-stage documents).
- AI-generated field metadata beyond placement (e.g. pre-filling label text, configuring validation rules) — the sender configures these in the normal field settings panel after accepting a suggestion.
