# ♻️ AI-Powered Sustainable Item Exchange & Reward System

An AI-powered system that uses **computer vision** to identify items and encourage sustainable reuse and recycling through a points-based reward system.

## 📌 Overview

Users can capture or upload an item image. The **YOLOv8s** model detects the item and identifies its type, size, and confidence score.

Points are initially marked as **pending** and become approved after the item is physically delivered and verified.

## 🎨 UI/UX Design

The system provides a simple and user-friendly mobile interface designed to make sustainable item exchange easy and engaging.

Main screens include:

- User registration and login
- Home screen
- Item upload and capture
- AI detection results
- Points and rewards
- Delivery and verification
- Transaction history
- Profile and settings

### Figma Prototype

[View the UI/UX Design on Figma](https://sculpt-dragon-65885603.figma.site/)

## 🎯 Objectives

- Identify items using AI
- Encourage reuse and recycling
- Reward users for sustainable actions
- Verify items before approving points
- Track points and transactions

## 🤖 AI Model

The system uses **YOLOv8s** with transfer learning for object detection.

| Parameter | Value |
|---|---|
| Model | YOLOv8s |
| Image Size | 640 × 640 |
| Epochs | 40 |
| Batch Size | 16 |
| Transfer Learning | Yes |

The model identifies the **item type, size, and confidence score**.

## 🔄 System Workflow

```text
Upload / Capture Item
        ↓
AI Detection
        ↓
Item Type + Size + Confidence
        ↓
Pending Points
        ↓
Physical Delivery
        ↓
Item Verification
        ↓
Approved Points
        ↓
Reward Redemption
```

---

## 🌍 Sustainability Impact

Baddel contributes to sustainability by:

- ♻️ Encouraging item **reuse instead of disposal**
- 📦 Supporting **circular economy** principles
- 🏷️ Enabling items to be **refurbished, resold, or recycled**
- 🎁 Using **gamified rewards** to motivate sustainable behavior
- 🌱 Reducing **environmental waste and pollution**

  
## 👥 Team

Afrah bashaddadah - Aya mohammed - Afnan kamel 
Built with ❤️ to make the world a little greener.

