# NYC Community Services Distribution Analysis

## 🎯 Project Overview

Machine learning and data mining analysis of community service distribution across New York City's five boroughs. This project employs both supervised and unsupervised learning techniques to identify patterns, disparities, and optimization opportunities in social service delivery.

**Student:** Yagnesh Vaghashiya (25002034)  
**Course:** CC7184 Data Mining and Machine Learning  
**Institution:** London Metropolitan University  
**Supervisor:** Dr. Subeksha Shrestha  
**Academic Period:** Summer Semester 2025/26

---

## 📊 Research Question

*How are community support services distributed across NYC boroughs, and what patterns emerge when analyzing geographical location, operating hours, and service availability?*

---

## 🔍 Key Findings

### Supervised Learning (Logistic Regression)
- **97% cross-validation accuracy** predicting borough from operational features
- Perfect classification on test set (100% precision, recall, F1-score)
- Standard deviation: 0.03 (highly consistent performance)

### Unsupervised Learning (K-Means Clustering)
- **Silhouette Score: 0.81** (excellent cluster separation)
- **Optimal K: 2 clusters** identified through silhouette analysis
- Strong geographical clustering patterns revealed
- Clear service distribution disparities across boroughs

### Borough Distribution
- **Brooklyn:** 49 programs (30%)
- **Bronx:** 40 programs (24%)
- **Queens:** 36 programs (22%)
- **Manhattan:** 28 programs (17%)
- **Staten Island:** 10 programs (6%)

**Key Insight:** Significant disparity in service availability, with Staten Island severely underserved.

---

## 🛠️ Technical Stack

### Programming & Tools
- **Python 3.x** - Primary programming language
- **Google Colab** - Development environment
- **Jupyter Notebooks** - Interactive analysis

### Libraries & Frameworks
```python
# Data Processing
pandas                  # Data manipulation
numpy                   # Numerical operations

# Machine Learning
scikit-learn           # ML algorithms & preprocessing
  - LogisticRegression # Supervised learning
  - KMeans             # Unsupervised clustering
  - PCA                # Dimensionality reduction
  - StandardScaler     # Feature scaling
  - LabelEncoder       # Categorical encoding

# Visualization
matplotlib.pyplot      # Base plotting
seaborn               # Statistical visualization
```

---

## 📁 Project Structure

```
nyc-community-services-analysis/
├── data/                                  # Dataset files
│   └── ACS_Community_Partners_20250708.csv
├── code/                                  # Python scripts
│   └── Code_Data_Mining.py
├── reports/                               # Academic documentation
│   └── 25002034_CC7184_Coursework_2025-26.pdf
├── visualizations/                        # Generated plots
│   ├── kmeans_clustering_pca.png
│   └── borough_distribution.png
├── documentation/                         # Additional docs
├── README.md                              # This file
├── requirements.txt                       # Python dependencies
├── .gitignore                            # Git configuration
└── LICENSE                               # MIT License
```

---

## 🔬 Methodology

### Framework: CRISP-DM (Cross-Industry Standard Process for Data Mining)

#### 1. **Business Understanding**
- Analyze community service distribution in NYC
- Identify underserved areas
- Support equitable resource allocation

#### 2. **Data Understanding**
- **Dataset:** NYC ACS Community Partners
- **Records:** 163 community programs
- **Features:** 30+ attributes including:
  - Program details (name, code, description)
  - Agency information
  - Geographic coordinates (latitude, longitude)
  - Operating hours (Mon-Fri)
  - Borough classification
  - Contact information

#### 3. **Data Preparation**
- **Null value handling:** Removed rows with missing BOROUGH, LATITUDE, LONGITUDE
- **Feature engineering:** 
  - Created `WEEKLY_HOURS` from daily operating schedules
  - Binned coordinates: `LAT_BIN`, `LON_BIN` (rounded to 2 decimals)
- **Encoding:** Label encoding for categorical variables (BOROUGH)
- **Scaling:** StandardScaler for feature normalization

#### 4. **Modeling**

##### Supervised Learning: Logistic Regression
**Objective:** Predict borough classification from operational features

**Features:**
- `LAT_BIN` - Binned latitude coordinates
- `LON_BIN` - Binned longitude coordinates
- `WEEKLY_HOURS` - Total weekly operating hours

**Target:** `BOROUGH_ENC` (encoded borough labels)

**Configuration:**
```python
LogisticRegression(
    multi_class='multinomial',
    solver='lbfgs',
    max_iter=1000
)
```

**Validation:**
- 70/30 train-test split
- 5-fold cross-validation
- Metrics: Precision, Recall, F1-Score, Confusion Matrix

##### Unsupervised Learning: K-Means Clustering
**Objective:** Discover natural groupings in service distribution

**Features:**
- `LATITUDE` - Exact latitude
- `LONGITUDE` - Exact longitude
- `WEEKLY_HOURS` - Weekly operating hours

**Process:**
1. Feature scaling with StandardScaler
2. Silhouette analysis for optimal K (tested K=2 to 10)
3. K-Means clustering with optimal K
4. PCA dimensionality reduction for 2D visualization

#### 5. **Evaluation**
- **Supervised:** Classification metrics, cross-validation scores
- **Unsupervised:** Silhouette score, cluster visualization
- **Business impact:** Service distribution analysis by borough

#### 6. **Deployment**
- Academic report with actionable insights
- Code shared on GitHub for reproducibility
- Recommendations for policymakers

---

## 📈 Results & Visualizations

### 1. K-Means Clustering (PCA Projection)
- **2 distinct clusters** identified
- Cluster 0 (Red): Main service concentration
- Cluster 1 (Blue): Outlier/distinct pattern
- **Silhouette Score: 0.81** (excellent separation)

### 2. Classification Performance
- **Training Accuracy:** 100%
- **Test Accuracy:** 100%
- **Cross-Validation:** 97% (±0.03)
- **All Classes:** Precision=1.00, Recall=1.00, F1=1.00

**Note:** Perfect accuracy suggests potential data leakage from coordinate features, which was acknowledged and addressed in analysis.

### 3. Service Distribution by Borough
Brooklyn leads with 49 programs, while Staten Island has only 10 programs - a nearly 5x disparity.

---

## 💡 Key Insights

### Geographic Patterns
✅ Strong spatial clustering - services group geographically  
✅ Clear borough boundaries reflected in clustering  
✅ Coordinate features highly predictive of borough  

### Service Availability
⚠️ **Major disparity:** Staten Island severely underserved (10 programs)  
⚠️ **Concentration:** Brooklyn has 5x more programs than Staten Island  
✅ **Bronx & Queens:** Relatively balanced distribution  

### Operational Patterns
✅ Weekly operating hours vary but show consistent patterns  
✅ Most programs operate standard business hours  
✅ Temporal features less predictive than spatial features  

---

## 🎯 Real-World Applications

### For Policymakers
- **Gap Analysis:** Identify underserved boroughs (e.g., Staten Island)
- **Resource Allocation:** Data-driven program expansion planning
- **Equity Metrics:** Quantifiable service distribution benchmarks

### For Non-Profit Organizations
- **Strategic Planning:** Where to establish new programs
- **Partnership Opportunities:** Identify service overlaps or gaps
- **Impact Assessment:** Measure reach across communities

### For Urban Planners
- **Accessibility Mapping:** Visualize service coverage
- **Transportation Planning:** Ensure transit connects to services
- **Community Development:** Align infrastructure with service needs

---

## 🚀 How to Use This Repository

### Prerequisites
```bash
Python 3.8+
pip (Python package manager)
```

### Installation

1. **Clone the repository:**
```bash
git clone https://github.com/YOUR_USERNAME/nyc-community-services-analysis.git
cd nyc-community-services-analysis
```

2. **Install dependencies:**
```bash
pip install -r requirements.txt
```

### Running the Analysis

**Option 1: Run Python Script**
```bash
cd code
python Code_Data_Mining.py
```

**Option 2: Google Colab (Recommended)**
1. Open [Google Colab](https://colab.research.google.com)
2. Upload `Code_Data_Mining.py`
3. Upload dataset from `data/ACS_Community_Partners_20250708.csv`
4. Run all cells

**Colab Link:** [View Project](https://colab.research.google.com/drive/1jAQoP7MqIDyUm6LvDRAMnOIrLlI60XxV)

### Output
- Classification report (precision, recall, F1-score)
- Confusion matrix
- Cross-validation scores
- K-Means clustering visualization (PCA)
- Silhouette score

---

## 📊 Dataset Information

### Source
**NYC Open Data** - ACS Community Partners  
**Filename:** `ACS_Community_Partners_20250708.csv`  
**Last Updated:** July 8, 2025

### Key Attributes

| Feature | Type | Description |
|---------|------|-------------|
| `PROGRAM_LST_ID` | Integer | Unique program identifier |
| `PROGRAM_CODE` | String | Program classification code |
| `PROGRAM_NAME` | String | Official program name |
| `AGENCY_NAME` | String | Managing agency |
| `BOROUGH` | Categorical | NYC borough (5 values) |
| `LATITUDE` | Float | Geographic coordinate |
| `LONGITUDE` | Float | Geographic coordinate |
| `OFFICE_MON_FROM_TIME` | String | Monday opening time |
| `OFFICE_MON_TO_TIME` | String | Monday closing time |
| *(similar for TUES-FRI)* | | Weekday operating hours |
| `ACTIVE_FLAG` | String | Program status (Y/N) |

### Data Quality
- **Total Records:** 163 programs
- **Missing Values:** Handled by dropping rows with null BOROUGH/coordinates
- **Duplicates:** None identified
- **Encoding:** UTF-8

---

## 🧪 Experimental Setup

### Hardware & Environment
- **Platform:** Google Colab (cloud-based)
- **Runtime:** Python 3.10
- **GPU:** Not utilized (CPU sufficient)
- **RAM:** 12GB allocated

### Model Hyperparameters

#### Logistic Regression
```python
multi_class='multinomial'    # Multinomial classification
solver='lbfgs'               # Limited-memory BFGS optimizer
max_iter=1000                # Maximum iterations
random_state=42              # Reproducibility
```

#### K-Means Clustering
```python
n_clusters=2                 # Optimal K from silhouette analysis
random_state=42              # Reproducibility
n_init=10                    # Number of initializations
```

#### PCA
```python
n_components=2               # 2D visualization
```

### Cross-Validation
- **Method:** 5-fold stratified cross-validation
- **Metric:** Accuracy
- **Results:** Mean=0.97, Std=0.03

---

## ⚠️ Limitations & Future Work

### Current Limitations

1. **Data Leakage Identified**
   - Exact coordinates directly predict borough
   - Perfect accuracy (100%) indicates overfitting
   - **Mitigation:** Used binned coordinates, acknowledged in report

2. **Temporal Features**
   - Only weekday hours analyzed
   - Weekend/holiday services not captured
   - Seasonal variations not considered

3. **Sample Size**
   - 163 programs is relatively small
   - Limited generalizability
   - Some boroughs have few samples (Staten Island: 10)

4. **Feature Engineering**
   - Additional features could improve model:
     - Service type/category
     - Program capacity
     - Demographics of service area

### Future Enhancements

#### Short-Term (Next 3 months)
- [ ] Add service type classification
- [ ] Include demographic data (census integration)
- [ ] Perform time-series analysis on program growth
- [ ] Create interactive dashboard (Plotly/Dash)

#### Medium-Term (6-12 months)
- [ ] Deep learning models (Neural Networks)
- [ ] Network analysis of service overlaps
- [ ] Predictive modeling for service demand
- [ ] Natural Language Processing on program descriptions

#### Long-Term (1+ years)
- [ ] Real-time service availability prediction
- [ ] Integration with transportation data
- [ ] Multi-city comparative analysis
- [ ] Recommendation system for service seekers

---

## 🎓 Academic Context

### Learning Objectives Achieved

✅ **Data Mining Techniques**
- Feature engineering and transformation
- Handling missing data and outliers
- Exploratory data analysis (EDA)

✅ **Supervised Learning**
- Logistic regression for multi-class classification
- Model evaluation metrics
- Cross-validation techniques

✅ **Unsupervised Learning**
- K-Means clustering algorithm
- Silhouette analysis for optimal K
- PCA for dimensionality reduction

✅ **CRISP-DM Methodology**
- Systematic data mining process
- Business understanding to deployment
- Iterative refinement

✅ **Professional Reporting**
- Academic paper (IEEE style)
- Code documentation
- GitHub repository management

---

## 📚 References

[1] Luo, Y., Bag, S., et al. (2022). MOF synthesis prediction enabled by automatic data mining and machine learning. *Angewandte Chemie International Edition*, 61(19), e202200242.

[2] Lukita, C., Bakti, L.D., et al. (2023). Predictive and analytics using data mining and machine learning for customer churn prediction. *Journal of Applied Data Sciences*, 4(4), 454-465.

[3] Taranto-Vera, G., Galindo-Villardón, P., et al. (2021). Algorithms and software for data mining and machine learning: A critical comparative view. *The Journal of Supercomputing*, 77, 11481-11513.

[4] Song, Y., & Wu, R. (2022). The impact of financial enterprises' excessive financialization risk assessment. *Computational Economics*, 60(4), 1245-1267.

[5] Ge, Q., Lu, X., et al. (2024). Data mining and machine learning in HIV infection risk research. *Artificial Intelligence in Medicine*, p.102887.

*Full references available in the academic report PDF.*

---

## 🤝 Contributing

This is an academic project completed for coursework. However, suggestions and feedback are welcome!

**To contribute:**
1. Fork the repository
2. Create a feature branch (`git checkout -b feature/improvement`)
3. Commit changes (`git commit -m 'Add improvement'`)
4. Push to branch (`git push origin feature/improvement`)
5. Open a Pull Request

---

## 📄 License

This project is licensed under the **MIT License** - see [LICENSE](LICENSE) file for details.

**Data License:** NYC Open Data is subject to the Open Data Law. Please acknowledge:
> "Data provided by NYC Open Data under the Open Data Law"

---

## 📞 Contact

**Yagnesh Vaghashiya**  
Student ID: 25002034  
London Metropolitan University  
Email: yav0051@my.londonmet.ac.uk

**Supervisor:**  
Dr. Subeksha Shrestha  
London Metropolitan University

---

## 🌟 Acknowledgments

- **Dr. Subeksha Shrestha** - Course supervision and guidance
- **NYC Open Data** - Dataset provision
- **London Metropolitan University** - Academic support
- **Google Colab** - Cloud computing platform
- **Scikit-learn Community** - ML library development

---

## 📊 Repository Statistics

![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)
![License](https://img.shields.io/badge/License-MIT-green.svg)
![Status](https://img.shields.io/badge/Status-Complete-success.svg)
![Course](https://img.shields.io/badge/Course-CC7184-orange.svg)

---

**Last Updated:** May 2026  
**Project Status:** ✅ Complete and Submitted

---

*This project demonstrates practical application of data mining and machine learning techniques to real-world urban service distribution challenges, with actionable insights for policymakers and community organizations.*
