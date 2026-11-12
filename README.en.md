# riki-midjourney

[中文](README.md) | English

**A Midjourney prompt analysis skill.** It takes an existing MJ still-image prompt — or a reference image — and breaks it into a readable visual structure: what it will actually generate, which parts are stated outright, which are inferred, and which were never specified at all. It ends with a Chinese test prompt you can copy and run immediately.

It does not create and it does not judge. By default it only answers "what does this prompt describe," never "is this prompt good."

---

## What problem it solves

Most prompt problems are not a shortage of words. They are unclear relationships between words:

- `soft diffused lighting` plus `deep set eyes` — the light flattens the eye sockets, so the two requirements cancel each other out
- `low saturation` with no cool light source anywhere — the image comes out muddy grey, never blue
- `full body` plus `head to toes` plus a whole horse — a vertical frame simply cannot hold it
- Nine garments listed, but no skirt length and no indication which shoulder the pauldron sits on — the silhouette changes every generation

This skill puts those hidden conflicts, missing facts and visual consequences on the table, so you know where the problem is before burning dozens of generations.

## When to use it

- You have an English prompt and want to know what it will actually produce
- You have a reference image you like and want to reverse-engineer a Midjourney prompt from it
- You want to understand what specific conditions a given look (low-key, blue cast, 1990s Hong Kong film stock) actually requires
- Generations keep coming out wrong and you need to locate whether it is a missing description, a mutual conflict, or a model limitation
- You want a specific dimension unpacked — wardrobe, material, light, composition — instead of a vague "it doesn't feel right"

## When not to use it

| Need | Use instead |
| --- | --- |
| Breaking a script into scenes, writing video timelines | `riki-storyboard` |
| Generating images or video directly | The image / video generation tool itself |
| Rating a prompt, scoring it, judging quality | Out of scope — this skill only decomposes |
| Rewriting a prompt | Only on an explicit "revise" request; it will not rewrite unprompted |

## Output structure

Every analysis follows the same five parts, in the same order:

**1. Original prompt** — quoted in full, never rewritten or trimmed.

**2. One-line visual summary** — the target image in a sentence.

**3. Multi-dimension breakdown table** — each dimension listed as "textual evidence / visual effect / information status".

| Dimension | What it covers |
| --- | --- |
| Subject and features | Identity, appearance, wardrobe, pose, expression, recognition anchors |
| Tonality | Brightness, light-dark ratio, contrast, black level, white level, haze, pressure |
| Texture and medium | Skin, fabric, metal, film grain, painterly or 3D surface |
| Realism | Documentary, cinematic realism, hyperreal, commercial photography, illustration, CG |
| Lighting | Source direction, color temperature, hardness, shadows, rim light, reflection |
| Color | Primary, secondary and accent colors, saturation, value, warm-cool relationships |
| Composition and viewpoint | Shot size, camera height, subject placement, negative space, foreground/midground/background, aspect ratio |
| Lens and focal length | Wide / normal / telephoto tendency, perspective, depth of field, background blur |
| Mood | How the emotional words map to pose, light, space and color |
| Environment and parameters | Time, place, weather, props, plus `--ar`, `--v`, `--stylize` and so on |

**4. Reusable keyword groups** — grouped by visual function, ready to drop into other prompts.

**5. Chinese test prompt** — one complete, faithful, copy-ready version.

Reply `ok` and you get the matching full English version, with no repeated analysis and no extra commentary.

## The three-state rule

This is the backbone of the whole method. Every conclusion must fall into exactly one of three states:

- **Stated**: written directly in the source, e.g. `soft diffused studio lighting`
- **Inferred**: a visual tendency derived from the combination of keywords, e.g. "a mid-grey backdrop will compress overall contrast"
- **Missing**: not provided at all, e.g. "light direction unspecified; the model will probably default to flat frontal light"

Inferences and gaps are never presented as facts. This matters — most "the image came out wrong" cases come from treating an inference as a given.

## Three input modes

**Plain text prompt**: analyze the text as given, without pulling in outside context.

**Reference image**: reverse-engineer from visible evidence — aspect ratio and shot size, subject layering, action and gaze, key light and shadow, tonality and color, texture and grain, then environment and atmospheric perspective. Only what is visible gets described. Lens models, exact focal lengths and shooting locations are explicitly marked as unverifiable.

**Diagnosis and revision**: this mode only activates on an explicit request to "check, troubleshoot, optimize or revise." It first explains the problem and its effect on the image, then must output one complete prompt integrating every change — never a fragment of replacement text.

## Usage examples

```text
Analyze this Midjourney prompt, focus on tonality and the character's wardrobe,
and produce a Chinese test prompt.

Full body frontal portrait, head to toes, centered vertical composition,
seamless medium gray studio background. Mature battle hardened Chinese general...
```

```text
Decompose a prompt from this reference image: composition, lighting, tonality,
color and material, then give me the Chinese test version.
```

```text
This prompt never produces a blue cast. Find out why and give me a complete revised version.
```

## Before / after examples

The three cases below come from real usage records; only the fragments relevant to each problem are kept.

### Example 1: Why this prompt never produces a blue cast

**Input (excerpt)**

```text
seamless medium gray studio background ... Aged bronze, antique gold, faded vermilion,
charcoal, ivory palette against deep black. 1990s Hong Kong wuxia film aesthetic,
soft diffused studio lighting, warm highlights, low saturation, fine film grain.
```

**Findings**

| Dimension | Finding | Status |
| --- | --- | --- |
| Tonality | Mid-grey backdrop, highlights specified as warm, all five colors warm or neutral — not one cool source anywhere | Stated |
| Lighting | Fully diffused with no light position given; figure and horse will sit flat against the backdrop, bone structure and metal thickness unreadable | Inferred |
| Color | Shadows only described as "neutral, slightly cool" — a degree of cool that MJ renders as neutral grey | Inferred |
| Missing | Light direction, black-level tendency, the specific mechanism for warm-cool separation | Missing |

**Revised**

```text
medium gray studio backdrop cooling into blue-gray toward the edges ...
a cold blue-grey moonlight from high behind-right rimming the rider's shoulder and the horse's neck ...
shadows sinking into low-saturation slate blue instead of black,
lifted film blacks with a faint blue-green cast,
low saturation with a clear warm-cool split
```

**Change log**

| Before | After | Why |
| --- | --- | --- |
| `medium gray backdrop` | `cooling into blue-gray toward the edges` | Lets the backdrop itself carry part of the cool cast |
| `soft diffused lighting, warm highlights` | Added `cold blue-grey moonlight from high behind-right` | Cool has to land on a concrete light source; adjectives cannot imply it |
| `neutral and slightly cool` shadows | `sinking into low-saturation slate blue` | "Slightly cool" equals neutral in MJ |
| none | Explicit `clear warm-cool split` | Without warm-cool separation, low saturation just turns the whole frame grey |

### Example 2: Full body plus a horse does not fit a vertical frame

**Input**

```text
Full body portrait, head to toes, frontal to camera, centered vertical composition ...
```

**Findings**

| Dimension | Finding | Status |
| --- | --- | --- |
| Composition | Head-to-toes already fills roughly 85–90% of a vertical frame; there is no room left for a horse | Inferred |
| Spatial relations | If rider and horse both face camera, the horse's head blocks the torso and the abdominal panel | Inferred |
| Missing | Horse head angle, how much of the horse enters frame, the hand-on-reins action | Missing |

**Revised**

```text
Full body portrait of a mounted rider, head to toes, frontal to camera,
centered vertical composition ...
Black horse in three-quarter turn, head and neck angled slightly to one side of frame ...
hands resting loosely on leather reins ...
```

**Change log**

| Before | After | Why |
| --- | --- | --- |
| Single figure, full body | Figure full body, horse limited to head, neck and chest | A vertical frame cannot hold the whole horse; the body must exit frame |
| Both facing camera | Figure frontal, horse in three-quarter turn | Prevents the horse's head from blocking the torso |
| No hands described | `hands resting loosely on leather reins` | Riding does not hold up without reins |

### Example 3: "Asian" was written, the output was still a Western face

**Input (excerpt)**

```text
A single East Asian man in his thirties ... aviator sunglasses ...
worn military-surplus clothing ...
```

**Findings**

| Dimension | Finding | Status |
| --- | --- | --- |
| Subject and features | `Asian` appears once, followed by eighty-odd words of Western military kit | Stated |
| Props | Sunglasses cover the eyes, and the eyes are the most direct ethnicity cue | Inferred |
| Missing | Concrete bone structure — eye shape, midface, nose root, hair texture | Missing |

**Revised**

```text
A single Chinese man in his mid to late twenties, Han Chinese, unmistakably from mainland China ...
Han Chinese face: broad cheekbones, a squarish jaw, monolid eyes with an epicanthic fold,
thick straight black eyebrows, a low straight nose bridge ... straight black hair ...
aviator sunglasses pushed up onto the brim ... authentic Chinese casting
```

**Change log**

| Before | After | Why |
| --- | --- | --- |
| `East Asian` | `Chinese` / `Han Chinese`, once at each end | `East Asian` is too diffuse and drifts toward Japanese, Korean or generic pan-Asian |
| No bone structure | Monolid, epicanthic fold, squarish jaw, low nose root, straight black hair | Ethnicity needs bone-structure anchors, not a single label |
| Sunglasses worn | `pushed up onto the brim` | Covering the eyes throws away the ethnicity cue immediately |

## Repository layout

```text
riki-midjourney/
├── README.md                                 # Chinese documentation
├── README.en.md                              # English documentation (this file)
├── SKILL.md                                  # Scope, workflow, fixed output format
├── references/
│   ├── midjourney-prompt-decomposition.md    # Ten-dimension definitions and Chinese test-prompt rules
│   └── image-prompt-decomposition.md         # Rules for reverse-engineering a reference image
└── LICENSE                                   # MIT
```

`SKILL.md` is the entry point. Files under `references/` load on demand and play no part in triggering.

## Install and invoke

Drop the repository into your skills directory, keep `SKILL.md` at the repository root, and make sure the frontmatter `name` matches the directory name:

```text
<skills-dir>/riki-midjourney/SKILL.md
```

Triggering reads only `name` and `description` from the `SKILL.md` frontmatter. This README is repository documentation and has no effect on triggering.

Invoke with `/riki-midjourney`.

## How it differs from its sibling skill

| Skill | Responsibility |
| --- | --- |
| `riki-midjourney` (this repo) | Analyzing and revising a single MJ prompt or a single reference image |
| `riki-storyboard` | Breaking a script into scenes, director tables, MJ stills and Seedance video prompts |

Both share the Riki Jin visual method, but one handles "how to describe one image" and the other handles "how to break down one scene."

## Design principles

1. **Quote the source first**, then explain it — never rewrite on the user's behalf
2. **Separate stated, inferred and missing** — never present an inference as fact
3. **Describe visual consequences**, never say "this is bad"
4. **Defer to official documentation for parameters** — never hard-code habits from older versions
5. **When revising, leave everything unasked untouched** — fix subject identity, spatial relations and composition before polishing style words

## License

MIT, see [LICENSE](LICENSE).
