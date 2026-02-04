# Nuhaus

Co-living landing page for creative/technical people in Tokyo.

## Domain Vocabulary

- **Building** — The 3D object. Represents a modular apartment complex.
- **Floor** — Each horizontal layer of blocks. One floor = one level of the building.
- **Footprint** — The XY size of the building (how many blocks per floor).
- **Residents** — Lottie walking-person animations. Symbolize people living in the building.
- **Tatami blocks** — The individual rectangular units. Visual metaphor for modular living spaces.

## Views

| View | Nav Label | Purpose | 3D State |
|------|-----------|---------|----------|
| Home | Modeller | Interactive configurator | Auto-rotate, user controls floors/footprint |
| Concept | Concept | Philosophy narrative (3 sections) | Expanded layers, residents visible, scroll-driven |
| Team | Team | Team bios | Chaos scatter animation, no interaction |

## Navigation

- **Continuous scroll**: Modeller ↔ Concept ↔ Team (scroll up/down navigates between views)
- **Nav buttons**: Click to jump directly to any view
- **Connect**: External CTA link (styled as pill button)

## Current Implementation

### Residents (Lottie)
- Appear only in Concept view with **fade transition** (opacity lerp)
- Placed probabilistically (15% chance per block) with minimum spacing
- Single shared texture from `walking-person.json`, converted to grayscale
- Size: 1.2 scale
- **Behavior system:**
  - States: `idle` (paused 2-5s) → `walking` (towards target) → `idle`
  - Each resident has own position, target, speed (0.4-0.7)
  - Sprite flips based on screen-space direction
  - Targets: random valid blocks within current `sizeLevel` bounds

### Building Controls
- XY pad maps to: X = footprint size (0-3), Y = floors (1-12)
- Blocks use ring-based layout (center core + expanding rings)
- `sizeLevel` determines which rings are visible

### Transitions
- All view transitions use staggered animations (GSAP)
- Concept → Team: content fades, camera moves, chaos builds gradually
- Modeller → Concept: expansion effect with delayed content fade-in

## Design System

### Typography Colors
- `text-white` — Headings, active nav
- `text-white/60` — Body text, descriptions
- `text-white/50` — Inactive nav items
- `text-white/40` — Secondary labels

### Layout
- Concept content: left-aligned on all screen sizes
- Desktop: text on left, 3D on right (no overlap)

## Notes

- Mobile skips SSAO, uses lower pixel ratio (1.0) to prevent WebGL context loss
- All 3D logic is inline in index.html (no build step)
- Concept overlay layer currently disabled (commented out)
