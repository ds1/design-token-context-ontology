# Design Token Context Ontology - Token Repository

This directory contains the comprehensive design token implementation of the Design Token Context Ontology specification.

## Directory Structure

```
tokens/
├── _meta/
│   └── ontology.json          # Ontology metadata, volatility classes, detection mechanisms
│
├── context/                   # Context Layer (Environment) - Read-only, detected
│   ├── appearance.json        # Color scheme, contrast, forced colors, dynamic color
│   ├── density.json           # Density mode, touch optimization, scale factor
│   ├── viewport.json          # Breakpoints, orientation, foldable states
│   ├── container.json         # Container queries, scroll state
│   ├── accessibility.json     # Motion, transparency, CVD modes
│   ├── locale.json            # Direction, writing mode, script type
│   ├── platform.json          # Platform, framework, rendering context
│   └── types.json             # Named context types (dimension intersections)
│
├── preference/                # Preference Layer (User Settings) - User-controllable
│   ├── appearance.json        # User color scheme, theme preferences
│   ├── density.json           # User density mode, spacing multipliers
│   └── accessibility.json     # User accessibility preferences
│
├── semantic/                  # Semantic Layer (Design Intent) - Author-defined
│   ├── color.json             # Surface, foreground, border, accent colors
│   ├── typography.json        # Font family, size, weight, line height
│   ├── spacing.json           # Spacing scale, insets, gaps
│   ├── elevation.json         # Surface levels, shadows, z-index
│   ├── status.json            # Success, warning, error, info tokens
│   └── emphasis.json          # Primary, secondary, tertiary emphasis
│
├── state/                     # State Layer (Interaction) - Runtime-dynamic
│   ├── interaction.json       # Mutually exclusive: rest, hover, active, disabled
│   └── combinable.json        # Additive: focus, selected, checked, expanded
│
└── component/                 # Component Layer (Implementation)
    └── button.json            # Example: Button component tokens
```

## Token Format

All tokens follow the **W3C Design Tokens Community Group** format:

```json
{
  "token-name": {
    "$value": "#0d6efd",
    "$type": "color",
    "$description": "Primary accent color",
    "$extensions": {
      "mode": {
        "dark": "#6ea8fe"
      },
      "ontology.volatility": "stable",
      "ontology.detection": "inferred"
    }
  }
}
```

## Layer Architecture

Dependencies flow **upward** through the layers:

```
Component → Semantic → Preference → Context
```

- **Component tokens** reference only semantic tokens
- **Semantic tokens** reference preference tokens
- **Preference tokens** respect context tokens
- **Context tokens** are detected/declared, read-only

## GitFig Integration

These tokens are compatible with the GitFig Figma plugin:

1. **File Mappings**: Each JSON file maps to a Figma Variables collection
2. **Mode Support**: Dark mode values in `$extensions.mode.dark`
3. **Format**: W3C format auto-detected by GitFig

### Suggested File Mappings

| File | Target Type | Collection Name |
|------|-------------|-----------------|
| `semantic/color.json` | variables | Colors |
| `semantic/typography.json` | variables | Typography |
| `semantic/spacing.json` | variables | Spacing |
| `semantic/elevation.json` | variables | Elevation |
| `semantic/status.json` | variables | Status |
| `component/button.json` | variables | Button |

## Ontology Extensions

Custom `$extensions` properties document ontology metadata:

| Extension | Description |
|-----------|-------------|
| `ontology.volatility` | immutable, stable, semi-stable, volatile, inherited |
| `ontology.detection` | inferred, declared, computed |
| `ontology.css-query` | CSS media query for detection |
| `ontology.aria` | ARIA attribute mapping |
| `ontology.layer` | Token layer (context, preference, semantic, state, component) |
| `ontology.combinable` | Whether state can combine with others |
| `ontology.exclusive-group` | Mutual exclusion group |

## Usage

### Direct Import

```javascript
import colorTokens from './tokens/semantic/color.json';
import statusTokens from './tokens/semantic/status.json';
```

### With Style Dictionary

```javascript
// config.json
{
  "source": ["tokens/**/*.json"],
  "platforms": {
    "css": {
      "transformGroup": "css",
      "buildPath": "dist/",
      "files": [{
        "destination": "tokens.css",
        "format": "css/variables"
      }]
    }
  }
}
```

### Context Resolution

```javascript
function resolveToken(tokenPath, contextCoordinate) {
  // 1. Collect context from ancestor chain
  // 2. Apply context type if active
  // 3. Check cross-category dependencies
  // 4. Resolve conflicts per precedence
  // 5. Compute value
  // 6. Transform for platform
}
```

## Version

This token repository implements **Design Token Context Ontology v1.0** (January 2026).

See the [whitepaper](../design-token-context-ontology-whitepaper.md) for complete specification.
