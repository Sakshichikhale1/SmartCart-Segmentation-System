# Smart Cart Customer Segmentation

## Overview
This project performs customer segmentation on a retail dataset using unsupervised learning techniques.  
The goal is to cluster customers based on their demographics, purchasing behavior, and engagement to identify patterns for targeted marketing strategies.

---

## Dataset
- **File:** `smartcart_customers.csv`  
- **Rows:** 2240  
- **Columns:** 22  

**Key Features:**
- **Demographics:** Year_Birth, Education, Marital_Status, Income  
- **Family:** Kidhome, Teenhome  
- **Purchase History:** MntWines, MntFruits, MntMeatProducts, MntFishProducts, MntSweetProducts, MntGoldProds  
- **Engagement:** NumDealsPurchases, NumWebPurchases, NumCatalogPurchases, NumStorePurchases, NumWebVisitsMonth, Recency  
- **Response & Complaints:** Complain, Response  
- **Customer Joining Date:** Dt_Customer  

---

## Preprocessing

### Missing Values
- Filled missing `Income` values with the median.

### Feature Engineering
- `Age` = 2026 − Year_Birth  
- `Customer_Tenure_Days` = days since joining  
- `Total_Spending` = sum of all product categories  
- `Total_Children` = Kidhome + Teenhome  
- Consolidated education levels:
  - Basic / 2nd Cycle → Undergraduate  
  - Graduation → Graduate  
  - Master / PhD → Postgraduate  
- Simplified marital status → `Living_With`: Partner vs Alone

### Drop Columns
- Removed original identifiers and raw spending columns:  
  `ID, Year_Birth, Marital_Status, Kidhome, Teenhome, Dt_Customer, Mnt*`  

### Encoding
- One-hot encoding for categorical features: `Education`, `Living_With`

### Scaling
- StandardScaler applied to numeric features

### Outlier Removal
- Removed extreme ages (>90) and extreme incomes (>600,000)

---

## Exploratory Analysis
- **Pair Plots:** Visualize relationships between features like Income, Age, Total_Spending  
- **Correlation Heatmap:** Identify feature correlations for clustering  
- **PCA:** Reduced dimensions to 3D for visualization; explained variance of first 3 components ~45%

---

## Clustering

### K-Means
- Determined optimal k using Elbow Method and Silhouette Score  
- **Optimal clusters:** 4  
- Visualized 3D PCA projection colored by cluster

### Agglomerative Clustering
- Ward linkage with n_clusters=4  
- 3D PCA projection visualization

---

## Cluster Characterization
- Added cluster labels to dataset  
- Visualized clusters:
  - Count per cluster  
  - Income vs Total_Spending scatter plot  

**Cluster Summary (mean values per cluster):**

| Cluster | Income  | Total_Spending | Age | Customer_Tenure_Days | Total_Children | Living_With |
|---------|---------|----------------|-----|---------------------|----------------|------------|
| 0       | 39,680  | 222            | 55  | 343                 | 1.24           | Partner    |
| 1       | 72,808  | 1,237          | 59  | 370                 | 0.51           | Partner    |
| 2       | 36,960  | 166            | 55  | 339                 | 1.27           | Alone      |
| 3       | 70,723  | 1,190          | 59  | 376                 | 0.46           | Alone      |

**Insights:**
- Clusters 1 and 3: High-income, high-spending customers → target for premium offers  
- Clusters 0 and 2: Moderate-to-low spending → potential for upselling  

---

## Visualization
- 3D PCA plots showing cluster separation  
- Scatterplots for Total_Spending vs Income  
- Countplots for cluster distribution  

---

## Libraries Used
- `pandas`, `numpy` – data handling  
- `matplotlib`, `seaborn` – visualization  
- `scikit-learn` – preprocessing, PCA, clustering, metrics  
- `kneed` – optimal K detection for K-Means  

---

## Workflow Diagram

```text
             ┌──────────────────────┐
             │   Raw Dataset        │
             │ smartcart_customers  │
             └─────────┬────────────┘
                       │
                       ▼
             ┌──────────────────────┐
             │ Data Preprocessing   │
             │ - Fill missing Income│
             │ - Drop irrelevant    │
             │   columns            │
             │ - Encode categorical │
             │ - Scale numeric      │
             └─────────┬────────────┘
                       │
                       ▼
             ┌──────────────────────┐
             │ Feature Engineering  │
             │ - Age                │
             │ - Customer Tenure    │
             │ - Total Spending     │
             │ - Total Children     │
             │ - Living_With        │
             └─────────┬────────────┘
                       │
                       ▼
             ┌──────────────────────────┐
             │ Dimensionality Reduction │
             │ - PCA (3 Components)     │
             └─────────┬────────────────┘
                       │
                       ▼
             ┌──────────────────────┐
             │ Clustering           │
             │ - K-Means (k=4)      │
             │ - Agglomerative      │
             └─────────┬────────────┘
                       │
                       ▼
             ┌──────────────────────┐
             │ Cluster Analysis     │
             │ - Visualize 3D PCA   │
             │ - Scatterplots       │
             │ - Countplots         │
             │ - Cluster Summary    │
             └─────────┬────────────┘
                       │
                       ▼
             ┌──────────────────────┐
             │ Insights & Marketing │
             │ - Identify high/low  │
             │   spenders           │
             │ - Target promotions  │
             └──────────────────────┘


