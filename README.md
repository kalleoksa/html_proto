# HTML Proto

A schema-driven prototyping setup. Generates self-contained HTML prototypes from plain-language descriptions, constrained by a defined design system. No frameworks, no build step, no external dependencies.

## Concept

Most prototyping starts at the wrong layer — tokens and components first, content last. This project inverts that order:

```
Content schema      — what exists in the domain (channel-agnostic)
Recipe              — how content maps to expression + business rules (channel-agnostic)
─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ channel boundary ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─
Template            — structural arrangement of slots (channel-specific)
Components          — how slots are rendered (channel-specific)
Tokens              — how components look (channel-specific)
```

Everything above the channel boundary is defined once and reusable across surfaces. Everything below is a serialisation of the same intent for a specific channel. See `schema-thesis.md` for the full argument.

## Files

| File | Purpose |
|---|---|
| `CLAUDE.md` | System instructions for Claude — defines rules, tokens, components, patterns, templates, and recipes. The single source of truth for all prototype generation. |
| `tokens.css` | Design tokens in three layers: primitive scale → semantic purpose → component scope. Change primitives to retheme; semantic and component tokens update automatically. |
| `product-schema.json` | Content schema for the `Product` entity. Defines all domain fields — name, price, variants, images, specs, features. Channel-agnostic. |
| `product-recipe.json` | Recipe for `Product / view` intent. Maps content fields to semantic slots (`identity`, `price_display`, `variant_selection`, `primary_action`, etc.) and encodes business rules in plain conditionals. No rendering assumptions. |
| `product-components.json` | Component catalogue for the product page. Each entry documents slots, token references, variants, and accessibility requirements. |
| `product-page.html` | Self-contained prototype — the product page rendered from the recipe. Vanilla HTML + CSS custom properties + vanilla JS. No external dependencies. |
| `schema-thesis.md` | Strategic document. Explains why the schema is the continuity artifact and how this approach addresses the handoff problem. |

## Token architecture

Tokens are defined in `tokens.css` and linked from prototypes. Three layers:

```
Primitive tokens    --primitive-gray-900: #1a1a1a
       ↓
Semantic tokens     --color-text-primary: var(--primitive-gray-900)
       ↓
Component tokens    --btn-font-size: var(--font-size-body)
```

**Rule:** components reference semantic tokens only — never primitives directly. Primitives are scale definitions, not design decisions.

### Spacing tokens

| Token | Value | Use |
|---|---|---|
| `--space-gap-xs` | 4px | Tight inline gap |
| `--space-gap-sm` | 8px | Default component gap |
| `--space-gap-md` | 12px | Relaxed component gap |
| `--space-gap-lg` | 20px | Loose component gap |
| `--space-layout-gap` | 16px | Layout-level gap |
| `--space-layout-gap-lg` | 24px | Large layout gap |
| `--space-section-gap` | 40px | Between sections |
| `--space-inset-xl` | 32px | Page-level inset |
| `--space-inset-2xl` | 48px | Section inset |
| `--space-inset-3xl` | 64px | Page bottom padding |

## Prototype rules

Enforced by `CLAUDE.md`:

- Single HTML file (unless multiple files are explicitly requested)
- CSS custom properties for all design tokens — no hardcoded values
- Vanilla JS only — no frameworks, no CDN dependencies
- State management via `state` object + `render()` function pattern
- Targeted `innerHTML` updates for partial UI changes — no `outerHTML` swaps, no `bindEvents()` calls inside event handlers
- Accessibility defaults applied unconditionally (focus styles, ARIA, label associations)
- Image placeholders via `placehold.co` — no inline SVG

## Generating a new prototype

1. Define or locate the content entity in a schema file
2. Write a recipe — content slots + business rules, no rendering assumptions
3. Describe the screen to Claude in plain language
4. Claude resolves: entity → recipe → template → components → HTML

If a component or token needed for the prototype is not defined in `CLAUDE.md`, Claude flags it and asks before generating. Nothing is invented outside the schema.

## Stack

- HTML5
- CSS custom properties
- Vanilla JS (ES2020)
- [placehold.co](https://placehold.co) for image placeholders
