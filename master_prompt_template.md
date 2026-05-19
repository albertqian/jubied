# FLUX.2 Klein 9B KV — Master Prompt System

A single universal scaffold plus a phrasing cheat sheet for the image-edit + image-to-image pipeline. Drop your image(s) into the workflow, fill the slots in the scaffold, paste it into the positive CLIP Text Encode node, run.

---

## Why a scaffold (and not free-text)

Klein 9B uses **Qwen 3 8B** as its text encoder. Qwen reads natural language reliably, but it rewards *structured*, *ordered* descriptions over poetic prose or tag soup. The scaffold below mirrors the order the model attends to: subject → transfer → additions → scene → preservation → style.

Tag-style prompting (`1girl, dress, beautiful, masterpiece, 8k`) does **not** work well here — it was trained on full-sentence captions, not booru tags.

---

## 1. The Master Scaffold

Copy this block into the green positive prompt node and replace every `{ ... }` slot. **Skip optional rows if they don't apply — do not leave empty placeholders in the final prompt.**

```
{SUBJECT — who or what is the main subject; reference the figure number, e.g. "the woman in Figure 1"}
{TRANSFER — what to apply or take from another figure, e.g. "wearing the dress from Figure 2"}
{ADDED — optional new elements not present in any reference, e.g. "holding a paper coffee cup"}
{SCENE — where it takes place: location, time of day, weather, indoor/outdoor, set dressing}
{PRESERVATION — what must NOT change: face, identity, body proportions, pose, distinguishing marks}
{STYLE — lighting, camera angle, lens feel, mood, photographic vs. illustrated, color palette}
```

### 1.1 The demo prompt, deconstructed

> *Have the man in Figure 1 put on the clothes from Figure 2, wear a hat, and carry a bag. Then, change the background environment to an African savannah while keeping the man in the same posture to give a natural outdoor feel.*

| Slot | Filled with |
| --- | --- |
| Subject | the man in Figure 1 |
| Transfer | put on the clothes from Figure 2 |
| Added | wear a hat, and carry a bag |
| Scene | change the background environment to an African savannah |
| Preservation | keeping the man in the same posture |
| Style | natural outdoor feel |

### 1.2 Three more worked examples (structure only — fill the slots with your own content)

**Outfit swap with location change**
```
The woman in Figure 1, wearing the outfit from Figure 2, standing on a rainy Tokyo street at night, neon reflections on wet pavement, keeping the same face and pose, cinematic 35mm photography with shallow depth of field.
```

**Product placement**
```
The handbag from Figure 2, held by the model in Figure 1, in a brightly lit white studio with a seamless backdrop, model's pose and expression unchanged, editorial fashion photography, soft diffused key light, neutral color grade.
```

**Scene relight**
```
The subject in Figure 1, in the same pose and outfit, relit with the lighting style of Figure 2, indoor setting unchanged, photorealistic, color-graded for warmth.
```

---

## 2. Reference-image syntax

The workflow treats each `LoadImage` as a numbered figure. Adjacent reference images map to **Figure 1**, **Figure 2**, **Figure 3** … in the order their Reference Conditioning subgraphs are chained. Refer to references by figure number, never by description.

| Write | Don't write |
| --- | --- |
| "the dress from Figure 2" | "the dress in the picture" |
| "the man in Figure 1" | "the guy in the photo" |
| "the lighting style of Figure 3" | "that nice lighting in the third image" |

Recommended slot convention when chaining more references:

| Figure | Role |
| --- | --- |
| Figure 1 | Subject anchor (identity / pose) |
| Figure 2 | Donor #1 — usually clothing, asset, or style |
| Figure 3+ | Additional donors — background, prop, color palette, lighting reference |

---

## 3. Cheat sheet — phrasing patterns

### 3.1 Identity preservation
- keep the same face and facial features as Figure 1
- preserve the body proportions and skin tone of the person in Figure 1
- maintain the original posture and hand position
- do not alter age, expression, or hair color
- keep the subject's tattoos and scars exactly as shown

### 3.2 Scene / environment
- place the scene in {location} at {time of day} with {weather}
- change the background to {location}, with {ambient detail}
- indoor studio, plain {color} backdrop, soft diffused light
- outdoor, golden hour, long shadows, slight haze
- bustling city street, motion blur on background pedestrians

### 3.3 Camera and composition
- shot at eye level, half-body framing
- low angle, wide-angle lens, full-body in frame
- shallow depth of field, subject in sharp focus, background bokeh
- centered composition, symmetric framing
- over-the-shoulder, subject looking away from camera

### 3.4 Lighting language
- soft window light from the left
- rim light behind the subject, dim key light
- high-key bright lighting, minimal shadow
- low-key, dramatic side light
- mixed practical lighting, warm tungsten and cool neon

### 3.5 Style and medium
- photorealistic, DSLR, 50mm prime, natural color grading
- cinematic, anamorphic, teal-and-orange grade
- editorial fashion photography, magazine cover style
- analog film, slight grain, faded highlights
- illustration, soft cel-shading
- oil painting, visible brush strokes

### 3.6 Operation verbs that work well
transfer · apply · swap · replace · merge · blend · place · combine · re-light · re-pose · re-frame · isolate · extend · continue

### 3.7 Phrases to avoid
- **Booru-style tag soup.** Use full sentences instead.
- **Vague directives** like "make it better" or "enhance". Give the model an object and a transformation.
- **Conflicting constraints** like "change the outfit but keep everything the same". Be explicit about what changes.
- **Negative prompts.** This workflow runs at CFG 1.0 with the negative zeroed out — they are non-functional here. See §5.

---

## 4. Mature / adult content scaffold

The scaffold structure is unchanged for mature content — only the slot contents differ. Be specific and clinical; vague or euphemistic prompts produce worse results than explicit ones.

| Slot | Guidance for mature content |
| --- | --- |
| Subject | Describe the figure neutrally; specify "adult" explicitly. Anchor identity to Figure 1. |
| Transfer | What is being transferred — pose, state of dress, body styling, lighting style. |
| Added | Props, accessories, modifications. |
| Scene | Location, mood, time of day, framing context. |
| Preservation | Identity / proportions / face. Critical for client-likeness work. |
| Style | Photographic style, lighting, grain, color treatment. |

**Non-negotiable rules for any mature-content use of this scaffold:**

1. Adult subjects only (18+). The client is responsible for verifying age of any reference.
2. Consensual depictions only. Reference imagery must be original or rights-cleared.
3. No real-person likeness without consent.

**Model caveat.** FLUX.2 Klein is heavily filtered at training. It can produce mature subject matter but not pornographic detail at the level of dedicated NSFW community models (Pony / NoobAI / HiDream-based fine-tunes). If the client needs hardcore explicit output, this model is the wrong tool and you should either decline that scope or substitute a different base model in the same workflow shape.

---

## 5. Parameter notes that affect prompt strategy

This workflow ships with values that change how prompts behave. If you change them, prompt strategy changes too.

| Parameter | Value in this workflow | What it means for prompting |
| --- | --- | --- |
| CFG (CFGGuider) | **1.0** | No classifier-free guidance. Negative prompt has **no effect** — the `ConditioningZeroOut` confirms this. Put all detail into the positive prompt. |
| Steps (Flux2Scheduler) | **4** | Aggressive fast preset. Soft on fine detail. Raise to 8–12 for portrait close-ups, jewelry, hair, fabric texture. |
| Sampler | **euler** | Stable. No reason to change unless you need stochastic variation. |
| Resolution | **inherits from input** (via GetImageSize) | The output matches Figure 1's resolution after rescale-to-total-pixels. Crop Figure 1 deliberately — the final composition tracks it. |
| Seed | randomized each run | Lock the seed to `fixed` once you have a composition you like, then iterate the prompt around it. |

---

## 6. Iteration loop (the only loop that works)

1. **Run the demo prompt verbatim** on the demo images first to confirm install.
2. **Swap in real references** and rewrite slots — keep the scaffold structure.
3. **Lock the seed** once layout is acceptable. Don't tune prompts on a randomized seed; you can't tell what's changing.
4. **Tune slots one at a time.** Change only the Style row, regenerate. Then only Preservation. Then only Scene. Changing multiple slots at once makes diagnosis impossible.
5. **Raise steps** from 4 → 8 only if fine detail is unsatisfactory *after* prompt tuning. Steps are not a substitute for a good prompt.

---

## 7. Quick reference card (print this)

```
[SUBJECT in Figure N], [TRANSFER from Figure M], [ADDED elements].
[SCENE: location + time + weather].
[PRESERVE: face / pose / proportions / identity].
[STYLE: lighting + camera + medium + color].
```

That's the whole system. Fill the slots, paste, run.
