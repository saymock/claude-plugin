---
name: design-from-code
description: Recreate an app's existing screens in saymock from its codebase (and the running app when available), as editable designs that use the project's real colors, fonts, icons and components. Use when the user asks to put the current app/screens/flow into saymock, to "design from the code", to make a design file of what is built, or to start a redesign from the existing UI.
---

# Codebase → saymock

Goal: saymock frames that look like the app does today and are built like a design system:
shared blocks as components, lists as templates, the project's tokens as Tailwind values. The
conventions of saymock HTML are in the MCP server's instructions: follow them.

## 1. Scope

Agree on the screens: a route, a flow (sign-up → onboarding → home), or the main screens. List
them with their route/file and the device (phone, tablet, desktop). Prefer a small, complete flow
over many half-done screens.

## 2. Collect the design system from the code

- **Colors, radii, shadows, spacing**: Tailwind theme (`@theme` in CSS, or `tailwind.config.*`),
  CSS variables, token files. Note hex values: saymock content uses Tailwind utilities with
  arbitrary values (`bg-[#0f172a]`) or the default palette, not the project's custom class names.
  Resolve `bg-primary` → its color for the design.
- **Fonts**: families and weights (`font-['Geist']`; upload a non-Google font with
  `get_upload_url` + curl, `list_assets` shows the workspace's fonts).
- **Icons**: the icon library (Lucide maps directly: `<i data-lucide="…">`; Material Symbols too;
  others: find the closest with `search_icons`, all names in one call).
- **Images and logos**: upload the real files (`get_upload_url` + curl), never redraw a logo.
- **Shared blocks**: layout shell, header, sidebar, tab bar, cards → saymock components
  (`data-component` once, `data-use` elsewhere), with props for what varies.

## 3. Reference screenshots (when the app runs)

If the app can be started (dev server, Storybook, a deployed URL), screenshot each screen at the
target size (e.g. Playwright, `npx playwright screenshot --viewport-size=393,852 <url> out.png`)
with realistic content. Upload them with `get_upload_url` + curl. They make the result measurable
with `compare_to_reference`. Without a running app, read the components' markup and styles
closely: they are the reference.

## 4. Build

- `create_file` (in the project the user names), one frame per screen with the right device,
  related screens side by side.
- Translate the component tree into HTML + Tailwind: same structure (flex/grid), same classes when
  they are Tailwind already, real copy from the code or i18n catalogs (not lorem ipsum), sample
  data that looks like production data in templates.
- Name the main blocks with `data-name` after the code components (`data-name="OrderSummary"`),
  so people and later `implement-design` runs can map them back.

## 5. Check

`check_layout` until no errors, `get_screenshot` to look at each screen, and
`compare_to_reference` against the app screenshots when you have them: fix the elements it names,
compare again. Finish with an `add_note` next to the screens saying where they come from
(routes, commit) and what was approximated.
