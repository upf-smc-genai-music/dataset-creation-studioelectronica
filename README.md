# Khoomei Vowel Texture Dataset  
### Mouth Openness via the Pink Trombone Vocal Tract Model (analysis‑based)  
**GenAI Music Course – Part 2, Assignment 1**  
**Authors:** Lydia & Jenny  

***

## Introduction
This dataset studies the relationship between mouth openness and vowel formant structure using parameters derived from analyses inspired by the *Pink Trombone* vocal tract simulator.  
It does **not** include or redistribute any original *Pink Trombone* code or assets.  

All data were generated independently for educational and research use within a free‑software context.

***

## Parameter Definition

| Name | Type | Range | Acoustic Meaning | Mapping Formula |
|------|------|-------|------------------|----------------|
| `openness` | Continuous | 0 – 1 | 0 = mouth open (/ɑ/); 1 = mouth closed (/i/, /u/) | `tongue.diameter (inverted)`: `1 - (diameter - 2.05)/(12.75 - 2.05)` |

**parameters.json**
```json
{
  "openness": {
    "name": "openness",
    "type": "continuous",
    "min": 0,
    "max": 1
  }
}
```

***

## Phoneme Reference

| Openness | Diameter | Phoneme | Example | F1 (Hz) |
|-----------|-----------|----------|----------|----------|
| 0.00 | 12.75 | /ɑ/ | “part” | 730 |
| 0.13 | 3.43 | /e/ | “pet” | 530 |
| ... | ... | ... | ... | ... |
| 1.00 | 2.05 | /u/ | “boot” | 300 |

*(Values follow Peterson & Barney 1952 vowel statistics.)*

***

## Dataset Overview

| Section | Files | Duration | Description |
|----------|-------|-----------|-------------|
| `vowel_grid_*` | 150 | 12.5 min | 5 tenseness levels × 30 openness steps |
| `vowel_walk_*` | 30 | 2.5 min | Random (Brownian) openness variations |
| **Total** | **180** | **≈ 15 min** | 5 s segments at 24 kHz |

Each CSV file contains 375 frames (75 fps) with linearly interpolated openness values.

***

## Synthesis Specifications

- **Fundamental frequency:** 110 Hz  
- **Sample rate:** 24 kHz  
- **Wobble:** 5.2 Hz  
- **Source model:** Sawtooth (−12 dB/oct) with lip radiation  
- **Resonators:** 4‑formant IIR (F2 bandwidth = 40 Hz)  
- **Processing:** GNU/Linux audio toolchain (SoX + FFmpeg + Python)

***

## Directory Layout

```
downloads/finale/studioelectronica-dataset/
└── raw/
    ├── parameters.json
    ├── vowel_grid_1.wav
    ├── vowel_grid_1.csv
    ├── vowel_walk_1.wav
    ├── vowel_walk_1.csv
    └── ... (179 more pairs)
```

***

## License
This documentation and dataset are released under the **GNU Free Documentation License v1.3 or later (GFDL)**.  
You are free to copy, distribute, and modify the materials provided this license is preserved in all copies.  

Attribution example:  
> *Based on the Khoomei Vowel Texture Dataset by Lydia & Jenny (2026), released under the GNU Free Documentation License v1.3+.*

No original *Pink Trombone* source or binaries are included. All parameter mappings are reconstructed and open for replication using free software.
***

Would you like this license block written as a standalone `LICENSE.txt` file too?
