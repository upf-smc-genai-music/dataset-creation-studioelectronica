# Khoomei Vowel Texture Dataset (2D)
## Vowel Backness (F1 x F2 parameters) via Pink Trombone Vocal Tract Model
### Gen AI Course Part 2, Assignment 1
#### Team: StudioElectronica (Lydia, Jenny)
---

## Parameters

| Name | Type | Range | Phonetic meaning | Acoustic correlate |
|------|------|-------|-----------------|-------------------|
| `openness` | continuous | 0–1 | 0=open(/ɑ/), 1=closed(/i/,/u/) | F1 frequency |
| `backness`  | continuous | 0–1 | 0=front(/i/,/e/), 1=back(/u/,/ɑ/) | F2 frequency |

Together they tile the full IPA vowel trapezoid.

Mappings to Pink Trombone API:
- `openness = 1 − (tongue.diameter − 2.05) / (12.75 − 2.05)`
- `backness  = 1 − (tongue.index − 2.30) / (27.20 − 2.30)`

---

## Vowel corners

| openness | backness | nearest vowel | example |
|---------|---------|--------------|--------|
| 0 | 0 | /æ/ | 'pat' |
| 0 | 1 | /ɑ/ | 'part' |
| 1 | 0 | /i/ | 'peat' |
| 1 | 1 | /u/ | 'boot' |

---

## Dataset

| Section | Files | Description |
|---------|-------|-------------|
| Grid | 120 | 12 openness × 10 backness steps (snake order) |
| Walk | 0 | 2D Brownian random walk through vowel space |
| **Total** | **120** | **10.0 min @ 5.0s/segment** |

---

## Synthesis

- **F0** = 110 Hz | **SR** = 24000 Hz | **Wobble** = 5.2 Hz
- F1/F2 estimated via RBF (thin-plate spline) interpolation over 10 PT phoneme anchors
- Glottal source: integrated sawtooth (parabolic, −12 dB/oct) + lip radiation
- 4-formant cascade IIR resonators (Klatt/Fant). F2 bandwidth = 40 Hz
- Phoneme anchors: https://github.com/zakaton/Pink-Trombone
- F1/F2 reference values: Peterson & Barney (1952)

## Annotation format

- Each `.wav` has a matching `.csv` with identical base filename
- CSV: 75 rows/second, two columns: `openness`, `backness`
- `parameters.json` defines both parameters as continuous [0,1]

## Contributions
Contributions are welcome! Feel free to open an issue for suggestions or bug reports, or submit pull requests for improvements.
