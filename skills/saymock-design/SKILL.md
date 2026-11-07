---
name: saymock-design
description: Design new screens or flows in saymock that are good and easy to edit — structure, components, templates, naming and the verify loop. Use when the user asks to design, mock up, sketch, wireframe or prototype screens, an app, a page or a flow in saymock (not to reproduce an existing screen from code: design-from-code).
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
- Prototype links with `data-goto` between screens when the user wants to click through.
- One `add_note` per flow explaining the choices and open questions.

## Iterating with the team

People comment on elements and address requests to you: the `inbox` prompt (or `list_tasks`)
gives them with the element they point at. Big explorations go into a branch (`create_branch`)
the user can review and merge; `save_version` before a large change.

## Handing off

When the design is approved, the `implement-design` skill (with `export_code`) turns it into code
in the project.
