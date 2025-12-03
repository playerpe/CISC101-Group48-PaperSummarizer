# modules/03_guardrails.md

## Module name
03_guardrails

## Purpose
Central safety/quality control. Standardize handling of missing/short sections, enforce no hallucinations, handle long papers.

## Inputs
- section_record
- context

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
