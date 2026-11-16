---
name: saymock-design
description: "Design new screens or flows in saymock that are good and easy to edit — structure, components, templates, naming and the verify loop. Use when the user asks to design, mock up, sketch, wireframe or prototype screens, an app, a page or a flow in saymock (not to reproduce an existing screen from code: design-from-code)."
---

# Designing in saymock

The MCP server's instructions describe the HTML conventions and tools; this skill is about
working method. Humans may be watching the board live: work screen by screen, not all at once.

## Before drawing

- Clarify the product, the audience, the platform (phone, tablet, desktop) and the screens of the
  flow. If the user has a codebase or brand, take its colors, fonts and logo
  (`get_upload_url` + curl for files; `list_assets` for the workspace's fonts and images).
- Decide the system once: a palette (one accent, neutrals, semantic colors), a type scale, a
  spacing scale, a radius. Use the same values on every screen.

## Build the first screen as the template of the others

- Shared blocks first, as components: status bar, header, tab bar, sidebar
  (`data-component` + `data-props`); later screens use them with `data-use`.
- Every list, grid, menu or table is a `<template data-each>` with realistic data (real names,
  amounts, dates; never "Item 1, Item 2" or lorem ipsum).
- `data-name` on the main blocks, so people find them in the layers.
- `check_layout` then `get_screenshot` before moving on: fix the first screen until it is right,
  the others inherit its components.

## Then the flow

- Place screens side by side in flow order; the states that matter (empty, error, loading,
  selected) as extra frames next to their screen.
- Prototype links with `data-goto` between screens when the user wants to click through: a
  sheet, dialog or drawer as an overlay (`data-overlay="bottom"`, a frame only as big as the
  panel), `data-transition="smart"` between two states of a screen whose elements share their
  `data-name`, `data-trigger="delay"` for a splash or a loader, swipes for carousels.
- Make it alive where it explains the design, at the cost of a few classes (the server's "Live
  interactions"): a list that really scrolls (enough template rows, `overflow-y-auto`), a
  selector or tabs as radios with `peer-checked:`, an accordion as `<details>`, press and hover
  effects on buttons. Tell the user to try it in the player or with the board's Interact tool (P).
  A state of the screen CSS cannot show (a like, a menu that opens): `data-toggle` and
  `data-on:` classes; data that changes gets its own frame and a `data-goto`.
- One `add_note` per flow explaining the choices and open questions, with `nearId` (the screen
  it is about) so it sits beside that screen; also notes for what a screen cannot show to whoever
  builds it (gestures, rules, edge cases): `get_specs` hands them to the implementer.
- Nothing on the board covers anything else: leave x/y out unless you lay out a grid yourself
  (a covering spot is moved anyway), and fix a `board-overlap` from `check_layout` (a long page
  that grew over a note) with the `update_nodes` it gives. Once a flow is laid out, or when the
  board is messy, `arrange_board` tidies it (rows aligned, even spacing, notes beside their screen).

## Iterating with the team

People comment on elements and address requests to you: the `inbox` prompt (or `list_tasks`)
gives them with the element they point at. Asked for alternatives ("propose 3 reworks", "a few
ideas", "des pistes"), call `propose`: one copy of the screen(s) per direction, kept on the screen
where people preview them in place and choose; edit each copy by its frameId, `describe_changes`
on each, never new frames on the board and never branches for this. A request to carry a chosen
option over to other screens comes with the exact `propose … report` call. One big change the
user reviews and merges goes into a branch (`create_branch`); `save_version` before a large change.

## Handing off

When the design is approved, the `implement-design` skill turns it into code in the project:
`export_code` for the web, `get_specs` for native apps (iOS, Android, Flutter…) or any stack.
