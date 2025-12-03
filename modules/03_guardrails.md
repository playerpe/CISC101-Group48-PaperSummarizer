Change Log:
- 2025-12-03: Added `evidence_mode` flag and strict evidence behavior, plus standardized warning messages for missing, empty, and very short sections.

# modules/03_guardrails.md

## Module name
03_guardrails

## Purpose
Central safety/quality control. Standardize handling of missing/short sections, enforce no hallucinations, handle long papers.

## Inputs
Inputs / Configuration:
- section_text: raw text of the section (may be empty or very short)
- section_status: "normal", "missing", "very_short"
- evidence_mode (string): either "normal" or "strict".
  - In "strict" mode, the summarizer may only include claims, equations, and results that clearly appear in the provided text.
- word_count_threshold (integer, default 50): below this count, a section is considered "very_short".


## Outputs
- guardrail_decision
- warning_entry

## Logic
1. Missing/empty → skip_with_warning.
2. Very short → skip_with_warning.
3. Normal → summarize_allowed.
4. Very long → chunk_then_summarize.
5. Enforce no hallucinations.
6. Return decision + warning.

Strict Evidence Mode:

1. If `evidence_mode == "strict"`:

   - The summarizer must:

     - Only include claims, equations, and results that are explicitly supported by `section_text`.
     - Avoid speculating about missing details, methods, or results.

   - If the section text does not provide enough information to summarize accurately:
     - Return a standardized message, for example:
       "The source text does not provide enough detail to summarize this section in strict evidence mode."
     - Do not attempt to infer or guess missing content.

2. If `evidence_mode == "normal"`:

   - The summarizer may use cautious interpolation or paraphrasing,
     but still must not invent major claims, equations, or results
     that are not supported by the text.
Missing / Empty / Very Short Section Handling:

1. If `section_status == "missing"` or section_text is empty or whitespace:

   - Do not generate a summary.
   - Return a standardized warning message, e.g.:
     "Section skipped: no usable text was provided."

2. If `section_status == "very_short"` or word_count < word_count_threshold (e.g., 50 words):

   - Emit a standardized warning message, e.g.:
     "Section very short: summary may be incomplete."

   - If a summary is still requested:
     - Generate the best possible short summary based only on the available text.
     - Remind the user in the summary or metadata that it is based on limited information.

3. Ensure that all warnings are collected so that the Rendering & Refinement module
   can display them in the final **Checks & Warnings** section.

