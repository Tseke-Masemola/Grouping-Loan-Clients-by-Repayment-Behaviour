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
</details>

## 🔬  Analysis

### 1. Exploratory Data Analysis (EDA)
We collected data on 200 Crestline Bank clients to understand their repayment behavior. We focused on
3 key variables: 
-   Prior Default status
-   Number of Late Payments
-   Loan Amount. 

Below are the
exploratory plots and our observations:


<details>
<summary><b>EDA</b></summary>


<div align="center">
  <img width="30%" alt="EDA: Late Payments" src="https://github.com/user-attachments/assets/52187f9a-3f8c-4c21-acbb-b34e6cde5011" />
  <img width="30%" alt="Silhouette Analysis" src="https://github.com/user-attachments/assets/6dbf5439-edf3-4291-801c-f92155fc5adb" />
  <img width="30%" alt="Final Clusters" src="https://github.com/user-attachments/assets/50394de0-66d8-47fd-98b5-aff185f15d7b" />
</div>


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
<summary><b>View complete cluster selection methodology...</b></summary>

#### Approach: K-means Clustering

We applied **K-means clustering** to uncover natural groupings among the bank's clients and understand how the three key factors—prior default history, number of late payments, and loan amount—interact with overall credit risk. K-means partitions the feature space into *K* distinct clusters, with each cluster representing a different combination or "profile" of these characteristics.

Our exploratory data analysis suggested the presence of **two broad groups**, driven mainly by whether a client has previously defaulted. To validate and refine this hypothesis, we used two complementary methods:

#### **Elbow Method Analysis**
The elbow method evaluates the **objective function**, which measures the within-cluster variation (how tightly grouped the observations inside each cluster are). By plotting this value across several choices of *K*, we look for the point where adding more clusters yields diminishing returns—the "elbow" where the objective function stops decreasing substantially.

**Our Findings from the Elbow Plot:**
- Significant reductions in the objective function occur from 2 to 4 groups
- Beyond 4 groups (K = 4), the reductions in the objective function are insignificant
- Selecting K > 4 would result in an unnecessarily complex model

#### **Silhouette Score Analysis**
The silhouette method assesses how well each individual observation is assigned under different values of *K*. It does this by comparing the observation's distance to its own cluster with its distance to the nearest alternative cluster.

**Score Interpretation:**
- **Score close to 1**: Point clearly belongs where it was placed
- **Score close to 0**: Point sits between two clusters and could have gone either way

**Our Findings from Silhouette Analysis:**
- The silhouette average is highest (closest to 1) for K = 2, suggesting 2 groups are statistically optimal
- This initially contradicted the elbow plot results, which suggested K = 4

#### **Deep Analysis: K=2 vs. K=4 Comparison**
We conducted a deeper analysis of individual cluster performance under both scenarios:

**For K = 4:**
- Three of the clusters are relatively poorly assigned compared to one
- The third cluster is particularly bad in terms of silhouette scores

**For K = 2:**
- Silhouette scores indicate better assignment overall
- The data naturally forms two major groups as suggested by EDA
- These groups correspond to:
  - **High loan amounts, few late payments, and mostly no prior default**
  - **Low loan amounts, many late payments, and a history of prior default**

#### **The K=4 Decision: Business Utility Over Statistical Purity**
While silhouette analysis suggested K=2 was statistically optimal, we selected **K=4** for critical business reasons:

1. **Business Utility vs. Statistical Fit**: K=2 was too coarse, forcing all clients into only two categories ("risky" vs. "safe"). This would lead to:
   - Loss of good business opportunities by not distinguishing between degrees of risk
   - Inability to implement tiered pricing strategies
   - Oversimplification that ignores important risk gradations

2. **Actionable Segmentation**: By allowing for four clusters, we obtained a more refined ranking structure that:
   - Gives each client a meaningful risk "grade" (A-D) based on their characteristics
   - Provides enough detail for flexible loan amount and interest rate setting
   - Remains simple enough to implement without becoming overly complex

3. **Balanced Solution**: K=4 represents the optimal balance where:
   - The elbow plot shows diminishing returns beyond this point
   - Business needs for tiered risk assessment are fully met
   - Operational simplicity is maintained compared to the bank's existing complex system

**Final Decision**: We prioritized **business utility** over pure statistical optimization. The K=4 solution provides clear risk gradation for differential pricing while maintaining practical simplicity—exactly what Crestline Bank needs to replace its overly complex rating system.
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

| File | Description | Download |
|------|-------------|----------|
| Complete Analysis | Full 10-page PDF report with all findings | [📥 Detailed Report](./Final-Grouping-Loan-Clients-by-Repayment-Behavior.pdf) |
| Rmarkdown File | Complete code | [📥 Complete Code](./Executive-Summary.pdf) |
| Dataset | Client data (CSV format) | [📥 Crestline Bank Dataset](./Crestline_Bank_Clients_Dataset.txt) |

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
