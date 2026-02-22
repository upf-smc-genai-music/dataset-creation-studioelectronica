# Khoomei Vowel Texture Dataset
## Mouth Openness via Pink Trombone Vocal Tract Model
### CMC Part 2, Assignment 1

---

## Parameter

| Name | Type | Range | Acoustic meaning |
|------|------|-------|-----------------|
| `openness` | continuous | 0–1 | 0=mouth open (/ɑ/), 1=mouth closed (/i/,/u/) |

Maps to `tongue.diameter` in the Pink Trombone API (inverted):  
`openness = 1 − (diameter − 2.05) / (12.75 − 2.05)`

---

## Phoneme positions (Pink Trombone API)

| openness | diameter | phoneme | example | F1 (Hz) |
|---------|---------|---------|---------|---------|
| 0.00 | 12.75 | /ɑ/ | 'part' | 730 |
| 0.13 | 3.43 | /e/ | 'pet' | 530 |
| 0.08 | 2.87 | /ɪ/ | 'pit' | 390 |
| 0.07 | 2.80 | /ə/ | 'pert' | 500 |
| 0.07 | 2.78 | /æ/ | 'pat' | 660 |
| 0.04 | 2.46 | /ʌ/ | 'put' | 640 |
| 0.01 | 2.20 | /i/ | 'peat' | 270 |
| 1.00 | 2.05 | /u/ | 'boot' | 300 |
| 1.00 | 2.05 | /ɒ/ | 'pot' | 600 |
| 1.00 | 2.05 | /ɔ/ | 'port' | 570 |

---

## Dataset

| Section | Files | Description |
|---------|-------|-------------|
| `vowel_grid_*` | 150 | Systematic grid: 5 tenseness × 30 openness steps |
| `vowel_walk_*` | 30 | Brownian random walk through openness space |
| **Total** | **180** | **15.0 min @ 5.0s/segment** |

---

## Synthesis

- **F0** = 110 Hz | **SR** = 24000 Hz | **Wobble** = 5.2 Hz
- Glottal source: integrated sawtooth (parabolic, −12 dB/oct) + lip radiation
- 4-formant cascade IIR resonators (Klatt/Fant). F2 bandwidth = 40 Hz (tight)
- Phoneme table: https://github.com/zakaton/Pink-Trombone
- F1/F2: Peterson & Barney (1952)

## Annotation format

- Each `.wav` has a matching `.csv` with identical base filename
- CSV: 75 rows/second, single column `openness`
- `parameters.json`: `{"parameter_1": {"name": "openness", "type": "continuous", "min": 0, "max": 1}}`
