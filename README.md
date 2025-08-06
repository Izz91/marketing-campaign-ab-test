# marketing-campaign-ab-test
# Evaluating Campaign Performance Through A/B Testing
 
## Objective
We evaluates the performance of two advertising campaigns, referred to as the Control Campaign and the Test Campaign. The objective is to determine if the Test Campaign significantly impacts conversion rates compared to the Control Campaign.

## Data Source and Preprocessing

The data used in this analysis is sourced from the 'marketing_campaign.csv' file found in Kaggle input directories.
 
     import kagglehub

     path = kagglehub.dataset_download("amirmotefaker/ab-testing-dataset")

     print("Path to dataset files:", path)

The pre-processing data involved:
- cleaning the data, 
- standardizing column names (e.g., to num_impressions, num_purchase),
-  handling missing values through appropriate imputation, 
- and converting date formats for temporal analysis. 

A binary converted metric is then derived from purchase data to serve as the primary Key Performance Indicator (KPI). Following data preparation, we segmented the dataset  into the respective Control and Test groups for comparative analysis.
 
The next critical step involved Distributional Analysis of key metrics. We paid attention to the *um_impressions* metric. Through the examination of histograms, QQ plots, and kurtosis values, we determined that the distribution of *num_impressions* is **highly skewed** and does not follow normal distribution. This finding is crucial as it allows the use of statistical methods that are robust to on-normal data for comparison of this metric.

<img width="702" height="547" alt="Image" src="https://github.com/user-attachments/assets/9cb91db5-dae2-4996-91e2-bdec476a4452" />

 *The right-skewed histogram all consistently demonstrate that the 'num_impressions' metric does not follow a normal distribution. This reinforces the non-parametric methods such as the Mann-Whitney U test is appropriate for the distributions comparison of this metric between the groups.*

<img width="609" height="470" alt="Image" src="https://github.com/user-attachments/assets/54f13476-75a4-41e6-851c-7b119ccc736d" />
                  
*A 'converted' is a binary variable (True/False) which is represented by a Bernoulli distribution. This is inherently non-Gaussian. While individual observations are binary, the *proportion* of conversions across a large sample size can be approximated by a normal distribution due to the Central Limit Theorem, allowing for parametric tests like the Z-test.* 



## Methodology

The primary focus of the A/B test is the Conversion Rate. The hypothesis framework established as:
- **Null Hypothesis (H0):** The conversion rate of the Test group is less than or equal to the Control group.
- **Alternative Hypothesis (H1):** The conversion rate of the Test group is greater than the Control group.

The A/B Testing Analysis employed multiple statistical approaches:
1. **Z-test for Proportions**
   A parametric test that is suitable for comparison of two proportions given sufficient sample size and observed events. We    applied to the *conversion rates*. While the Test group showed a slightly higher conversion rate, the calculated p-value from the one-tailed Z-test is above the predefined significance level (alpha = 0.05). This leads us  to a **failure to reject** the null hypothesis.

2. **Mann-Whitney U Test**
   Given the non-normal distribution of *num_impressions*, the non-parametric Mann-Whitney U test is utilized to compare the distributions of impressions between the groups. This test revealed a **statistically significant difference** in the distribution of *num_impressions* (p-value < 0.05). This indicates that the **Test Campaign influenced** the pattern of impression delivery differently from the Control Campaign.

3. **Bootstrapping**
    To provide a robust, assumption-free assessment for the difference in conversion rates, we conduct a bootstrapping analysis. By resampling the data, we generate the distribution of the difference in conversion rate. The resulting 95% confidence interval for the difference included zero. The bootstrapped one-tailed p-value also indicated **no statistically significant difference in conversion rates** at the 0.05 significance level.

<img width="781" height="547" alt="Image" src="https://github.com/user-attachments/assets/bc8b8fed-ecd8-4568-b325-4bde37661a3f" />

*Fail to reject the Null Hypothesis. Bootstrapping analysis does not show a statistically significant difference on the test campaign has a lower or equal conversion rate than the control campaign at alpha=0.05.*

## Key Findings and Insights on Distributional Assumptions:
- **'num_impressions' Metric:** The distribution of 'num_impressions' is highly right-skewed and exhibits characteristics of a non-Gaussian, potentially power-law distribution. This means a small segment of users accounts for a disproportionately large number of impressions. For such metrics, traditional parametric tests assuming normality (like a t-test) would be inappropriate without transformations or large sample sizes via the CLT. The Mann-Whitney U test was used here as a robust non-parametric alternative, confirming a significant difference in the distribution of impressions between groups (p-value: 0.0003).
- **'converted' Metric (Conversion Rate):** As a binary outcome derived from 'num_purchase', 'converted' follows a Bernoulli distribution. However, for large sample sizes, the sampling distribution of the *proportion* of conversions approaches normality due to the Central Limit Theorem, justifying the use of a Z-test.
- **Z-test for Conversion Rates:** The test group achieved a conversion rate of 1.0000 compared to the control group's 0.9667. The one-tailed Z-test yielded a p-value of 0.1566. At a significance level of 0.05, we fail to reject the null hypothesis.
- **Bootstrapping for Conversion Rates:** This robust, assumption-free method provided a 95% confidence interval for the difference in conversion rates of (0.0000, 0.1000). The one-tailed bootstrapped p-value was 0.3623. This does not strongly support the conclusion of a higher conversion rate in the test group.

## Conclusion 

Based on the comprehensive statistical analysis employing the Z-test for proportions, Mann-Whitney U test, and bootstrapping, there is **insufficient statistical evidence to conclude that the Test Campaign resulted in a significantly higher conversion rate** compared to the Control Campaign at the 0.05 significance level.

While the Test Campaign did significantly alter the distribution of impressions, this **did not translate to a statistically significant improvement in the primary conversion metric** within the observed period and sample size.

## Recommendation:
-  Based on the A/B testing (Z-test and Bootstrapping), there is no strong evidence to suggest that the 'test' campaign leads to a statistically significant increase in conversion rates compared to the 'control' campaign.
- Therefore, it is **not recommended to roll out the 'test' campaign based solely on conversion rate improvement**. Consideration of other metrics (e.g., cost per acquisition, engagement metrics) or conducting a longer-duration test with a larger sample size may be needed to gain more definitive insights. 

## Final Thoughts
This project underscores the importance of understanding the underlying data distributions in A/B testing. Blindly applying parametric tests without checking for normality or considering alternative methods (like non-parametric tests or bootstrapping) can lead to wrong conclusions. 

