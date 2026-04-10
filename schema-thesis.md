# The Schema as Continuity Artifact

## The core problem

Today's product development breaks at every handoff:
- Concepts live in workshops and sticky notes
- Decisions live in Confluence
- Designs live in Figma
- Logic lives in developers' heads

Each translation loses fidelity. By the time something ships, the original intent is barely traceable.

## The insight

A schema-driven approach makes the design intent the continuity artifact — something that starts in the earliest strategic conversation and accumulates detail all the way to production code without losing fidelity.

## The stack (top to bottom)

```
Content schema      — what exists in the domain (channel-agnostic)
Recipe              — how content intent maps to expression + business rules (channel-agnostic)
─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ channel boundary ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─
Template            — structural arrangement of slots (channel-specific)
Component + patterns — how slots are rendered (channel-specific)
Tokens              — how components look (channel-specific)
```

The order matters. Most teams build this upside down — starting with tokens and components, then figuring out content last.

The channel boundary is the key architectural line. Everything above it is defined once and reused across all channels. Everything below it is a channel-specific serialization of the same intent.

## Two kinds of recipes

**Schema recipe** — design artifact. Human- and machine-readable. Encodes intent, mapping rules, business logic, conditions. Owned by design/content. Lives in the schema. Channel-agnostic.

**Technical recipe** — engineering artifact. A transformer, mapper, or adapter in the backend. Parses data, combines sources, normalizes fields. Owned by engineering. Lives in code. Channel-specific.

They are related but separate concerns. The schema recipe says *what* should happen and *why*. The technical recipe says *how* to make the data ready for it.

The schema recipe might say:
> map `product.monthly_price` + `product.currency` → `card__metric`

The technical recipe handles:
> fetch price from pricing API, fetch currency from user locale service, combine, format, handle nulls.

The schema recipe is the spec. The technical recipe is the implementation of the data pipeline that fulfills it. This distinction matters organizationally — they have different owners, different audiences, and different lifecycles. Conflating them causes designers and engineers to talk past each other.

When we say "recipe" in this framework we mean the schema recipe only.

---

## The recipe is the design act

Adding a component to the schema is a technical act.
Adding a recipe is a design act.

The recipe encodes:
- Which template to use
- How content fields map to component slots
- Business rules, conditions, exceptions
- Personalization logic

This is what currently lives scattered across Confluence, frontend conditionals, and undocumented designer assumptions. The recipe makes it a first-class design artifact with a defined owner.

## Channel-agnostic by design

The schema has no channel. `InsuranceProduct` + `compare` intent is the same recipe regardless of where it surfaces:

- Auth web → three-column comparison grid
- Mobile → single column, max 2 products
- Chat → text summary of top 3 differences  
- Voice → spoken comparison of key attributes

Content is the same. Business rules are the same. Intent is the same. Only the expression changes. Channel teams become implementers of shared intent rather than independent interpreters of a degraded brief.

## The designer's new role

A design system is a language:
- Tokens are adjectives (how things look)
- Components are nouns (what things are)
- Relations are grammar (how they connect)
- Recipes are verbs (what intent drives expression)

The designer who defines the recipe is doing strategic work — encoding what a business entity means and how it should be expressed across every context. That is fundamentally different from "person who makes the Figma file look right."

This is also the answer to the craft crisis: designers who define schemas in the strategic room are irreplaceable. Designers who refine Figma files after the brief is written are not.

## The prototyping machine

The schema enables a prototyping machine — a Claude Code setup that generates working prototypes from plain language descriptions, constrained by the schema.

- Designer describes a screen
- Machine resolves entity + intent → recipe → template → components
- Output: self-contained HTML file, on-schema, no code required from designer

This is not Figma with AI. This is the schema producing real outputs that already encode the business logic. A developer looking at the prototype can read the recipe and understand the intent, not just the appearance.

## The path

**Pilot 1 — How designers work better**
Replace Figma + Confluence with schema-driven prototypes. Bounded, low-risk, no organizational change required from anyone else. One team, one flow, 4–6 weeks.

**Pilot 2 — Strategic concepting**
Run a concepting session that produces a schema instead of workshop outputs. Prove the thread: concepting session → schema → prototype → content structure → production. Gets the right people in the room — product, business, service design.

The second pilot is only credible after the first. Let the demonstration make the organizational argument, not the pitch.

## The long game

When the schema starts at conception and accumulates detail as work progresses:
- No redesign when a new channel is added — it's a new serializer for existing recipes
- No lost context at handoffs — the schema carries the intent
- No scattered business logic — the recipe is the single source of truth
- No lorem ipsum problem — content structure is defined before components, not after

This is not a design tools problem. It is an organizational change problem with a design solution. The entry point is narrow (designers work better). The destination is a fundamentally different operating model for how organizations build products.
