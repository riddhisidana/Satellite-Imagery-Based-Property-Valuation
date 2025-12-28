# Satellite-Imagery-Based-Property-Valuation
This project builds a multimodal regression pipeline to predict residential property prices by combining tabular housing attributes with satellite imagery. The model captures not only traditional structural features but also environmental and neighborhood context like green cover, road density, and waterfront proximity using aerial images.
satellite-property-valuation/
│
├── data/
│   ├── raw/
│   │   ├── train.xlsx
│   │   ├── test.xlsx
│   │
│   ├── processed/
│   │   ├── train_processed.csv
│   │   ├── test_processed.csv
│
├── images/
│   ├── train/
│   │   ├── 1000102.png
│   │   ├── 1000200.png
│   │   └── ...
│   │
│   ├── test/
│
├── notebooks/
│   ├── 01_eda.ipynb
│   ├── 02_preprocessing.ipynb
│   ├── 03_baseline_tabular_model.ipynb
│   ├── 04_image_feature_extraction.ipynb
│   ├── 05_multimodal_model.ipynb
│
├── src/
│   ├── data_fetcher.py
│   ├── dataset.py
│   ├── model.py
│   ├── train.py
│   ├── utils.py
│
├── outputs/
│   ├── predictions.csv
│   ├── metrics.txt
│
├── report/
│   ├── project_report.pdf
│   ├── architecture_diagram.png
│
├── requirements.txt
├── README.md
└── .gitignore
