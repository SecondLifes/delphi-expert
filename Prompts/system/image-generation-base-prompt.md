# Image Generation Base Prompt — README Banners

The standard rules and structure for producing a kit's three README
banner images (Overview / Core Features / Design & Philosophy). Every
`Prompts/image-prompts.md` / `Prompts/image-prompts.tr-TR.md` in this workspace
(root, `blank-scaffold`, and every `spec-kits/*`) is a **customization**
of this file, not a parallel copy of it — this file owns the universal
rules and structure; the per-kit file owns only the mascot's
stack-specific accessory, the accent color, the anchor object, and the
three scenes' actual prompt text.

This file exists because the universal rules got copy-pasted near-
verbatim into 5+ separate files early on, and a lesson learned in one
place (the mascot concept, the ban on factory/gear backdrops, the
mandatory anchor-object rule) had to be manually re-applied to every
other file by hand. **This file is the single authoritative source of
the golden rules — but it is not auto-propagated.** Every copy (root,
`blank-scaffold`, and each `spec-kits/*`) is a separate physical file;
there is no hard-link or sync-hardlinks.ps1 pairing for `Prompts/`
content the way there is for `.claude/skills` ↔ `.agents/skills`. When
this file changes, manually re-copy it to every other location (`rad-
template-builder`'s Maintenance Mode convention covers this for
blank-scaffold → existing-kit propagation) and re-verify each per-kit
`image-prompts.md` still complies — don't rely on "update it here once"
alone to mean every consumer is current.

## Why a mascot, not abstract energy

Two failure modes, both observed on this workspace's own kits before
converging here:

1. **Pure abstract energy** (glowing crystals/beams/orbs) — could be an
   ad for literally anything. Proven on this workspace's own root README
   banners.
2. **"Concrete objects" without a consistent character, and an
   underspecified "workshop" mood** — the model defaults to a generic
   sci-fi robot-factory scene (gears, pipes, catwalks) that has nothing
   to do with the kit's actual subject, and "a laptop with a glowing code
   window" reads as any language/tool, not a specific one. Proven on this
   workspace's own `batch-script-expert` kit's first real generation.

What works: **one consistent mascot character** — a small, friendly
robot — interacting with a **mandatory, kit-specific anchor object** that
no other stack would plausibly show. Same principle as Docker's whale or
GitHub's Octocat: the character is the throughline across all three
images; the scene and the anchor object are what make each kit's set
unmistakably about *that* kit.

## Golden rules (apply in every per-kit file, do not restate — reference this section)

1. **One mascot design per kit, held constant across all three images.**
   Small, friendly, rounded robot body. Per-kit customization is limited
   to: (a) one small accessory tied to the kit's domain (a flame for a
   red-branded language, a CRT-monitor head for a terminal-heavy kit, a
   prism/lens for an auditing kit, etc.), and (b) the accent-colored
   glowing joints/screen-glow. Never redesign the base body per kit.
2. **A mandatory, kit-specific anchor object in every one of the three
   images — not implied, not a generic laptop.** The specificity test is
   **object + distinguishing structural detail**, not the object alone —
   ask: "with no legible text or logo, would this exact object, in this
   exact form, make sense for a *different* stack's kit too?" A bare
   terminal window or a bare magnifying glass fails this alone (most
   CLI/audit kits could claim either); what passes is the object plus a
   detail unique to this kit's own domain. Good examples already in use:
   a classic black Command Prompt window whose blinking underscore cursor
   sits inside a distinctly DOS-era thin-bordered chrome, not a generic
   modern terminal (`batch-script-expert`); a RAD-Studio/Delphi-style IDE
   window with its component-palette sidebar and Object Inspector panel
   shape, both structurally specific to that IDE (`delphi-expert`); a
   magnifying glass held over a document that itself shows a distinctive
   annotated-finding-markers layout, not a plain page (`prompt-analyzer-
   expert`).
3. **No sci-fi factory/gear/pipe/industrial backdrop, unless a factory is
   literally, specifically what the kit is about.** For every ordinary
   kit (a language/stack/domain expert), a plain desk, workbench, or
   office setting only — state the exclusion explicitly and negatively in
   every prompt (`no machinery or factory elements`); do not rely on a
   vague "workshop" mood description to keep the model away from a
   factory trope, it will not. **The one confirmed exception is this
   workspace's own root README** — `AI Spec-Kits Maker` genuinely *is* a
   factory that forges other kits, so its own three images deliberately
   keep a forge/factory motif (a workbench, a conveyor line producing
   identical templates). Don't generalize that exception to any other
   kit without an equally literal justification — "it sounds cool" is not
   one. **Because this exception exists, Golden Rule 8's baseline negative
   prompt does not apply verbatim to an exception file** — see Golden
   Rule 8 for exactly what's exempted and why.
4. **No legible text/letters/logos/watermarks** — except a UI chrome
   element's own structural shape (a title bar, button-shapes, a grid of
   plain rows) which is iconic, not prose. Never bake a title into the
   artwork; titles live in Markdown below the image.
5. **Accent color stays constant across a kit's own three images, and
   must be visually distinct from every other kit already built.** The
   list below **is** the authoritative color registry — there is no
   separate manifest file. Before picking a new color, check this list;
   after a new kit's color is decided, add it here in the same edit (not
   as a follow-up) so the registry never lags behind an actual kit.
   Current claims: `delphi-expert` = deep red/burnt-orange,
   `batch-script-expert` = terminal green on black, `prompt-analyzer-
   expert` = violet-to-cyan prism, this workspace's own root = molten
   gold/amber on graphite.
6. **Distinctness across the three scenes within one kit is still
   mandatory — and different *action* alone is not enough.** A real
   generation for `batch-script-expert` used three different actions
   (sitting, standing, leaning in) but all three still came back reading
   as "the same picture" — same medium-shot front-on framing, same
   distance from camera, same lighting angle, every time. Action and
   **camera/composition** must both change:
   - **Image 1 (Overview):** wide or medium-wide shot, mascot small-to-
     medium in frame, anchor object clearly visible but not filling the
     frame — an establishing shot.
   - **Image 2 (Core Features):** medium shot, mascot roughly centered
     and facing the camera, presenting outward — a "hero/product shot"
     framing, noticeably closer and more frontal than Image 1.
   - **Image 3 (Design & Philosophy):** close-up or a dramatic
     off-center/diagonal composition, tighter crop than either of the
     other two, an unusual camera angle (from above, from the side, low
     angle) — should look like a different photographer took it. The
     anchor object must still read clearly in this tighter crop — either
     fully in frame at close range, or deliberately out-of-focus/partially
     cropped in a way that's still recognizable, never cropped out of the
     scene entirely for the sake of the tighter framing.
   State the shot type and framing explicitly in the prompt text itself
   (e.g. `wide establishing shot`, `medium hero shot, centered`,
   `close-up, dramatic diagonal composition, low camera angle`) — don't
   rely on the action description alone to force different-looking
   results.
7. **Aspect ratio 1280×640 (2:1), PNG, no visible watermark.**
8. **Baseline negative prompt — every per-kit file's own negative prompt
   must include all of this verbatim, additions are fine, removals are
   not, EXCEPT for a Golden Rule 3 exception file (currently only this
   workspace's own root), which drops the last five terms
   (`factory background, gears, pipes, industrial machinery, sci-fi
   facility`) since they directly contradict its approved factory motif —
   every other term still applies unchanged:**
   `text, letters, words, logos, watermark, low quality, blurry,
   distorted hands, extra limbs, scary or menacing robot, photorealistic
   human, generic stock art, different mascot design between images,
   factory background, gears, pipes, industrial machinery, sci-fi
   facility`

   **This baseline must actually appear in the per-kit file, not just be
   referenced.** A pointer back to this section ("see the base prompt's
   golden rules") is not sufficient by itself — the per-kit file's own
   ready-to-paste content (a dedicated negative-prompt block, or repeated
   in each of the three prompt blocks if the target image model has no
   separate negative-prompt field) must contain the literal terms above
   (exception-adjusted per Golden Rule 3 where it applies), so it can be
   copy-pasted straight into a generator with nothing missing.

## What a per-kit `Prompts/image-prompts.md` actually needs to contain

Given this file owns the universal rules, a per-kit file only needs:

1. A one-line pointer back to this file for the golden rules (don't
   restate them).
2. The kit's own mascot accessory + accent color (Golden Rule 1/5).
3. The kit's own anchor object, named concretely (Golden Rule 2).
4. **The actual baseline negative prompt (Golden Rule 8), verbatim and
   exception-adjusted** — this is content, not a rule to reference, so it
   must be pasted in full, not linked.
5. The three actual, ready-to-paste prompt text blocks — these can't be
   avoided, since they're literally what gets pasted into an image
   model — one per image slot, each naming the specific scene/action for
   that slot (Golden Rule 6).

## Output

Generated images save as PNG under each kit's own `docs/images/`
(`overview.png`, `core-features.png`, `design-philosophy.png`), shrunk
with `tools/resize-images.bat` if oversized, then swapped into that kit's
`README.md`/`README.tr-TR.md` commented-out placeholders.
