---
name: Personal
version: alpha
colors:
  primary: "#111827"
  secondary: "#4b5563"
  accent: "#2563eb"
  accent-hover: "#1d4ed8"
  surface: "#ffffff"
  muted: "#f3f4f6"
  border: "#e5e7eb"
  footer: "#9ca3af"
typography:
  h1:
    fontFamily: Inter
    fontSize: 3.5rem
    fontWeight: 700
    lineHeight: 1.1
  h2:
    fontFamily: Inter
    fontSize: 2.25rem
    fontWeight: 700
    lineHeight: 1.2
  body-lg:
    fontFamily: Inter
    fontSize: 1.25rem
    fontWeight: 400
    lineHeight: 1.6
  body:
    fontFamily: Inter
    fontSize: 1rem
    fontWeight: 400
    lineHeight: 1.6
  label:
    fontFamily: Inter
    fontSize: 0.875rem
    fontWeight: 500
    lineHeight: 1.4
  nav:
    fontFamily: Inter
    fontSize: 0.875rem
    fontWeight: 500
    lineHeight: 1
  tag:
    fontFamily: Inter
    fontSize: 0.75rem
    fontWeight: 500
    lineHeight: 1
rounded:
  sm: 6px
  md: 12px
  lg: 16px
spacing:
  xs: 8px
  sm: 16px
  md: 32px
  lg: 64px
  xl: 96px
components:
  button-primary:
    backgroundColor: "{colors.primary}"
    textColor: "{colors.surface}"
    rounded: "{rounded.md}"
    padding: "12px 24px"
  button-secondary:
    backgroundColor: "{colors.surface}"
    textColor: "{colors.secondary}"
    rounded: "{rounded.md}"
    padding: "12px 24px"
  card:
    backgroundColor: "{colors.surface}"
    rounded: "{rounded.lg}"
    padding: 20px
  tag-pill:
    backgroundColor: "{colors.muted}"
    textColor: "{colors.secondary}"
    rounded: "{rounded.sm}"
    padding: "4px 8px"
---

## Overview

Swiss minimalism meets personal warmth. A personal portfolio site that feels like a well-set hardcover book — generous whitespace, one font family, deep ink on white paper, with a single blue accent for interactions.

The design communicates competence through restraint. Nothing competes for attention. The hierarchy is carried by size and weight alone — one font (Inter), four weights, no decorations.

The reader should feel like they are browsing a printed object adapted for screen, not a website trying to be flashy.

## Colors

A monochrome foundation with one accent. The palette is 95% neutral — let content carry the personality.

- **Primary (#111827):** Near-black ink for headlines, body text, and primary buttons. Never pure black — warm enough to feel printed.
- **Secondary (#4b5563):** Sophisticated gray for body text, borders, and metadata. Enough contrast for readability without the harshness of pure black.
- **Accent (#2563eb):** The only color in the system. Used for links, interactive states. Its scarcity makes interactions unmistakable.
- **Accent hover (#1d4ed8):** Darker blue for hover state on links. Subtle enough not to startle.
- **Surface (#ffffff):** White page background. Clean, neutral, recedes completely.
- **Muted (#f3f4f6):** Near-white gray for tag backgrounds, skeleton states. Just enough tint to be visible.
- **Border (#e5e7eb):** Hairline gray for card borders, section dividers. Present but invisible.
- **Footer (#9ca3af):** Muted gray for copyright text and secondary metadata.

## Typography

One family, one axis of variation (weight). Inter was chosen for its excellent readability on screen at sizes from 12px to 60px and its neutral, professional character.

- **Headlines** sit at 3.5rem (56px) with `-1.68px` tracking. Large but restrained — a book title, not a billboard.
- **Section titles** at 2.25rem (36px). Clear hierarchy break without shouting.
- **Body text** at 1rem (16px) with 1.6 line height for comfortable reading at ~65 character measure.
- **Navigation and labels** at 0.875rem (14px) with tighter line height — compact enough for chrome, readable enough to navigate.
- **Tags** at 0.75rem (12px) — the smallest type in the system, for metadata that supports content.

## Layout & Spacing

Generous but not wasteful. The page has a clear vertical rhythm with large section gaps.

- Section padding: `py-20 md:py-28` — vertical breathing room scales with viewport.
- Max content width: `max-w-5xl` (1024px) — wide enough for a portfolio grid, narrow enough to never feel like running text.
- Card gaps: `gap-6` (24px) — tight enough to feel like a grid, loose enough for each card to be a distinct object.
- Nav height: `h-16` (64px) — compact chrome that stays out of the way.

## Shapes

Everything is softly rounded. Not playful — just not sharp.

- Buttons, inputs: `rounded-lg` (12px) — visibly rounded, signals interactivity.
- Cards: `rounded-xl` (16px) — slightly rounder, objects meant to be looked at.
- Tags: `rounded-md` (6px) — almost square, a label that doesn't want attention.

## Components

### button-primary

Dark pill. Used for hero CTA. Solid near-black background with white text. Hover lightens to `gray-800`. The visual weight of a filled button signals "this is the primary action."

### button-secondary

Outline pill. Used for secondary actions. White background, gray border and text. Hover darkens the border. Pairs with button-primary to offer choice without competing.

### card

Rectangle with slight shadow on hover. The card itself is a white rounded box with a 1px hairline border. On hover the border darkens and a subtle shadow appears — enough to feel responsive, not enough to feel like a popup.

### tag-pill

Small gray pill for technology labels on portfolio cards. Muted background, secondary text. Information that supports the card content without competing with it.

## Do's and Don'ts

- **Don't** use more than one accent color. Blue is the entire color vocabulary for interaction.
- **Don't** add gradients, glass effects, or drop shadows as decoration. Shadows appear only on interactive hover.
- **Don't** use more than 4 font weights (400, 500, 600, 700).
- **Don't** center-align body text longer than 2 lines. Left-aligned text is more readable.
- **Don't** add animations longer than 200ms. State changes should feel instant.
- **Do** let whitespace do the work. A section with 128px padding is not empty — it is breathing.
- **Do** use size contrast, not color contrast. The difference between h1 and body is size, not hue.
- **Do** keep cards at consistent heights within a row. Use fixed image placeholders (192px) even without real screenshots.
- **Do** respect `prefers-reduced-motion`. All transitions collapse to 0ms when requested.
