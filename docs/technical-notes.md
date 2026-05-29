# Technical notes

Implementation details, gotchas, and bug fixes that aren't obvious from the code itself. Each entry is something that would cost time to re-discover.

## The mobile padding bug (load-bearing — read this before touching CSS)

**Symptom:** On mobile viewports, text and images touch the left and right edges of the screen with no padding. Desktop looks fine.

**Cause:** Section elements (`.hero`, `.section`, `.scene`, `.closing`, `.segment-hero`, `.site-footer`) and the `.page` class were applied to the same HTML elements (e.g. `<section class="hero page">`). Both used CSS shorthand `padding:` declarations:

```css
.hero { padding: 2.5rem 0 2rem; }  /* sets left/right to ZERO */
.page { padding: 0 1.5rem; }       /* tries to set left/right to 1.5rem */
```

Shorthand `padding:` is a single declaration that sets all four sides. When two `padding:` shorthand declarations apply to the same element, CSS cascade picks one winner; the other is fully discarded. The section rules won because of source order, so `.page`'s left/right padding was thrown away entirely.

**Fix:** Split all padding declarations on dual-classed elements into directional properties:

```css
.hero { padding-top: 2.5rem; padding-bottom: 2rem; }
.page { padding-left: var(--gutter); padding-right: var(--gutter); }
```

Directional properties don't fight each other the way shorthand declarations do.

**Watch out:** any future section class that uses shorthand `padding:` and is combined with `.page` will silently reintroduce this bug. Always use `padding-top` / `padding-bottom` only on section classes, and `padding-left` / `padding-right` only on `.page`.

A defensive `img { max-width: 100%; height: auto; }` is also in place globally to prevent any image from overflowing its container.

## File naming conventions

- **Segment images:** `[segment]-[scene][modifier].jpg`. Original: `contractor-1.jpg`. Compressed: `contractor-1c.jpg`. The `c` immediately after the scene number is the compressed marker.
- **Home page images:** `home-[position]-[descriptor][modifier].jpg`. Original: `home-2-tools.jpg`. Compressed: `home-2-tools-c.jpg`. The home page uses a separator hyphen before the `c` because the names are multi-word descriptors.

Why two conventions? The segment names are short numeric scene markers where `1c` reads cleanly. The home page names are descriptive strings where `home-2-tools-c` is clearer than `home-2-toolsc`. Either pattern is fine as long as the existing convention is preserved.

**The HTML references only `-c` versions.** Originals are kept in `images/` as backup. If you ever want to delete the originals to reduce repo size, that's safe — nothing references them.

## Deployment workflow

- **Hosting:** Vercel, autobuild from the `main` branch.
- **Custom domain:** sopsnobodyreads.com.
- **Deploy time:** Usually 30–90 seconds from `git push` to live.
- **Cache behavior:** Vercel's CDN edge cache can hold stale assets for 1–2 minutes after a deploy. This is normal. Force-refresh or wait if newly-added images don't appear immediately.
- **Troubleshooting stale assets:** If a deploy claims green but assets are missing or wrong, the most reliable fix is to push a no-op commit (e.g. add an HTML comment to one file). This forces Vercel to do a fresh clone-and-build rather than reusing build cache. We hit this once during the rebuild — see the commit history for `force Vercel redeploy after scene-3 image upload`.

## Git branch hygiene during this rebuild

The full site rebuild happened on a `rebuild` branch over multiple days. When complete, `rebuild` was merged into `main` via a GitHub PR, and a release tag `pre-rebuild-2026-05-28` preserves the prior state of `main`. The `rebuild` branch itself was deleted after merging.

For future large changes, the same pattern is worth repeating: branch, build, review the PR carefully, tag the pre-merge state of main as a safety net, then merge and delete the branch.

## Files in the repo that aren't part of the main nav flow

- `pitch.html` — Older one-page pitch document. Not in the main nav. Used for direct sharing in conversations.
- `tablet-pitch.html` — Tablet-optimized version of the pitch. Same purpose.
- `for/manufacturers/index.html` — **Orphan page.** Archived but not deleted. No nav link to it; reachable only by direct URL. See decisions.md for the reasoning.
- `demo/` — The LOTO course. The conversion artifact. Linked from the home page "A demo" section.
- `images/contents.md` — Single-character placeholder file (probably a directory marker). Harmless.
- `images/after-loto-player.png`, `images/before-osha-sop.png` — Pitch-deck assets, not used in the main site flow.

If any of these break or change, refer to this list before assuming it's part of the main site.

## CSS architecture summary

- **Single stylesheet:** `styles.css`. No build step, no preprocessor, no PostCSS. The file is short enough (under 400 lines) that splitting would add complexity without value.
- **No CSS framework.** No Tailwind, no Bootstrap, no utility-class system. Plain CSS with custom properties for design tokens.
- **Mobile-first breakpoints:** `@media (max-width: 640px)` for phone-and-small-tablet adjustments, `@media (max-width: 460px)` for narrowest viewports (currently only hides the "Book the call" link in nav).
- **Layout primitives:** `.page` for horizontal page constraint, `.prose` for the reading column (max-width 36rem), `.figure` for image containers.
- **`text-wrap: pretty` is set on `.prose p`** to avoid orphan words at line breaks. Browser support is recent (2024+) but degrades gracefully.

## Things Sean explicitly does not want

- **No Next.js.** Plain HTML/CSS or lightweight alternatives only. This site is the canonical example of what "lightweight" means here: hand-written HTML, no framework, no build step.
- **No JavaScript unless necessary.** The current site has zero custom JS. The only client-side logic is HTML `<details>` for the nav dropdown.
- **No analytics tracking pixels, no marketing tags, no third-party scripts.** This is a personal practice site, not a SaaS funnel.

If any of those change in the future, document why in decisions.md.
