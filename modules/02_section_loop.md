# modules/02_section_loop.md

## Module name
02_section_loop

## Purpose
Iterate over normalized sections and generate per-section summaries. Defer to Guardrails if missing/short. Enforce 150-word max and terminology normalization.

## Inputs
- normalized_sections
- shared_state
- target_audience

## Outputs
- section_summaries
- updated_shared_state

## Logic
1. Iterate sections.
2. If missing/short → Guardrails.
3. Extract key content.
4. Normalize terminology.
5. Compose ≤150-word summary.
6. Update shared_state.
7. Finalize entry.
8. Return outputs.
