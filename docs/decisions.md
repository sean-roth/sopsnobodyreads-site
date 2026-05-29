# Decisions log

Chronological record of the meaningful choices made during the rebuild. Captures the *why*, not just the *what*. New entries go at the top.

---

## 2026-05-28 — Site shipped to production after multi-day rebuild

The `rebuild` branch was merged into `main` and deployed to sopsnobodyreads.com. A safety tag `pre-rebuild-2026-05-28` preserves the prior state of `main` in case rollback is ever needed.

Final post-launch polish pass:
- Mobile padding fix (CSS shorthand padding bug — see technical-notes.md).
- Image compression (~80% size reduction across all 13 images via `-c` suffix convention).
- Copy refinements on contractor opener, property-managers ("phone" → "tablet"), warehouses ("different floor" → "dealing with vendors").
- Footer copyright added to all live pages.

## 2026-05-28 — Manufacturers segment cut, archived not deleted

**Decision:** Remove the manufacturers segment from the nav. Keep the orphan page at `/for/manufacturers/index.html` in the repo for potential future revival.

**Why:** The manufacturers segment fought us at every level — conceptually and technically. Conceptually, manufacturing's primitive is *the identical unit*, which is inherently abstract; the value of a single bolt is its relationship to a thousand other bolts. Technically, the image model literally cannot render five identical objects reliably, which is the exact thing the metaphor required.

We tried multiple framings (drift across iterations, the die/stamp concept, abstract geometric forms) and every approach failed for the same underlying reason — invented primitives don't render consistently, and the model's defaults dragged us toward bureaucratic seals with gibberish text.

The other three segments have native primitives that are *meaningful alone* and *in the business itself*: doors for property managers, boxes for warehouses, tools/daylight for contractors. Manufacturing didn't have an equivalent that we could invent from first principles. It would have needed a concrete object from a real Cook County manufacturer's floor — not a primitive reasoned toward in the abstract.

Cutting it was also the right *prioritization* call: Sean is not actively marketing to manufacturers in the current sales push, and continuing to fight the image model on a segment that wouldn't get used was avoidance of the actual goal (getting clients).

**Revival criteria:** If/when there is a concrete primitive object from a real manufacturer's floor — something visually distinct and singular — the segment can be revived. The orphan page still has its original copy as a starting point.

## 2026-05-28 — Property managers scene 3: knob-flip exploit

**Decision:** For property managers scene 3 (the resolution image with the door fully open), accept that the doorknob ends up on the *left* side instead of the right (where it is in scenes 1 and 2).

**Why:** The image model has a strong default of opening doors at the hinge edge rather than the knob edge. Repeated attempts to force the model to open the door at the knob edge while keeping the knob on the right side produced inconsistent results (often dramatic 3D perspective changes that broke the flat-graphic style established in scenes 1 and 2).

The solution was to *use* the model's flipping tendency rather than fight it: prompt for knob-on-the-LEFT with hinges-on-the-RIGHT, which the model rendered cleanly in the same flat front-view style as the other two scenes. The knob position differs across the triptych, but most visitors won't compare scenes 1 and 3 side by side — they scroll through sequentially with paragraphs of copy in between. The visual coherence (flat front-view in all three) is worth more than knob-position continuity.

This is the broader lesson: when fighting an image model's default behavior, sometimes the right move is to reframe the spec to *use* the default instead of resisting it.

## 2026-05-28 — Contractor opener: reframed twice

**Decision:** Final contractor opener: *"Your best worker is mid-project, three days in. The new hire is shadowing him, like he should be. Every fifteen minutes there's another question — about something the new hire could check himself, if anyone had handed him a way to."*

**Why:** The original opener ("on his fourth job of the day... should have picked up two jobs ago") treated contractor work like service calls — implying high volume and quick turnover. That's wrong for the actual target market: small commercial trade contractors on multi-day projects. A plumbing or electrical crew on a remodel is on the same project for days.

The first reframe corrected the time scale (mid-project, three days in) but accidentally introduced a different problem: it implied the new hire wasn't *retaining* what the senior taught, which framed the senior as a failing teacher and (more importantly) suggested the training *replaces* the senior-to-junior relationship.

The final version preserves the shadowing relationship explicitly ("like he should be") and identifies the actual pain accurately: the new hire is interrupting with questions that have written answers, because no one handed him a way to check himself. The training is a source of truth the worker can consult on his own time. It supports the human relationship, it doesn't replace it.

**The principle this surfaces:** when copy implies the product diminishes or replaces human relationships that the customer values, rewrite. The training is infrastructure, not a substitute teacher.

## 2026-05-27 — Image vocabulary: object-state-changes instead of figures

**Decision:** All segment-page triptychs use the same visual vocabulary: a single object (or small object cluster) shown in three states across the three scenes. No human figures, no hands, no faces.

**Why:** The brand's editorial restraint reads better when objects do the narrative work. Showing workers on tablets, hands holding phones, or people doing tasks pushes the visual register toward stock-photo corporate, which contradicts everything else about the site.

The vocabulary that worked:
- **Contractors:** ladder/toolbox/materials → open toolbox → same equipment at sunrise. The object cluster is the constant; what changes is its state and the time-of-day signal.
- **Property managers:** closed door → door ajar → door open with empty room beyond. The single object (door) progresses through three states.
- **Warehouses:** row of items with one askew → two items isolated side-by-side → row corrected. The composition changes between scenes but the visual unit (forest-green-on-cream items on a baseline) stays consistent.

**The principle:** when a visual triptych needs to tell a story, change the *state* of the same object rather than introducing new objects. The visitor's eye reads transformation more readily than comparison.

## 2026-05-25 — Switched to image-to-image for triptych consistency

**Decision:** Generate scene 1 of each segment with text-to-image. Generate scenes 2 and 3 using image-to-image with scene 1 as the source.

**Why:** Each segment's three images need to feel like the *same world* across the triptych — same lighting, same composition, same visual register. Text-to-image generation produces three independent images that don't share enough visual DNA. Image-to-image preserves composition and style while changing the specific elements we ask to change.

This means we got lucky on warehouses (all three scenes are visually coherent first-try) and worked harder on others (property managers door-direction required multiple iterations). But the workflow itself was the right call. See image-generation-notes.md for the specific prompt patterns that worked.

## 2026-05-25 — Home page: meditative, not transactional

**Decision:** The home page has 8 sections including hero, materials-exist, do-they-understand, what-the-work-is, process, costs, demo, and short-call. Each is its own beat. Segment pages are linked from a nav dropdown, not from the home page body.

**Why:** The home page is the brand's tone-setting surface. A prospect lands there and reads slowly, scrolls, considers. The segment pages do the conversion work. Putting segment-page CTAs in the home page body would make the home page feel like a marketing funnel; it should feel like reading a thoughtful magazine.

This is also why the home page does not have testimonials, logos, feature grids, or social proof. The restraint *is* the credibility signal.

## 2026-05-24 — "Who is this for" instead of "Industries" or "Use cases"

**Decision:** Nav dropdown labeled "Who is this for" instead of conventional alternatives.

**Why:** "Industries" implies a vertical-market sales motion. "Use cases" implies a feature-mapped enterprise pitch. "Who is this for" matches the brand's editorial register — it asks the visitor to self-identify rather than positioning the practice as a vendor with verticals. Small wording difference, meaningful tone difference.
