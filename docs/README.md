# SOPs Nobody Reads — site docs

This folder preserves the thinking behind sopsnobodyreads.com — not just what got built, but why each decision was made. The site itself is small. The reasoning under it is what should survive.

## What's in here

- **[brand.md](./brand.md)** — Voice, design principles, the constraint philosophy. Read this before writing any new copy or generating any new images.
- **[decisions.md](./decisions.md)** — A chronological log of the meaningful choices made during the rebuild, with the reasoning preserved. When future-Sean asks "why did I do it this way," this is the answer.
- **[image-generation-notes.md](./image-generation-notes.md)** — Hard-won lessons from generating the site's illustrations using Seedream 4.5 on Replicate. Failure modes, prompt patterns, what works and what doesn't.
- **[technical-notes.md](./technical-notes.md)** — Implementation details that aren't obvious from the code itself. CSS gotchas, deployment workflow, file naming conventions.

## When to update these docs

After any meaningful change to the site, ask: did this change require a *decision*? If yes, add a dated entry to decisions.md. If the change involved a tricky bug fix, add a note to technical-notes.md. If you ran new image generations, add prompt patterns to image-generation-notes.md.

If the change was purely a typo fix or a minor copy edit that doesn't change the brand voice, you don't need to document it. The git commit message is sufficient.

## Repo structure (for reference)

```
/
├── index.html              # Home page (8 sections)
├── for/
│   ├── contractors/        # Live segment page
│   ├── property-managers/  # Live segment page
│   ├── warehouses/         # Live segment page
│   └── manufacturers/      # ARCHIVED — orphan page, no nav link
├── images/                 # All site images, both -c (compressed, in use) and originals (backup)
├── styles.css              # Single stylesheet
├── pitch.html              # Older pitch page (not in main nav)
├── tablet-pitch.html       # Tablet pitch page (not in main nav)
├── demo/                   # The LOTO demo (the conversion artifact)
├── vercel.json             # Vercel deploy config
└── docs/                   # You are here
```

## The one thing that matters most

From decisions.md: **this brand is about constraint.** Every choice should drill down to the idea and communicate only that. If a feature, copy block, or image makes the site *fuller*, it almost certainly makes it *worse*. The pressure to add more is constant. Resist it.
