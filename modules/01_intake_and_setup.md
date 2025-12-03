# modules/01_intake_and_setup.md

## Module name
01_intake_and_setup

## Purpose
Validate inputs, normalize section labels, compute section word counts, mark sections as normal/missing/very short, and initialize shared structures.

## Inputs
- paper_sections
- section_order
- target_audience

## Outputs
- normalized_sections
- warnings_initial
- shared_state (terminology_map, contributions_list, section_summaries)

## Logic
1. Validate inputs.
2. Normalize labels.
3. Assemble sections with word counts and status.
4. Record warnings.
5. Initialize shared_state.
6. Return outputs.
