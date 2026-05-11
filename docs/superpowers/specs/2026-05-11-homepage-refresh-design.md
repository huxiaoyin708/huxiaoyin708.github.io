# Homepage Refresh Design

## Objective

Refresh Xiaoyin Hu's personal homepage without replacing the current Jekyll site structure. The new homepage should feel more polished, modern, and academic while keeping the existing `Publications` and `Experience` pages intact.

## Chosen Direction

Use the `Profile-first + Clean Blue` design:

- Keep the current Jekyll/Academic Pages foundation.
- Redesign only the homepage and the minimum shared styling needed for it.
- Use a clean blue visual system: light blue background, white content surfaces, blue accents, and restrained dark text.
- Make the first screen read as a professional academic profile rather than a template page.

## Homepage Structure

The homepage should contain these sections in order:

1. Hero profile section
   - Large name: `Xiaoyin Hu`
   - Title line: Associate Professor, School of Mathematical Sciences, Shenzhen University
   - Current portrait from `images/photo.jpg`
   - Short research summary
   - Quick links for Publications, Google Scholar, and Email
   - Research keyword chips for nonsmooth optimization, Riemannian optimization, and machine learning

2. Selected publications section
   - Show three representative or recent papers from the existing publications content.
   - Each item should include title and venue/year.
   - Include a `View all` link to `/publications/`.

3. Highlight cards
   - Current grant card with the NSFC Youth Fund item.
   - Current position card with Shenzhen University and date range.

## Visual Style

- Overall tone: clean, modern, credible, and academic.
- Palette: very light blue page background, white cards, medium blue primary actions, navy text, muted slate secondary text.
- Typography: system sans-serif stack for a crisp modern look.
- Components: rounded cards with subtle borders and shadows, pill-shaped research tags, compact action buttons.
- Avoid decorative gradients or excessive visual effects; the page should stay professional and readable.

## Scope

In scope:

- Add a homepage-specific layout or class structure.
- Update `_pages/about.md` so the homepage uses the new structure.
- Add focused SCSS for the new homepage presentation.
- Keep existing navigation labels and URLs.
- Preserve the current `photo.jpg`, author metadata, publications page, and experience page.

Out of scope:

- Migrating to a different Jekyll theme.
- Reworking the publications page design.
- Adding new pages or interactive features.
- Changing deployment configuration.

## Implementation Notes

- Prefer a small dedicated homepage layout if that keeps the markup clearer than forcing everything through the existing `single` layout.
- The homepage should be responsive: on mobile, stack portrait, intro, publications, and highlight cards vertically.
- Do not depend on external UI libraries.
- Keep the page compatible with GitHub Pages and the existing Jekyll build setup.

## Verification

Before completion, verify:

- The homepage renders without missing includes or layouts.
- `_config.yml` and `_data/navigation.yml` still parse as YAML.
- The homepage has no obvious broken local asset references.
- The layout works at desktop and mobile widths.
- If a local Jekyll build cannot run because of the local Ruby version, report that clearly and include the checks that did run.
