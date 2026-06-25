# Gait Analysis

A computer vision pipeline for extracting, interpolating, and analyzing human gait kinematics from video footage. Computes joint and segment angles frame-by-frame, handles missing data via cubic spline interpolation, and produces per-cycle visualizations with standardized gait phase breakdowns.

---

## Pipeline Overview

```
Video (.mp4)
    │
    ▼
Video Processing.ipynb      ← contour detection, key point extraction, angle computation → CSV (raw)
    │
    ▼
Imputation.ipynb            ← cubic spline interpolation of missing frames → CSV (full)
    │
    ▼
Analysis and Visualization.ipynb  ← gait cycle segmentation, phase analysis, plots
```

`Video Inspection.ipynb` can be run independently at any point to visually verify key point detection on individual frames.

---

## Notebooks

### `Video Processing.ipynb`
Processes a gait video and outputs raw angle data to CSV.

- Loads an `.mp4` file and extracts frames at the native FPS for a fixed 30-second window
- Converts each frame to grayscale, applies Gaussian blur, and uses binary thresholding to segment the subject from the background
- Detects contours and computes centroid midpoints for four body segments: trunk, thigh, leg, and foot
- Derives seven angles per frame — **hip, knee, ankle** (joint angles) and **trunk, thigh, leg, foot** (segment angles) — from the slopes between key points, with left/right plane correction
- Writes results to a CSV with columns: `Frame, Trunk, Thigh, Leg, Foot, Hip, Knee, Ankle`
- Renders an interactive frame navigator (keyboard: `f` forward, `b` backward, `q` quit) with overlaid angles and key points

### `Video Inspection.ipynb`
Standalone viewer for verifying that key point detection is working correctly on a given subject before running the full pipeline.

### `Imputation.ipynb`
Cleans the raw CSV output and fills gaps.

- Loads a raw CSV by subject ID (e.g. `P010L`)
- Reports missing value counts per column
- Fills gaps using **cubic spline interpolation** for smooth, physiologically plausible curves
- Saves the completed dataset to a separate output directory
- Produces side-by-side before/after plots for visual verification

### `Analysis and Visualization.ipynb`
Performs cycle-level analysis on the interpolated data.

- Segments the continuous angle time series into individual gait cycles using manually defined heel-strike frame indices
- Reports cycle length statistics (mean, min, max frames; average cycle duration in seconds)
- Normalizes each cycle to 100 points via quadratic interpolation for cross-cycle comparison
- Plots individual cycles alongside mean ± SD envelopes for hip, knee, and ankle
- Runs a standardized **gait phase analysis** across eight phases (Initial Contact → Terminal Swing) and summarizes min/max angles per phase per joint
- Generates a comprehensive multi-panel visualization per subject

---

## Sample Output

![Angle plots before and after interpolation](https://github.com/user-attachments/assets/ea1dfdeb-0e16-4ebc-9999-db3a6d260852)
![Comprehensive gait cycle analysis](https://github.com/user-attachments/assets/56d6053f-c3ec-4f67-a91d-309718503070)

---

## Requirements

```
opencv-python
numpy
pandas
matplotlib
scipy
seaborn
```

Install with:

```bash
pip install opencv-python numpy pandas matplotlib scipy seaborn
```

---

## Usage

1. **Set paths** — the notebooks currently use hardcoded Windows paths (e.g. `E:\Gait Analysis NMH\Video\`). Update the `video_path`, `csv_file_path`, and `csv` load paths at the top of each notebook to match your local directory structure.

2. **Run `Video Processing.ipynb`** — enter the subject name and walking plane (`r`/`l`) when prompted. Choose method `a` (standard) or `b` (exceptional) for key point selection. The notebook writes a raw CSV to your output directory.

3. **Run `Imputation.ipynb`** — set `sub_id` to your subject ID (e.g. `'P010L'`). The notebook fills missing frames and saves a cleaned CSV.

4. **Run `Analysis and Visualization.ipynb`** — add your subject's heel-strike frame indices to the `subjects` dictionary, set `subject_ID`, and run all cells to generate cycle plots and phase summaries.

---

## File Structure

```
Gait-Analysis/
├── Video Processing.ipynb          # Frame extraction, angle computation, CSV export
├── Video Inspection.ipynb          # Interactive key point verification viewer
├── Imputation.ipynb                # Missing value interpolation
└── Analysis and Visualization.ipynb  # Cycle segmentation, phase analysis, plots
```

Input videos and CSVs are expected in separate directories outside the repo (configurable via path variables in each notebook).

---

## Gait Phases

The analysis notebook segments each normalized cycle into eight standardized phases:

| Phase | Cycle % |
|---|---|
| Initial Contact | 0–2% |
| Loading Response | 2–12% |
| Midstance | 12–30% |
| Terminal Stance | 30–50% |
| Pre-swing | 50–62% |
| Initial Swing | 62–75% |
| Mid Swing | 75–87% |
| Terminal Swing | 87–100% |

Min and max joint angles are reported per phase for hip, knee, and ankle.
