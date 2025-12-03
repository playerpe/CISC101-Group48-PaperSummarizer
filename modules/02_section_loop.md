Change Log:
- 2025-12-03: Added `summary_level` configuration and conditional logic for short vs detailed section summaries (paragraph + bullet list).

# modules/02_section_loop.md

## Module name
02_section_loop

## Purpose
Iterate over normalized sections and generate per-section summaries. Defer to Guardrails if missing/short. Enforce 150-word max and terminology normalization.

## Inputs
- normalized_sections
- shared_state
- contributions_store: shared structure for recording key contributions
- target_audience

## Outputs
- section_summaries
- updated_shared_state

Section Loop Logic:

1. For each section in normalized_sections (in the order given by `section_order`):

   a. Read section metadata:
      - label
      - text
      - status (e.g., "normal", "missing", "very_short")
      - word_count

   b. If status is "missing" or text is empty:
      - Call Guardrails module with reason = "missing_or_empty_section".
      - Record the standardized warning produced by Guardrails.
      - Do NOT attempt to summarize this section.

   c. Else if status is "very_short" (e.g., word_count < 50):
      - Call Guardrails module with reason = "very_short_section".
      - Record the standardized warning.
      - Optionally still attempt a short summary, but mark it as based on limited text.

   d. Else (status is normal):
      - Normalize terminology in the section text using terminology_map
        (e.g., via the Terminology & Acronym Tracker module).

      - Apply `summary_level`:

        - If `summary_level == "short"`:
          - Generate a concise 1–2 sentence summary of this section.
          - Ensure it is **≤ 150 words**.
          - Store this as `section_summary.short`.

        - If `summary_level == "detailed"`:
          - Generate a short paragraph summary of this section (3–5 sentences).
          - Then generate a bullet list of **3–5 key points** that capture important details,
            such as major results, methods, or arguments.
          - Ensure the combined section summary text respects the **≤ 150 words**
            constraint.
          - Store this as `section_summary.detailed.paragraph` and
            `section_summary.detailed.bullets`.

      - Update shared structures:
        - Send any detected key contributions to `contributions_store`.
        - Register important terms in `terminology_map`.

2. Return a list or mapping of section labels → section summary objects, including any warnings.
