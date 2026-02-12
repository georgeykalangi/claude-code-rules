---
description: Accessibility (a11y) standards for frontend and UI work
globs:
  - "**/*.tsx"
  - "**/*.jsx"
  - "**/*.vue"
  - "**/*.svelte"
  - "**/*.html"
  - "**/components/**/*"
  - "**/pages/**/*"
  - "**/views/**/*"
---

# Accessibility Standards

## Core Principles (WCAG 2.1)
- **Perceivable**: Content must be presentable in ways all users can perceive.
- **Operable**: UI must be navigable by keyboard, screen reader, and assistive tech.
- **Understandable**: Content and UI behavior must be predictable and readable.
- **Robust**: Content must work across assistive technologies and browsers.

## Semantic HTML
- Use correct HTML elements: `<button>` for actions, `<a>` for navigation, `<nav>`, `<main>`, `<article>`.
- Never use `<div>` or `<span>` for interactive elements. Use native elements first.
- Use heading hierarchy (`h1` > `h2` > `h3`). Don't skip levels.
- Use `<label>` elements explicitly associated with form inputs (htmlFor/for).

## Keyboard Navigation
- All interactive elements must be keyboard accessible (Tab, Enter, Escape, Arrow keys).
- Visible focus indicators on all focusable elements. Never `outline: none` without replacement.
- Logical tab order that matches visual layout.
- Trap focus in modals/dialogs. Return focus when closed.
- Provide skip links for repetitive navigation.

## Images & Media
- All `<img>` elements must have `alt` text. Decorative images: `alt=""`.
- Alt text should describe the image's purpose, not just its content.
- Videos should have captions. Audio should have transcripts.
- Don't rely on color alone to convey information.

## ARIA (Use Sparingly)
- Use ARIA only when native HTML semantics aren't sufficient.
- Common patterns: `aria-label`, `aria-describedby`, `aria-expanded`, `aria-hidden`, `role`.
- Never use `aria-hidden="true"` on focusable elements.
- Test ARIA with actual screen readers — incorrect ARIA is worse than no ARIA.

## Forms
- Every input needs a visible label. Placeholder text is not a label.
- Group related fields with `<fieldset>` and `<legend>`.
- Show error messages inline near the relevant field.
- Use `aria-invalid` and `aria-describedby` for error states.
- Support autocomplete attributes for common fields (name, email, address).

## Color & Contrast
- Minimum contrast ratio: 4.5:1 for normal text, 3:1 for large text (WCAG AA).
- Don't use color as the only indicator (add icons, text, or patterns).
- Test with color blindness simulators.
- Ensure UI is usable in high-contrast mode.

## Testing
- Test with keyboard only (no mouse).
- Test with a screen reader (VoiceOver on Mac, NVDA on Windows).
- Use automated tools: axe, Lighthouse accessibility audit, eslint-plugin-jsx-a11y.
- Check with browser devtools accessibility inspector.
