---
name: implement-design
description: Implement screens designed in saymock as code in the current project, faithful to the design and to the codebase's own conventions (components, design tokens, routing, data). Use when the user asks to build, code, implement, integrate or "make real" a saymock screen, flow or component, or gives a saymock file URL (…/f/<fileId>) and asks for code.
---

# saymock design → code

Goal: the screen in the app looks like the saymock frame, and the code looks like the rest of the
codebase. `export_code` gives a faithful translation; your job is the adaptation.

## 1. Find the design

- A saymock URL `…/f/<fileId>` gives the file; otherwise `list_files` and ask if several match.
- `get_file` → pages, frames (screens) with ids and sizes, components. Pick the frames the user
  means; for a flow, take all its screens (they share components). Look at each with
  `get_screenshot` (scale 2) before writing code: it is the reference you will compare against.
- Open comments may carry decisions: `get_comments` on the file.

## 2. Study the codebase first

Before exporting, learn what the project already has, so the export is mapped onto it rather than
pasted next to it:

- Framework and styling: React / Vue / Svelte / other; Tailwind version and config (`@theme` or
  `tailwind.config.*`), CSS variables, design-token files. saymock designs use Tailwind v4
  utilities; on Tailwind v3 a few need translating (`size-*` exists since 3.4; `grid-cols-(--x)` →
  `grid-cols-[var(--x)]`; `bg-linear-*` → `bg-gradient-*`).
- UI components: buttons, inputs, cards, dialogs, nav, icons (`components/ui`, a shadcn/ui setup,
  a design-system package). Reuse them instead of the exported raw elements whenever they render
  the same thing.
- Icons library, routing, data fetching, i18n (user-visible strings may have to go into a
  catalog), file and naming conventions.

## 3. Export

`export_code` with the frame ids (all screens of the flow in one call, so shared components come
once) and `framework`: "react" or "vue" when the project uses one, "html" otherwise (a standalone
resolved page to translate into the project's templates: Svelte, Astro, Blade, ERB…).

What it gives you:
- one file per screen and per saymock component (typed props, defaults from the design);
- template rows as a data array + loop: that is sample content, replace it with real data
  (props, API, store) unless the screen is static;
- notes: fonts to load, images hosted by saymock (download them into the project's assets if it
  keeps its own), prototype links (`data-goto`) to wire to the router.

## 4. Adapt

- Map raw elements to the project's components (a `<button class="rounded-lg bg-blue-600 …">` →
  the project's `<Button>`), and hard-coded colors to its tokens when they match (`bg-[#2563eb]` →
  `bg-primary`). Keep the design's spacing, sizes and typography otherwise.
- Keep saymock component boundaries as components; name them like the codebase does.
- Interactions the design only shows (hover, open menu, selected tab) become state.
- Screens are designed at a fixed frame size (e.g. 393×852 for a phone): make the root fill the
  page/viewport instead of hard-coding that size.

## 5. Verify

Run the app (or its component preview / Storybook) and compare with the saymock screenshot:
layout, spacing, typography, colors, icons. If you can take a screenshot of the running page,
upload it with `get_upload_url` + curl and use `compare_to_reference` on the saymock frame for a
measured diff. Fix, then run the project's lint, typecheck and tests.

Report what you built, which project components you reused, and what is left (data wiring,
states not designed). If the design itself has a problem (missing state, inconsistency), say so
or leave a comment on the element with `reply_comment`/a note rather than silently diverging.
