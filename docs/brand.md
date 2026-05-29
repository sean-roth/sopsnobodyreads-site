# Brand voice and design principles

## The core principle

**This brand is about constraint.** Drill down to the idea and communicate only that.

Everything else flows from this. When in doubt, cut. When tempted to add a feature grid, a testimonials section, a longer pitch, an additional segment — stop and ask whether the addition serves the idea or muddies it. Almost always: muddies.

## Voice

### What it sounds like

- **Editorial, measured, cave-painting register.** Spare prose that trusts the reader to make connections. Sentences sit on their own lines when they're load-bearing. Short paragraphs over long ones.
- **First person singular.** Sean is the sole practitioner. The brand uses "I," never "we." If a sentence would naturally use "we capture the procedure correctly," rewrite to passive voice: "the procedure is captured correctly."
- **Names what's true rather than what flatters.** No marketing hype. No superlatives. "A small practice built around a specific kind of translation" is the tone, not "the leading provider of custom training solutions."
- **Specific over general.** "The PDF on the office computer. The binder in the truck." These name the actual artifacts. Generic phrasing ("training materials") would be weaker.

### What it sounds like on home vs segment pages

- **Home page = meditative.** Each section is a feeling that hangs in the air. The visitor reads slowly. The role of the home page is to set tone and frame the problem; it is not transactional.
- **Segment pages = transactional.** Three-image triptych structure (problem → intervention → resolution). Name the pain, validate the emotion, show the better life. These pages convert.

### What to avoid

- The word "we." Use passive voice instead.
- Marketing hype, superlatives, jargon ("solutions," "leverage," "empower," "streamline").
- Diminishing the human relationships the training supports. The training is a *source of truth* the worker can check on their own time. It is not a replacement for the senior teaching the junior. When this distinction blurs in copy, rewrite.
- Suggesting any visual or tonal register that competes with the cave-painting/editorial-restraint tone. No emojis. No exclamation points in body copy. No "!" anywhere.

## Design principles

### Palette

Defined in `styles.css` `:root`:

- `--ink: #1f3a2e` — Forest green. Primary text and illustration color.
- `--bg: #f4ede0` — Warm cream. Background and negative space.
- `--accent: #3a1f2e` — Dark aubergine. Links, accent details in illustrations.
- `--utility: #6b5d4a` — Warm gray. Secondary text, footer.
- `--rule: rgba(31, 58, 46, 0.12)` — Hairline dividers.

Do not introduce additional colors without a strong reason. The whole visual identity rests on this palette being tight.

### Typography

- **Lora** (serif): display and body. Italic for hero-sub.
- **IBM Plex Sans**: utility only (nav, footer, scroll cues, figcaptions).

The serif/sans split is load-bearing. Serif is for the content the visitor is meant to read carefully. Sans is for chrome and navigation. Keep this distinction.

### Layout

- Single column, generous breathing room, narrow reading column (`--measure: 36rem` ~ 576px).
- Each home-page section is its own beat — visitors should be able to scroll, stop, breathe, and continue. Hairline dividers between consecutive sections reinforce the beat structure.
- No grids of features. No card layouts. No icon rows. The site is a sequence, not a dashboard.

### Images

- **Two colors only**: deep forest green on warm cream, with dark aubergine for accents (shadows, sun, time-of-day indicators).
- **Flat graphic shapes only.** No painterly brushwork, no realism, no photographic textures.
- **Editorial register** — think Christoph Niemann, New Yorker spot illustrations, primitive cave-painting iconography. Not corporate-cute, not cartoon, not 3D rendered.
- **No text inside images.** The image model produces gibberish text reliably; any concept that requires readable text is a non-starter. Abandon early.
- **No figures, no hands, no human silhouettes** unless absolutely load-bearing. The brand reads better when objects do the work.

## What's load-bearing about the LOTO demo

The full OSHA Lockout/Tagout course at `/demo` is the conversion artifact. It is the clearest way to show what the work looks like without explaining. A prospect can experience the training in 30 minutes and understand the offering more deeply than any copy would teach them.

The demo also functions as a technical credibility signal: it works on a seven-year-old phone. New hires on Cook County job sites are not carrying the newest device. The training meeting the worker where the worker actually is matters more than any feature.

**The demo should always be linked from the home page.** If a future redesign removes the demo link, that is almost certainly a mistake.

## Pricing structure (for reference)

The site is designed around three tiers. Copy and segment narratives should match what's actually being sold:

- **Pilot module — $3,500.** One topic. A real proof of concept.
- **Onboarding starter — $14,000 to $18,000.** Four topics. **This is the tier the segment pages model.** Segment narratives should depict new-hire scenarios, not veteran-refresher scenarios, because the onboarding tier is the most common buy.
- **Full program — starting at $32,000.** Sequenced full curriculum. OSHA topics included where the work requires them.
- **Hosting — $200–$400/month, optional.** Only if the client doesn't already have an LMS.

If segment-page copy starts to drift toward veteran-experience scenarios (e.g. "He has walked into 200 apartments this year"), that drift contradicts the pricing tier the segment is designed to sell. Rewrite to a new-hire frame.
