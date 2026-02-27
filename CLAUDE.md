# Chainsaw

An interactive browser artwork. The user drags across a photograph to make chainsaw cuts — the image tears open to reveal a seething dark red interior, with jagged edges, embedded splinters, flying debris particles, and looping chainsaw audio. Repeated cutting gradually uncovers a hidden image beneath the photograph.

Live at: https://alanblackwell.github.io/chainsaw/

## What it does

- **Background**: Animated dark red "seething" layer — 9 radial gradient blobs drifting slowly, pulsing in intensity
- **Image layer**: `source.png` scaled to fill the viewport, rendered to an offscreen canvas; wound areas erased with `destination-out` compositing to reveal layers beneath
- **Hidden image**: `robot.png` sits between the seething layer and `source.png`. A persistent offscreen reveal mask (`revealCvs`) accumulates the cut polygon at low alpha (~0.04) each frame the chainsaw is active, gradually making `robot.png` visible through the wound areas over repeated cuts. The reveal is permanent — it persists after cuts heal.
- **Triple-tap flash**: Three short clicks or taps within 800ms triggers a full reveal — `source.png` fades to transparent over ~2s to expose the complete `robot.png`, then fades back over ~1.5s, leaving the accumulated partial reveal in place. A tap is distinguished from a cut by short press duration (<300ms) and minimal pointer movement.
- **Cuts**: User drag (mouse or touch) creates a cut path. Each path point carries stable per-point noise values for edge jaggedness and splinter placement
- **Chainsaw kerf**: A jagged polygon (30px base width, ±10px tooth amplitude) is erased along the cut path, with vibration animated each frame
- **Wound edges**: Dark red jagged edge lines + outward splinter spikes drawn along both sides of the kerf
- **Wound glow**: Pulsing red glow along the cut using `screen` blend mode
- **Debris flakes**: Elliptical particles (splinters and chunks) thrown from the cut, with gravity and drag, fading out
- **Audio**: `chainsaw.mp3` (a real recording) played in a looping Web Audio source with random start offset, slight pitch variation, and a two-tap dungeon echo effect. Fades in on mousedown, fades out (2.5s) on mouseup
- **Healing**: Completed cuts fade over 200 frames; up to 5 cuts retained at once

## Files

| File | Role |
|------|------|
| `index.html` | Entire application — single self-contained HTML/JS file |
| `source.png` | Background photograph (front layer) |
| `robot.png` | Hidden image gradually revealed by cutting |
| `chainsaw.mp3` | Chainsaw audio recording (looped during cuts) |

## Key constants (in index.html)

| Constant | Value | Meaning |
|----------|-------|---------|
| `KERF` | 30px | Base chainsaw cut width |
| `TEETH` | 10px | Jagged edge amplitude |
| `CUT_LIFE` | 200 frames | How long a completed cut takes to heal |
| `MAX_CUTS` | 5 | Maximum simultaneous cuts retained |
| `FLASH_HOLD` | 120 frames | Duration of full reveal on triple-tap (~2s) |
| `FLASH_FADE` | 90 frames | Duration of fade-back after flash (~1.5s) |

## Deployment

GitHub Pages, served directly from the `main` branch root.
No build step — edit `index.html` and push to deploy.
