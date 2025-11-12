Medicare Chronic Condition & Provider Cost Analysis (2009)

Project Overview

This project performs an in-depth analysis of 2009 Medicare beneficiary and outpatient claims data. The primary goals are to understand the prevalence and cost associated with various chronic conditions, analyze demographic distributions, and benchmark healthcare provider costs relative to the conditions they treat.

The analysis identifies common combinations of chronic illnesses, examines which conditions drive the highest total and per-member costs, and pinpoints providers who are consistently more expensive than the average for the specific conditions they manage.

Dataset

The analysis is based on two sample CSV files:

DE1_0_2009_Beneficiary_Summary_File_Sample_20.csv: Contains demographic information and chronic condition flags for Medicare beneficiaries in 2009.

DE1_0_2008_to_2010_Outpatient_Claims_Sample_20.csv: Contains individual outpatient claims records for beneficiaries from 2008 to 2010.

Project Workflow

Data Loading & Cleaning:

Loaded the beneficiary summary and outpatient claims datasets.

Handled missing values, primarily by dropping rows with null claim dates, while retaining records with a null death date (as these represent living beneficiaries).

Converted relevant date columns (BENE_BIRTH_DT, BENE_DEATH_DT, CLM_FROM_DT, CLM_THRU_DT) to datetime objects for proper handling.

Dropped irrelevant financial columns (e.g., Inpatient and Carrier reimbursement columns) to focus the analysis purely on outpatient data.

Data Filtering:

Filtered the df_claims dataset to include only records from the year 2009. This aligns the claims data temporally with the 2009_Beneficiary_Summary file.

Feature Engineering:

Renamed the cryptic chronic condition columns (e.g., SP_ALZHDMTA) to human-readable names (e.g., Alzheimer).

Created a new Chronic_Illness_Combination column in the beneficiary data. This column concatenates the names of all chronic conditions a beneficiary has (where the flag is 1).

To simplify analysis, this combination was further categorized: if a beneficiary has three or more conditions, they were grouped into a "Multiple" category.

Data Merging:

Performed an inner join on the cleaned 2009 beneficiary data and the filtered 2009 claims data using DESYNPUF_ID as the key. This created the final analytical dataset.

Analysis & Key Findings

The analysis was divided into demographic summaries and provider benchmarking. For most analyses, beneficiaries with "Multiple" conditions or no listed conditions were filtered out to focus on specific 1- and 2-condition combinations.

1. Patient Demographics & Condition Prevalence

Race Distribution: A pie chart was generated to show the racial breakdown of the beneficiary population. The majority of members in this sample are White, followed by Black, Other, and Hispanic.

Chronic Condition Prevalence: A bar chart identified the "Top 10 Most Common Chronic Illness Combinations." "Heart Disease" as a single condition was the most prevalent, followed by the "Diabetes, Heart Disease" combination.

2. Chronic Illness Cost Analysis

A dual-axis cost analysis was performed to understand which conditions are most expensive.

Total Expenditure: Identified which conditions drive the highest overall spending. "Heart Disease" had the highest total CLM_PMT_AMT.

Cost per Member: Identified which conditions are most expensive on a per-patient basis. Combinations like "Arthritis, Stroke" and "Cancer, COPD" emerged as having the highest average cost per beneficiary, even if their total cost was not in the top 10.

3. Provider Benchmarking

This analysis aimed to identify providers with consistently high costs relative to the conditions they treat.

Provider-Condition Cost: Calculated the cost_per_member for each unique AT_PHYSN_NPI (Attending Physician) and Chronic_Illness_Combination.

Cost Variability: Calculated the mean and standard deviation of cost_per_member for each condition across all providers. This identified treatments with high cost variability (e.g., "Arthritis, Stroke").

Data Refinement: The analysis was refined by filtering out provider-condition pairs with only one patient. This ensures that the averages are more stable and not skewed by single, high-cost claims.

Provider "Expensiveness" Metric:

Calculated the average cost_per_member for each illness across all providers.

Created a cost_difference_from_average metric for each provider (Provider's Cost - Illness Average).

Averaged this "cost difference" for each provider across all conditions they treat.

Final Result: Identified and plotted the "Top 10 Consistently Expensive Providers"—those with the highest positive average cost difference, indicating they consistently charge more than the norm for the conditions they handle.

Tools Used

Pandas: For data loading, cleaning, manipulation, and aggregation.

NumPy: For numerical operations.

Matplotlib & Seaborn: For data visualization (pie charts, bar plots).
