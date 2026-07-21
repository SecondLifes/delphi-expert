# AI Image Prompts — README Banners

Three banner images for this kit's `README.md` / `README.tr-TR.md`.
Generate with any capable image model (Nano Banana Pro, Midjourney v7,
Flux, GPT-Image, etc.) at a **wide 16:9 banner aspect ratio**, save as
PNG under `docs/images/` (`overview.png`, `core-features.png`,
`design-philosophy.png` — shrink oversized model output first with
`tools/resize-images.bat`). The `README.md`/`README.tr-TR.md` image tags
are already live; new pictures replace the old ones the moment the files
land.

This file is **self-contained** — no shared base prompt exists anymore.
Every spec-kit owns a completely distinct visual world; this kit must
never reuse another kit's palette, central object, or metaphor.

## Art direction — "The Oracle's Flame"

Delphi is named after the ancient sanctuary of Delphi, and its symbol has
always been the flame. This kit's philosophy is *"Zero-Leak by
Construction"* — nothing entrusted to the craftsman is ever lost. The
imagery: **an ancient Greek temple world where a sacred flame burns with
engineered precision**, rendered as epic painterly matte-paintings. No
robots, no mascots, no computers — the metaphor carries everything.

- **World:** the clifftop sanctuary of Delphi — marble columns, bronze
  tripods, engraved stelae, mountain dusk.
- **Palette:** moonlit white marble and dusk indigo-purple sky against a
  single powerful red-orange flame (Delphi's classic flame red).
- **Style:** epic painterly matte-painting, dramatic natural light,
  volumetric fire glow, museum-quality detail.
- **Consistency:** all three images share this same world and palette;
  each uses a different shot type and camera angle.

## Negative Prompt (paste into every generation)

```
text, letters, readable words, logos, watermark, low quality, blurry,
humans, faces, gods, statues of people, robots, mascots, cartoon style,
computers, screens, keyboards, modern objects, daylight blue sky,
different art style between images
```

## Image 1 — Overview (`docs/images/overview.png`)

**Slot:** top of the README, under the title/badges.
**Shot:** wide establishing shot from across the valley.

**Prompt:**
```
An epic wide matte-painting of the ancient sanctuary of Delphi on a
mountain cliff at deep dusk: moonlit white marble columns of a circular
temple, and at its center a bronze tripod holding one powerful,
perfectly steady red-orange flame — the only warm light in the scene.
Sparks rise from the flame high into the indigo-purple sky and settle
into faint glowing constellations of abstract geometric glyphs, as if
the fire itself is writing among the stars. A single eagle circles far
above the temple. Mist in the valley below, dramatic scale, epic
painterly matte-painting, volumetric fire glow, wide 16:9 banner
composition, museum-quality detail.
```

## Image 2 — Core Features (`docs/images/core-features.png`)

**Slot:** top of the "Key Guidelines" / core-features section.
**Shot:** medium symmetrical shot at altar height.

**Prompt:**
```
A medium, perfectly symmetrical shot of an ancient bronze tripod brazier
standing between two marble columns at dusk: its single red-orange flame
divides into four calm, steady tongues of fire, and each tongue cradles
a floating emblem forged of glowing bronze light — two interlocked
solid rings that cannot separate (nothing acquired is ever lost), a
balanced two-pan scale in perfect equilibrium (principled structure), a
laurel wreath closed into a complete circle (proven and verified work),
and a fluted column capital carrying weight (layered architecture).
Moonlit marble surfaces, indigo dusk behind, the four emblems clearly
distinct in silhouette and evenly spaced. Epic painterly matte-painting,
volumetric fire glow, wide 16:9 banner composition, museum-quality
detail.
```

## Image 3 — Design & Philosophy (`docs/images/design-philosophy.png`)

**Slot:** top of the "Design & Philosophy" section.
**Shot:** dramatic low-angle close-up, night.

**Prompt:**
```
A dramatic low-angle close-up at night: a freshly carved marble stele
standing tall among weathered ancient ruins, its surface engraved with
an immaculate, unbroken geometric pattern of interlocking channels that
glow faintly red-orange from within — every joint of the pattern
flawless, no channel left open, no line left unfinished. A bronze
chisel and mallet rest at its base, work just completed. The sacred
flame burns out of focus in the background, its light reflecting off
the polished new marble while the old ruins stay dark — an old craft,
still building things that outlast their builders. Indigo night sky,
epic painterly matte-painting, volumetric fire glow, wide 16:9 banner
composition, museum-quality detail.
```
