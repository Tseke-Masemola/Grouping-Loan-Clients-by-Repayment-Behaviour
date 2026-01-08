# Loan Client Grouping by Repayment Behavior

## 📊 Business Problem

Crestline Bank's current credit rating system uses overly detailed and complex rating levels (AAA, AA+, AA, A-, B+, etc.) that create operational inefficiencies. The system causes two main problems:

<details>
<summary><b>Two main problems</b></summary>

1. **Confusing for Staff & Costly**: With numerous subtle rating distinctions, bank staff struggle to align interest rates and loan amounts with actual client risk. This leads to potential revenue loss through:
   - Offering low rates to risky clients (increasing default losses)
   - Charging high rates to safe clients (losing profitable business)

2. **Wastes Time and Resources**: Significant resources are spent maintaining and explaining minor rating differences that don't meaningfully impact risk assessment or loan management decisions.
</details>

## 🎯 Objective

1. To **increase revenue and decrease losses** for Crestline Bank by reshaping how we assess client creditworthiness.
2. Replace the current complex rating framework with a **simpler, practical way to group clients by creditworthiness**.

## 📈 Executive Summary & Key Insights

Crestline Bank's clients naturally separate into **4 distinct risk profiles** based on 3 key behavioral variables: 
  - Prior Default status
  - Number of Late Payments
  - Loan Amount

<details>
<summary><b>Key insights</b></summary>

### 🔑 Key Insights:
1. **5+ late payments** is a strong warning sign of potential default
2. **Prior default history** makes a client significantly riskier (consistently linked to more late payments and smaller loan requests)
3. **4 clusters (A-D)** provide the optimal balance between statistical fit and business utility—better than the statistically optimal 2-cluster solution
4. **Good repayment habits** (few late payments) correlate with clients requesting larger loan amounts, suggesting trust-building behavior

### 🎯 Business Impact:
This 4-tier system simplifies decision-making while maintaining risk sensitivity, enabling:
- More accurate interest rate pricing
- Better loan amount decisions  
- Reduced operational complexity
- Improved client relationship management
</details>

## 🔬  Analysis

### 1. Exploratory Data Analysis (EDA)
We collected data on 200 Crestline Bank clients to understand their repayment behavior. We focused on
three key variables: Prior Default status, Number of Late Payments, and Loan Amount. Below are the
exploratory plots and our observations:


<details>
<summary><b>EDA</b></summary>

**Finding**: Prior defaulters have significantly more late payments than non-defaulters.

**Evidence**:
*Add 3 boxplots*

- **Late Payments vs. Prior Default:** Clients with a prior default have significantly more late payments
(mean ~6) than those without (mean ~2). The number of late payments acts as a trust barometer:
clients with 5 or more late payments are very likely to have defaulted before. This shows a clear
pattern: past defaulters are consistently late payers.
- **Loan Amount vs. Prior Default:** Prior defaulters consistently request smaller loan amounts.
Clients without a prior default request a wider range of, and generally much larger, loan amounts.
This makes practical sense: if a client has a high risk of default, the bank would approve a smaller loan
to limit potential losses, and the client may also lack the confidence to ask for more.
- **Late Payments vs. Loan Amount:** Clients with fewer late payments tend to request larger loan
amounts. This suggests that good repayment habits build trust, which allows clients to confidently
request, and the bank to consider approving, larger loans.

Clearly, the plots tell a consistent story. Before any further analysis, 2 key risk drivers are:
1. having 5+ late payments is a strong warning sign of potential default.
2. a prior default history makes a client significantly riskier now.

</details>

### 2. Determining Optimal Clusters (K=4)

<details>
<summary><b>View cluster selection rationale and evidence...</b></summary>

**Finding**: Silhouette analysis favored K=2 for statistical purity, but business requirements justified K=4 for actionable risk tiers.

**Evidence**:
![Silhouette Comparison](images/silhouette_comparison.png) *Side-by-side silhouette plots showing K=4 vs. K=2*

**Statistical Evaluation**:
- **Elbow Method**: Showed diminishing returns beyond K=4
- **Silhouette Scores**: Highest average score at K=2 (0.61) vs. K=4 (0.52)
- **Business Utility**: K=2 was too coarse (only "risky" vs. "safe"), losing important risk gradations needed for pricing

**What it means**: We prioritized business utility over pure statistical optimization. The K=4 solution provides:
- Clear risk gradation for differential pricing
- Actionable segments for relationship management
- Practical simplicity over the bank's existing complex system
</details>

### 3. Final Cluster Profiles

<details>
<summary><b>View detailed cluster characteristics and risk grades...</b></summary>

| Cluster | Risk Grade | Prior Default Rate | Avg. Late Payments | Avg. Loan Amount | Key Characteristics |
|---------|------------|-------------------|-------------------|------------------|-------------------|
| **Cluster 1** | **A** | 0% | 1.2 | $45,200 | Excellent repayment discipline, high loan amounts, no negative history |
| **Cluster 2** | **B** | 5% | 3.4 | $32,100 | Strong repayment behavior, minor late payments, generally safe |
| **Cluster 3** | **C** | 38% | 6.7 | $18,500 | Moderate risk, several late payments, requires monitoring |
| **Cluster 4** | **D** | 92% | 9.1 | $8,300 | High risk, poor repayment habits, prior defaults, small loans |

**Interpretation**:
- **A-grade**: "Prime" clients deserving preferential rates and higher limits
- **B-grade**: "Standard" clients with generally good behavior  
- **C-grade**: "Watchlist" clients needing careful management and potentially higher rates
- **D-grade**: "High-risk" clients requiring restrictive terms or rejection

**Visual Representation**:
![K-means Clustering Results](images/final_clusters.png) *Four distinct client groups with clear separation in late payments and loan amounts*
</details>

## 💡 Strategic Recommendations for Crestline Bank

<details>
<summary><b>View complete implementation framework...</b></summary>

### New Credit Decision Framework:

| Risk Grade | Client Profile | Recommended Action | Loan Policy Guidelines |
|------------|----------------|-------------------|------------------------|
| **A** | Excellent repayment, high trust | Nurture & retain | **Lowest rates**, highest limits, automated approval |
| **B** | Generally reliable with minor issues | Encourage & grow | **Competitive rates**, standard limits, fast-track approval |
| **C** | Moderate risk, needs monitoring | Manage & review | **Higher rates**, reduced limits, manual review required |
| **D** | High risk, poor repayment history | Restrict or decline | **Highest rates** (if approved), minimal limits, senior approval required |

### Implementation Roadmap:
1. **Phase 1 (0-3 months)**: Pilot the 4-tier system with new loan applications
2. **Phase 2 (3-6 months)**: Reclassify existing portfolio using new framework
3. **Phase 3 (6-12 months)**: Integrate with automated decisioning systems
4. **Phase 4 (Ongoing)**: Quarterly review and recalibration of cluster boundaries

### Expected Benefits:
- **15-20% reduction** in credit decision time
- **10-15% improvement** in risk-based pricing accuracy
- **Simplified training** for new staff (4 tiers vs. 10+ ratings)
- **Clearer client communication** about their credit standing
</details>

## 📁 Project Files

<details>
<summary><b>View file details and descriptions...</b></summary>

| File | Description | Format |
|------|-------------|--------|
| `Final_Grouping_Loan_Clients.Rmd` | Complete R Markdown analysis with all code, visualizations, and narrative | R Markdown |
| `Crestline_Bank_Clients_Dataset.txt` | Sample dataset containing 200 clients with PriorDefault, NumLatePayments, and LoanAmount variables | CSV (comma-separated) |
| `README.md` | This project documentation file | Markdown |
| `Crestline Bank Logo.png` | Bank logo used in report | PNG image |

**Note**: The dataset contains synthetic data created for this analysis to demonstrate the methodology while protecting real client confidentiality.
</details>

## 🔧 How to Reproduce

<details>
<summary><b>View detailed reproduction steps and system requirements...</b></summary>

### System Requirements:
- **R** (version 4.0 or higher)
- **RStudio** (recommended) or any R IDE
- **Internet connection** (for package installation)

### Step-by-Step Instructions:

1. **Clone or download** the project repository to your local machine
2. **Open RStudio** and set the project directory as your working directory
3. **Install required packages** (if not already installed):
   ```r
   install.packages("dplyr")
