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
- Concept content: left-aligned on mobile, centered on desktop (`items-start md:items-center`)
- Desktop: centered text block (`max-w-xl`), 3D building offset right via camera target
- Team: compact Variant E layout — name + short descriptor left, role pill right
- Both pages use `bg-black/60` overlay for text readability

## Camera Architecture

- **OrbitControls** drives camera only in Modeller (auto-rotate, user interaction)
- **GSAP** drives camera in Concept and Team views — `controls.update()` is NOT called
- `cameraTransitioning` flag prevents OrbitControls from fighting GSAP during Home transitions
- `killActiveTransitions()` kills all GSAP tweens on state/camera/target before any view change
- All camera positions stay in the same hemisphere (upper-right quadrant) — no 180° swings

### Camera Positions (Desktop)
| View | camera.position | controls.target |
|------|----------------|-----------------|
| Home | (16, 12, 16) | (0, 0, 0) |
| Concept 0 | (10, 8, 14) | (-4, 3, 0) |
| Concept 1 | (10, 5, 14) | (-4, 0, 0) |
| Concept 2 | (10, 2, 14) | (-4, -3, 0) |
| Team | (14, 14, 16) | (0, 2, 0) |

## Residents

- Each sprite has its **own material clone** (independent opacity, shared textures)
- Direction flip swaps `.map` on the per-sprite material, not the whole material
- Random initial facing direction

## Notes

- Mobile skips SSAO, uses lower pixel ratio (1.0) to prevent WebGL context loss
- All 3D logic is inline in index.html (no build step)
- Nav button event listeners use arrow functions to prevent Event object leaking as args
