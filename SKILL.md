---
name: photo-to-zine-postcard
description: Turn a user-provided travel or landmark photograph into a landscape 3:2 "city in poetry" postcard — the photo's main subject repainted as a vignetted ink-and-watercolor illustration on ivory paper, with a large calligraphic place-name title, romanized subtitle, small red seal, a real two-line classical poem with attribution, quiet corner labels, and a matching functional postcard back. Use when the user shares a photo and asks for a postcard, zine card, poetic city card, or watercolor/ink illustration of a landmark (e.g. 明信片, 照片转插画, 水彩明信片, 水墨插画, 诗意城市, 城市诗笺).
---

# SKILL.md — photo-to-zine-postcard (spec v1.1.0)

## 1. Name

**photo-to-zine-postcard** — "City in Poetry" edition

A compact, self-contained, platform-agnostic specification for turning one source photo into a poetic illustrated postcard front and a matching back.

Use only this document as the design specification.
Do not blend in outside style conventions or a different version of this skill.

Reference example (Changsha · 杜甫江阁):

- source: `assets/changsha-dufu-jiangge-photo.jpg`
- front: `assets/changsha-dufu-jiangge-front.png`
- back: `assets/changsha-dufu-jiangge-back.png`
- filled prompts used: `examples/changsha-dufu-jiangge-prompts.md`

The example is a visual reference, not an extra rule; this document wins on any conflict.

**Scope:** v1 is designed for Chinese places, poems, and calligraphy. Section 8 and Section 9 give the adjustments for non-Chinese places.

---

## 2. Goal

Generate two coordinated images:

1. **Front** — the photo's main subject repainted as an ink-and-watercolor illustration on the left, a calligraphic title and poem on the right
2. **Back** — a functional postcard back in the same paper and visual language

The front must feel:

- poetic and quiet
- clearly derived from the source photo
- hand-painted, not photographic
- spacious, with real paper showing around the painting
- collectible and print-friendly

---

## 3. Supported Agents & Invocation

This specification is **agent-agnostic**. It works with anything that can either generate an image directly, or orchestrate a call to something that can.

### Mode A — Direct image-generation chat assistants

Examples: ChatGPT, Gemini, Claude.ai with image generation enabled.

1. Load this entire document as the specification. Do not act from a partial summary or from memory of a previous version.
2. Take the user's uploaded source photo plus any inputs (Section 4).
3. Look at the photo and fill the **Photo Analysis** (Section 14).
4. Fill the Front Prompt Template (Section 15) and Back Prompt Template (Section 16).
5. Generate both images, passing the source photo as the reference image, and return them.
6. Check the front against the Quick Checklist (Section 18). If the title, plaque, or seal text is garbled, regenerate once; then tell the user about any remaining defect.

### Mode B — Agentic / coding tools without native image generation

Examples: Claude Code, other CLI coding agents, scripts, automation pipelines.

1. Read this document as the specification.
2. Collect inputs per Section 4. Ask the user for missing ones if interactive.
3. **Open and look at the source photo yourself** and fill the Photo Analysis (Section 14). Never fill palette, viewpoint, or signage from guesswork.
4. Fill the Front and Back Prompt Templates.
5. **If an image-generation tool is available** (an MCP image tool, a configured image API invoked via script, etc.):
   - Call the tool once per side, passing the source photo as the reference image.
   - Save both outputs, view them, check them against Section 18, and report the file paths plus any defects.
6. **If no image-generation tool is available**:
   - Do not attempt to render the postcard as text, ASCII art, SVG, or HTML.
   - Output the filled Front and Back prompts as ready-to-paste text (or save them to a file) and tell the user to paste them, with the photo, into any image-capable assistant.

### Rules that apply regardless of mode

- Treat this file as the single source of truth.
- Never fabricate a place name, poem, or attribution (Sections 4 and 9).
- If the platform cannot accept an input image, say so instead of working from a text description of the photo.

---

## 4. Inputs

**Required:**

- source photo

**Strongly recommended** (ask if interactive):

- **place name** — the landmark or scene name used as the big title, e.g. `杜甫江阁`
- **city** — e.g. `长沙 / Changsha`

If the user does not provide the place name, you may take it from clearly legible signage in the photo (the example's plaque reads 杜甫江阁) or from an identification you are confident about. If you are not confident, ask; if you cannot ask, use the city alone or a short neutral scene title, and never invent a landmark name.

**Optional:**

- **poem** — two lines of verse plus author and title. If not provided, see Section 9.
- **romanized subtitle** — default: pinyin (or the standard romanization) of the place name in spaced capitals, e.g. `DU FU JIANG GE`
- **series label** — default: `A CITY IN POETRY`
- **tagline** — a short closing phrase for the lower-right corner, e.g. `湘江潮落 · 诗意长留` (see Section 10)
- **language** — default: Chinese title and poem with English/romanized secondary labels

---

## 5. Default Format

- card ratio: landscape `3:2` (fixed; this edition has no portrait layout)
- reference print size: `150 × 100 mm` (6 × 4 in)
- background: warm ivory / rice paper with visible but fine paper grain
- resolution: generate at the image model's largest native 3:2 size (e.g. `1536 × 1024`), then upscale to at least `1800 × 1200 px` (≈300 dpi at 150×100 mm) before printing; 4× upscaling is recommended
- file format: PNG, sRGB
- bleed: if printing through a vendor, keep text and seal at least ~3 mm inside the trim edge

---

## 6. Fixed Front Layout

Do not redesign the layout.

```
┌──────────────────────────────────────────────────────────┐
│ CITY (spaced caps)                      SERIES LABEL     │
│ ──                                      (stacked, small) │
│ 城                                       ──              │
│ 市                                                       │
│      ░░ ink-wash sky ░░                                  │
│   ░░░   ☾          ░░░        大 号 书 法 标 题          │
│  ░░  ┌───────────┐  ░░        ROMANIZED SUBTITLE   [印]  │
│ ░░   │  painted  │   ░░                                  │
│ ░░   │  subject  │    ░░      诗句第一行，               │
│  ░░  └───────────┘ ░░░░░░     诗句第二行。               │
│   ░░ foreground / setting ░░       —— 作者《篇名》       │
│     ░░░░░░░░░░░░░░░░░░░░░░░░░░░                          │
│                                           tagline · 标语  │
└──────────────────────────────────────────────────────────┘
```

1. **Painting** — left ~55–65% of the width, bleeding slightly toward the lower and left edges
2. **Title block** — right ~35–40%, vertically around the upper-middle
3. **Poem block** — directly below the title block
4. **Corner labels** — top-left, top-right, bottom-right

Do not add any other visual modules.

---

## 7. The Painting

### Subject

- Repaint the **main subject of the source photo** (building, landmark, landscape) faithfully: keep its silhouette, proportions, roof lines, number of tiers, signage position, and distinctive structural details.
- Keep the photo's **camera angle**: a low upward shot stays low and looking up, a frontal shot stays frontal, a side view stays a side view. You may widen the frame a little to show surrounding setting, but do not move the viewpoint (e.g. do not turn a close upward shot into a distant view across water).
- Keep the photo's lighting mood (night stays night, golden hour stays golden, overcast stays soft).

### Signage

- Signage may be repainted but must keep its **exact characters and reading direction**. Traditional Chinese horizontal plaques often read right to left — keep that order.
- If it cannot be rendered legibly, paint it as an indistinct plaque shape with no readable characters. A blank plaque is always better than wrong text.

### Scene completion

The painting may extend the photo's frame with a restrained, plausible setting that fits the real place, the camera angle, and the time of day:

- sky treated as loose dry-brush ink-wash strokes
- one atmospheric accent such as a crescent moon, a few clouds, or mist
- surrounding trees, foliage, steps, or terrain visible from the same angle

Do not add other landmarks, water or skylines that the angle could not see, fireworks, boats, birds, people, or text that would change what the place is.

### Rendering

- ink-and-watercolor / gouache with fine ink linework on the architecture
- dry-brush, ragged vignette edges that fade into the paper — **no rectangular frame, no hard photo border**
- paper visible around the painting, especially at the top-right toward the text
- crisp detail on the subject; looser, more abstract strokes toward the edges

### Palette

Derive a limited palette from the photo's lighting, typically:

1. a dominant light color (e.g. warm lantern gold, soft daylight cream)
2. a deep atmosphere color (e.g. indigo / ink blue, misty grey-blue)
3. a dark structural color (e.g. deep green-black for trees, warm umber for timber)

plus the ivory paper and one small vermilion accent for the seal. Do not introduce colors the photo does not suggest.

### People and faces

Omit visitors from the painting, or reduce them to tiny, indistinct figures. Never paint an identifiable face.

---

## 8. Title Block

- **Title** — the place name in large, expressive brush calligraphy (行书 / 行楷 style for Chinese), dark ink-blue or ink-black, the largest text on the card
- **Romanized subtitle** — below the title, small, widely letter-spaced serif capitals
- **Seal** — one small vermilion square or vertical seal to the right of the subtitle, containing the city name (e.g. `长沙`) in seal or regular script

**Non-Chinese places:** use refined hand lettering (brush script or calligraphic serif) in the local language for the title; use a transliteration or English name as the subtitle only if it differs from the title, otherwise omit the subtitle; the seal contains the city's short name or initials in Latin capitals. Keep the same hierarchy.

---

## 9. Poem Block

- Two lines of verse, medium size, each line ending with its punctuation
  - Chinese: elegant Kai / Song (楷 / 宋) style
  - other languages: an elegant book serif, italic optional
- An attribution line below, right-aligned
  - Chinese: `—— 作者《篇名》`
  - other languages: `— Author, Title`

### Poem source rules

1. If the user provided a poem, use it exactly.
2. Otherwise choose a **real, well-known** poem genuinely associated with the place or city, quoted **word-for-word** with the correct author and title. Example: 杜甫江阁 → `夜醉长沙酒，晓行湘水春。` —— 杜甫《发潭州》. Prefer public-domain classical poems.
3. If you are not certain of the exact wording, author, and title, ask the user, or omit the poem block entirely. Never write a fabricated or paraphrased poem and attribute it to a real poet.
4. Original, user-requested verse may be used without an author line or with the user's own name.

---

## 10. Corner Labels

All small, quiet, and thin:

- **Top-left** — city name in widely spaced English capitals (`CHANGSHA`), a short rule, then the Chinese city name set vertically (`长 / 沙`); for non-Chinese places, the local-language city name set horizontally below the rule
- **Top-right** — the series label set as stacked small capitals (`A / CITY / IN / POETRY`), with a short rule below
- **Bottom-right** — optional tagline, at most 10 Chinese characters (or ~6 words) in two short phrases joined by ` · `

### Tagline rules

- Use the user's tagline if given.
- Otherwise you may write a short original phrase drawn from the scene (river, season, light), e.g. `湘江潮落 · 诗意长留`. It must not look like a quotation and must not carry an attribution.
- If nothing fits naturally, omit it.

---

## 11. Forbidden Front Elements

Do not add:

- the original photo as an embedded photographic image
- a rectangular frame around the painting
- more than one seal
- more than two lines of verse
- color swatches, palette strips, sample cards
- badges, logos, QR codes, stickers
- long captions, keyword lists, dates or GPS coordinates
- English translation of the poem (unless requested)
- rounded card corners

If uncertain, simplify.

---

## 12. Back

Generate a matching landscape `3:2` back on the same ivory paper.

Include:

- thin outer border in ink blue
- one vertical divider slightly right of center
- stamp box in the upper-right
- 3 or 4 address lines on the right
- large blank message area on the left

Optional:

- small `POST CARD · 明信片` text near the upper-left
- a very faint ink-wash echo of the front painting's skyline or silhouette along the bottom-left, low enough not to block writing
- the place name and city in tiny text near the lower-left

Do not add the poem block, a second seal, large painting, or anything that reduces writing space.

---

## 13. Priority Order

1. faithful repaint of the source photo's main subject, from the same camera angle
2. fixed front/back layout (Sections 6 and 12)
3. correct place name — never invented
4. real, correctly attributed poem — or none
5. correct or blank signage — never garbled
6. vignetted ink-and-watercolor rendering with visible paper
7. restrained palette derived from the photo
8. quiet corner labels
9. omit everything else

If any later rule conflicts with an earlier rule, keep the earlier rule.

---

## 14. Photo Analysis (fill before the templates)

Look at the source photo and write these down; they feed the `{braces}` below.

| Field | Example (杜甫江阁) |
|---|---|
| subject | three-tier Chinese pavilion with sweeping upturned eaves, white stone balustrades and stairs at the base |
| camera angle | low angle, close, looking steeply up at the front-left corner |
| signage | dark plaque under the top eave reading 杜甫江阁, right to left |
| time & light | night, warm floodlit gold eaves and balconies against a black sky |
| dominant light color | warm lantern gold |
| atmosphere color | deep indigo ink blue |
| dark structural color | deep green-black (trees), warm umber (timber) |
| atmospheric accent | a thin crescent moon |
| setting | dark trees at both sides, white stone stairs in the foreground |
| mood | quiet, nocturnal, festive glow |

---

## 15. Front Prompt Template

Substitute real values in `{braces}`. Delete a line entirely when its value is absent — never write a placeholder such as "TITLE" or "Unknown". For non-Chinese places, replace `{style phrase}` with `a refined ink-and-watercolor "city in poetry"` and adjust the text lines per Sections 8–10.

```text
Create a landscape 3:2 illustrated postcard front on warm ivory rice paper with fine visible grain, in an elegant {style phrase, default: Chinese "city in poetry"} style.

LEFT (about 60% of the width): repaint the main subject of the reference photo — {subject} — as a refined ink-and-watercolor / gouache illustration. Keep the same camera angle as the photo: {camera angle}. Keep the silhouette, proportions, roof lines, number of tiers, plaque position and structural details faithful to the photo. Lighting: {time & light}. Fine ink linework on the architecture, {dominant light color} highlights. Sky as loose dry-brush {atmosphere color} ink-wash strokes with {atmospheric accent}. Complete the scene with restrained {setting}. The painting fades into the paper with ragged dry-brush vignette edges; no rectangular frame. Omit visitors or keep them tiny and indistinct; no identifiable faces.

SIGNAGE: {signage, e.g. the plaque reads "杜甫江阁" from right to left} — paint these exact characters in this exact order, or leave the plaque blank if they cannot be rendered cleanly. No other readable text inside the painting.

RIGHT (about 40% of the width), on open paper:
- large expressive brush-calligraphy title: "{place name}" in dark ink blue
- below it, small widely spaced serif capitals: "{ROMANIZED SUBTITLE}"
- one small vermilion seal to the right of the subtitle reading "{seal text}"
- two lines of verse in elegant Kai/Song style: "{line 1}" / "{line 2}"
- right-aligned attribution: "{attribution, e.g. —— 杜甫《发潭州》}"

CORNERS, small and thin:
- top-left: "{CITY IN SPACED CAPS}", a short rule, then "{local city name}" {set vertically | set horizontally}
- top-right: "{SERIES LABEL}" as stacked small capitals with a short rule below
- bottom-right: "{tagline}" in small text

All text must be spelled exactly as given, character for character. Limited palette: {dominant light color}, {atmosphere color}, {dark structural color}, ivory paper, one vermilion seal.

Do not embed the photo itself, add a frame, color swatches, extra seals, logos, badges, extra text, or a translation of the poem.

Quality: highest detail, crisp ink lines, clean watercolor without mud, refined paper texture, low noise, largest available 3:2 resolution, suitable for later 4x upscaling.

Mood: {mood}, collectible.
```

---

## 16. Back Prompt Template

```text
Create a matching landscape 3:2 postcard back on the same warm ivory rice paper, with the same thin ink-blue line style as the front.

Keep it functional and mostly blank:
- thin outer border
- one vertical divider slightly right of center
- stamp box in the upper-right
- 3 or 4 address lines on the right
- large blank message area on the left

Optionally: small "POST CARD · 明信片" near the upper-left; a very faint ink-wash echo of the {place name} silhouette along the lower-left edge, low enough not to block writing; tiny "{place name} · {city}" near the lower-left.

Do not add the poem, a painting, extra seals, or decoration that reduces writing space.

Quality: clean sharp lines, refined paper texture, low noise, largest available 3:2 resolution, suitable for later 4x upscaling.
```

---

## 17. Common Failure Patterns (Avoid These)

- the photo is pasted in as a photograph instead of being repainted
- the painting sits in a hard rectangle instead of fading into the paper
- the building's structure drifts (wrong number of tiers, different roof shape, invented towers)
- the viewpoint drifts (a close upward shot becomes a distant waterfront panorama)
- garbled, mirrored, or reordered characters in the title, plaque, or seal
- a made-up poem attributed to a real poet, or a real poem with wrong wording
- the right side gets crowded with extra text, multiple seals, or translations
- the palette turns rainbow-colored or oversaturated instead of ink blue + light color
- the back loses writing space to a large painting

---

## 18. Quick Checklist

### Front

- [ ] landscape 3:2, ivory paper with grain
- [ ] subject repainted faithfully from the photo
- [ ] same camera angle as the photo
- [ ] plaque text exact and in the right order, or blank
- [ ] vignette edges, no frame, paper visible
- [ ] place name correct and calligraphic
- [ ] romanized subtitle and one seal
- [ ] poem real and correctly attributed, or omitted
- [ ] corner labels small and quiet
- [ ] limited palette derived from the photo
- [ ] no identifiable faces
- [ ] all text spelled exactly as given
- [ ] upscaled to at least 1800 × 1200 px before printing

### Back

- [ ] same paper and line style
- [ ] writing and address space preserved
- [ ] stamp box present

---

## 19. One-Line Definition

**Repaint the photo's main subject — same angle, exact or blank signage — as a vignetted ink-and-watercolor illustration on the left of a 3:2 ivory card, set the real place name in large brush calligraphy with a romanized subtitle, one seal, and a real, correctly attributed two-line poem on the right, add quiet corner labels, and pair it with a functional matching back.**
