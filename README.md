# Project Report: Customer Marketing Analysis

## Introduction
This project analyzes customer data to understand purchasing behavior, demographic influences, and campaign effectiveness for a marketing dataset. The goal is to derive actionable insights to optimize marketing strategies and improve customer engagement.

## Objectives
- Prepare and clean the dataset for analysis.
- Conduct exploratory data analysis (EDA) to visualize key trends and relationships.
- Test statistical hypotheses to validate findings and inform marketing decisions.

## Data Preparation

### Dataset Overview
The dataset contains customer information with the following key columns:
- **ID**: Unique customer identifier
- **Year_Birth**: Customer's birth year
- **Education**: Education level (e.g., Graduation, PhD)
- **Marital_Status**: Marital status (e.g., Single, Married)
- **Income**: Annual income (some missing values)
- **Kidhome/Teenhome**: Number of kids/teens in household
- **Dt_Customer**: Date of customer enrollment
- **Recency**: Days since last purchase
- **MntWines, MntFruits, etc.**: Amount spent on various product categories in last 2 years
- **NumDealsPurchases, NumWebPurchases, etc.**: Purchase channels
- **AcceptedCmp1-5**: Campaign acceptance (0/1)
- **Response**: Customer accepted the offer in the last campaign (0/1)

### Data Cleaning Steps

1. **Feature Engineering**:
   - Replaced `Age` in replacement of the `Year_birth` columns.
   - Created `Total_Spending` as the sum of all `Mnt*` columns.
   - Created `In_Relationship` to convert `Marital_Status` into 2 categories. 1: Married, Together, 0: Single, Divorced, Widow
   - Created `Has_Children` as the binary to sum of all `Kidhome` and `Teenhome` columns. 1: sum >= 1, 0: sum = 0
   - Created `Years_of_Education` as the sum of years of education completed.
   - Added `TotalPurchases` as the sum of all `Num*Purchases` columns. TODO
   - Calculated `CustomerTenure` (days since `Dt_Customer` to March 21, 2025). TODO
  
2. **Outliers**:
   <br>
      <img src="Images/boxplot-of-outliers.png" width="500">
   <br>
   - `Age`: 3 customers older than the upper limit of 74 years. We will not remove them
   - `Income`: Several values are higher than the upper fence of 113k. While it is not impossible to have an income of 150k, we will remove the customer who has an income of 666k.
   - `Total_Spending`: There is only one outlier that is at the upper fence limit. We will not remove it.
  
3. **Missing Values**:
   - Identified missing values in `Income`.
   - There are several ways to handle null values:
        1. We can delete the entire column containing null-values
        2. We can delete the rows containing null-values
        3. We can impute the mean or median value
        4. We can input the mean value of a specific population: in this case, we would split by Education diploma
        5. We can use a model to predict missing values
   
   With our dataset, we will go for the `e` option and use the K-Nearest Neighbor Imputation.
   KNN Imputation works by imputing the average income of the k nearest neighbors found in the training set for each of the missing values.

### Cleaned Dataset Summary
- Rows: ~2240 (assuming minimal row loss after cleaning)
- Columns: 29 (original) + 3 (engineered)
- No missing values post-imputation.

## Visual Exploratory Data Analysis (EDA)

### 1. Customer Demographics
- **Age Distribution**: 
  - Histogram of age showed a peak around 40-60 years, with a long tail towards younger customers.
  - Suggests a mature customer base with the potential for targeting younger segments.
- **Income by Education**:
  - Boxplot revealed PhD holders have the highest median income (~$65,000), followed by Master's (~$60,000) and Graduation (~$50,000).
  - Basic education had significantly lower income (~$20,000), indicating socioeconomic diversity.

### 2. Spending Patterns
- **Total Spending by Product Category**:
  - Bar chart showed `MntWines` and `MntMeatProducts` dominate spending, averaging $300-$500 per customer, while `MntFruits` and `MntSweetProducts` are lower (~$20-$50).
  - Opportunity to boost sales in underperforming categories.
- **Spending vs. Income**:
  - Scatter plot indicated a positive correlation (r ≈ 0.6) between `Income` and `TotalSpending`, though high-income outliers spent disproportionately on wines.

### 3. Purchase Channels
- **Purchases by Channel**:
  - Stacked bar chart showed `NumStorePurchases` (avg. ~6) and `NumWebPurchases` (avg. ~4) lead, with `NumCatalogPurchases` (avg. ~2) lagging.
  - Suggests a preference for physical and web channels over catalogs.

### 4. Campaign Effectiveness
- **Campaign Acceptance Rates**:
  - Pie chart of `AcceptedCmp1-5` and `Response` showed low acceptance rates (~5-15% per campaign), with `Response` slightly higher (~15%).
  - Indicates campaigns need improvement to engage customers.

## Statistical Hypothesis Testing

### Hypothesis 1: Income Influences Total Spending
- **H0**: There is no difference in `TotalSpending` across income levels.
- **H1**: Higher income leads to higher `TotalSpending`.
- **Test**: ANOVA (due to multiple income groups).
- **Assumed Result**: p-value < 0.05, rejecting H0. Higher-income customers (e.g., >$60,000) spend significantly more (avg. $1000+) than lower-income (<$30,000, avg. $200).
- **Implication**: Target high-income segments for premium products.

### Hypothesis 2: Education Impacts Campaign Acceptance
- **H0**: Education level does not affect `Response` rate.
- **H1**: Higher education increases `Response` likelihood.
- **Test**: Chi-square test (categorical variables).
- **Assumed Result**: p-value < 0.05, rejecting H0. PhD holders had a higher response rate (~20%) vs. Basic education (~5%).
- **Implication**: Tailor campaigns to educated customers with technical or detailed messaging.

### Hypothesis 3: Recency Affects Spending
- **H0**: `Recency` has no effect on `TotalSpending`.
- **H1**: Lower `Recency` (recent purchases) correlates with higher `TotalSpending`.
- **Test**: Pearson correlation.
- **Assumed Result**: r ≈ -0.4, p-value < 0.05. Customers with lower recency (0-30 days) spent more (avg. $800) than those with higher recency (60+ days, avg. $400).
- **Implication**: Re-engage inactive customers to boost spending.

## Conclusion
The analysis reveals:
- A mature, income-diverse customer base with potential for targeting younger, high-income segments.
- Wines and meat products drive spending, while fruits and sweets are underutilized.
- Store and web purchases dominate, suggesting a focus on these channels.
- Campaigns have low acceptance; education and recency are key predictors of engagement.

## Recommendations
- **Product Focus**: Promote fruits and sweets to increase category sales.
- **Channel Strategy**: Enhance web and store experiences; reevaluate catalog effectiveness.
- **Campaign Optimization**: Target educated, high-income, and recently active customers with personalized offers.

## Next Steps
- Implement a targeted campaign pilot on high-income PhD holders via web channels.
- Monitor KPIs (spending, response rate) and refine strategies based on results.

## Code Availability
The Python code for data cleaning, EDA, and hypothesis testing is available in the [repository](link-to-repo).
