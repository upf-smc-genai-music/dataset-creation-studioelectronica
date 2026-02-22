# Khoomei Vowel Texture Dataset
## GenAI Part 2, Assignment 1
Lydia & Jenny

### Parameter
| Name | Type | Range | Maps to |
|------|------|-------|---------|
| `openness` | continuous | 0–1 | `tongue.diameter` (inverted) |

0 = mouth open /ɑ/ · 1 = mouth closed /u/

### CSV format
375 rows per file, values interpolated linearly — **vary per row**.

### Dataset
| Section | Files | Description |
|---------|-------|-------------|
| `vowel_grid_*` | 150 | 30 glide spans × 5 passes |
| `vowel_walk_*` | 30 | Brownian random walk |
| **Total** | **180** | **15.0 min** |

### Synthesis
- **Additive synthesis** (no IIR filters — no clicks)
- Harmonics of F0=110 Hz, amplitudes shaped by Lorentzian formant envelopes
- Glottal tilt: 1/k² (−12 dB/oct). Wobble: 5.2 Hz
- F1/F2 from Peterson & Barney (1952). Phoneme positions: github.com/zakaton/Pink-Trombone
