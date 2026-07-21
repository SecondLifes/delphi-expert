# AI Image Prompts — README Banners

Three banner images for this kit's `README.md` / `README.tr-TR.md`.
Generate with any capable image model (Nano Banana Pro, Midjourney v7,
Flux, GPT-Image, etc.), save as PNG under `docs/images/` (shrink
oversized model output first with `tools/resize-images.bat`), then follow
the placement instructions in `README.md`/`README.tr-TR.md`.

**Golden rules, mascot concept, and structure:** see
[`Prompts/system/image-generation-base-prompt.md`](system/image-generation-base-prompt.md) —
this file only customizes it for this kit, it doesn't restate it.

## This kit's customization

- **Mascot accessory:** a small flame-shaped accessory on its head or
  chest (a nod to Delphi's classic red flame logo).
- **Accent color:** deep red and burnt-orange, evoking Delphi's classic
  red flame logo.
- **Anchor object (mandatory in all three images):** a classic
  RAD-Studio/Delphi-style IDE window — a rectangular window with a
  blue-tinted title bar, a component-palette sidebar of small plain icon
  tiles, a code-editor pane with glowing amber-red monospace lines
  (illegible), and a property-inspector panel of plain two-column rows.
  No factory/gear/pipe backdrop — a plain desk only.
- **Shot variety (see base prompt Golden Rule 6):** vary shot type and
  camera angle across the three images, not just the mascot's action —
  same action with the same medium-shot framing three times still reads
  as one repeated image.

## Negative Prompt (mandatory baseline, Golden Rule 8)

Paste this exactly into the negative-prompt field for all three images:

```
text, letters, words, logos, watermark, low quality, blurry, distorted
hands, extra limbs, scary or menacing robot, photorealistic human, generic
stock art, different mascot design between images, factory background,
gears, pipes, industrial machinery, sci-fi facility
```

## Image 1 — Overview

**Slot:** top of `README.md`, right under the title/badges.
**Shot:** wide establishing shot — mascot small-to-medium in frame, desk
and surroundings visible.

**Prompt:**
```
A wide establishing shot, charming illustration of a small builder-robot
mascot (a compact, rounded robot with a small flame-shaped accessory on
its head, glowing deep-red/burnt-orange joints on a dark graphite
chassis) sitting at a plain desk seen from a slight side angle, both
hands near a keyboard, facing a classic RAD-Studio/Delphi-style IDE
window floating just above the desk — a rectangular window with a
blue-tinted title bar, a component-palette sidebar of small plain icon
tiles along one edge, a code-editor pane with glowing amber-red monospace
lines (illegible, just code-like), and a property-inspector panel of
plain two-column rows on the other side. Beside the window, a small
glowing padlock/latch icon sits closed and secure, and a couple of folder
icons with visible corner-folds sit stacked neatly. The whole scene —
mascot, desk, and window — is visible with room around it. Plain dark
graphite background, no machinery or factory elements, warm red-orange
rim lighting. Cinematic wide composition, high production value, digital
illustration, detailed.
```

## Image 2 — Core Features

**Slot:** at the top of the "Key Guidelines Taught to AI" section.
**Shot:** medium hero shot — mascot centered, facing the camera directly,
noticeably closer than Image 1.

**Prompt:**
```
A medium hero shot, centered composition: the same small builder-robot
mascot (flame-shaped head accessory, glowing deep-red/burnt-orange
joints on a dark graphite chassis, consistent design with the other
images in this set), facing directly toward the camera at chest height,
the same classic RAD-Studio/Delphi-style IDE window (blue-tinted title
bar, component-palette sidebar, code-editor pane, property-inspector
panel) floating just behind one shoulder, presenting four distinct
glowing icons floating between itself and the viewer: two interlocking
geometric joints that never separate (zero-leak memory management); a
set of modular connected platforms (SOLID/dependency injection); a
pulsing circular feedback-loop icon (the TDD red-green-refactor cycle);
and a stack of glowing rectangular platforms (layered clean
architecture). Plain dark graphite background, no machinery or factory.
Deep red and burnt-orange, each icon clearly distinct from its neighbors
in silhouette. Cinematic, charming, digital illustration, detailed.
```

## Image 3 — Design & Philosophy: Zero-Leak by Construction

**Slot:** at the top of the "Design & Philosophy" closing section.
**Shot:** close-up, dramatic diagonal composition, low camera angle —
noticeably tighter and more unusual framing than Images 1 and 2.

**Prompt:**
```
A tight close-up shot from a low, slightly diagonal camera angle: the
same small builder-robot mascot (flame-shaped head accessory, glowing
deep-red/burnt-orange joints on a dark graphite chassis, consistent
design with the other images in this set), its hands filling most of the
frame, carefully and precisely snapping two glowing interlocking
geometric joint-pieces together, checking closely that they lock with
zero gap between them, a small satisfied glow radiating outward the
instant they click into place — the same classic RAD-Studio/Delphi-style
IDE window glowing softly out of focus in the background. Dramatic close
framing, shallow depth of field. Plain dark graphite background, no
machinery or factory, warm red-orange lighting, a strong sense of
deliberate, careful construction. Cinematic, charming, digital
illustration, detailed.
```
