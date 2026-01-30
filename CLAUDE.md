# Nuhaus

Co-living landing page for creative/technical people in Tokyo.

## Domain Vocabulary

- **Building** — The 3D object. Represents a modular apartment complex.
- **Floor** — Each horizontal layer of blocks. One floor = one level of the building.
- **Footprint** — The XY size of the building (how many blocks per floor).
- **Residents** — Lottie walking-person animations. Symbolize people living in the building.
- **Tatami blocks** — The individual rectangular units. Visual metaphor for modular living spaces.

## Views

| View | Purpose | 3D State |
|------|---------|----------|
| Builder (Home) | Interactive configurator | Auto-rotate, user controls floors/footprint |
| Concept | Philosophy narrative | Expanded layers, residents visible, scroll-driven |
| About | Team/contact | Chaos scatter animation |

## Current Implementation

### Residents (Lottie)
- Appear only in Concept view (`sprite.material.opacity` tied to view state)
- Placed probabilistically (15% chance per block) with minimum spacing
- Single shared texture from `walking-person.json`, converted to grayscale
- Size: 1.2 scale (increased for visibility)
- **Behavior system:**
  - States: `idle` (paused 2-5s) → `walking` (towards target) → `idle`
  - Each resident has own position, target, speed (0.4-0.7)
  - Sprite flips via negative `scale.x` when walking left
  - Targets: random valid blocks within current `sizeLevel` bounds

### Building Controls
- XY pad maps to: X = footprint size (0-3), Y = floors (1-12)
- Blocks use ring-based layout (center core + expanding rings)
- `sizeLevel` determines which rings are visible

## Notes

- Mobile skips SSAO, uses lower pixel ratio (1.0) to prevent WebGL context loss
- All 3D logic is inline in index.html (no build step)
