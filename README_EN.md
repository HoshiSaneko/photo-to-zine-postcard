# Photo to Zine Postcard

[中文](README.md) · **English**

Turn your own travel or landmark photo into a "city in poetry" watercolor postcard: the photo's subject repainted as an ink-and-watercolor illustration, paired with a calligraphic place name, a line of classical verse, and a small red seal.

![Du Fu River Pavilion, Changsha — poetic night postcard](assets/changsha-dufu-jiangge-front.png)

This repository provides a reusable, **platform-agnostic** [`SKILL.md`](SKILL.md) that turns one source photo into a coordinated postcard set. It works with ChatGPT, Gemini, Claude.ai, or agentic tools like Claude Code:

- **Front** — landscape `3:2`. On the left, the photo's main subject is faithfully repainted in ink and watercolor, fading into the paper with dry-brush edges. On the right: the place name in large brush calligraphy, a romanized subtitle, a small vermilion seal, two lines of verse with attribution, and quiet corner labels.
- **Back** — a matching functional postcard back with stamp box, divider, address lines, and writing space.

## Quick Start

### Option A: generate directly in chat (ChatGPT / Gemini / Claude.ai or similar)

1. Give the assistant this repository, or upload [`SKILL.md`](SKILL.md).
2. Upload a photo you took, ideally with the place name and city.
3. Ask it:

```text
Please read SKILL.md from this repository and follow it as the only design specification.
Generate a Photo to Zine Postcard front and back from my uploaded photo. Place: Du Fu River Pavilion, Changsha.
```

You can supply your own poem. Otherwise the assistant picks a real classical poem associated with the place and checks its wording and attribution; if unsure, it asks or leaves the poem out.

### Option B: use it from Claude Code or another agentic tool

- If an image-generation tool/API is configured, ask the agent:

  ```text
  Read SKILL.md from this repository as the design specification. Using this photo, call the
  available image-generation tool with the Front and Back Prompt Templates to produce the
  postcard front and back, and save the results.
  ```

- If no image-generation tool is available, the agent outputs the filled-in Front and Back prompts for you to paste, together with your photo, into any image-capable assistant.

Either way, the agent should look at the photo first and fill the Photo Analysis in `SKILL.md` Section 14 (subject, camera angle, signage, palette) before using the templates. See Section 3, "Supported Agents & Invocation", in `SKILL.md`.

## Design Principles

1. **Faithful to the photo** — the repainted subject keeps the real silhouette, tiers, rooflines, signage position, camera angle, and lighting mood; plaque text is either exact and in its original reading order, or left blank — never garbled.
2. **Ink and watercolor with open paper** — the painting fades into ivory rice paper; no rectangular frame.
3. **Real names, real poems** — never invent a place name; verse must be quoted exactly with correct author and title, or omitted.
4. **Restrained layout** — one calligraphic title, one seal, two lines of verse, small corner labels, nothing more.
5. **Palette from the photo** — typically one light color (lantern gold), one atmosphere color (indigo), one dark structural color.

## Default Output

- landscape `3:2`
- reference size `150 × 100 mm / 6 × 4 in`
- warm ivory rice-paper texture
- repainted subject on the left, calligraphic title and poem on the right
- one vermilion seal
- matching functional postcard back
- generated at the model's largest native 3:2 size (e.g. `1536 × 1024`), upscaled to at least `1800 × 1200 px` before printing; 4× upscaling recommended

## Examples

**Changsha · Du Fu River Pavilion (杜甫江阁)**

| Source photo | Postcard front |
|---|---|
| ![Source photo](assets/changsha-dufu-jiangge-photo.jpg) | ![Postcard front](assets/changsha-dufu-jiangge-front.png) |

Verse: 夜醉长沙酒，晓行湘水春。—— Du Fu, *Leaving Tanzhou* (《发潭州》)

The tagline 湘江潮落 · 诗意长留 is an original phrase, not a quotation. The full prompts used are in [`examples/changsha-dufu-jiangge-prompts.md`](examples/changsha-dufu-jiangge-prompts.md).

### Ningbo Postcards

These examples explore Tianyi Ge, Dongqian Lake, and the Old Bund with varied scenery and layouts. The short phrases in these images are original copy, not quotations from classical poetry.

| Tianyi Ge · Garden and Pond | Dongqian Lake · Lighthouse |
|---|---|
| ![Tianyi Ge postcard](assets/ningbo/tianyi-ge.png) | ![Dongqian Lake lighthouse postcard](assets/ningbo/dongqian-lake-lighthouse.png) |

| Old Bund · Bridge Lights | Dongqian Lake · Lakeside Palms |
|---|---|
| ![Ningbo Old Bund postcard](assets/ningbo/old-bund.png) | ![Dongqian Lake palms postcard](assets/ningbo/dongqian-lake-palms.png) |

**Dongqian Lake · Quiet Waterfront**

![Dongqian Lake waterfront postcard](assets/ningbo/dongqian-lake-waterfront.png)

### Printed Examples

Photos of the printed postcards show the paper texture and finished results. Lighting can affect the colors seen in these photos.

| Tianyi Ge | Dongqian Lake Lighthouse |
|---|---|
| ![Printed Tianyi Ge postcard](assets/printed/tianyi-ge.jpg) | ![Printed lighthouse postcard](assets/printed/dongqian-lake-lighthouse.jpg) |

| Old Bund | Dongqian Lake Palms |
|---|---|
| ![Printed Old Bund postcard](assets/printed/old-bund.jpg) | ![Printed lakeside palms postcard](assets/printed/dongqian-lake-palms.jpg) |

**Dongqian Lake · Printed Waterfront Postcard**

![Printed waterfront postcard](assets/printed/dongqian-lake-waterfront.jpg)

## Customize Your Own Version

You can change the illustration style, paper, typography, ratio, corner labels, back design, and more. Change one dimension at a time; the "painting on one side, text on the other, open paper, one seal, two lines of verse" structure is what keeps results stable. For major style experiments, a variant is preferable to making the default skill more complicated.

## Repository Structure

```text
photo-to-zine-postcard/
├── SKILL.md
├── README.md
├── README_EN.md
├── LICENSE
├── examples/
│   └── changsha-dufu-jiangge-prompts.md
└── assets/
    ├── changsha-dufu-jiangge-photo.jpg
    ├── changsha-dufu-jiangge-front.png
    ├── ningbo/     # Ningbo postcard images
    └── printed/    # Photos of printed postcards
```

## Current Version

**v1.1.0**
- new Photo Analysis step: look at the photo and record angle, signage, and palette before prompting
- keep the photo's camera angle; plaque text must be exact and keep its reading direction, or be left blank
- resolution rule changed to "largest native size, then upscale", matching common image models
- templates no longer hardcode values; added title, seal, and attribution rules for non-Chinese places
- tagline may be original but must not pose as a quotation; added filled example prompts

**v1.0.0** — first public release.

## Contributing

Pull requests, new examples, and derivative styles are welcome. Please include the source photo and result for examples, and only submit photos you have permission to publish.

## License

MIT License. See [`LICENSE`](LICENSE).
