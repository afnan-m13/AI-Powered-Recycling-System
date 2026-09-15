# ♻️ AI-Powered Sustainable Item Exchange & Reward System

> An intelligent mobile platform that uses computer vision to detect items, reward users with points, and promote a circular economy.

---

## 📌 Table of Contents

- [Project Overview](#-project-overview)
- [Problem Statement](#-problem-statement)
- [Objectives](#-objectives)
- [System Architecture](#-system-architecture)
- [AI Model — YOLOv8](#-ai-model--yolov8)
- [Detection Results](#-detection-results)
- [Training Performance](#-training-performance)
- [System Workflow](#-system-workflow)
- [Points & Reward System](#-points--reward-system)
- [Technologies Used](#-technologies-used)
- [Project Structure](#-project-structure)
- [How to Run the Detection Script](#-how-to-run-the-detection-script)
- [Sustainability Impact](#-sustainability-impact)

---

## 📖 Project Overview

An AI-powered mobile application designed to promote sustainability by encouraging users to exchange unwanted items instead of discarding them.

Users capture or upload a photo of an item. The system automatically:

1. Detects and identifies the item using a deep learning model
2. Assigns **pending reward points** based on the item
3. Verifies the item after physical delivery
4. Converts pending points to **approved points** redeemable for coupons

---

## 🔴 Problem Statement

Modern society generates increasing amounts of waste from unwanted but reusable items. Key challenges include:

- Lack of intelligent platforms for item exchange and reuse
- No incentive systems to motivate sustainable behavior
- Manual classification is slow and error-prone
- Environmental pollution from discarded reusable items

The system addresses these challenges by combining AI-powered detection with a verified reward system.

---

## 🎯 Objectives

- ✅ Build a mobile app that lets users upload/capture item images
- ✅ Implement AI object detection (YOLOv8) for automatic item identification
- ✅ Design a verified point system (pending → approved)
- ✅ Allow users to redeem points for coupons via partner platforms
- ✅ Develop a backend for users, items, and transactions
- ✅ Create an admin dashboard for item verification
- ✅ Promote reuse, resale, and recycling through gamification

---

## 🏗️ System Architecture

```text
┌─────────────────┐        ┌──────────────────┐        ┌──────────────────┐
│  Mobile App     │──────▶ │  Backend Server  │──────▶ │  AI Model        │
│  (Flutter)      │        │  (FastAPI/Node)  │        │  (YOLOv8s)       │
└─────────────────┘        └──────────────────┘        └──────────────────┘
         │                          │                           │
         │                          ▼                           │
         │                 ┌──────────────────┐                 │
         │                 │  Database        │ ◀───────────────┘
         │                 │  (PostgreSQL /   │
         │                 │   Firebase)      │
         │                 └──────────────────┘
         │                          │
         ▼                          ▼
┌─────────────────┐        ┌──────────────────┐
│  User Account   │        │  Admin Dashboard │
│  Points/Coupons │        │  (Verification)  │
└─────────────────┘        └──────────────────┘
```
### Components

| Component | Description |
|-----------|-------------|
| **Mobile App** | User interface — capture images, view points, redeem coupons |
| **Backend Server** | Manages logic, runs AI model, handles point transactions |
| **AI Model** | YOLOv8s — detects item type, bounding box, confidence score |
| **Database** | Stores users, items, pending/approved points, coupons |
| **Admin Dashboard** | Manual verification, approve/reject items, convert points |

---

## 🤖 AI Model — YOLOv8

The detection model is based on **YOLOv8s** (You Only Look Once, version 8, small variant), fine-tuned on a custom furniture/item dataset.

### Training Configuration

| Parameter | Value |
|-----------|-------|
| Base Model | `yolov8s.pt` (pretrained) |
| Dataset | `furniture_data_split.yaml` |
| Epochs | 40 |
| Batch Size | 16 |
| Image Size | 640 |
| Optimizer | Auto |
| Augmentation | RandAugment, Mosaic, Flip LR |
| Transfer Learning | ✅ Yes |

### Model Output Per Detection

```json
{
  "type": "chair",
  "size": "big"
}
```

Size is automatically determined by the bounding box area ratio:
- `small` → < 1% of image area
- `medium` → 1–4% of image area
- `big` → > 4% of image area

---

## 🖼️ Detection Results

### Sample 1 — General Scene Detection
<img width="810" height="1080" alt="result_bus" src="https://github.com/user-attachments/assets/25d711e5-5815-41bd-82c4-feaacc723a0e" />


### Sample 2 — Furniture Detection
<img width="227" height="222" alt="result_furniture" src="https://github.com/user-attachments/assets/6b576025-e9ec-4c8c-990f-e56a5067b105" />


### Sample 3 — Item Detection
<img width="1000" height="1000" alt="result_bb" src="https://github.com/user-attachments/assets/e5508b33-6e12-441d-9bb6-512d872ae969" />


---

## 📊 Training Performance

### F1 Score Curve
<img width="2250" height="1500" alt="train_f1" src="https://github.com/user-attachments/assets/6857bcaa-f519-4039-b3d0-99fca786a8b6" />


### Precision-Recall Curve
<img width="2250" height="1500" alt="train_pr" src="https://github.com/user-attachments/assets/16a3e8a6-bf8b-4397-be75-025673868dbb" />


### Confusion Matrix
<img width="3000" height="2250" alt="confusion_matrix" src="https://github.com/user-attachments/assets/a5fae938-2a60-43b9-bfd8-6c222caa5d4f" />


---

## 🔄 System Workflow

```
User opens app
      │
      ▼
Captures / uploads item image
      │
      ▼
Image sent to backend server
      │
      ▼
AI model detects item (type + size + confidence)
      │
      ▼
System assigns PENDING points
      │
      ▼
User chooses delivery method:
  ├── Drop-off at Baddel location
  └── Request Baddel pickup (may deduct service points)
      │
      ▼
Admin performs physical inspection
      │
      ├── ✅ Approved → Pending points → APPROVED points added to account
      └── ❌ Rejected → Points not converted
      │
      ▼
User redeems approved points for coupons / partner incentives
```

---

## 🏆 Points & Reward System

| Stage | Description |
|-------|-------------|
| **Detection** | AI detects item → system calculates point value |
| **Pending** | Points assigned but locked until physical verification |
| **Delivery** | User delivers item (drop-off or pickup service) |
| **Verification** | Admin inspects item physically |
| **Approved** | Points unlocked and added to user account |
| **Redemption** | User exchanges points for coupons or partner rewards |

> **Why pending points?** To prevent fraud — points are only confirmed after the real item is received and verified by an administrator.

---

## 🛠️ Technologies Used

| Layer | Technology |
|-------|-----------|
| Mobile App | Flutter |
| AI Detection | YOLOv8s (Ultralytics) |
| Backend | Python FastAPI / Node.js |
| Database | PostgreSQL / Firebase |
| Cloud Storage | Cloud storage service |
| Admin Dashboard | Web technologies |
| Training Environment | Google Colab |

---

## 📁 Project Structure

```
baddel/
├── my_model.pt                  # Trained YOLOv8s model weights
├── detect.py                    # Detection script
├── train/
│   ├── args.yaml                # Training configuration
│   ├── BoxF1_curve.png          # F1 score curve
│   ├── BoxPR_curve.png          # Precision-Recall curve
│   ├── BoxP_curve.png           # Precision curve
│   ├── BoxR_curve.png           # Recall curve
│   └── confusion_matrix.png     # Confusion matrix
├── runs/
│   └── detect/                  # Detection output images
└── screenshots/                 # README screenshots
```

---

## ▶️ How to Run the Detection Script

### Requirements

```bash
pip install ultralytics
```

### Run

```bash
python detect.py
```

Then enter the image path when prompted:

```
Enter image name or path: chair.jpg
```

### Example Output

```
=== RESULT ===
Image: chair.jpg
Total Objects: 2

Object Summary:
  - chair: 1
  - table: 1

Details:
  1. Type: chair
     Size: big
  2. Type: table
     Size: medium
```

---

## 🌍 Sustainability Impact

Baddel contributes to sustainability by:

- ♻️ Encouraging item **reuse instead of disposal**
- 📦 Supporting **circular economy** principles
- 🏷️ Enabling items to be **refurbished, resold, or recycled**
- 🎁 Using **gamified rewards** to motivate sustainable behavior
- 🌱 Reducing **environmental waste and pollution**

---

## 📱 App Design

View the full UI prototype on Figma:  
👉 [Baddel on Figma](https://sculpt-dragon-65885603.figma.site/)

---

## 👥 Team

Afrah bashaddadah - Aya mohammed - Afnan kamel 


Supervisor: Dr. Passent Elkafrawy


Built with ❤️ to make the world a little greener.

