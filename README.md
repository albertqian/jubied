# FLUX2 Klein 9B KV Fiverr Pipeline

A client-ready ComfyUI package for **text + reference image** generation using the `flux2_klein_9b_kv_v2.json` workflow.

## Repository name

Suggested repository/project name:

`flux2-klein-9b-kv-fiverr-pipeline`

## Included files

- `flux2_klein_9b_kv_v2.json` — main ComfyUI workflow
- `master_prompt_template.md` — universal/master prompt system
- `project_walkthrough.md` — simple Fiverr handoff and client setup guide
- `brief.png` — project brief reference
- `client_info.png` — client notes/reference

## Quick usage

1. Open ComfyUI and load `flux2_klein_9b_kv_v2.json`.
2. Set Figure 1 and Figure 2 in the `LoadImage` nodes.
3. Copy a prompt from `master_prompt_template.md` into **CLIP Text Encode (Positive Prompt)**.
4. Queue generation and review output.

## Delivery intent

This repository is structured for Fiverr delivery:
- easy client onboarding,
- reusable prompt scaffolding,
- clear revision boundaries and troubleshooting.
