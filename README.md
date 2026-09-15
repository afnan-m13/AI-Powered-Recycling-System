# Baddel بدّل — Smart Recycling Powered by AI

**Effat University | CS 3072 | Data Science | Spring 2026**  
Aya Mohammed · Afrah Bashaddadah ·  Afnan Kamel
Supervisor: Dr. Passent Elkafrawy

---

## 1. Project Objective

**Baddel** is an AI-powered recycling application that uses real-time object detection to identify and classify furniture/recyclable items, rewarding users for recycling habits. It addresses the global recycling gap by replacing slow, error-prone manual sorting with a YOLO-based detection model — making AI research practically deployable and locally relevant to **Saudi Vision 2030** sustainability goals (SDG 11, 12, 13).

### Problem
- Global plastic recycling rates remain critically low
- Traditional sorting is slow and error-prone
- Existing CNN models can't detect multiple objects in real time
- No practical deployment pathway for research-level AI models

### Solution
A locally deployed YOLO object detection model trained on furniture images, integrated into a mobile-friendly app that classifies recyclable items and rewards users — directly supporting Vision 2030's circular economy agenda.

---

## 2. Dataset

**Name:** Pix3D — Furniture Dataset  
**Source:** Kaggle  
**Link:** [https://www.kaggle.com/datasets/siddhuneehal/pix3d-furniture](https://www.kaggle.com/datasets/siddhuneehal/pix3d-furniture)

> ⚠️ Dataset is too large to upload. Please download from the Kaggle link above.

### About the Data
Real-world furniture images annotated in **YOLO format** (class label + bounding box coordinates).

| Detail | Info |
|---|---|
| Classes | bed, bookcase, chair, desk, sofa, table, tool, wardrobe, misc |
| Format | Images (`.jpg`/`.png`) + Labels (`.txt`) + Annotations (`.json`) |
| Unmatched images removed | 146 |
| Corrupted files | 0 |
| Dominant class | Chair (3,043 instances) |
| Underrepresented classes | Tool (37), Misc (51) |

---

## 3. Analysis & EDA

Data analysis was conducted **after collection and before model training** to ensure quality, consistency, and readiness.

### Cleaning Pipeline (5 Stages)
1. **Remove Unmatched Images** — 146 images with no label file were removed
2. **Validate YOLO Labels** — all label files checked for valid class IDs and normalized bounding box coordinates
3. **Deduplicate** — exact duplicates detected via MD5 hashing and removed
4. **Quality Filter** — very small/unreadable images removed; no corrupted files found
5. **Visual Bounding Box Inspection** — bounding boxes overlaid on samples to confirm annotation accuracy

### Key EDA Findings
- **Class imbalance**: Chair dominates; tool and misc are underrepresented → addressed during training
- **Resolution variation**: Wide range of image sizes → standardized via resizing to 640×640
- **Bounding box coverage**: Objects cover a significant portion of images → favorable for detection
- **Annotation quality**: All labels correctly formatted and validated

---

## 4. ML Solution — YOLO Object Detection

**Model:** YOLOv8 (`yolo8s.pt`)  
**Framework:** Ultralytics  
**Training environment:** Google Colab (T4 GPU)

### Training Configuration
| Parameter | Value |
|---|---|
| Model | yolo8s.pt |
| Epochs | 40 |
| Image size | 640×640 |
| Train/Val split | 80% / 20% |

### Pipeline Summary
1. Mount Google Drive → access Pix3D dataset
2. Split dataset into train/val folders (90/10 split, random seed 42)
3. Generate YAML config with class names
4. Train YOLO model
5. Evaluate with mAP, precision, recall per epoch
6. Export best weights (`best.pt`) for deployment

### Code
- `model_training.ipynb` — full training pipeline (Google Colab)
- `DS_cleanData_project.ipynb` — EDA and data cleaning

---

## 5. How to Run

### Prerequisites
- Google account (for Colab + Drive)
- Kaggle account (to download dataset)
- Python 3.12+ (for local deployment)

---

### Step 1 — Download the Dataset
1. Go to [Kaggle Pix3D Dataset](https://www.kaggle.com/datasets/siddhuneehal/pix3d-furniture)
2. Download and extract the archive
3. Upload `furniture_images.zip` to your **Google Drive**

---

### Step 2 — Run EDA & Cleaning
Open `DS_cleanData_project.ipynb` in Google Colab and run all cells.  
This handles data validation, deduplication, and quality filtering.

---

### Step 3 — Train the Model
Open `model_training.ipynb` in Google Colab (use a **T4 GPU** runtime).

```bash
# Install dependency
!pip install ultralytics

# Train YOLO model
!yolo detect train data=/content/furniture_data_split.yaml model=yolo11s.pt epochs=40 imgsz=640
```

Best weights are saved to:
```
/content/runs/detect/train/weights/best.pt
```

---

### Step 4 — Test the Model
```bash
!yolo detect predict model=runs/detect/train/weights/best.pt source=data/validation/images save=True
```
Results are saved in `/content/runs/detect/predict/`

---

### Step 5 — Deploy Locally (PC)

```bash
# Create and activate conda environment
conda create --name yolo-env1 python=3.12 -y
conda activate yolo-env1

# Install Ultralytics
pip install ultralytics

# (Optional) GPU support
pip install --upgrade torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu124

# Run detection on webcam
python yolo_detect.py --model my_model.pt --source usb0 --resolution 1280x720

# Or run on a folder of images
python yolo_detect.py --model my_model.pt --source path/to/images/
```

> Download `yolo_detect.py` from the [Model/](./Model/) folder or from the [project GitHub](https://github.com/EdjeElectronics/Train-and-Deploy-YOLO-Models/blob/main/yolo_detect.py)

---

## 6. Project Structure

```
├── Model/                    # Trained YOLO model weights
├── Output/                   # Detection results and evaluation metrics
├── Presentation/             # Baddel-presentation.pptx
├── UI-UX prototype/          # App interface mockups
├── DS_cleanData_project.ipynb  # EDA & cleaning notebook
├── model_training.ipynb        # YOLO training notebook
└── Final_Project_Dataset.pdf   # Dataset reference (link + instructions)
```

---

## 7. Deliverables

| # | Deliverable | Status |
|---|---|---|
| 1 | Project objective, analysis, and ML solution | This README |
| 2 | Data and code |  Notebooks + Dataset PDF |
| 3 | EDA outcomes and analysis solution |  `DS_cleanData_project.ipynb` |
| 4 | Model disseminated to stakeholders |  `Model/` folder |
| 5 | Presentation of outcomes |  `Baddel-presentation.pptx` |
