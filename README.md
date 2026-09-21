Diabetes Dataset – Data Preprocessing & Feature Engineering

A complete, end-to-end data preprocessing pipeline built on a real clinical diabetes dataset — covering data cleaning, missing value treatment, outlier detection, feature encoding, and feature scaling, all in Python using Pandas and Scikit-learn.

Overview

Raw healthcare data is rarely analysis-ready: it contains inconsistent column names, mixed data types, missing values, and clinically-plausible-but-statistically-extreme outliers. This project takes a raw diabetes patient dataset and transforms it into a clean, model-ready format, while documenting the reasoning behind every preprocessing decision — an approach that mirrors real-world data science workflows more than a one-line dropna() would.

Dataset
Source: diabetes.csv
Domain: Clinical / healthcare records used to classify patients as non-diabetic, pre-diabetic, or diabetic
Key fields: Patient demographics (Age, Gender), lab measurements (Urea, Creatinine, HbA1c, Cholesterol, Triglycerides, HDL, LDL, VLDL), BMI, and a diagnostic class label (CLASS: N / P / Y)
Objectives
Load and inspect the raw dataset
Clean and standardize column names and categorical values
Handle missing values and duplicate records
Detect and treat outliers using domain-appropriate methods
Encode categorical features for downstream modeling
Scale numerical features to a common range
Workflow
1. Data Loading & Inspection

Loaded the dataset with Pandas and inspected it using head(), tail(), shape, columns, info(), and dtypes to understand structure, data types, and column composition before touching the data.

2. Data Cleaning
Renamed ambiguous columns (ID → Visit_ID, No_Pation → Patient_ID) for clarity
Verified categorical value consistency in Gender (F/M) and CLASS (N/P/Y)
Generated statistical summaries with describe()
Checked for duplicate records
Visualized numerical distributions with box plots to spot outliers early
3. Missing Value & Duplicate Handling
Calculated the percentage of missing values per column
Since missingness was under 5%, rows with nulls were dropped rather than imputed, to avoid introducing bias into a small dataset
Re-checked for duplicate rows post-cleaning
4. Outlier Handling

Outliers were treated differently depending on what they represented clinically — not with a single blanket rule:

Column(s)	Method	Rationale
AGE, HbA1c, BMI	Retained as-is	Extreme values can be clinically meaningful (e.g., high HbA1c signals disease severity) rather than data errors
Cr (Creatinine)	Capped at 99.5th percentile	Task-specified threshold to remove only the most extreme, unreliable readings
Urea	Capped at 99.9th percentile	Task-specified threshold, less aggressive than Cr due to its distribution
LDL, VLDL, HDL, TG, Chol	IQR method (1.5×IQR)	Standard statistical approach for lipid panel values without a domain-specified threshold
Patient_ID, Visit_ID, Unnamed: 0	Excluded from outlier analysis	These are identifiers, not measurements — "extreme values" are meaningless here
5. Feature Encoding

Applied Label Encoding to the Gender column (F/M) to convert it into a numeric format suitable for model training.

6. Feature Scaling

Applied StandardScaler (z-score standardization) to the numerical clinical features (AGE, Urea, Cr, HbA1c, Chol, TG, HDL, LDL, VLDL, BMI).

Why standardization over normalization: these features have very different natural ranges and units (e.g., Cr spans roughly 6–401 while HDL spans roughly 0.4–1.9). Standardization rescales every feature to a mean of 0 and standard deviation of 1, so no single feature dominates a distance-based or gradient-based model purely because of its raw scale. Identifier columns were excluded from scaling since they carry no measurement information.

Key Insights
Creatinine (Cr) showed the most extreme right-skew and outliers among lab values
Urea, HbA1c, TG, HDL, LDL, and VLDL also showed some outlier presence
AGE and BMI were more tightly distributed, with only a few extreme observations
Identifier columns must be explicitly excluded from statistical outlier/scaling logic — a step that's easy to miss and that silently corrupts results if skipped
Tools & Libraries
Python 3
Pandas – data loading, inspection, cleaning
NumPy – numerical operations
Matplotlib / Seaborn – exploratory visualization (box plots)
Scikit-learn – LabelEncoder, StandardScaler
Project Structure
diabetes-data-preprocessing/
│
├── Data_preprocessing.ipynb   # Full preprocessing pipeline (notebook)
└── README.md                  # Project documentation
How to Run
Clone the repository
bash
   git clone https://github.com/Nivetha-B93/diabetes-data-preprocessing.git
   cd diabetes-data-preprocessing
Install dependencies
bash
   pip install pandas numpy seaborn matplotlib scikit-learn
Open and run the notebook
bash
   jupyter notebook Data_preprocessing.ipynb
Author

Nivetha B Aspiring Data Scientist | Python, SQL, Power BI, Tableau, Machine Learning LinkedIn · GitHub# Data-preprocessing
