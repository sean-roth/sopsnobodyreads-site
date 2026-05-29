# Image generation notes

What was learned generating the site's illustrations using Seedream 4.5 on Replicate. These notes are meant to save the next round of image work from re-discovering the same lessons.

## Model and workflow

- **Model:** `bytedance/seedream-4.5` on Replicate. Earlier work used 4.0; 4.5 has materially better instruction-following and style consistency.
- **Interface:** Two paths — the Replicate API (via MCP) for text-to-image, and the Replicate web UI at https://replicate.com/bytedance/seedream-4.5 for image-to-image (which requires uploading a source image).
- **Cost:** ~$0.04 per generation. A full segment triptych typically cost $0.50–$1.00 including iterations. Total image spend for the site build was roughly $5.
- **Prompts are limited to 2000 characters.** Beyond that, the model auto-truncates.
- **No seed parameter exposed in the MCP tool.** Each generation is fresh.

## The brand's visual prompt template

Every image prompt should anchor the brand's visual language up front. The pattern that worked:

```
Editorial illustration in the tradition of Christoph Niemann.
[Describe the subject and composition.]
Rendered in deep forest green as flat graphic shapes on a uniformly warm
cream background.
[Specific compositional details — placement, scale, orientation.]
Flat graphic shapes only, no painterly brushwork, no realism, no perspective.
Two colors only: deep forest green [subject], warm cream background.
No figures, no hands, no [other anti-instructions].
New Yorker spot illustration about [the metaphor in one phrase].
```

The "in the tradition of Christoph Niemann" cue and the "New Yorker spot illustration" closer are doing real load-bearing work. They steer the model away from corporate-illustration aesthetics and toward editorial restraint.

## Anti-instructions matter more than expected

The model has strong defaults that need explicit refusal:

- "No portals, no doorways, no perfect circles" — prevents the model from inventing common compositions.
- "No painterly brushwork, no realism, no perspective" — keeps the flat graphic style.
- "No figures, no hands" — when the metaphor doesn't require humans.
- "No text, no labels, no numbers" — see below.

If you don't say what you *don't* want, the model will fill those gaps with its training-data defaults, which tend toward generic editorial illustration with hands, faces, perspective, and atmospheric effects.

## What absolutely doesn't work

### Text inside images

**Abandon any concept that requires readable text.** The model produces gibberish lettering reliably (we got "BATH," "LIPANA," and other invented words on stamp/seal attempts). This was a major reason the manufacturers "die/stamp" concept failed — every render dragged toward notary-seal aesthetics with fake text.

If a concept *needs* text to communicate, redesign the concept rather than fighting the model.

### Five-of-the-same

The model cannot reliably render five identical objects in a row. This is a hard limit. Any metaphor that requires "the same thing repeated to show consistency" will fail. We hit this trying to show a manufacturing line of correctly-built parts; the brackets always drifted in subtle ways even when prompted explicitly.

Workaround: if the metaphor needs repetition, use marks/stamps/impressions (which are *allowed* to vary slightly and still read as the same stamp) rather than discrete objects.

### Subtle differences that need to be obvious

The warehouse "two similar items shown side by side" concept worked because we asked for items that were *clearly distinct when shown together* but similar enough to be confused individually. If we'd asked for items that were *almost identical except for a tiny detail*, the model would either make them identical or make them wildly different — never the threading needle of "obviously similar, subtly distinct."

## Y-axis flipping behavior

**The single most important model behavior to understand.** When asked for mirror-symmetric edits (e.g. "change a sunset on the right to a sunrise on the left"), the model often just flips the entire image on the Y-axis. Technically correct — sun is now on the other side — but the asymmetric details that anchored the composition are now flipped too.

**Two ways to handle this:**

1. **Fight the flip with asymmetric anchors.** Pin specific objects to specific sides explicitly: "The ladder stays on the LEFT side of the cluster. The toolbox stays in the MIDDLE. The materials stay on the RIGHT." Then state the change as something that *doesn't* require a mirror: "The shadows that currently stretch left now stretch right because the morning sun is now on the left." Add a "do not mirror or flip" instruction up front and as a closer. This worked for the contractor sunset → sunrise edit.

2. **Use the flip as a feature.** If you can reframe the spec so the natural flipped result *is* what you want, do that. The property-managers scene 3 worked this way: we wanted a door open at the knob edge, but the model kept opening at the hinge edge no matter what. The solution was to flip the entire door specification — ask for knob-on-LEFT (with hinges-on-RIGHT), and the model rendered exactly the open-door configuration we wanted, just mirrored from our original mental model. The knob position is inconsistent with scenes 1 and 2, but it reads cleanly on sequential scroll.

## Image-to-image prompt patterns

For edits to existing images (scene 1 → scene 2 → scene 3 progressions):

### Use causal physics framing instead of positional commands

*Bad:* "Show a thin sliver of cream on the right edge of the door."
*Good:* "A door opens at the knob edge, not the hinge edge. The doorknob is on the right side, so the door opens by pulling the right edge away from the doorframe. A thin sliver of cream appears along the right edge by the knob."

The model follows physical logic better than spatial coordinates. Tell it *why* the change makes sense, not just *where* the change should appear.

### Use "replace" not "keep" when changing multiple elements

*Bad:* "Keep the leftmost bracket the same, change brackets 2 through 5."
*Good:* "Use the leftmost bracket as a template. Replace brackets 2 through 5 with copies of the leftmost."

"Keep" framing makes the model conservative about *all* changes, including ones you wanted. "Replace" framing gives explicit permission to override the source for the parts you want changed.

### Avoid "exactly the same"

Saying "keep everything else exactly the same" makes the model reproduce the source image rather than editing it. Counterintuitive but real. Be specific about what should be preserved ("keep the cream background, the green ground line, the row of items in the same horizontal position") rather than using blanket preservation language.

## Concept selection: native primitives only

The deepest lesson from the manufacturers failure: **the brand's visual vocabulary only works for objects that are meaningful alone and exist in the business itself.**

The three working segments all have this:
- **Property managers:** doors. A door is meaningful as a single object (threshold, entry, being a guest). Property managers' entire business is *the doors they serve*.
- **Warehouses:** boxes. A single box is a unit of count, right or wrong, present or missing. Warehousing exists because of countable boxes.
- **Contractors:** tools and daylight. A toolbox is meaningful alone. Daylight is the constraint the work runs against.

Manufacturing's primitive is *the identical unit* — which is inherently abstract (a single bolt means nothing) and unrenderable (the model can't do five identical objects). Invented primitives (the die, the stamp, the abstract "reference shape") all failed because they had no anchor in the business itself.

**For any future segment work:** before generating any images, identify the segment's native primitive. Is it singular? Is it meaningful as a single object? Does it exist in the actual business? If any of those answers is no, the segment will fight you. Either find a different primitive or skip the segment.

## Compression workflow

Final pass: all 13 images were compressed via squoosh.app using MozJPEG at quality ~75–80. The compressed versions live alongside the originals with a `-c` suffix:
- Home page images: `home-hero-torch-c.jpg`, `home-2-tools-c.jpg`, etc.
- Segment images: `contractor-1c.jpg`, `property-managers-1c.jpg`, etc.

Original files are preserved in `images/` as backup. The HTML references only `-c` versions. This kept the originals available if we ever need to re-compress at different settings or regenerate variants.

Compression results: 78–91% size reduction per image with no visible quality loss at site display sizes. Home page first-load dropped from ~3.1MB to ~488KB. This is the perf win that made the Samsung Galaxy test feel fast.
