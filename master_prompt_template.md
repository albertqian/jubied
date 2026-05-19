# FLUX.2 Klein 9B KV — Universal Master Prompt System

A clean, reusable prompt system for **image + text → final image** generation using your `flux2_klein_9b_kv_v2.json` ComfyUI workflow.

---

## 1) Copy-Paste Master Prompt (Universal)

Use this in the **CLIP Text Encode (Positive Prompt)** node.

```text
Use the main subject from Figure 1 as the identity anchor.
Apply [what to transfer] from Figure 2 to the subject in Figure 1.
Add [optional new elements/props/details].
Place the subject in [scene/location], at [time of day], with [lighting/weather/atmosphere].
Preserve [face, body proportions, skin tone, pose, expression, key marks] exactly as Figure 1.
Render in [style/medium], with [camera framing/lens], and [color mood/grade].
```

> Replace all bracketed sections. Remove lines that do not apply.

---

## 2) Fast “Slot” Version (for speed)

```text
[SUBJECT in Figure 1], [TRANSFER from Figure 2], [ADDED DETAILS].
[SCENE + TIME + ATMOSPHERE].
[PRESERVE IDENTITY + POSE].
[STYLE + CAMERA + COLOR].
```

---

## 3) Demo Prompt (from workflow style)

```text
Have the man in Figure 1 wear the clothes from Figure 2, add a hat and a shoulder bag, place him in an African savannah during golden hour, preserve his face and posture exactly, photorealistic outdoor editorial style with natural warm light.
```

---

## 4) High-Quality Prompt Rules (important)

- Write **clear full sentences** (best for Qwen text encoder behavior in this pipeline).
- Refer to references as **Figure 1**, **Figure 2**, **Figure 3** (if added).
- Be explicit about **what changes** and **what must stay the same**.
- Keep prompt focused: subject → transfer → scene → preserve → style.
- Avoid tag soup (`masterpiece, 8k, best quality`) and vague text (`make it better`).

---

## 5) Powerful Phrase Bank

### Identity Preservation
- keep the same face structure and expression as Figure 1
- preserve body proportions, skin tone, and hairline from Figure 1
- maintain original hand placement and posture
- do not alter age appearance or facial identity

### Transfer Actions
- apply the outfit from Figure 2
- transfer the lighting style from Figure 2
- replace the background with the environment from Figure 2
- merge accessory details from Figure 2 into Figure 1

### Scene & Camera
- street at night with neon reflections on wet pavement
- bright indoor studio with soft diffused key light
- golden hour backlight with shallow depth of field
- eye-level medium shot, 50mm lens, clean composition

---

## 6) Ready-to-Use Prompt Blueprints

### A) Outfit Swap + New Background
```text
Use the woman in Figure 1 as the identity anchor. Apply the full outfit from Figure 2 to her. Place her in a rainy Tokyo street at night with neon reflections. Preserve her face, body proportions, and pose exactly as Figure 1. Photorealistic cinematic fashion image, 35mm lens look, shallow depth of field, cool-magenta urban grade.
```

### B) Product Placement
```text
Use the model in Figure 1 as the identity anchor. Transfer the handbag from Figure 2 into her right hand. Place her in a clean white studio with soft overhead diffusion. Preserve her facial identity and pose exactly. Editorial commercial photography style, balanced contrast, neutral color grading.
```

### C) Lighting Transfer
```text
Use the subject in Figure 1 as the identity anchor. Keep the same outfit and pose, and transfer only the lighting mood from Figure 2. Keep the same room layout. Preserve face and proportions exactly. Photorealistic image, soft cinematic contrast, warm highlight rolloff.
```

---

## 7) Parameter-Aware Notes for This Workflow

- **CFG is 1.0** in this setup → negative prompting has little/no practical effect.
- Put all key instructions in the **positive prompt**.
- Default steps are fast (4). For finer hair/fabric/skin detail, increase to **8–12**.
- Lock seed to fixed after composition is good, then iterate prompt details.

---

## 8) One-Minute Prompt QA Checklist

Before Queue:
- Did I clearly state **Figure 1 identity anchor**?
- Did I specify exactly what to transfer from **Figure 2**?
- Did I define scene + lighting?
- Did I explicitly preserve face/pose/proportions?
- Did I add style/camera direction?

If yes, run.
