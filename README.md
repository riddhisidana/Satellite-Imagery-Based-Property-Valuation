# 🛰️ Satellite Imagery-Based Property Valuation

> **CDC × YHills Open Project 2025–2026**  
> Riddhi Sidana (23322023)

A multimodal regression framework that fuses structured housing data with satellite imagery–derived CNN embeddings to predict residential property prices — going beyond traditional hedonic models by incorporating visual, environmental, and neighborhood context.

---

## 🎯 Problem Statement

Traditional property valuation relies solely on structured attributes like square footage and bedrooms — ignoring the environmental quality, green cover, road density, and neighborhood layout visible from above. This project asks:

> *Can satellite imagery make a model smarter about what a neighborhood feels like, not just what a house looks like on paper?*

---

## 📦 Dataset

| Property | Details |
|---|---|
| **Tabular Train** | 16,209 rows · 21 features |
| **Tabular Test** | 5,404 rows · 20 features |
| **Train Images** | 16,110 satellite images (400×400 px) |
| **Test Images** | 5,396 satellite images |
| **Target** | `price` (continuous — log-transformed) |
| **Missing Values** | None |
| **Image Source** | Google Maps Static API · Zoom 19 · 400×400 resolution |

**Feature Categories:** Structural (sqft_living, floors, bedrooms, bathrooms), Quality (grade, condition), Environmental (waterfront, view), Location (lat, long, zipcode), Temporal (yr_built, yr_renovated), Engineered (`house_age`)

---

## ⚙️ End-to-End Architecture

```
Tabular Data ──► Preprocessing ──────────────────────────────┐
                  (outlier removal,                           │
                   log(price), feature selection)             │
                                                              ▼
                                                    Early Fusion (Concatenate)
                                                              │
Satellite Coords ► Google Maps API ► ResNet50 (frozen) ──────┘
                   (400×400 images)   (2048-D embeddings)
                                      ► PCA (256-D)
                                                              │
                                                              ▼
                                                    XGBoost Regressor
                                                              │
                                              ┌───────────────┴───────────────┐
                                           Grad-CAM                         SHAP
                                     (Visual Explainability)       (Tabular Explainability)
                                                              │
                                                    Predicted Price (exp transform)
```

---

## 🧠 Visual Feature Engineering (CNN)

Satellite images were processed through a **pretrained, frozen ResNet50** (ImageNet weights) with `include_top=False` and global average pooling:

- Output: **2048-dimensional embeddings** per property image
- Captures: green cover density, road proximity, plot layout, open space, waterfront proximity
- PCA applied to reduce to **256 dimensions** — improves numerical stability, removes redundancy
- No fine-tuning: frozen weights prevent overfitting on the relatively small image set

---

## 📊 Model Performance Comparison

| Model Type | Data Modality | Features | RMSE (log-price) | R² |
|---|---|---|---|---|
| **Tabular Only** ✅ | Structured data | 14 tabular features | **0.163** | **0.901** |
| **Multimodal Fusion** | Tabular + Satellite | 16 tabular + PCA embeddings | 0.169 | 0.894 |
| Image Only | Satellite imagery | ResNet50 embeddings (2048-D) | 0.367 | 0.464 |

> The multimodal model achieves **R² = 0.894** — within 0.7% of the tabular-only ceiling — while adding interpretable visual context that purely structured models cannot access. Satellite imagery alone captures ~46% of price variance, confirming it encodes real signal.

---

## 🔍 Model Explainability

### SHAP — Tabular Model
Top predictors by SHAP importance:
```
grade > sqft_living > lat > long > sqft_living15 > view > bathrooms > waterfront
```
- Higher `grade` and `sqft_living` → strong positive price impact
- `waterfront` and `view` contribute secondary but meaningful premiums
- Latitude dominates longitude, reflecting north-south neighbourhood stratification in the dataset

### Grad-CAM — Satellite Images
Applied to `conv5_block3_out` (final ResNet50 conv layer):

| Property Tier | CNN Attention |
|---|---|
| **Low-priced** | Dense road networks, constrained layouts, limited open space |
| **Mid-priced** | Mixed residential structures, partial greenery |
| **High-priced** | Green cover, waterfront proximity, low-density open layouts |

> Grad-CAM confirms the model has learned economically meaningful visual signals — not spurious correlations.

---

## 🌍 Geospatial Analysis Highlights

- High-value properties cluster in **narrow lat/long bands** corresponding to established neighbourhoods — not distributed uniformly
- **Green cover intensity** shows a positive non-linear relationship with price, but only when combined with favorable location
- **KDE density plots** reveal urban hotspots that overlap strongly with high-price clusters
- Low-priced satellite images: dense built-up areas, road proximity, minimal landscaping
- High-priced satellite images: larger plots, private open spaces, waterfront access, heavy tree cover

---

## 💡 Key Financial & Economic Insights

- **Construction grade is the single strongest structural predictor** — design quality matters more than size alone
- **Waterfront and view** are non-replicable amenities directly capitalized into price — durable premiums
- **Condition acts as a hygiene factor** — buyers expect baseline maintenance; incremental gains diminish beyond it
- **Dense road proximity imposes negative externalities** — noise and congestion suppress residential value despite accessibility
- **Green cover depreciates more slowly than physical structures** — long-term value stabilizer for investment
- The multimodal framework empirically validates **hedonic pricing theory**: buyers price properties as bundles of structural, locational, and environmental attributes

---

## 🔭 Future Scope

- Fine-tune ResNet50 on real estate–specific satellite imagery
- Incorporate temporal imagery to capture neighbourhood evolution over time
- Experiment with late fusion and end-to-end multimodal architectures
- Integrate street-level imagery (Google Street View) for facade-level signals
- Test higher-resolution imagery for finer environmental context

---

## 🛠 Tech Stack

![Python](https://img.shields.io/badge/Python-3.10-blue?logo=python)
![TensorFlow](https://img.shields.io/badge/TensorFlow-ResNet50-orange?logo=tensorflow)
![XGBoost](https://img.shields.io/badge/XGBoost-Regressor-green)
![SHAP](https://img.shields.io/badge/SHAP-Explainability-purple)
![GradCAM](https://img.shields.io/badge/Grad--CAM-Visual%20XAI-red)
![Google Maps API](https://img.shields.io/badge/Google%20Maps%20Static%20API-Image%20Acquisition-blue?logo=googlemaps)
![scikit-learn](https://img.shields.io/badge/scikit--learn-PCA%20%7C%20Pipelines-orange?logo=scikit-learn)

---

## 📁 Repository Structure

```
satellite-property-valuation/
├── data/
│   ├── train.csv
│   └── test.csv
├── images/
│   ├── train/          # 16,110 satellite images (400×400)
│   └── test/           # 5,396 satellite images
├── notebooks/
│   └── property_valuation_multimodal.ipynb
├── reports/
│   └── Satelite_Imagery_CDC.pdf
└── README.md
```

---
