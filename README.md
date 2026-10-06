# Crop Production Analysis

## Project Overview

**Crop Production Analysis** is a data analysis project that explores
the relationship between soil nutrients, environmental conditions, and
crop types.

The notebook analyzes agricultural data containing soil nutrient levels
(Nitrogen, Phosphorus, and Potassium), temperature, humidity, soil pH,
rainfall, and the corresponding crop label. The main focus is on
understanding the dataset, identifying and treating outliers,
visualizing feature distributions, comparing crop requirements, and
examining correlations among numerical variables.

> **Note:** The notebook performs exploratory and descriptive analysis.
> Although the dataset context mentions building a predictive
> crop-recommendation model, this notebook does **not** contain a
> machine-learning model-training or prediction section.

------------------------------------------------------------------------

## Problem Statement

Different crops require different combinations of soil nutrients and
environmental conditions. Understanding these requirements can help
identify patterns in agricultural data and support informed
crop-selection decisions.

This project aims to explore:

-   The distribution of soil and environmental variables.
-   The presence of missing values and outliers.
-   Crop-wise requirements for Nitrogen (N), Phosphorus (P), and
    Potassium (K).
-   Differences in soil pH across crop types.
-   Relationships between environmental variables.
-   Correlations among the numerical features.

------------------------------------------------------------------------

## Dataset

### Dataset Context

The notebook describes the dataset as an agricultural dataset intended
to support precision agriculture and crop recommendation.

According to the notebook, the dataset was created by augmenting
rainfall, climate, and fertilizer datasets available for India. The
source is stated as the **Indian Chamber of Food and Agriculture
(ICFA)**.

### Dataset Size

-   **Rows:** 2,200
-   **Columns:** 8
-   **Total values:** 17,600
-   **Missing values:** 0

### Dataset Features

  Feature         Description
  --------------- --------------------------------------
  `N`             Ratio of Nitrogen content in soil
  `P`             Ratio of Phosphorous content in soil
  `K`             Ratio of Potassium content in soil
  `temperature`   Temperature in degrees Celsius
  `humidity`      Relative humidity percentage
  `ph`            Soil pH value
  `rainfall`      Rainfall in mm
  `label`         Crop type / crop class

### Data Types

-   `N`, `P`, `K` → Integer
-   `temperature`, `humidity`, `ph`, `rainfall` → Float
-   `label` → Object/String

### Dataset Source

The notebook credits:

**Indian Chamber of Food and Agriculture (ICFA)**\
https://www.icfa.org.in/

------------------------------------------------------------------------

## Technologies and Libraries

The project is implemented in Python using Jupyter/Google Colab.

### Libraries Used

``` python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
```

### Main Technologies

-   Python
-   Google Colab
-   Jupyter Notebook
-   Pandas
-   NumPy
-   Matplotlib
-   Seaborn

------------------------------------------------------------------------

## Project Workflow

The notebook follows the workflow below:

1.  Load the dataset.
2.  Inspect the structure and dimensions of the data.
3.  Examine numerical statistics.
4.  Check data types.
5.  Check for missing/null values.
6.  Visualize numerical feature distributions.
7.  Identify potential outliers using box plots.
8.  Calculate IQR-based lower and upper bounds.
9.  Cap outlier values for selected numerical features.
10. Visualize relationships between environmental variables.
11. Compare soil pH across different crops.
12. Analyze N, P, and K requirements by crop.
13. Calculate the correlation matrix.
14. Visualize correlations using a heatmap.
15. Summarize important observations.

------------------------------------------------------------------------

## Data Loading

The notebook loads the CSV file using Pandas:

``` python
df = pd.read_csv("/content/drive/MyDrive/Dataset/Crop_recommendation.csv")
```

The dataset is stored in Google Drive and accessed through Google Colab.

------------------------------------------------------------------------

## Dataset Exploration

The notebook examines:

-   Dataset shape
-   Number of rows
-   Number of columns
-   Total number of values
-   Descriptive statistics
-   Column information
-   Data types
-   Missing values
-   Column names

The dataset contains **2,200 records and 8 columns**.

No missing values were found:

``` text
No.of Nulls present in the dataset: 0
```

------------------------------------------------------------------------

## Exploratory Data Analysis

### 1. Distribution of Numerical Features

Histograms are created for the numerical columns to understand their
distributions:

``` python
df.hist(bins=15, figsize=(10,10))
```

The analysis considers the distributions of:

-   Nitrogen
-   Phosphorus
-   Potassium
-   Temperature
-   Humidity
-   Soil pH
-   Rainfall

------------------------------------------------------------------------

## 2. Outlier Analysis

Box plots are used to identify potential outliers in the numerical
variables.

The notebook checks:

-   Temperature
-   Nitrogen
-   Phosphorus
-   Potassium
-   Humidity
-   pH
-   Rainfall

The notebook identifies potential outliers in several columns and
applies an IQR-based treatment.

### IQR Method

The following function calculates the lower and upper bounds:

``` python
def remove_outlier(col_name):
    sorted(col_name)
    Q1, Q3 = col_name.quantile([0.25, 0.75])
    IQR = Q3 - Q1
    lower_bound = Q1 - 1.5 * IQR
    upper_bound = Q3 + 1.5 * IQR
    return lower_bound, upper_bound
```

The project then caps values outside the calculated bounds rather than
deleting complete rows.

For example:

``` python
df['P'] = np.where(df['P'] > high, high, df['P'])
df['P'] = np.where(df['P'] < low, low, df['P'])
```

This approach is applied to:

-   `P`
-   `K`
-   `ph`
-   `rainfall`
-   `temperature`
-   `humidity`

The `N` column is inspected but is not modified through this
outlier-capping procedure.

------------------------------------------------------------------------

## 3. Temperature vs Humidity

A scatter plot is used to analyze the relationship between temperature
and humidity:

``` python
sns.scatterplot(
    x='temperature',
    y='humidity',
    hue='label',
    data=df
)
```

### Observation

The notebook concludes that **no clear relationship was identified
between temperature and humidity values** from this visualization.

The crop label is also used as the hue to observe whether different crop
categories form visible patterns.

------------------------------------------------------------------------

## 4. Soil pH by Crop

A violin plot is created to compare soil pH distributions across crop
types:

``` python
sns.violinplot(
    x="label",
    y="ph",
    data=df
)
```

### Observation

The notebook notes that **chickpea requires a relatively high pH**
compared with the other crop categories in the analysis.

------------------------------------------------------------------------

## 5. Nitrogen, Phosphorus and Potassium Requirements

Box plots are used to compare N, P, and K values across crop types.

``` python
sns.boxplot(x="label", y="N", data=df)
sns.boxplot(x="label", y="P", data=df)
sns.boxplot(x="label", y="K", data=df)
```

### Observations from the Notebook

-   **Cotton** is identified as requiring high Nitrogen (N) values.
-   **Grapes and Apple** are identified as having high Phosphorus (P)
    and Potassium (K) requirements.
-   The notebook also identifies **Grapes and Apple** as having high
    Potassium values.

The crop-wise analysis helps demonstrate that different crops have
different nutrient profiles.

------------------------------------------------------------------------

## 6. Potassium Analysis

The notebook calculates the mean Potassium value for each crop:

``` python
df.groupby(['label'])['K'].mean().sort_values(ascending=False)
```

The resulting order highlights higher average potassium values for crops
such as:

-   Apple
-   Grapes
-   Chickpea

and lower average values for crops such as:

-   Orange
-   Blackgram
-   Lentil

This provides a useful crop-wise comparison of potassium requirements.

------------------------------------------------------------------------

## 7. Correlation Analysis

The numerical columns are selected using:

``` python
num_df = df.select_dtypes('number')
```

A correlation matrix is then calculated:

``` python
corr = num_df.corr()
```

### Important Correlations

The strongest positive relationship observed in the correlation matrix
is between:

-   **Phosphorus (P) and Potassium (K): approximately 0.562**

Other notable relationships include:

-   Temperature and Humidity: approximately **0.212**
-   Nitrogen and Humidity: approximately **0.191**

The correlation between Temperature and pH is very close to zero:

-   Temperature and pH: approximately **-0.021**

The correlation heatmap provides an overall visual representation of
these relationships.

------------------------------------------------------------------------

## Key Findings

Based on the analysis performed in the notebook:

1.  The dataset contains **2,200 records and 8 columns**.
2.  There are **no missing values** in the dataset.
3.  Several numerical variables contain potential outliers that are
    treated using IQR-based bounds.
4.  The notebook uses **outlier capping**, rather than removing entire
    observations.
5.  Temperature and humidity do not show a clear relationship in the
    plotted data.
6.  Crop types have different soil pH distributions.
7.  The notebook identifies **chickpea** as requiring relatively high
    soil pH.
8.  **Cotton** is identified as requiring high Nitrogen levels.
9.  **Grapes and Apple** show high nutrient requirements for Phosphorus
    and Potassium in the notebook's analysis.
10. Phosphorus and Potassium have the strongest positive correlation
    among the numerical variables, at approximately **0.562**.
11. The correlation analysis suggests that most other feature
    relationships are weak to moderate.

------------------------------------------------------------------------

## Visualizations Used

The project includes the following visualizations:

-   Histograms
-   Box plots
-   Scatter plot
-   Violin plot
-   Bar plot
-   Correlation heatmap

These visualizations are used to understand:

-   Feature distributions
-   Outliers
-   Crop-wise nutrient requirements
-   Soil pH differences
-   Environmental relationships
-   Feature correlations

------------------------------------------------------------------------

## Project Structure

A recommended project structure is:

``` text
Crop-Production-Analysis/
│
├── Crop_Production_Analysis.ipynb
├── Crop_recommendation.csv
├── README.md
└── images/
    └── visualization_outputs/
```

The notebook itself contains the complete analysis workflow.

------------------------------------------------------------------------

## How to Run the Project

### Option 1: Google Colab

1.  Open the notebook in Google Colab.
2.  Upload the dataset to Google Drive.
3.  Update the dataset path if required.
4.  Mount Google Drive.
5.  Run the notebook cells sequentially.

The notebook uses:

``` python
from google.colab import drive
drive.mount('/content/drive')
```

### Option 2: Jupyter Notebook

Install the required libraries:

``` bash
pip install pandas numpy matplotlib seaborn
```

Then open the notebook:

``` bash
jupyter notebook Crop_Production_Analysis.ipynb
```

Update the CSV file path in the data-loading cell to match your local
dataset location.

------------------------------------------------------------------------

## Limitations

The current notebook focuses primarily on exploratory and descriptive
analysis.

The following components are **not implemented in the notebook**:

-   Machine learning model training
-   Crop recommendation prediction
-   Train/test split
-   Model evaluation metrics
-   Hyperparameter tuning
-   Deployment of a prediction application

These can be considered future enhancements.

------------------------------------------------------------------------

## Future Enhancements

The project can be extended by:

1.  Building a machine-learning model to predict the most suitable crop.
2.  Splitting the data into training and testing datasets.
3.  Comparing classification algorithms such as:
    -   Decision Tree
    -   Random Forest
    -   K-Nearest Neighbors
    -   Support Vector Machine
    -   XGBoost
4.  Evaluating models using:
    -   Accuracy
    -   Precision
    -   Recall
    -   F1-score
    -   Confusion Matrix
5.  Performing feature importance analysis.
6.  Building an interactive crop recommendation application.
7.  Deploying the application using Streamlit or another web framework.
8.  Adding additional agricultural, geographical, weather, or soil
    features.

------------------------------------------------------------------------

## Conclusion

This project performs an exploratory analysis of agricultural data to
understand how soil nutrients and environmental conditions vary across
different crop types.

The analysis confirms that crop categories exhibit different nutrient
and soil-condition patterns. In particular, the notebook highlights
differences in Nitrogen, Phosphorus, Potassium, and pH requirements
across crops.

The correlation analysis also shows that **Phosphorus and Potassium have
the strongest positive relationship among the numerical features** in
the dataset.

Overall, the project demonstrates how **Python, Pandas, Matplotlib, and
Seaborn** can be used to clean, explore, visualize, and derive insights
from agricultural data. The analysis can serve as a foundation for
developing a machine-learning-based crop recommendation system in a
future phase.

------------------------------------------------------------------------

## Author

**Piyush Pankaj**

Data Science & AI Trainer / Consultant

------------------------------------------------------------------------

## Acknowledgement

Dataset information in the notebook credits the **Indian Chamber of Food
and Agriculture (ICFA)**.

Dataset source mentioned in the notebook:

https://www.icfa.org.in/
