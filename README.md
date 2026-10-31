# Egg Segmentation and Video Tracking

A computer vision project developed for the CV Intern Task. The project covers pretrained-model-based egg segmentation, classical image processing, segmentation evaluation, and video-based tracking of individual eggs.

**Author:** Basavaraj N Asuti
**GitHub Repository:** [https://github.com/basavarajasuti69/egg-segmentation](https://github.com/basavarajasuti69/egg-segmentation)

---

## Table of Contents

- [Project Overview](#project-overview)
- [Tasks and Implementation](#tasks-and-implementation)
- [Task 1: Reference Mask Generation](#task-1-reference-mask-generation)
- [Task 2: Classical Segmentation](#task-2-classical-segmentation)
- [Task 3: Evaluation and Analysis](#task-3-evaluation-and-analysis)
- [Task 4: Video Tracking](#task-4-video-tracking)
- [Results Summary](#results-summary)
- [Repository Structure](#repository-structure)
- [Requirements](#requirements)
- [Setup and Execution](#setup-and-execution)
- [Limitations and Future Improvements](#limitations-and-future-improvements)
- [AI Tool Disclosure](#ai-tool-disclosure)
- [Submission Checklist](#submission-checklist)

---

## Project Overview

The objective of this project is to segment eggs in images, develop a classical image-processing algorithm that approximates the reference masks, quantitatively evaluate its performance, and track individual eggs in a video.

The project is organized into four tasks:

1. Generate reference segmentation masks using a pretrained segmentation workflow.
2. Develop and compare classical image-processing segmentation methods.
3. Evaluate the selected method against reference masks using quantitative metrics and qualitative analysis.
4. Apply an egg detection and tracking pipeline to a video, assigning separate track labels to two eggs.

The development and evaluation results are documented in the technical report included in the `report/` directory.

---

## Tasks and Implementation

### Task 1: Reference Mask Generation

A pretrained segmentation workflow was used to generate reference masks for 110 dataset images.

The recorded approximate distribution was:

| Image category               | Number of images |
| ---------------------------- | ---------------: |
| Images containing one egg    |               92 |
| Images containing two eggs   |                7 |
| Images containing three eggs |               11 |
| **Total**                    |          **110** |

The generated masks provide approximate reference annotations for the subsequent evaluation.

Because the masks were automatically generated, they may contain inaccurate boundaries, missed eggs, or merged regions. Therefore, the reported evaluation scores measure agreement with these generated references rather than independently human-verified ground truth.

### Task 2: Classical Segmentation

The dataset was divided into development and held-out evaluation sets in a 4:1 ratio.

| Split               | Number of images |
| ------------------- | ---------------: |
| Development         |               88 |
| Held-out evaluation |               22 |
| **Total**           |          **110** |

Several classical image-processing configurations were tested. The selected configuration was **Trial 7**, a hybrid segmentation approach using shape and area validation with a GrabCut fallback.

The selected configuration was frozen after development-set experimentation. The held-out evaluation set was used to assess its performance.

### Development Experiment Results

| Trial            |   Mean IoU | Median IoU |  Mean Dice | Images with IoU < 0.5 |
| ---------------- | ---------: | ---------: | ---------: | --------------------: |
| 1                |     0.3428 |     0.2393 |     0.4607 |                    64 |
| 2                |     0.3545 |     0.2470 |     0.4562 |                    65 |
| 3                |     0.4949 |     0.5099 |     0.5924 |                    43 |
| 4                |     0.1778 |     0.0000 |     0.2014 |                    72 |
| 5                |     0.4957 |     0.4570 |     0.6055 |                    49 |
| 6                |     0.5718 |     0.6320 |     0.6719 |                    37 |
| **7 — Selected** | **0.6405** | **0.8057** | **0.7022** |                **26** |

Trial 7 achieved the highest recorded development-set mean IoU among the listed trials and was selected for held-out evaluation.

The complete experiment log should be provided in `experiment_log.csv`. The technical report documents the experimental results and the reasoning behind the final selection.

### Task 3: Evaluation and Analysis

The selected classical segmentation method was evaluated on 22 held-out images.

#### Evaluation Results

| Metric                |  Result |
| --------------------- | ------: |
| Evaluation images     |      22 |
| Mean IoU              | 0.53199 |
| Median IoU            | 0.58182 |
| Mean Dice             | 0.60393 |
| Images with IoU < 0.5 |      10 |
| Images with IoU ≥ 0.5 |      12 |

#### Evaluation Metrics

**Intersection over Union (IoU)** measures the overlap between the predicted mask and reference mask:

`IoU = Intersection / Union`

**Dice coefficient** measures overlap between the predicted and reference masks:

`Dice = 2 × Intersection / (Predicted Area + Reference Area)`

Higher values indicate greater overlap.

#### Interpretation

The selected method achieved a development-set mean IoU of 0.6405 and a held-out mean IoU of approximately 0.5320.

The difference indicates that performance was lower on the held-out evaluation images. Potential challenges include variations in lighting, shadows, background appearance, and egg boundaries.

The evaluation outputs include per-image metrics, a summary of the results, and a list of difficult evaluation cases. These files support further analysis of segmentation errors.

---

## Task 4: Video Tracking

The final tracking pipeline was applied to the supplied video.

### Video Information

| Property       | Value                    |
| -------------- | ------------------------ |
| Video duration | Approximately 60 seconds |
| Frame rate     | 24 FPS                   |
| Total frames   | 1,440                    |
| Resolution     | 1920 × 1080              |
| Track labels   | Egg 1 and Egg 2          |

### Tracking Results

| Metric                       | Result |
| ---------------------------- | -----: |
| Filtered detection records   |  1,229 |
| Frames containing detections |    763 |
| Egg 1 observations           |    630 |
| Egg 2 observations           |    599 |

### Tracking Method

The tracking pipeline uses filtered contour detections and centroid-distance matching with two persistent track slots.

The output assigns separate labels to the two eggs. Constraining the tracker to two track slots prevents unlimited track-ID creation, but it does not guarantee perfect identity preservation during occlusions or missed detections.

Hands, foreground objects, and partial occlusions can interfere with detection and tracking.

### Output Video

**[Watch the final egg-tracking video on Google Drive](https://drive.google.com/file/d/1vIVKLtKufem7F6pb_bMC0VSyC-Na3iEo/view?usp=sharing)**

The output tracking data is provided in `egg_tracks_two_ids.csv`.

**Access note:** The Google Drive video must be shared with **Anyone with the link — Viewer** so the reviewer can open it.

---

## Results Summary

| Task   | Result                                              |
| ------ | --------------------------------------------------- |
| Task 1 | Generated reference masks for 110 images            |
| Task 2 | Selected Trial 7 using development-set results      |
| Task 3 | Held-out mean IoU: 0.53199                          |
| Task 4 | Processed a 1,440-frame video with two track labels |

The final classical segmentation approach was selected using development-set results and evaluated on the held-out split. The video tracking pipeline produced a two-ID tracking output and a corresponding CSV file.

Refer to the PDF report for the recorded results, experiment comparisons, limitations, and additional analysis.

---

## Repository Structure

The current repository contains the notebook and principal result files. The following structure shows the intended organization; move files into these folders if you choose to adopt it.

```text
egg-segmentation/
├── README.md
├── requirements.txt
├── .gitignore
├── egg_segmentation_tracking_submission.ipynb
├── experiment_log.csv
├── evaluation_metrics.csv
├── evaluation_summary.json
├── worst_evaluation_cases.csv
├── egg_tracks_two_ids.csv
└── report/
    └── egg_segmentation_tracking_report.pdf
```

### Important Files

| File                                          | Purpose                                             |
| --------------------------------------------- | --------------------------------------------------- |
| `egg_segmentation_tracking_submission.ipynb`  | Main notebook containing the project implementation |
| `experiment_log.csv`                          | Recorded development experiments                    |
| `evaluation_metrics.csv`                      | Per-image held-out evaluation metrics               |
| `evaluation_summary.json`                     | Summary of evaluation results                       |
| `worst_evaluation_cases.csv`                  | Identification of difficult evaluation cases        |
| `egg_tracks_two_ids.csv`                      | Video tracking records                              |
| `report/egg_segmentation_tracking_report.pdf` | Technical report                                    |

The video is hosted separately on Google Drive to avoid committing a large video file to GitHub.

---

## Requirements

The project uses Python and computer vision and machine-learning libraries.

The environment may require:

- Python
- NumPy
- OpenCV
- Pandas
- Matplotlib
- PyTorch
- torchvision
- Segment Anything, if required by the pretrained segmentation implementation
- Any additional packages imported by the notebook

The exact dependencies and compatible versions should be verified against the notebook and the provided `requirements.txt` before running the project.

---

## Setup and Execution

### Option 1: Run in Google Colab

1. Open the notebook in this repository.
2. Open it in Google Colab.
3. Make the dataset available in your Google Drive or Colab environment.
4. Mount Google Drive if required by the notebook.
5. Install the dependencies listed in `requirements.txt`.
6. Update the dataset and video paths if they differ from those used during development.
7. Run the notebook cells in order.
8. Inspect the generated reference masks, segmentation predictions, evaluation outputs, and tracking video.

Example Google Drive mounting code:

```python
from google.colab import drive
drive.mount("/content/drive")
```

Install dependencies in a Colab cell using:

```python
%pip install -r requirements.txt
```

Run the installation command from the directory containing `requirements.txt`. Additional dependencies or version adjustments may be required depending on the notebook environment.

### Option 2: Run Locally

1. Clone the repository:

```bash
git clone https://github.com/basavarajasuti69/egg-segmentation.git
cd egg-segmentation
```

2. Create and activate a virtual environment:

```bash
python -m venv .venv
source .venv/bin/activate
```

On Windows, activate the environment with:

```powershell
.venv\Scripts\activate
```

3. Install the dependencies:

```bash
pip install -r requirements.txt
```

4. Open the notebook in Jupyter or upload it to Google Colab.
5. Provide the dataset and video files at the paths expected by the notebook.
6. Run the notebook cells in order.

**Reproducibility note:** The dataset and input video are not included in the repository. The notebook may require Google Drive paths or environment-specific setup. Verify the actual paths and package versions before execution.

---

## Limitations and Future Improvements

The recorded results indicate several areas for further improvement:

- Improve mask boundaries and reduce false-positive background regions.
- Handle variations in lighting, shadows, and background appearance.
- Investigate difficult evaluation images using the saved per-image metrics and visualizations.
- Validate generated reference masks manually to establish more reliable ground truth.
- Improve egg detection when hands or other objects partially cover an egg.
- Improve identity preservation through occlusion and missed detections.
- Evaluate the approach on additional images and videos.

These are potential future improvements, not claims that the changes have already been implemented.

---

## AI Tool Disclosure

AI assistance was used during development to help troubleshoot implementation issues, interpret errors and evaluation outputs, and organize project documentation.

The implementation, reported metrics, and final submission should be reviewed by the author to ensure they accurately represent the completed work. The author should be prepared to explain the segmentation method, evaluation metrics, tracking logic, and limitations during the interview.

---

## Author

**Basavaraj N Asuti**

GitHub: [https://github.com/basavarajasuti69/egg-segmentation](https://github.com/basavarajasuti69/egg-segmentation)

---

## Submission Checklist

- [x] Public GitHub repository created.
- [x] Main project notebook uploaded.
- [x] Experiment log uploaded.
- [x] Evaluation CSV and JSON uploaded.
- [x] Tracking CSV uploaded.
- [x] Confirm the final PDF report is present in `report/`.
- [x] Confirm the Google Drive video is accessible to anyone with the link.
- [x] Verify the notebook and README instructions match the final implementation.
- [x] Ensure the repository contains no credentials or unnecessary large files.
- [x] Review the final project and be prepared to explain the implementation.
