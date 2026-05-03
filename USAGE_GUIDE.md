# Usage Guide - NYC Community Services Analysis

## 🚀 Quick Start

### Method 1: Google Colab (Recommended - No Setup Required)

1. **Open the Colab Notebook:**
   - Visit: [Project Colab Link](https://colab.research.google.com/drive/1jAQoP7MqIDyUm6LvDRAMnOIrLlI60XxV)

2. **Upload Dataset:**
   - Click the folder icon (📁) on the left sidebar
   - Upload `data/ACS_Community_Partners_20250708.csv`

3. **Run the Analysis:**
   - Click `Runtime` → `Run all`
   - OR press `Ctrl+F9` (Windows/Linux) or `Cmd+F9` (Mac)

4. **View Results:**
   - Classification report appears after supervised learning
   - Clustering visualization displays at the end
   - Silhouette score prints in console

---

### Method 2: Local Python Environment

#### Step 1: Clone Repository
```bash
git clone https://github.com/YOUR_USERNAME/nyc-community-services-analysis.git
cd nyc-community-services-analysis
```

#### Step 2: Create Virtual Environment (Optional but Recommended)
```bash
# Windows
python -m venv venv
venv\Scripts\activate

# Mac/Linux
python3 -m venv venv
source venv/bin/activate
```

#### Step 3: Install Dependencies
```bash
pip install -r requirements.txt
```

#### Step 4: Run the Analysis
```bash
cd code
python Code_Data_Mining.py
```

---

## 📊 Understanding the Output

### 1. Borough Distribution
```
BOROUGH
Brooklyn        49
Bronx           40
Queens          36
Manhattan       28
Staten Island   10
```
**Interpretation:** Number of community programs per borough

---

### 2. Supervised Learning Results

#### Classification Report
```
              precision    recall  f1-score   support

           0       1.00      1.00      1.00        11
           1       1.00      1.00      1.00        10
           2       1.00      1.00      1.00         8
           3       1.00      1.00      1.00         7
           4       1.00      1.00      1.00         3
```

**What it means:**
- **Precision:** Of all predicted as this borough, % that were correct
- **Recall:** Of all actual this borough, % that were found
- **F1-Score:** Harmonic mean of precision and recall
- **Support:** Number of samples in test set

**Perfect scores (1.00) indicate:**
- Model learned patterns very well
- Possible data leakage (coordinates strongly predict borough)

---

#### Cross-Validation Accuracy
```
Cross-Validation Accuracy (mean): 0.97
Accuracy Std Dev: 0.03
```

**What it means:**
- Model achieves **97% accuracy** on average
- **Low variation (±3%)** = consistent performance
- Generalizes well to unseen data

---

### 3. Unsupervised Learning Results

#### Optimal K Determination
```
Optimal K (clusters): 2
```

**What it means:**
- Dataset naturally forms **2 distinct groups**
- Determined by highest silhouette score

---

#### Silhouette Score
```
Silhouette Score: 0.81
```

**Interpretation:**
- **Range:** -1 to 1
- **0.81 = Excellent** cluster quality
- Clusters are well-separated and compact

**Score Guide:**
- 0.7 - 1.0: Strong, well-defined clusters ✅
- 0.5 - 0.7: Reasonable clusters
- 0.25 - 0.5: Weak clusters
- < 0.25: Poor clustering

---

#### Clustering Visualization

The PCA plot shows:
- **X-axis (PCA 1):** First principal component (highest variance)
- **Y-axis (PCA 2):** Second principal component
- **Colors:** Different clusters
  - Cluster 0 (Red): Main service concentration
  - Cluster 1 (Blue): Distinct pattern/outlier

**How to read:**
- Points close together = similar programs
- Points far apart = different characteristics
- Clear separation = well-defined clusters

---

## 🔧 Customizing the Analysis

### Modify Features for Clustering

**Current features:**
```python
features_unsup = ['LATITUDE', 'LONGITUDE', 'WEEKLY_HOURS']
```

**Try adding:**
```python
# Example: Add borough as numeric feature
df['BOROUGH_NUM'] = df['BOROUGH'].map({
    'Brooklyn': 1, 
    'Bronx': 2, 
    'Queens': 3, 
    'Manhattan': 4, 
    'Staten Island': 5
})

features_unsup = ['LATITUDE', 'LONGITUDE', 'WEEKLY_HOURS', 'BOROUGH_NUM']
```

---

### Change Number of Clusters

**Find optimal K manually:**
```python
# Test different K values
for k in range(2, 11):
    kmeans = KMeans(n_clusters=k, random_state=42)
    labels = kmeans.fit_predict(X_scaled_unsup)
    score = silhouette_score(X_scaled_unsup, labels)
    print(f"K={k}: Silhouette Score = {score:.3f}")
```

**Force specific K:**
```python
# Use 3 clusters instead of optimal
kmeans_final = KMeans(n_clusters=3, random_state=42)
```

---

### Add New Visualizations

#### Scatter Plot by Borough
```python
import matplotlib.pyplot as plt

plt.figure(figsize=(10, 6))
for borough in df['BOROUGH'].unique():
    subset = df[df['BOROUGH'] == borough]
    plt.scatter(subset['LONGITUDE'], subset['LATITUDE'], 
                label=borough, alpha=0.7)
plt.xlabel('Longitude')
plt.ylabel('Latitude')
plt.title('Service Distribution by Borough')
plt.legend()
plt.show()
```

#### Weekly Hours Distribution
```python
import seaborn as sns

plt.figure(figsize=(10, 6))
sns.boxplot(data=df, x='BOROUGH', y='WEEKLY_HOURS')
plt.xticks(rotation=45)
plt.title('Weekly Operating Hours by Borough')
plt.ylabel('Hours')
plt.show()
```

---

## 🐛 Troubleshooting

### Issue 1: "File not found" Error

**Error:**
```
FileNotFoundError: [Errno 2] No such file or directory: 'ACS_Community_Partners_20250708.csv'
```

**Solution:**
```python
# Check current directory
import os
print(os.getcwd())

# Update file path
df = pd.read_csv("../data/ACS_Community_Partners_20250708.csv")
# OR use absolute path
df = pd.read_csv("/full/path/to/data/ACS_Community_Partners_20250708.csv")
```

---

### Issue 2: Module Not Found

**Error:**
```
ModuleNotFoundError: No module named 'sklearn'
```

**Solution:**
```bash
pip install scikit-learn
# OR install all requirements
pip install -r requirements.txt
```

---

### Issue 3: Deprecation Warning

**Warning:**
```
multi_class='multinomial' is deprecated
```

**Solution:**
```python
# Remove multi_class parameter (it's now default)
log_model = LogisticRegression(solver='lbfgs', max_iter=1000)
```

---

### Issue 4: Low Silhouette Score

**If you get score < 0.5:**

**Possible causes:**
1. Data not scaled properly
2. Wrong features selected
3. Inappropriate K value

**Try:**
```python
# Ensure scaling
from sklearn.preprocessing import StandardScaler
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)

# Test different feature combinations
features_v1 = ['LATITUDE', 'LONGITUDE']
features_v2 = ['LATITUDE', 'LONGITUDE', 'WEEKLY_HOURS']
features_v3 = ['WEEKLY_HOURS', 'BOROUGH_ENC']
```

---

## 📈 Advanced Analysis Ideas

### 1. Service Coverage Analysis
```python
# Calculate distance to nearest service
from scipy.spatial.distance import cdist

# Get service locations
service_coords = df[['LATITUDE', 'LONGITUDE']].values

# Define grid of NYC
lat_range = np.linspace(40.5, 40.9, 50)
lon_range = np.linspace(-74.3, -73.7, 50)
grid = np.array([(lat, lon) for lat in lat_range for lon in lon_range])

# Find distance to nearest service for each grid point
distances = cdist(grid, service_coords).min(axis=1)

# Visualize coverage
plt.scatter(grid[:, 1], grid[:, 0], c=distances, cmap='RdYlGn_r', s=20)
plt.colorbar(label='Distance to Nearest Service')
plt.title('Service Coverage Map')
plt.show()
```

---

### 2. Time-Based Analysis
```python
# Which days have most services open?
days = ['MON', 'TUES', 'WED', 'THURS', 'FRI']
day_counts = []

for day in days:
    from_col = f'OFFICE_{day}_FROM_TIME'
    # Count non-null values
    day_counts.append(df[from_col].notna().sum())

plt.bar(days, day_counts)
plt.title('Service Availability by Day')
plt.ylabel('Number of Programs Open')
plt.show()
```

---

### 3. Agency Analysis
```python
# Top agencies by number of programs
top_agencies = df['AGENCY_NAME'].value_counts().head(10)

plt.figure(figsize=(12, 6))
top_agencies.plot(kind='barh')
plt.title('Top 10 Agencies by Number of Programs')
plt.xlabel('Number of Programs')
plt.tight_layout()
plt.show()
```

---

## 💡 Tips for Better Results

### Data Quality
✅ **Remove null values** in critical columns  
✅ **Check for duplicates** before analysis  
✅ **Validate coordinates** (should be within NYC bounds)  
✅ **Standardize time formats** for consistency  

### Feature Engineering
✅ **Create meaningful features:**
- Distance from city center
- Borough population density
- Service type categories
- Program age/establishment date

### Model Tuning
✅ **Try different algorithms:**
```python
from sklearn.ensemble import RandomForestClassifier
from sklearn.svm import SVC
from sklearn.neighbors import KNeighborsClassifier
```

✅ **Use GridSearchCV** for hyperparameter tuning:
```python
from sklearn.model_selection import GridSearchCV

param_grid = {
    'C': [0.1, 1, 10],
    'max_iter': [100, 500, 1000]
}

grid = GridSearchCV(LogisticRegression(), param_grid, cv=5)
grid.fit(X_train, y_train)
print(f"Best params: {grid.best_params_}")
```

---

## 📚 Further Learning Resources

### Scikit-learn Documentation
- [Logistic Regression](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html)
- [K-Means Clustering](https://scikit-learn.org/stable/modules/generated/sklearn.cluster.KMeans.html)
- [PCA](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.PCA.html)

### Tutorials
- [Clustering Tutorial](https://scikit-learn.org/stable/modules/clustering.html)
- [Classification Metrics](https://scikit-learn.org/stable/modules/model_evaluation.html)
- [Cross-Validation](https://scikit-learn.org/stable/modules/cross_validation.html)

---

## 🎯 Expected Runtime

| Task | Estimated Time |
|------|----------------|
| Data Loading | < 1 second |
| Feature Engineering | < 2 seconds |
| Supervised Training | < 5 seconds |
| Cross-Validation | < 10 seconds |
| Clustering (K=2-10) | < 15 seconds |
| PCA & Visualization | < 3 seconds |
| **Total** | **< 40 seconds** |

*Times based on Google Colab with standard CPU runtime*

---

## ❓ FAQs

**Q: Can I use a different dataset?**  
A: Yes! Ensure it has:
- Geographic coordinates (latitude/longitude)
- Categorical target variable (like borough)
- Numerical features for clustering

**Q: How do I save the visualizations?**  
A: Add before `plt.show()`:
```python
plt.savefig('clustering_plot.png', dpi=300, bbox_inches='tight')
```

**Q: Can I export the clusters?**  
A: Yes:
```python
# Save clusters to CSV
df[['PROGRAM_NAME', 'BOROUGH', 'CLUSTER']].to_csv('clustered_programs.csv', index=False)
```

**Q: How do I cite this project?**  
A: See [CITATION.md](CITATION.md) for proper citation formats.

---

**Need More Help?**  
Open an issue on GitHub or contact: yav0051@my.londonmet.ac.uk
