 # Satellite-Imagery-Based-Property-Valuation
 
OVERVIEW:

This project builds a multimodal property valuation system that predicts real estate prices by combining structured housing attributes with satellite imagery.Unlike traditional pricing models that rely only on numerical features, this approach incorporates visual environmental context—such as green cover, road density, and neighborhood layout—extracted from satellite images.
The goal is to demonstrate how computer vision and machine learning can jointly enhance real estate valuation by capturing both property-specific and neighborhood-level signals.

REPOSITORY STRUCTURE:

├── README.md

├── data_fetcher.py             ( The script used to download images from the API )

├── preprocessing.ipynb          ( Data Cleaning, EDA and feature engineering )

├── model_training.ipynb         (Tabular, image-only, and the training loop for the multimodal model) 

├── gradcam.ipynb                (Grad-CAM explainability)

├── outputs/

       └── final_predictions.csv    # Final price predictions


DATASET DESCRIPTION

1️⃣ Tabular Data: _train.xlsx , test.xlsx_

_Key features include:_

price (target), bedrooms, bathrooms, sqft_living, sqft_above, sqft_basement , sqft_lot, sqft_living15, sqft_lot15, condition, grade, view, waterfront
, latitude, longitude

🔹 Engineered features: _house_age (derived from year built)_

2️⃣ Satellite Imagery Data

Satellite images were fetched programmatically using latitude and longitude. Each property is mapped to a satellite image capturing its local environment.

🔹 Images are stored as: _images_train/, images_test/_

# METHODOLOGY

🔹 Collected and preprocessed structured housing data and programmatically acquired corresponding satellite images using geographic coordinates to capture neighborhood-level environmental context.

🔹 Performed exploratory and geospatial data analysis to study the relationship between property prices, structural attributes, and surrounding visual characteristics.

🔹 Leveraged a pretrained ResNet50 convolutional neural network to extract high-dimensional visual embeddings from satellite imagery using transfer learning.

🔹 Trained and evaluated tabular-only, image-only, and multimodal fusion regression models by combining visual embeddings with structured features using gradient-boosted decision trees.

🔹 Applied Grad-CAM to the CNN feature extractor to visually interpret which regions of satellite imagery contributed most to the model’s learned representations.

_Since the final predictor is tree-based and non-differentiable, Grad-CAM is applied to the CNN used for feature extraction, which is the correct and defensible approach._
