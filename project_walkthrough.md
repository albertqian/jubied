# FLUX.2 Klein 9B KV — Fiverr Delivery Walkthrough

A simple, modern, client-ready handoff guide.

---

## What the client gets

```text
flux2-klein-9b-kv-pipeline/
├── flux2_klein_9b_kv_v2.json
├── master_prompt_template.md
├── project_walkthrough.md
├── brief.png
└── client_info.png
```

---

## Quick Start (Client View)

### 1) Install required model files
Place files here:

- `ComfyUI/models/diffusion_models/flux-2-klein-9b-kv-fp8.safetensors`
- `ComfyUI/models/text_encoders/qwen_3_8b_fp8mixed.safetensors`
- `ComfyUI/models/vae/flux2-vae.safetensors`

### 2) Open ComfyUI and load workflow
Drag `flux2_klein_9b_kv_v2.json` onto the canvas.

### 3) Insert images
- Top `LoadImage` = **Figure 1** (identity anchor)
- Bottom `LoadImage` = **Figure 2** (transfer source)

### 4) Paste prompt
Copy a filled prompt from `master_prompt_template.md` into:
**CLIP Text Encode (Positive Prompt)**

### 5) Generate
Click **Queue** and review output in `ComfyUI/output/`.

---

## Operator Notes (You as Seller)

- Keep Figure 1 tightly cropped around desired framing.
- Use clear sentence prompts, not tag prompts.
- Keep seed randomized for exploration, then fixed for refinement.
- Raise steps from 4 → 8/12 only when detail is insufficient.

---

## Fiverr Delivery Flow (Professional)

## Phase A — Before Delivery

### Checklist
- Workflow loads without red nodes.
- Prompt template tested with one real client-like example.
- One sample output image exported.
- ZIP package cleaned and named.

Suggested filename:
`flux2-klein-9b-kv-pipeline-v1.zip`

---

## Phase B — Final Delivery Message (Copy Template)

```text
Hi [Client Name],

Your FLUX.2 Klein 9B KV pipeline is ready ✅

Included in the ZIP:
1) flux2_klein_9b_kv_v2.json (ComfyUI workflow)
2) master_prompt_template.md (universal prompt system)
3) project_walkthrough.md (step-by-step setup/use guide)
4) brief.png and client_info.png (project references)

How to start fast:
- Install 3 model files listed in the walkthrough
- Load the JSON in ComfyUI
- Set Figure 1 + Figure 2 images
- Paste your prompt from the template
- Click Queue

If you want, I can do one free optimization pass for your first real use-case prompt.

Thanks!
```

---

## Phase C — Revision Boundaries

### Included revisions
- prompt wording optimization
- small workflow tuning (steps, resolution balance)
- adding one extra reference slot (Figure 3)

### Out-of-scope (extra quote)
- changing to a different base model family
- identity plugins (InstantID, PuLID, ReActor integration)
- custom frontend/web app around ComfyUI
- LoRA training / model finetune

---

## Troubleshooting (Client Friendly)

- **Red nodes:** ComfyUI outdated → update and restart.
- **Output ignores Figure 2:** reference chain wiring issue.
- **Face drift:** add stronger preservation line in prompt.
- **Soft details:** increase steps to 8–12.
- **OOM/VRAM error:** lower resolution scaling.

---

## Suggested “Simple + Modern” Service Packaging on Fiverr

### Gig Title
**Custom FLUX ComfyUI Image Pipeline (Text + Reference Image Editing)**

### 3 Tiers
- **Basic:** Ready workflow + prompt template
- **Standard:** Workflow + prompt system + 1 revision pass
- **Premium:** Workflow + prompt system + 3 reference setup + onboarding video

### Add-ons
- Extra reference slot expansion
- NSFW-safe scope consultation
- Prompt library for niche (fashion/ecom/portrait)

---

## Final Handoff Standard

Deliver these every time:
- ✅ workflow JSON
- ✅ master prompt system
- ✅ client walkthrough
- ✅ one tested sample output
- ✅ one clean delivery message

That consistency is what wins repeat Fiverr clients.
