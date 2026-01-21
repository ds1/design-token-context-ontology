# The complete context ontology for design tokens

Design tokens resolve to different values based on **context**—the environmental conditions and user states that affect visual presentation. This ontology catalogs every context dimension affecting token resolution, classifies them by volatility, defines inheritance rules, and documents real-world implementation patterns from nine major design systems and the W3C Design Tokens Community Group specification (v2025.10).

The fundamental insight from this research: context is not a single dimension but a **multi-dimensional matrix** where appearance, density, viewport, interaction, accessibility, locale, platform, brand, and state all intersect. Effective design token systems must handle **12+ context categories**, manage **volatility hierarchies** from immutable to interaction-level changes, and resolve **conflicts** when contexts collide.

-----

## 1. Complete enumeration of context dimensions

### 1.1 Appearance and theme contexts

|Dimension           |Values                              |Detection Method      |Volatility |
|--------------------|------------------------------------|----------------------|-----------|
|Color mode          |`light`, `dark`, `auto`             |`prefers-color-scheme`|Stable     |
|High contrast       |`default`, `more`, `less`           |`prefers-contrast`    |Stable     |
|Forced colors       |`active`, `none`                    |`forced-colors`       |Immutable  |
|Inverted colors     |`inverted`, `none`                  |`inverted-colors`     |Stable     |
|Brand theme         |Brand-specific palettes             |Declared              |Semi-stable|
|Color scheme variant|`dimmed`, `colorblind`, `tritanopia`|Declared              |Stable     |

GitHub Primer implements **9 distinct themes** across light and dark modes, including accessibility variants like `light_high_contrast`, `dark_colorblind`, and `dark_tritanopia`. This represents current industry best practice for accessibility-first theming.

### 1.2 Density and spacing contexts

|Dimension         |Values                              |Detection Method      |Volatility |
|------------------|------------------------------------|----------------------|-----------|
|Density mode      |`compact`, `comfortable`, `spacious`|Declared/preference   |Stable     |
|Touch optimization|`touch`, `pointer`                  |`pointer: coarse/fine`|Stable     |
|Content density   |`empty`, `sparse`, `normal`, `dense`|Programmatic          |Volatile   |
|Scale factor      |`medium` (desktop), `large` (mobile)|Declared              |Semi-stable|

Adobe Spectrum’s scale system (`medium` vs `large`) adjusts touch targets and spacing for desktop versus mobile contexts.  Material Design 3’s density system uses a **4dp base grid** with compact, medium, and expanded variants. 

### 1.3 Viewport and responsive contexts

|Dimension    |Values                             |Detection Method      |Volatility |
|-------------|-----------------------------------|----------------------|-----------|
|Breakpoint   |`xs`, `sm`, `md`, `lg`, `xl`, `xxl`|`@media (width)`      |Semi-stable|
|Orientation  |`portrait`, `landscape`            |`@media (orientation)`|Semi-stable|
|Display type |`screen`, `print`                  |`@media print`        |Contextual |
|Pixel density|`1x`, `2x`, `3x`                   |`resolution`          |Immutable  |

### 1.4 Container sizing contexts

|Dimension         |Values                    |Detection Method          |Volatility|
|------------------|--------------------------|--------------------------|----------|
|Container width   |`narrow`, `medium`, `wide`|`@container (inline-size)`|Volatile  |
|Container context |Custom property values    |`@container style()`      |Inherited |
|Parent constraints|`constrained`, `fluid`    |Container queries         |Inherited |

Container style queries enable **context propagation** through custom properties:

```css
@container style(--context: sidebar) {
  blockquote { font-size: 15px; }
}
```

### 1.5 Interaction states (mutually exclusive)

|State         |Description                |Detection                |Volatility|
|--------------|---------------------------|-------------------------|----------|
|Rest/default  |Base appearance            |None                     |—         |
|Hover         |Pointer over element       |`:hover`                 |Volatile  |
|Focus         |Keyboard/programmatic focus|`:focus-visible`         |Volatile  |
|Active/pressed|Currently being activated  |`:active`                |Volatile  |
|Disabled      |Not currently interactive  |`[disabled]`, `:disabled`|Volatile  |
|Dragged       |Element being dragged      |Programmatic             |Volatile  |

Material Design 3’s **state layer system** handles interaction states through color overlays at specific opacities: hover at **8%**, focus at **10%**, and pressed at **12%**. State layers are not additive—only one applies at a time. 

### 1.6 Additive/combinable states

|State             |Description              |Detection                   |Volatility |
|------------------|-------------------------|----------------------------|-----------|
|Selected          |Chosen from options      |`[aria-selected]`           |Volatile   |
|Checked           |Toggle is on             |`:checked`                  |Volatile   |
|Expanded/collapsed|Disclosure state         |`[aria-expanded]`           |Volatile   |
|Loading           |Action in progress       |Programmatic                |Volatile   |
|Error/invalid     |Validation failure       |`:invalid`, `[aria-invalid]`|Volatile   |
|Warning           |Cautionary state         |Programmatic                |Volatile   |
|Success           |Operation completed      |Programmatic                |Volatile   |
|Read-only         |Viewable but not editable|`[readonly]`                |Semi-stable|
|Visited           |Previously accessed      |`:visited`                  |Stable     |

### 1.7 Accessibility contexts

|Dimension              |Values                                    |Detection Method              |Volatility|
|-----------------------|------------------------------------------|------------------------------|----------|
|Reduced motion         |`reduce`, `no-preference`                 |`prefers-reduced-motion`      |Stable    |
|Contrast preference    |`more`, `less`, `no-preference`           |`prefers-contrast`            |Stable    |
|Reduced transparency   |`reduce`, `no-preference`                 |`prefers-reduced-transparency`|Stable    |
|Forced colors          |`active`, `none`                          |`forced-colors`               |Immutable |
|Color vision deficiency|`protanopia`, `deuteranopia`, `tritanopia`|Declared                      |Stable    |

### 1.8 Locale and internationalization contexts

|Dimension     |Values                                       |Detection Method   |Volatility|
|--------------|---------------------------------------------|-------------------|----------|
|Text direction|`ltr`, `rtl`                                 |`[dir]`, `:dir()`  |Stable    |
|Writing mode  |`horizontal-tb`, `vertical-rl`, `vertical-lr`|Declared           |Stable    |
|Language      |BCP 47 tags                                  |`[lang]`, `:lang()`|Stable    |
|Number format |Locale-specific                              |Intl API           |Stable    |

Adobe Spectrum’s `<sp-theme>` component manages `dir` and `lang` attributes for all descendants, propagating locale context through the component tree.  

### 1.9 Platform and runtime contexts

|Dimension        |Values                                     |Detection Method |Volatility|
|-----------------|-------------------------------------------|-----------------|----------|
|Platform         |`web`, `ios`, `android`, `windows`, `macos`|Build-time       |Immutable |
|Framework        |`react`, `vue`, `swift`, `kotlin`          |Build-time       |Immutable |
|Browser engine   |`chromium`, `firefox`, `webkit`            |Feature detection|Immutable |
|Rendering context|`ssr`, `csr`, `native`                     |Build-time       |Immutable |

### 1.10 Elevation and surface contexts

|Dimension    |Values                                                            |Detection Method|Volatility|
|-------------|------------------------------------------------------------------|----------------|----------|
|Surface level|`sunken`, `default`, `raised`, `overlay`                          |Declared        |Inherited |
|Shadow depth |`level-0` through `level-5`                                       |Derived         |Inherited |
|Z-index layer|`background`, `content`, `floating`, `modal`, `popover`, `tooltip`|Declared        |Inherited |

Atlassian’s **surface-aware token** `utility.elevation.surface.current` automatically adapts based on nesting context—a card inside a modal receives appropriate surface values without explicit declaration.  

### 1.11 Semantic and feedback contexts

|Dimension|Values                                          |Detection Method|Volatility|
|---------|------------------------------------------------|----------------|----------|
|Status   |`neutral`, `info`, `success`, `warning`, `error`|Declared        |Volatile  |
|Emphasis |`primary`, `secondary`, `tertiary`              |Declared        |Inherited |
|Intent   |`action`, `navigation`, `destructive`           |Declared        |Inherited |

### 1.12 Structural contexts

|Dimension     |Values                                          |Detection Method|Volatility |
|--------------|------------------------------------------------|----------------|-----------|
|Component type|`control`, `container`, `feedback`, `navigation`|Declared        |Inherited  |
|Region        |`header`, `sidebar`, `footer`, `main`, `overlay`|Declared        |Semi-stable|
|Nesting level |`root`, `nested`, `deeply-nested`               |Computed        |Inherited  |

-----

## 2. Volatility hierarchy classification

Context dimensions fall into five volatility tiers that determine when values are resolved and how they propagate:

### Tier 1: Immutable (build-time resolution)

**Characteristics**: Set once during build, never change at runtime.

- Platform target (iOS, Android, web)
- Brand identity fundamentals
- Core primitive token values
- Browser/engine capabilities

**Resolution strategy**: Compile-time transforms via Style Dictionary or equivalent. Output platform-specific files (`.css`, `.swift`, `.kt`). 

### Tier 2: Stable (session-level resolution)

**Characteristics**: Set at application initialization, rarely change during session.

- User-preferred color scheme
- Reduced motion preference
- High contrast preference
- Locale and text direction
- User density preference
- Viewport size category

**Resolution strategy**: CSS media queries, CSS custom properties at `:root`. Changes trigger limited re-renders.

```css
:root {
  --color-bg: white;
}
@media (prefers-color-scheme: dark) {
  :root { --color-bg: #1a1a1a; }
}
```

### Tier 3: Semi-stable (navigation-level resolution)

**Characteristics**: Change on route transitions or explicit user action.

- Page theme overrides
- Section-specific contexts (dark sidebar in light app)
- Feature flags
- Content region contexts

**Resolution strategy**: CSS cascade with data attributes, React Context providers, CSS cascade layers.

```css
[data-theme="dark"] {
  --color-bg: #1a1a1a;
}
```

### Tier 4: Volatile (interaction-level resolution)

**Characteristics**: Change frequently during user interaction.

- Hover, focus, active states
- Selected, expanded states
- Loading, validation states
- Content density changes

**Resolution strategy**: CSS pseudo-classes, JavaScript state management. Must be highly performant.

```css
.button:hover { --button-bg: var(--color-primary-hover); }
.button:active { --button-bg: var(--color-primary-active); }
```

### Tier 5: Inherited (flow through component tree)

**Characteristics**: Propagate from parent to child unless explicitly overridden.

- Theme context
- Text direction
- Density settings
- Surface level

**Resolution strategy**: CSS inheritance, React Context, Figma’s “Auto” mode inheritance.

-----

## 3. Context type taxonomy with intersection patterns

Real-world systems define **named context patterns** where multiple dimensions intersect predictably.

### 3.1 Surface contexts

Control background appearance based on visual hierarchy depth:

|Context           |Use Case                                |Typical Properties                 |
|------------------|----------------------------------------|-----------------------------------|
|`surface.sunken`  |Inset wells, grouped content backgrounds|Background darker/lighter than page|
|`surface.default` |Base canvas level                       |Page background color              |
|`surface.raised`  |Cards, tiles, elevated containers       |Background + shadow level 1-2      |
|`surface.overlay` |Modals, dialogs, drawers                |Background + shadow level 3-4      |
|`surface.floating`|Tooltips, popovers, menus               |Background + shadow level 5        |

IBM Carbon’s **layering model** defines alternating patterns for light themes (White → Gray 10 → White) and progressive lightening for dark themes (Gray 100 → Gray 90 → Gray 80).

### 3.2 Interactive control contexts

Define token resolution for interactive elements:

|Context         |Use Case              |State Considerations               |
|----------------|----------------------|-----------------------------------|
|`control.button`|Buttons, CTAs         |Primary/secondary/tertiary × states|
|`control.input` |Text fields, textareas|Default/focus/error/disabled       |
|`control.select`|Dropdowns, selects    |Closed/open × states               |
|`control.toggle`|Switches, checkboxes  |Off/on × states                    |
|`control.slider`|Range inputs          |Track/thumb × states               |

### 3.3 Feedback and status contexts

Communicate system status through consistent color patterns:

|Context           |Color Family       |Use Cases                 |
|------------------|-------------------|--------------------------|
|`feedback.neutral`|Gray/default       |Informational messages    |
|`feedback.info`   |Blue               |Help, guidance            |
|`feedback.success`|Green              |Completion, confirmation  |
|`feedback.warning`|Yellow/amber       |Caution, attention needed |
|`feedback.error`  |Red                |Failure, validation errors|
|`feedback.loading`|Primary + animation|In-progress states        |

### 3.4 Navigation region contexts

Structural regions with consistent theming patterns:

|Context      |Typical Theme |Special Considerations         |
|-------------|--------------|-------------------------------|
|`nav.header` |Primary brand |Fixed positioning, high z-index|
|`nav.sidebar`|Often inverted|May use inline theming         |
|`nav.footer` |Subdued       |Often sunken surface           |
|`nav.tabs`   |Matches parent|Selection states critical      |
|`nav.toolbar`|Raised surface|Dense spacing                  |

### 3.5 Content area contexts

Define reading and data presentation patterns:

|Context          |Spacing Scale|Typography Scale|
|-----------------|-------------|----------------|
|`content.article`|Comfortable  |Larger body text|
|`content.form`   |Moderate     |Standard body   |
|`content.table`  |Compact      |Smaller, denser |
|`content.card`   |Comfortable  |Varied hierarchy|
|`content.list`   |Compact      |Consistent items|

### 3.6 Overlay contexts

Z-layer management with appropriate isolation:

|Context          |Z-Index Range|Backdrop        |Focus Trap|
|-----------------|-------------|----------------|----------|
|`overlay.menu`   |100-199      |None            |No        |
|`overlay.tooltip`|200-299      |None            |No        |
|`overlay.popover`|300-399      |Optional        |Optional  |
|`overlay.drawer` |400-499      |Semi-transparent|Yes       |
|`overlay.modal`  |500-599      |Opaque/semi     |Yes       |
|`overlay.toast`  |600-699      |None            |No        |

-----

## 4. Inheritance and scoping rule specifications

### 4.1 Three-tier token hierarchy model

The industry-standard architecture adopted by Material Design 3, Norton Design System, and others:

**Tier 1: Primitive/Reference tokens**

```
color.blue.500: #0066CC
space.4: 16px
```

- Raw values with systematic names
- Context-agnostic
- Never used directly in components
- Form the immutable foundation

**Tier 2: Semantic/Alias tokens**

```
color.interactive: {color.blue.500}
color.text.primary: {color.gray.900}
```

- Reference primitives
- Add meaning and intent
- Enable theming by remapping
- Primary API for component developers

**Tier 3: Component tokens**

```
button.color.background: {color.interactive}
button.color.background.hover: {color.blue.600}
```

- Scoped to specific components
- Reference semantic tokens
- Handle component-specific states

### 4.2 CSS custom property inheritance

CSS custom properties inherit through the DOM tree by default:

```css
:root {
  --color-primary: #0066cc;           /* Global primitive */
  --color-action: var(--color-primary); /* Global semantic */
}

.card {
  --card-bg: var(--surface-raised);    /* Scoped to card */
}

.card .button {
  /* Inherits --color-action from :root */
  /* Inherits --card-bg from .card */
  background: var(--card-bg);
}
```

### 4.3 Context boundary definitions

Contexts reset at specific boundaries:

|Boundary Type |Behavior                           |Example                             |
|--------------|-----------------------------------|------------------------------------|
|Theme Provider|New theme context starts           |`<ThemeProvider colorScheme="dark">`|
|Portal/Dialog |Often resets to root context       |React Portal, Modal                 |
|Shadow DOM    |CSS properties cross, classes don’t|Web Components                      |
|iframe        |Complete isolation                 |Third-party embeds                  |
|CSS `@scope`  |Scoped styling boundary            |`@scope (.card) { }`                |

### 4.4 Figma Variables inheritance model

Figma’s mode inheritance follows a DOM-like pattern: 

```
Page → Section → Frame → Group → Layer
  ↓        ↓        ↓       ↓       ↓
(mode propagates down unless explicitly set)
```

- Objects with “Auto” mode traverse up to find nearest explicitly set mode
- If no parent has explicit mode, collection’s default mode applies 
- Component instances inherit from parent frame, not component definition

### 4.5 Context composition rules from DTCG

The W3C Design Tokens specification (v2025.10) defines composition via resolvers:

```json
{
  "composition": [
    { "source": "primitives" },
    { "source": "semantic", "modifiers": { "theme": "dark" } },
    { "source": "component-overrides" }
  ]
}
```

**Resolution rules**:

- Later sources override earlier sources for same-path tokens
- Deep merge semantics: local tokens extend inherited, overriding same-path values
- Child tokens inherit `$type` from parent groups via `$extends`

-----

## 5. Resolution algorithm principles

### 5.1 Priority order (highest to lowest)

When multiple contexts apply, resolution follows this precedence:

1. **Inline/component-level overrides** — Explicit prop or style values
1. **Context-specific tokens** — Tokens scoped to current context
1. **State-specific tokens** — Hover, focus, disabled overrides
1. **Theme-specific tokens** — Current theme’s semantic values
1. **Semantic defaults** — Default semantic token mappings
1. **Primitive fallbacks** — Raw value fallbacks

### 5.2 State combination precedence

When multiple interaction states apply simultaneously:

```
Disabled > Selected > Focused > Active > Hover > Default
```

Disabled always wins because interaction is prevented. Material Design 3’s state layer system addresses this by applying only **one** overlay at a time, with the highest-precedence state taking effect. 

### 5.3 CSS fallback chains

```css
.button {
  background: var(
    --button-bg-hover,
    var(--button-bg, 
      var(--color-action, 
        #0066cc))
  );
}
```

Resolution traverses the chain until finding a defined value.

### 5.4 Theme resolution order

```
Component Token → Semantic Token → Primitive Token → Hardcoded Fallback
```

### 5.5 Multi-dimensional resolution (Tokens Studio pattern)

When themes have multiple dimensions (brand × mode × density), Tokens Studio generates all permutations: 

```javascript
// Input themes
{ theme: "casual", mode: "light" }
{ theme: "casual", mode: "dark" }
{ theme: "business", mode: "light" }
{ theme: "business", mode: "dark" }

// Token set composition
"light_casual": ["core", "light", "casual"]
"dark_casual": ["core", "dark", "casual"]
"light_business": ["core", "light", "business"]
"dark_business": ["core", "dark", "business"]
```

**Set resolution**: “Last set wins”—when multiple sets define the same token path, the later set’s value applies. 

-----

## 6. Detection mechanism inventory

### 6.1 Auto-detected contexts (CSS media queries)

|Context             |Media Feature                 |Support            |
|--------------------|------------------------------|-------------------|
|Color scheme        |`prefers-color-scheme`        |Baseline (2020)    |
|Reduced motion      |`prefers-reduced-motion`      |Baseline (2020)    |
|Contrast preference |`prefers-contrast`            |Wide (2022+)       |
|Forced colors       |`forced-colors`               |Chrome/Edge/Firefox|
|Display type        |`print`, `screen`             |Universal          |
|Viewport dimensions |`width`, `height`             |Universal          |
|Orientation         |`orientation`                 |Universal          |
|Pointer type        |`pointer`, `hover`            |Wide               |
|Reduced transparency|`prefers-reduced-transparency`|Emerging           |

### 6.2 Auto-detected via container queries

|Context             |Query Type                       |Support                |
|--------------------|---------------------------------|-----------------------|
|Container dimensions|`@container (inline-size)`       |Baseline (2023)        |
|Container style     |`@container style(--prop: value)`|Chrome 111+, Safari 18+|

### 6.3 Programmatically detected

|Context            |Detection Method                         |
|-------------------|-----------------------------------------|
|Interaction states |DOM events, `:hover`, `:focus`, `:active`|
|Component lifecycle|Framework state management               |
|Validation state   |Form validation API                      |
|Selection state    |`aria-selected`, `aria-checked`          |
|Expansion state    |`aria-expanded`                          |

### 6.4 Must be explicitly declared

|Context           |Declaration Method                     |
|------------------|---------------------------------------|
|Brand/theme       |`data-theme`, `class`, Context Provider|
|Platform target   |Build configuration                    |
|Density preference|User settings, `data-density`          |
|Locale/direction  |`lang`, `dir` attributes               |
|Surface level     |`data-surface`, Context Provider       |
|Region            |Semantic HTML, ARIA landmarks          |
|Intent            |Component props (`variant`, `tone`)    |

### 6.5 Mixed detection

Some contexts can be auto-detected but may be user-overridden:

|Context     |Auto-Detection            |Override Method           |
|------------|--------------------------|--------------------------|
|Color scheme|`prefers-color-scheme`    |Theme toggle, `data-theme`|
|Motion      |`prefers-reduced-motion`  |App settings              |
|Density     |Viewport/touch detection  |User preference           |
|Direction   |`lang` attribute inference|Explicit `dir`            |

-----

## 7. Comprehensive edge case catalog with proposed handling

### 7.1 Nested themes (dark inside light, light inside dark)

**Scenario**: Dark sidebar inside light page, with light modal over dark sidebar.

```html
<body class="light-theme">
  <aside class="dark-theme">
    <div class="modal light-theme">
      <!-- Which context applies to modal children? -->
    </div>
  </aside>
</body>
```

**Solutions**:

- **CSS cascade**: Each theme class redefines all tokens; closest ancestor wins
- **Context providers**: React/Vue providers create explicit scope boundaries
- **Carbon approach**: Inline theming mixins create isolated zones
- **Reset patterns**: Use `color-scheme` CSS property to signal browser adaptation

```css
[data-theme="dark"] { color-scheme: dark; --color-bg: #1a1a1a; }
[data-theme="light"] { color-scheme: light; --color-bg: #ffffff; }
```

### 7.2 High contrast mode intersecting with brand colors

**Scenario**: Brand colors fail WCAG contrast requirements when Windows High Contrast Mode activates.

**Solutions**:

- Detect with `@media (forced-colors: active)`
- Map brand colors to system colors (`Canvas`, `CanvasText`, `LinkText`, `ButtonText`)
- Never rely solely on color for meaning—add icons, patterns, or text labels
- GitHub Primer provides dedicated `_high_contrast` theme variants

```css
@media (forced-colors: active) {
  .button {
    border: 2px solid currentColor;
    forced-color-adjust: none; /* Opt out selectively */
  }
}
```

### 7.3 Reduced motion affecting state transitions

**Scenario**: User prefers reduced motion, but state changes need visual feedback.

**Solutions**:

- Reduce duration, don’t eliminate feedback: `animation-duration: 0.01ms`
- Provide alternative non-motion feedback (color change, border)
- Keep essential animations (loading spinners) but reduce intensity
- “Reduce” ≠ “none”—users expect minimized, not eliminated motion

```css
@media (prefers-reduced-motion: reduce) {
  * {
    animation-duration: 0.01ms !important;
    transition-duration: 0.01ms !important;
  }
  /* But keep loading indicators */
  .spinner { animation-duration: 2s !important; }
}
```

### 7.4 RTL layouts in LTR containers (bidirectional text)

**Scenario**: Arabic quotation inside English paragraph.

**Solutions**:

- Use CSS logical properties exclusively: `margin-inline-start`, not `margin-left`
- Set `dir` attribute on containing element
- Use Unicode directional marks (RLM, LRM) for inline content
- Test thoroughly with `ConfigProvider` direction settings

```css
.card {
  padding-inline: var(--space-4);
  margin-block-end: var(--space-2);
}
```

### 7.5 Container queries versus viewport queries

**Scenario**: Component needs container-responsive layout but viewport-responsive typography.

**Solutions**:

- Use container queries for component-internal layout
- Use viewport queries only for page-level decisions
- Combine: container queries for width, viewport for orientation
- Provide component-level wrapper with explicit `container-type`

```css
.card-wrapper { container-type: inline-size; }

@container (width > 400px) {
  .card { flex-direction: row; }
}

@media (min-width: 768px) {
  :root { --font-size-body: 1.125rem; }
}
```

### 7.6 Third-party embed context isolation

**Scenario**: Embedded widget (iframe, webview) doesn’t inherit parent theme context.

**Solutions**:

- Pass theme via URL parameters: `?theme=dark`
- Use `postMessage` API for theme synchronization
- Provide theme override API for embedded components
- Consider Shadow DOM for controlled isolation without iframe

```javascript
// Parent
iframe.contentWindow.postMessage({ theme: 'dark' }, '*');

// Iframe
window.addEventListener('message', (e) => {
  if (e.data.theme) document.body.dataset.theme = e.data.theme;
});
```

### 7.7 Multiple simultaneous interaction states

**Scenario**: Button is focused AND hovered AND selected.

**Solutions**:

- Define clear precedence: `disabled > selected > focused > hover > default`
- Material Design state layers: Apply only highest-precedence overlay
- Combine visual indicators: Ring for focus + fill change for hover
- Shopify Polaris: Uses `tone` + `color` modifiers for state composition

```css
.button:focus-visible { outline: 2px solid var(--focus-ring); }
.button:hover { background: var(--button-bg-hover); }
.button:focus-visible:hover {
  /* Both indicators visible */
  outline: 2px solid var(--focus-ring);
  background: var(--button-bg-hover);
}
```

### 7.8 Print context conflicts with dark theme

**Scenario**: Dark theme tokens produce unreadable printed content.

**Solutions**:

- Force light theme for print media
- Remove decorative shadows and gradients
- Ensure adequate contrast for black-and-white printing

```css
@media print {
  :root {
    --color-bg: white;
    --color-text: black;
  }
  .shadow, .gradient { display: none; }
}
```

### 7.9 Dynamic font scaling breaking layouts

**Scenario**: User increases system font size to 200%; fixed layouts break.

**Solutions**:

- Use `rem` units for typography tokens
- Test at 200% zoom (WCAG 1.4.4 requirement)
- Use `clamp()` for fluid typography
- Avoid fixed pixel heights on containers with text content

```css
.heading {
  font-size: clamp(1rem, 2.5vw, 2rem);
  line-height: 1.2;
}
```

### 7.10 Color blindness and status colors

**Scenario**: Red/green status indicators indistinguishable for colorblind users.

**Solutions**:

- Never use color as sole differentiator
- Add icons, patterns, or text labels
- GitHub Primer provides `_colorblind` and `_tritanopia` theme variants
- Test with color blindness simulators

-----

## 8. Implementation feasibility assessment

### 8.1 CSS native implementation

|Capability              |Support Level|Notes                              |
|------------------------|-------------|-----------------------------------|
|Color modes             |★★★★★        |Media queries + cascade            |
|Density modes           |★★★★☆        |Custom properties + data attributes|
|Viewport contexts       |★★★★★        |Media queries                      |
|Container contexts      |★★★★☆        |Container queries (Baseline 2023)  |
|Interaction states      |★★★★★        |Pseudo-classes                     |
|Accessibility contexts  |★★★★★        |Media queries                      |
|Locale/direction        |★★★★★        |`:dir()`, logical properties       |
|Platform contexts       |★★☆☆☆        |Build-time only                    |
|Surface inheritance     |★★★☆☆        |Requires explicit scoping          |
|Multi-dimensional themes|★★★☆☆        |Combinatorial CSS required         |

### 8.2 Figma Variables implementation

|Capability              |Support Level|Notes                          |
|------------------------|-------------|-------------------------------|
|Color modes             |★★★★★        |Native mode support            |
|Density modes           |★★★★☆        |Via separate collections       |
|Viewport contexts       |★★☆☆☆        |Manual frame variants          |
|Container contexts      |★☆☆☆☆        |Not supported                  |
|Interaction states      |★★★☆☆        |Component variants             |
|Accessibility contexts  |★★★★☆        |As additional modes            |
|Locale/direction        |★★★★☆        |String variables + modes       |
|Platform contexts       |★★★★★        |Code syntax per platform       |
|Surface inheritance     |★★★★★        |Frame-based inheritance        |
|Multi-dimensional themes|★★☆☆☆        |One collection active at a time|

**Key limitation**: Figma supports only one collection’s mode per frame; cannot switch modes across multiple collections simultaneously.

### 8.3 Tokens Studio implementation

|Capability              |Support Level|Notes                        |
|------------------------|-------------|-----------------------------|
|Color modes             |★★★★★        |Theme groups                 |
|Density modes           |★★★★★        |As additional theme dimension|
|Viewport contexts       |★★★☆☆        |Via token set composition    |
|Container contexts      |★★☆☆☆        |Not directly supported       |
|Interaction states      |★★★★☆        |Token modifiers, sets        |
|Accessibility contexts  |★★★★★        |Theme options                |
|Locale/direction        |★★★★☆        |Via token sets               |
|Platform contexts       |★★★★★        |Style Dictionary output      |
|Surface inheritance     |★★★★☆        |Token references             |
|Multi-dimensional themes|★★★★★        |Full permutation support     |

Tokens Studio’s **theme permutation** generates all combinations: `brand × mode × density × locale`. This is the most complete implementation for multi-dimensional theming.

-----

## 9. Comparison matrix: how major design systems handle each dimension

|Dimension            |Figma              |Tokens Studio  |Spectrum      |M3             |SLDS          |Atlassian      |Primer         |Carbon            |Polaris         |
|---------------------|-------------------|---------------|--------------|---------------|--------------|---------------|---------------|------------------|----------------|
|**Color modes**      |Modes              |Theme groups   |CSS classes   |ColorScheme    |Theme settings|data-color-mode|data-color-mode|SCSS mixins       |Provider        |
|**Density**          |Separate collection|Theme dimension|Scale (M/L)   |Layout tokens  |Comfy/Compact |Planned        |—              |—                 |—               |
|**Viewport**         |Manual             |Token sets     |—             |Breakpoints    |—             |—              |—              |—                 |—               |
|**Container**        |—                  |—              |—             |—              |—             |—              |—              |—                 |—               |
|**Interaction**      |Component variants |Modifiers      |Pseudo-classes|State layers   |Hooks         |Tokens         |Pseudo-classes |Interaction tokens|Pseudo-classes  |
|**Accessibility**    |Modes              |Theme options  |High contrast |—              |—             |Planned        |Built-in themes|Manual            |Experimental    |
|**Locale/RTL**       |String vars        |Token sets     |sp-theme dir  |—              |—             |data-dir       |—              |—                 |—               |
|**Platform**         |Code syntax        |SD output      |CSS-focused   |Multi-platform |LWC-focused   |Multi-product  |Web-focused    |React/Web         |React           |
|**Surface/elevation**|—                  |—              |CSS cascade   |Tonal elevation|—             |Surface tokens |—              |Layering model    |Nested providers|
|**Brand/theming**    |Modes              |Unlimited      |system attr   |—              |Theme settings|—              |Override themes|Zone theming      |Provider        |

**Key insight**: No single system handles all dimensions comprehensively. The most complete coverage comes from combining Tokens Studio (multi-dimensional theming) with CSS-based runtime mechanisms.

-----

## 10. Gaps and open questions in current industry approaches

### 10.1 Specification gaps

|Gap                                    |Current State                |Impact                             |
|---------------------------------------|-----------------------------|-----------------------------------|
|**No native modes in DTCG base format**|Requires resolver module     |Extra complexity for simple cases  |
|**Responsive/viewport tokens**         |Not standardized             |Each tool implements differently   |
|**Container context tokens**           |Not addressed                |Component-level theming unclear    |
|**Animation context tokens**           |Reduced motion not formalized|Inconsistent accessibility handling|
|**Platform-specific contexts**         |Build-time only              |No runtime platform detection      |
|**Cross-file token resolution**        |Edge cases remain            |Multi-file systems have ambiguity  |

### 10.2 Tooling gaps

|Gap                             |Description                                           |
|--------------------------------|------------------------------------------------------|
|**Figma multi-collection modes**|Cannot switch modes across collections simultaneously |
|**Container query tokens**      |No design tool supports component-level context       |
|**Accessibility theme previews**|Limited simulation of reduced motion, forced colors   |
|**Conflict detection**          |No tools flag token collisions across contexts        |
|**Resolution debugging**        |Hard to trace why a token resolved to a specific value|

### 10.3 Open architectural questions

1. **Where should context live?**
- Inline in tokens vs. external resolvers
- Trade-off: simplicity vs. flexibility
1. **How to handle context inheritance at boundaries?**
- Do portals inherit parent context?
- Do iframes sync themes automatically?
1. **What’s the right granularity?**
- Per-component tokens vs. semantic tokens
- Overhead of thousands of component-specific tokens
1. **How to version context changes?**
- Adding a new theme variant
- Deprecating an interaction state
1. **Runtime vs. build-time resolution balance**
- Performance cost of runtime CSS variables
- Flexibility cost of build-time resolution

### 10.4 Emerging patterns worth monitoring

- **CSS Container Style Queries**: When browser support stabilizes, enables true component-level context propagation
- **CSS `@scope`**: Explicit styling boundaries without Shadow DOM
- **DTCG Resolvers**: Official spec approach may drive tooling convergence
- **Zero-runtime CSS-in-JS** (Panda CSS, vanilla-extract): Build-time context resolution with CSS variable output
- **Design Token Linting**: Atlassian’s approach to lifecycle management with eslint/stylelint plugins

-----

## Synthesis: toward a unified context model

The research reveals that effective design token context systems share common architectural principles despite implementation differences:

**Contexts are multi-dimensional**. No single axis (light/dark) suffices; real products need appearance × density × viewport × accessibility × locale × brand at minimum.

**Volatility determines mechanism**. Immutable contexts resolve at build-time. Stable contexts use CSS media queries. Volatile contexts require CSS pseudo-classes or JavaScript state.

**Inheritance follows component trees**. Tokens flow from root to leaf unless explicitly bounded by theme providers, portals, or isolation primitives.

**Conflicts resolve by specificity**. The cascade pattern—later overrides earlier, specific overrides general—applies across all implementations.

**Detection complements declaration**. Auto-detection (media queries, container queries) handles user preferences; explicit declaration handles product decisions (brand, region).

The W3C DTCG specification v2025.10 establishes **resolvers** as the standard for multi-dimensional theming, while the base format remains intentionally simple. This separation enables progressive adoption: simple systems use inline tokens, complex systems add resolver modules.

Current gaps center on responsive tokens, container-level context, and cross-tool interoperability. As container style queries mature and tooling converges on DTCG, expect these gaps to narrow. The fundamental context taxonomy documented here—12 dimensions across 5 volatility tiers with 7 named context patterns—provides a comprehensive framework for designing and implementing context-aware design token systems.
