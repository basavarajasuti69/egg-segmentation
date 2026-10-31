# Egg Segmentation and Video Tracking

Computer Vision Intern Task: reference-mask generation, classical segmentation experiments, held-out evaluation, and egg tracking in a video.

> **Submission note:** Replace the Google Drive video placeholder below with a public “Anyone with the link — Viewer” share link before submitting. Add the actual notebook/source code and experiment log to the folders indicated below.

## Project tasks

### Task 1 — Reference masks
A pretrained segmentation workflow was used to generate masks for 110 dataset images. The recorded approximate distribution was 92 images containing one egg, 7 containing two eggs, and 11 containing three eggs. Automatically generated masks may contain boundary errors and should be treated as approximate reference annotations.

### Task 2 — Classical segmentation and experiments
The data was split into 88 development images and 22 held-out evaluation images (4:1). The selected development-set configuration was Trial 7: a hybrid segmentation method with shape/area validation and a GrabCut fallback.

| Trial | Mean IoU | Median IoU | Mean Dice | Images with IoU < 0.5 |
|---|---:|---:|---:|---:|
| 1 | 0.3428 | 0.2393 | 0.4607 | 64 |
| 2 | 0.3545 | 0.2470 | 0.4562 | 65 |
| 3 | 0.4949 | 0.5099 | 0.5924 | 43 |
| 4 | 0.1778 | 0.0000 | 0.2014 | 72 |
| 5 | 0.4957 | 0.4570 | 0.6055 | 49 |
| 6 | 0.5718 | 0.6320 | 0.6719 | 37 |
| **7 (selected)** | **0.6405** | **0.8057** | **0.7022** | **26** |

### Task 3 — Held-out evaluation
Evaluation on 22 held-out images produced:

- Mean IoU: **0.53199**
- Median IoU: **0.58182**
- Mean Dice: **0.60393**
- Images with IoU < 0.5: **10**
- Images with IoU ≥ 0.5: **12**

IoU is intersection-over-union. Dice is twice the intersection divided by the sum of predicted and reference mask areas. These metrics measure agreement with the generated reference masks, not independently human-verified ground truth.

### Task 4 — Video tracking
The source video was processed at 24 FPS over 1,440 frames (about 60 seconds). The final output has two track labels, Egg 1 and Egg 2.

- Filtered detection records: 1,229
- Frames containing detections: 763
- Egg 1 observations: 630
- Egg 2 observations: 599
- Output file: `egg_tracking_two_ids.mp4`
- Track data: `egg_tracks_two_ids.csv`

**Video output:** `[Add public Google Drive video link here]`

The tracker uses filtered contour detections and centroid-distance matching with two persistent track slots. A two-ID constraint prevents the number of labels from growing, but it does not guarantee perfect identity preservation during occlusion or missed detections. Hands and other foreground objects may cause errors.

## Repository layout

```text
.
├── README.md
├── requirements.txt
├── .gitignore
├── notebooks/
│   └── egg_segmentation_tracking.ipynb   # Add the final Colab notebook
├── report/
│   └── egg_segmentation_tracking_report.pdf
├── experiments/
│   └── experiment_log.csv                 # Add the actual experiment log
└── outputs/
    ├── evaluation_metrics.csv
    ├── evaluation_summary.json
    └── egg_tracks_two_ids.csv
```

## Setup and run

1. Open the notebook in Google Colab.
2. Mount Google Drive if the notebook reads files from Drive.
3. Install the dependencies listed in `requirements.txt` (and any additional versions documented in the notebook).
4. Make sure the dataset and input video exist at the paths expected by the notebook.
5. Run notebook cells in order.
6. Check the generated masks, evaluation results, and output tracking video.

The exact dependency versions and paths should be verified against the final notebook before publishing.

## AI-tool disclosure

AI assistance was used during development to help draft/debug code, interpret errors and metrics, and structure documentation. The author should review the final code and results and disclose AI assistance according to the task instructions.

## Author

Basavaraj N Asuti

## Submission checklist

- [ ] Add the final `.ipynb` notebook and/or source code.
- [ ] Add `experiment_log.csv`.
- [ ] Add the evaluation CSV and JSON.
- [ ] Add the PDF report.
- [ ] Add a public Google Drive link to the output video.
- [ ] Confirm the repository is public.
- [ ] Check that no credentials, private data, or unnecessary large dataset files are committed.
