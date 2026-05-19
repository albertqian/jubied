# FLUX.2 Klein 9B KV — Project Walkthrough

A complete delivery guide for the ComfyUI image-edit pipeline. This is the client-facing document — hand it over with the workflow.

---

## At a glance

|  |  |
| --- | --- |
| Model | FLUX.2 Klein 9B KV (FP8) |
| Text encoder | Qwen 3 8B (FP8 mixed) |
| VAE | flux2-vae |
| Input | Two reference images + a text prompt |
| Output resolution | Inherits Figure 1 (default 1024×1024) |
| Typical run time | 6–15 seconds on a 16 GB+ GPU |
| Workflow file | `flux2_klein_9b_kv_v2.json` |

---

## What's in the delivery

```
flux2-klein-9b-kv-pipeline/
├── flux2_klein_9b_kv_v2.json       annotated workflow
├── master_prompt_template.md        universal prompt scaffold + cheat sheet
├── project_walkthrough.md           this document
└── demo_images/
    ├── man_in_street.png
    └── safari_outfit.png
```

---

## Step 1 — Install the model files

> All three files must be on disk *before* you load the workflow. Missing files show as red error boxes in ComfyUI.

| File | Place in folder | Source |
| --- | --- | --- |
| `flux-2-klein-9b-kv-fp8.safetensors` | `ComfyUI/models/diffusion_models/` | huggingface.co/black-forest-labs/FLUX.2-klein-9b-kv-fp8 |
| `qwen_3_8b_fp8mixed.safetensors` | `ComfyUI/models/text_encoders/` | huggingface.co/Comfy-Org/flux2-klein-9B |
| `flux2-vae.safetensors` | `ComfyUI/models/vae/` | huggingface.co/Comfy-Org/flux2-dev |

Final layout:

```
ComfyUI/
└── models/
    ├── diffusion_models/
    │   └── flux-2-klein-9b-kv-fp8.safetensors
    ├── text_encoders/
    │   └── qwen_3_8b_fp8mixed.safetensors
    └── vae/
        └── flux2-vae.safetensors
```

---

## Step 2 — Update ComfyUI

The Klein 9B nodes (`FluxKVCache`, `Flux2Scheduler`, `EmptyFlux2LatentImage`) are only available on recent builds.

```
cd ComfyUI
git pull
```

Then restart the server. If you use **ComfyUI Desktop**, open **Manager → Update All** instead.

---

## Step 3 — Load the workflow

1. Open ComfyUI in the browser.
2. Drag `flux2_klein_9b_kv_v2.json` onto the canvas.
3. Verify every node loads with no red borders. If anything is red, ComfyUI is out of date — `git pull` and reload.

---

## Step 4 — Plug in your images

The workflow exposes two `LoadImage` nodes in the **Input** group:

| Node | Maps to | Role |
| --- | --- | --- |
| `LoadImage` (top) | **Figure 1** | Subject anchor — the person or object you want preserved |
| `LoadImage` (bottom) | **Figure 2** | Transfer source — outfit, style, prop, scene element to apply onto Figure 1 |

Click each `LoadImage`, choose your image. Done.

> Crop Figure 1 to the framing you want — the output resolution tracks it.

---

## Step 5 — Write the prompt

Open the green **CLIP Text Encode (Positive Prompt)** node. Paste your filled scaffold from `master_prompt_template.md`.

A minimal valid prompt fills at least the **Subject**, **Transfer**, and **Scene** slots. Preservation and Style are strongly recommended.

---

## Step 6 — Run

Click **Queue**. The output saves to `ComfyUI/output/` with prefix `flux2_klein/edit_*`.

---

## Step 7 — Iterate

The only iteration loop that produces predictable improvement:

1. Run with `randomize` seed until composition is right.
2. Switch seed mode to `fixed` to lock the composition.
3. Tune the prompt one slot at a time. Don't change two slots in the same run — you won't know which one moved the output.
4. Raise `Flux2Scheduler` steps from **4 → 8** only if fine detail is unsatisfactory *after* prompt tuning. Steps are not a substitute for a good prompt.
5. Lower the value in `ImageScaleToTotalPixels` if the run is too slow — it controls megapixels.

---

## Adding more reference images

The Reference Conditioning subgraph is designed to chain. To add a third reference:

1. Duplicate the second `Reference Conditioning` subgraph node (right-click → Clone).
2. Add a third `LoadImage` and a third `ImageScaleToTotalPixels` in the Input group.
3. Wire `LoadImage → ImageScaleToTotalPixels → pixels` on the new subgraph.
4. Route its conditioning outputs into the next stage, replacing the previous endpoint into `CFGGuider`.
5. In your prompt, refer to it as "Figure 3".

---

## What this workflow *will not* do

Set client expectations clearly before you start. This workflow:

- **Cannot generate explicit pornographic detail.** FLUX.2 Klein is heavily filtered at training. It handles mature subject matter (figure studies, suggestive posing, partial nudity) but not hardcore detail. For that, the same workflow shape needs to be re-pointed at a different base model (Pony / NoobAI / HiDream-based fine-tunes). Quote that work separately.
- **Cannot perfectly preserve faces** without a dedicated identity adapter (PuLID, InstantID). Klein preserves identity better than vanilla FLUX but expect some drift on close-up portrait swaps. Add explicit preservation language and crop tight on Figure 1.
- **Cannot do exact text rendering** in the output image. The model is decent at short text but not reliable for logos, signs, or any phrase longer than a few words.
- **Cannot run on under-8 GB VRAM** in this FP8 configuration. For 6–8 GB cards, suggest the GGUF Q4/Q5 variants of Klein once available.

---

## Troubleshooting

| Symptom | Cause | Fix |
| --- | --- | --- |
| Output ignores Figure 2 | Reference Conditioning subgraph not wired through | Open the second subgraph; both `CONDITIONING` outputs must reach `CFGGuider` |
| Output looks soft / washed | Too few steps for the detail level | Raise `Flux2Scheduler` steps to 8–12 |
| Output identity drifts | Preservation slot empty or vague | Add explicit identity language to the prompt |
| Out-of-memory error | VRAM too low for FP8 9B | Lower megapixels in `ImageScaleToTotalPixels`, or switch to a GGUF quant |
| Red nodes on load | ComfyUI out of date | `git pull` and restart |
| Same output every run | Seed is `fixed` | Switch back to `randomize` |
| Negative prompt seems ignored | It is ignored — CFG is 1.0 | Put detail into the positive prompt |

---

## Delivering this project on Fiverr

### Packaging checklist

Zip exactly this and attach it to the delivery message:

```
flux2-klein-9b-kv-pipeline.zip
├── flux2_klein_9b_kv_v2.json
├── master_prompt_template.md
├── project_walkthrough.md
└── demo_images/
    ├── man_in_street.png
    └── safari_outfit.png
```

Keep the ZIP under 25 MB so Fiverr accepts it inline. The demo images are small PNGs, so this is comfortable.

### Delivery message — copy and adapt

> Hi {client name},
>
> Delivery is attached. The ZIP contains:
>
> - the annotated ComfyUI workflow (`flux2_klein_9b_kv_v2.json`),
> - a universal prompt scaffold and cheat sheet (`master_prompt_template.md`) you paste directly into the positive prompt node,
> - the full setup walkthrough (`project_walkthrough.md`), and
> - two demo images you can use to verify everything is working.
>
> **Start here:** open `project_walkthrough.md`. It walks through model installation, loading the workflow, and your first generation. Should take 10–15 minutes including downloads.
>
> Two notes on the defaults — they're tuned for speed, not maximum quality:
>
> 1. CFG is set to 1.0, which means **the negative prompt has no effect**. Put all detail into the positive prompt. The cheat sheet explains how.
> 2. Steps are set to 4. Raise to 8–12 in the `Flux2Scheduler` node if you want finer detail on portraits, fabric, or hair.
>
> Run the demo prompt verbatim on the demo images first to confirm the install. Then swap in your own references and use the scaffold to write your prompt.
>
> Happy to revise once you've run a few generations. Let me know what use case you're targeting and I'll tune the scaffold to match.

### Revision policy — set scope upfront

Offer **two free revisions** scoped to:

1. Prompt scaffold adjustments for the client's specific use case.
2. Workflow tweaks: adding a third reference slot, changing default resolution, changing step / CFG defaults.

Out of scope for free revisions:

- Substituting a different base model (NSFW community models, video models, etc.) — quote separately.
- Identity-preservation adapters (PuLID, InstantID, ReActor) — quote separately.
- Full UI / front-end work around the workflow — quote separately.
- Training a LoRA or fine-tune for the client's subject — quote separately.

### Pricing read

The brief lists $100 for SFW *and* NSFW workflows. Two things to flag:

1. $100 is at the low end for two production workflows. If the SFW workflow is this one and the NSFW workflow is a second pipeline pointed at a different base model, that's effectively two deliveries' worth of work — consider scoping NSFW as a follow-up gig at a separate price point.
2. The client may expect explicit output from "NSFW workflow" that Klein 9B cannot produce. Get alignment on what *kind* of NSFW content is in scope before quoting — figure studies and suggestive imagery are very different from explicit content, and need different base models.

---

## One-line summary

Drop your images into the two `LoadImage` nodes, paste a filled-in scaffold from `master_prompt_template.md` into the green prompt node, click Queue.
