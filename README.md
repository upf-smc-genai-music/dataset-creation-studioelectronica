# Khoomei Vowel Texture Dataset (2D)

> Vowel Backness (F1 x F2 parameters) via Pink Trombone Vocal Tract Model

**Course:** Generative AI for Music -- SMC Masters, UPF (2025-26)
**Assignment:** Dataset Curation (Part 2, Assignment 1)
**Team:** StudioElectronica (Lydia, Jenny)

---

## Table of Contents

- [Overview](#overview)
- [Parameters](#parameters)
- [Vowel Corners](#vowel-corners)
- [Dataset Summary](#dataset-summary)
- [Synthesis Details](#synthesis-details)
- [Annotation Format](#annotation-format)
- [Data Requirements (Assignment Spec)](#data-requirements-assignment-spec)
- [Required File Structure](#required-file-structure)
- [Other Considerations](#other-considerations)
- [Contributions](#contributions)

---

## Overview

A 2D parametric vowel texture dataset synthesized via a Pink Trombone vocal tract model. The two control axes -- openness and backness -- tile the full IPA vowel trapezoid, allowing continuous traversal of vowel space.

---

## Parameters

| Name       | Type       | Range | Phonetic meaning                   | Acoustic correlate |
|------------|------------|-------|-------------------------------------|--------------------|
| `openness` | continuous | 0-1   | 0=open(/a/), 1=closed(/i/,/u/)     | F1 frequency       |
| `backness` | continuous | 0-1   | 0=front(/i/,/e/), 1=back(/u/,/a/)  | F2 frequency       |

Together they tile the full IPA vowel trapezoid.

**Mappings to Pink Trombone API:**
- `openness = 1 - (tongue.diameter - 2.05) / (12.75 - 2.05)`
- `backness  = 1 - (tongue.index - 2.30) / (27.20 - 2.30)`

---

## Vowel Corners

| openness | backness | nearest vowel | example |
|----------|----------|---------------|---------|
| 0        | 0        | /ae/          | 'pat'   |
| 0        | 1        | /a/           | 'part'  |
| 1        | 0        | /i/           | 'peat'  |
| 1        | 1        | /u/           | 'boot'  |

---

## Dataset Summary

| Section   | Files | Description                                        |
|-----------|-------|----------------------------------------------------|
| Grid      | 120   | 12 openness x 10 backness steps (snake order)      |
| Walk      | 0     | 2D Brownian random walk through vowel space        |
| **Total** | **120** | **10.0 min @ 5.0s/segment**                     |

---

## Synthesis Details

- **F0** = 110 Hz | **SR** = 24000 Hz | **Wobble** = 5.2 Hz
- F1/F2 estimated via RBF (thin-plate spline) interpolation over 10 PT phoneme anchors
- Glottal source: integrated sawtooth (parabolic, -12 dB/oct) + lip radiation
- 4-formant cascade IIR resonators (Klatt/Fant). F2 bandwidth = 40 Hz
- Phoneme anchors: [Pink-Trombone](https://github.com/zakaton/Pink-Trombone)
- F1/F2 reference values: Peterson & Barney (1952)

---

## Annotation Format

- Each `.wav` has a matching `.csv` with identical base filename
- CSV: 75 rows/second, two columns: `openness`, `backness`
- `parameters.json` defines both parameters as continuous [github](https://github.com/orgs/community/discussions/132182)

---

## Data Requirements (Assignment Spec)

### Audio Files

Most audio file formats and sampling rates are supported, although the final model will always operate at 24kHz.

**Supported Audio Formats:** .wav, .mp3, .flac, .aac, .ogg, .m4a, .wma, .aiff, .au, .ra, .3gp, .amr, .ac3, .dts, .ape, .mka, .opus

### Annotations (CSV Files)

Each audio file must have a corresponding CSV file with the exact same base name:

| Audio File | CSV File              | Status                             |
|------------|-----------------------|------------------------------------|
| piano.wav  | piano.csv             | Correct                            |
| flute.mp3  | flute_annotations.csv | Incorrect - name mismatch!         |

**The CSV annotation files must follow this specific structure:**

Header row with parameter names (must match those in parameters.json)
One row per frame (75 frames per second)
Continuous parameters: Numeric values (will be normalized to range) [github](https://github.com/orgs/community/discussions/132182)
Categorical parameters: Non-negative integers representing each class (0, 1, 2, ...)

**Example CSV:**
```
saturation	reverb	instrument
10.5	40.3	0
10.0	40.32	1
9.8	40.25	0
...	...	...
```

### Parameter Specifications

Information about the parameters to be controlled must be stored in a parameters.json file. Each parameter must specify its name and type (either continuous or class). Additional features will depend on the type of the parameter as follow:

**Continuous Parameters** (numeric values like tempo, volume, saturation):
min/max: Range used for normalization
unit: Physical unit (e.g., bpm, %, dB)

**Class Parameters** (categorical values like instrument type, genre):
classes: Ordered list of class names (order determines integer mapping)

**Example parameters.json:**
```json
{
    "parameter_1": {"name": "saturation", "type": "continuous", "unit": "dB", "min": 0,   "max": 12},
    "parameter_2": {"name": "reverb",     "type": "continuous", "unit": "%",  "min": 0,   "max": 100},
    "parameter_3": {"name": "instrument", "type": "class",      "classes": ["piano", "flute"]}
}
```

---

## Required File Structure

**Option 1: Simple Structure (all data together)**

```
dataset_folder/
└── raw/                         # Raw input data
    ├── parameters.json          # Parameter configuration file
    ├── piano.wav                # Audio files (various formats supported)
    ├── piano.csv                # Corresponding CSV annotations
    ├── flute.mp3                # More audio files...
    ├── flute.csv                # More CSV files...
    └── ...
```

**Option 2: Split Structure (separate train/validation/test sets) If you want to use specific data splits for training, validation, and testing.**

```
dataset_folder/
└── raw/                         # Raw input data
    ├── parameters.json          # Parameter configuration file
    ├── train/
    │   ├── piano.wav            # Training audio files
    │   ├── piano.csv            # Training CSV annotations
    │   └── ...
    ├── validation/
    │   ├── flute.mp3            # Validation audio files
    │   ├── flute.csv            # Validation CSV files
    │   └── ...
    └── test/
        ├── violin.mp3           # Test audio files
        ├── violin.csv           # Test CSV files
        └── ...
```

---

## Other Considerations

Here we will create a set of wav files of a sound "texture" that varies systematically in some dimension (Recall the water dataset "fill level"). The dataset should be 10-15 minutes of audio roughly equally distributed over the various parameter values. The data can all be in .wav (or other audio formats, see below) file (with varying parameter) or in multiple audio files.

If you are generating the sounds synthetically/algorithmically, you can generate these .csv files at the same time. If you are recording, plan ahead so that the parameter files don't have to be created by hand - they can be created by processing the file (extracting the parameter, for example), or can be recorded so that the parameter values linearly from beginning to end, or is constant for the duration of the corresponding wave file, so that the parameter file is easy to generate.

**OPTIONAL**: The dataset format description should be adequate for this homework. But, if you want to see how a dataset works in RNeNcodec, you can download the "quickstart" (watterfill) example (see the README) for that repository. (You could even trying prepping, training, and playing!)
Code (zipped project): [https://drive.google.com/file/d/1R4GdBB9lbLVdaE4ZExBlmR03jH8ebkpV/view?usp=sharing](https://drive.google.com/file/d/1R4GdBB9lbLVdaE4ZExBlmR03jH8ebkpV/view?usp=sharing)

**Deadlines**
The assignment is due by February 23rd before class on GitHub Classroom (you may work in pairs)
Your dataset should be sharable for others in the class and for demoing.
Invitation to create github classroom repository for dataset assignment: (see Slack)

**Final Notes**
**Assignments Submission**
The data repository should include (where appropriate):
- A README.md describing the project.
- A requirements.txt file listing all Python dependencies, if needed, ensuring they can be easily installed.
- Python 3.10 code (a notebook is fine) for demonstrating the project goals if appropriate.

---

## Contributions

Contributions to our project are welcome! Feel free to open an issue for suggestions or bug reports, or submit pull requests for improvements.

Jenny & Lydia

---

**Generative AI Music**
musikalkemist/smc-musicgen-course
