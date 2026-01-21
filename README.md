# Design Token Context Ontology

A comprehensive specification for multi-dimensional design token resolution.

## The Problem

Design tokens have become foundational infrastructure for scalable design systems. The industry has converged on a tiered abstraction model—primitives, options, decisions, components, instances—but **context-awareness remains fragmented**.

Current systems treat context as flat modes (`light`/`dark`, `desktop`/`mobile`) rather than as a **multidimensional coordinate system** with inheritance, intersection, and resolution rules. When a button needs to resolve its background color, the answer depends on:

- Color scheme preference (light, dark, high-contrast)
- Density mode (compact, comfortable, spacious)
- Interaction state (rest, hover, active, disabled)
- Accessibility requirements (forced colors, reduced motion)
- Platform capabilities (color gamut, touch input)
- Locale considerations (RTL, text expansion)
- Container context (available space, scroll position)
- And dozens more dimensions...

**This project provides the missing specification.**

## What's Included

### [Whitepaper](design-token-context-ontology-whitepaper.md)

The complete specification defining:

| Aspect | Description |
|--------|-------------|
| **12 Context Categories** | Appearance, Density, Viewport, Container, Interaction States, Combinable States, Accessibility, Locale, Platform, Elevation, Semantic, Structural |
| **93 Dimensions** | Complete enumeration with values, volatility class, and detection mechanism |
| **5 Volatility Classes** | Immutable → Stable → Semi-stable → Volatile → Inherited |
| **78 Cross-Category Dependencies** | Documented relationships requiring coordination |
| **52 Conflict Scenarios** | Resolution rules for dimension intersections |
| **Named Context Types** | Governed shortcuts for common patterns (mobile-dark, kiosk, print) |
| **Resolution Algorithm** | Explicit precedence ordering for token resolution |

### [Token Repository](tokens/)

W3C Design Tokens format implementation with 19 JSON files:

```
tokens/
├── context/          # 93 context dimensions (8 files)
├── preference/       # User-controllable settings (3 files)
├── semantic/         # Design intent tokens (6 files)
├── state/            # Interaction & combinable states (2 files)
├── component/        # Example component tokens
└── manifest.json     # GitFig integration mappings
```

## Architecture

The ontology defines a **layered token architecture** with unidirectional dependencies:

```
┌─────────────────────────────────────────────────────────────┐
│  Context Layer        viewport, platform, locale            │
│                       Detected automatically, read-only     │
├─────────────────────────────────────────────────────────────┤
│  Preference Layer     prefers-*, density, accessibility     │
│                       User-controllable, system-detectable  │
├─────────────────────────────────────────────────────────────┤
│  Semantic Layer       status, emphasis, intent              │
│                       Author-defined, context-independent   │
├─────────────────────────────────────────────────────────────┤
│  State Layer          interaction, combinable states        │
│                       Runtime-dynamic, composable           │
├─────────────────────────────────────────────────────────────┤
│  Component Layer      {component}-{property}-{state}        │
│                       References semantic layer only        │
└─────────────────────────────────────────────────────────────┘
        ↑ Dependencies flow upward only
```

## Design Philosophy

This ontology supports a **Single Source of Truth (SSOT) architecture** with controlled contextual variation:

1. **Explicit chains** — Every resolved token value traces back through a documented path
2. **Governed types** — Context types are finite and governed, not arbitrary
3. **Learnable friction** — Exceptions are logged and promoted to tokens when patterns emerge
4. **Predictable blast radius** — Changes at any tier have bounded, knowable effects

## Quick Start

### Use the Tokens

```javascript
// Import semantic tokens
import colors from './tokens/semantic/color.json';
import status from './tokens/semantic/status.json';

// Access with dark mode variant
const primary = colors.accent.default.$value; // #0d6efd
const primaryDark = colors.accent.default.$extensions.mode.dark; // #6ea8fe
```

### With Style Dictionary

```json
{
  "source": ["tokens/**/*.json"],
  "platforms": {
    "css": {
      "transformGroup": "css",
      "buildPath": "dist/",
      "files": [{ "destination": "tokens.css", "format": "css/variables" }]
    }
  }
}
```

### With GitFig (Figma)

The tokens are compatible with [GitFig](https://github.com/nicholasareed/gitfig) for syncing to Figma Variables. See `tokens/manifest.json` for suggested file mappings.

## Context Categories

| Category | Dimensions | Key Concepts |
|----------|------------|--------------|
| **Appearance** | 11 | Color scheme, contrast, forced colors, dynamic color, gamut |
| **Density** | 9 | Density mode, touch optimization, scale factor, font scaling |
| **Viewport** | 11 | Breakpoints, orientation, foldables, safe areas, virtual keyboard |
| **Container** | 7 | Container queries, aspect ratio, scroll state |
| **Interaction** | 9 | Rest, hover, active, disabled, pending, dragged (mutually exclusive) |
| **Combinable** | 18 | Focus, selected, checked, expanded, invalid (can combine) |
| **Accessibility** | 8 | Reduced motion, transparency, CVD modes, focus visibility |
| **Locale** | 10 | Direction, writing mode, script type, text expansion |
| **Platform** | 10 | Platform, framework, rendering context, performance tier |
| **Elevation** | 8 | Surface levels, shadows, z-index, backdrop blur |
| **Semantic** | 8 | Status, emphasis, intent, urgency, provenance |
| **Structural** | 10 | Region, nesting depth, list position, component lifecycle |

## Resolution Precedence

When dimensions conflict, resolution follows this order (highest to lowest):

1. **Accessibility overrides** — `forced-colors`, `prefers-contrast`, minimum targets
2. **Explicit declarations** — Dimensions set directly on element
3. **Context type** — Dimensions from applied named context type
4. **Inherited context** — Dimensions from ancestor chain
5. **Platform defaults** — Platform-specific default values
6. **System defaults** — Ontology-defined defaults

## License

MIT

## Contributing

Issues and pull requests welcome. See the whitepaper for the complete specification that implementations should follow.
