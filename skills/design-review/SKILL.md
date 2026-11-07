---
name: design-review
description: Review screens designed in saymock for UX, visual consistency and accessibility, and report the findings in the conversation or as a note on the board. Use when the user asks to review, critique, audit or check a saymock design, screen or flow, or asks whether a design is ready.
---

# Design review

Goal: a short list of concrete, prioritized problems, each tied to an element and a fix, not a
generic checklist.

## 1. Look

- `get_file` for the frames; `get_screenshot` of each screen (scale 2, and zoom with `nodeId` on
  dense areas). Review a flow screen by screen, in order.
- `check_layout` on each frame: misalignments, clipped text, unknown icons, empty elements are
  measured there; include its errors.
- `get_html` (or `find_nodes`) to check what the screenshot cannot show: real text vs. images of
  text, `alt` on images, form labels, heading order, button vs. link.

## 2. Check

- **Hierarchy and clarity**: one primary action per screen, clear titles, scannable content,
  consistent placement of navigation and actions across screens.
- **Consistency**: same component for the same thing (buttons, inputs, cards), spacing on one
  scale, a limited type scale, colors from one palette. Copies of a block that should be one
  component (`make_component`), rows that should be one template.
- **Accessibility**: text contrast (WCAG AA: 4.5:1 body, 3:1 large text and UI parts; compute it
  from the hex values in the classes), touch targets ≥ 44×44 pt on mobile, text size ≥ 12 px,
  states not conveyed by color alone, focus/disabled/error states designed, labels on inputs.
- **States and edge cases**: empty, loading, error, long text, many items, first use.
- **Platform fit**: safe areas and status bar on phones, tab bar vs. sidebar, responsive frames
  (`md:`/`lg:` variants) when the screen targets several sizes.

## 3. Report

Rank the findings: blocking, important, polish. For each: the screen, the element (its id from
`find_nodes`), the problem, the fix.

Leave them where the team works when the user wants it: an `add_note` next to the screen with the
summary. Fix directly only what the user asked you to fix; otherwise propose.
