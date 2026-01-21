## Background

The design token industry has converged on a tiered abstraction model (primitives → options → decisions → components → instances), but context-awareness remains fragmented and underdeveloped. Current systems treat context as flat modes (light/dark, desktop/mobile) rather than as a multidimensional coordinate system with inheritance, intersection, and resolution rules.

This project aims to create a comprehensive context ontology that:

1. **Enumerates all context dimensions** that affect how design tokens should resolve
2. **Classifies dimensions by volatility** (immutable → stable → semi-stable → volatile → inherited)
3. **Defines context types** as named intersections of common dimension combinations
4. **Specifies inheritance and scoping rules** for how context flows through component trees
5. **Establishes resolution algorithms** for handling dimension intersections and conflicts
6. **Documents detection mechanisms** for contexts that can be inferred vs. must be declared
7. **Handles edge cases** comprehensively with documented rationale

## Design Philosophy

This ontology supports a **Single Source of Truth (SSOT) architecture** with controlled contextual variation. Key principles:

- Every resolved token value traces back through an explicit chain
- Context types are finite and governed, not arbitrary
- Exceptions are logged and can be promoted to formal tokens when patterns emerge
- The system learns from friction rather than suppressing it
- Predictable blast radius at every abstraction tier