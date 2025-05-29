# AB_testing

## Experimentation Overview
At the time of this experiment, Udacity courses currently have two options on the course overview page: "start free trial", and "access course materials". If the student clicks "start free trial", they will be asked to enter their credit card information, and then they will be enrolled in a free trial for the paid version of the course. After 14 days, they will automatically be charged unless they cancel first. If the student clicks "access course materials", they will be able to view the videos and take the quizzes for free, but they will not receive coaching support or a verified certificate, and they will not submit their final project for feedback.

In the experiment, Udacity tested a change where if the student clicked "start free trial", they were asked how much time they had available to devote to the course. If the student indicated 5 or more hours per week, they would be taken through the checkout process as usual. If they indicated fewer than 5 hours per week, a message would appear indicating that Udacity courses usually require a greater time commitment for successful completion, and suggesting that the student might like to access the course materials for free. At this point, the student would have the option to continue enrolling in the free trial, or access the course materials for free instead. This screenshot shows what the experiment looks like.

The hypothesis was that this might set clearer expectations for students upfront, thus reducing the number of frustrated students who left the free trial because they didn't have enough time—without significantly reducing the number of students to continue past the free trial and eventually complete the course. If this hypothesis held true, Udacity could improve the overall student experience and improve coaches' capacity to support students who are likely to complete the course.

The unit of diversion is a cookie, although if the student enrolls in the free trial, they are tracked by user-id from that point forward. The same user-id cannot enroll in the free trial twice. For users that do not enroll, their user-id is not tracked in the experiment, even if they were signed in when they visited the course overview page

## Experiment Design
**Change tested**:  
- After clicking "Start free trial", students were asked:  
  **"How much time can you devote to the course?"**  
  - **≥5 hours/week**: Proceed to checkout as usual.  
  - **<5 hours/week**: Suggest free access instead.
  
**Hypothesis**:  
- Setting clearer expectations upfront would:  
  1. **Reduce frustration** among students who lack time.  
  2. **Retain enrollments** from committed students (≥5 hrs/week).


 ## Experiment Design Process
 1. Selecting Metrics
    - Invariant metrics
    - Evaluation metrics
 2. Calculate the Standard error of metrics
 3. Estimate the sample size for control and experiment group
 4. Determine the exposure of the experiment
 5. Perform Sanity Checks
 6. Analyze results & Provide recommendations

### 1. Selecting Metrics:
 **Invariant Metrics (Sanity Checks)**

 1. **Number of Cookies** :- number of unique cookies to view the course overview page
 2. **Number of Clicks** ("Start free trial") :- number of unique cookies to click the "Start free trial" button
 3. **Click-Through-Probability (CTP)**:- Number of Clicks/Number of Cookies
    
     Since the experiment triggers after the page loads, there should not be any significant difference in the above metrics between control and experiment group. Any siginifcant difference would indicate a trafffic imbalance issue.

 **Evaluation Metrics**

 1. **Gross Conversion** :- number of user-ids enroll in the free trial divided by number of unique cookies to click the "Start free trial" button
    
    Expected Direction : **Decrease**  
    If the change made performs as expected the number of user_ids enroll in the course should decrease. Because we expect only commited students to enroll.

 2. **Retention** :- number of user-ids to remain enrolled past the 14-day boundary divided by number of user-ids to complete checkout
    
    Expected Direction : **Increase**  
    The number of user_ids retained after 14 days in the course should increase. Because we know only commited students enrolled.

 3. **Net Conversion** :- number of user-ids to remain enrolled past the 14-day boundary divided by the number of unique cookies to click the "Start free trial" button
    
    Expected Direction : **Increase**  
    The net conversion should see an Increase, as we expect students to entroll and continue past 14 day free trail to increase over number of unique cookies to click the "Start free trial" button which is same for both groups

### 2. Calculate the Standard error of metrics:
 Based on the given baseline estimates for each of this metrics we can analytically calculate the standard error.  
 we can use Binomial proportion formula to estimate standard error because they are fundamentally binary outcomes (success/failure) with fixed trial counts, satisfying the core assumptions of binomial distributions.  

 $$ SE = \sqrt{\frac{p(1-p)}{n}} $$

 where:  
 - \( p \) = Baseline probability of success (e.g., 0.20 for 20% conversion)  
 - \( n \) = Sample size (e.g., number of clicks or enrollments)

| Metric                                                   | Baseline Value |
| --------------------------------------------------------- | -------------- |
| Unique cookies to view course overview page per day      | 40,000         |
| Unique cookies to click "Start free trial" per day        | 3,200          |
| Enrollments per day                                     | 660            |
| Click-through-probability on "Start free trial"          | 0.08           |
| Probability of enrolling, given click                   | 0.20625        |
| Probability of payment, given enroll                    | 0.53           |
| Probability of payment, given click                     | 0.1093125      |



 ### Standard Error Calculations for Evaluation Metrics
 
 | Metric          | Baseline Rate (p) | Sample Size (n) | Calculation                      | Standard Error |
 |-----------------|-------------------|-----------------|----------------------------------|----------------|
 | **Gross Conversion** | 0.20625          | 3,200 clicks    | $$\sqrt{\frac{0.20625 \times 0.79375}{3200}}$$ | 0.0071         |
 | **Net Conversion**   | 0.1093125        | 3,200 clicks    | $$\sqrt{\frac{0.1093125 \times 0.8906875}{3200}}$$ | 0.0055         |
 | **Retention**        | 0.53             | 660 enrollments | $$\sqrt{\frac{0.53 \times 0.47}{660}}$$         | 0.0194         |

 Note: When the sample size is large engough Analytical estimates of Standard Error would be sufficient, incase of small sample size estimating SE using Empirical estimates is suggested.  

### 3. Estimate the sample size for control and experiment groups
 Sample size estimation depends on several factors.
 1. Estimated SE from the previous step.
 2. Type I error rate **$\alpha$** or Significance level = 0.05
 3. Type II error rate **$\beta$** = 0.2 
 4. Minimum detectable change in the metric **d_min**


#### Determine Critical Z-value

 For a significance level of $\alpha$ = 0.05 (two-tailed test), calculate the critical Z-value:
 
 $Z_{\alpha/2} = 1.96$
 
 This defines the rejection region for the null hypothesis.

#### Iterative Sample Size Estimation

 Evaluate sample sizes ($N$) in the range [0, 100,000] to find the minimum $N$ where:
 
 $\beta(N) \leq 0.2$ (Power $\geq 80\%$)
  
  ![Alt text](https://github.com/sindla97/AB_testing/blob/main/Image.jpeg) 
 
 Here, $\beta$ represents the Type II error rate, computed as:
 
 $\beta = P(\text{Fail to reject } H_0 \mid \text{True effect} = d_{min})$
 
 where $d_{min}$ is the minimum detectable effect size of practical significance.

#### Statistical Interpretation

 $\beta$ is derived from the cumulative distribution function (CDF) of the effect's sampling distribution, integrated from $-\infty$ to $d_{min}$. The constraint $d_{min} > \alpha$ ensures the effect size exceeds statistical noise.

 ```python
 def get_z_star(alpha):
    """Returns the critical z-value for a two-tailed test."""
    return -norm.ppf(alpha / 2)

 def get_beta(z_star, s, d_min, N):
     """Calculates the Type II error rate (beta) for a given sample size."""
     SE = s / np.sqrt(N)
     return norm.cdf(z_star * SE, loc=d_min, scale=SE)

 def required_size(s, d_min, Ns=range(1, 100000), alpha=0.05, beta=0.2):
     """Finds the smallest N where beta <= desired threshold."""
     z_star = get_z_star(alpha)
     for N in Ns:
         if get_beta(z_star, s, d_min, N) <= beta:
             return N
     return -1
 ```
 Using the above calculation we determine the N_clicks_gross, N_clicks_net based on the s_gross and s_net
 ```   
 s_gross = np.sqrt(p_gross * (1 - p_gross) * 2)  # Pooled SE for two groups
 s_net = np.sqrt(p_net * (1 - p_net) * 2)
 
 # Calculate required clicks per group
 N_clicks_gross = required_size(s=s_gross, d_min=d_min_gross, alpha=alpha, beta=beta)
 N_clicks_net = required_size(s=s_net, d_min=d_min_net, alpha=alpha, beta=beta)
 
 # Convert clicks to pageviews
 N_pageviews_gross = int(np.ceil(N_clicks_gross / CTP))
 N_pageviews_net = int(np.ceil(N_clicks_net / CTP))
 ```
 Gross Conversion:  
 Requires **52748** clicks for two groups.  
 At 8% CTP, this translates to **659350** pageviews for two groups.  
 Net Conversion:  
 Requires **55770** clicks for two groups.  
 At 8% CTP, this translates to **697125** pageviews for two groups.  
 Total Pageviews:  
 Max of two metrics groups translates to **697125**

### 4. Determine the exposure of the experiment
 To determine the required runtime for this A/B test, we begin with the statistically derived sample size of 697,125 total pageviews. Given Udacity's baseline of 40,000 daily pageviews, a 50% traffic allocation yields 20,000 pageviews/day (10,000 per group). 

Experiment Duration (days) $= \frac{\text{Total Required Pageviews}}{\text{Daily Diverted Traffic}} = \frac{697,125}{20,000} = 34.85 \approx 35 \text{ days}$

To accomodate for any variations in daily SE of the Gross and Net conversions we can have 33-35 days run period.

###  5. Perform Sanity Checks
Sanity checks are conducted to ensure that the random assignment of users to control and experiment groups was properly implemented. Specifically, we test whether the number of page views and clicks are evenly distributed between the two groups. For page views, this confirms that traffic was split at random, validating the integrity of the experimental setup. Similarly, checking the distribution of clicks helps identify any unintentional bias in user exposure or interaction due to the assignment mechanism. Passing these sanity checks provides confidence that any observed differences in outcome metrics can be attributed to the experimental treatment rather than flaws in group allocation.
**Pageviews** 
```
# No of pageviews
# H0: proportion_pv_exp = proportion_pv_ctrl
# H1: proportion_pv_exp <> proportion_pv_ctrl


ctrl_PV=df_control['Pageviews'].sum()
exp_PV=df_experiment['Pageviews'].sum()

total=(ctrl_PV+exp_PV)

print(total,ctrl_PV,exp_PV)

from statsmodels.stats.proportion import proportions_ztest

zstat, pval = proportions_ztest(count=[ctrl_PV,exp_PV], nobs=[total,total], value=0.0, alternative='two-sided')
print(f"Two-proportion z-test p-value: {pval:.3f} , Zstat : {zstat}")

Two-proportion z-test p-value: 0.133 , Zstat : 1.503097941694545
There is no evidence the Pageviews in control and experiment groups are different
```
**No of Clicks** 
```
# No of clicks
# H0: proportion_clicks_exp = proportion_clicks_ctrl
# H1: proportion_clicks_exp <> proportion_clicks_ctrl


ctrl_clicks=df_control['Clicks'].sum()
exp_clicks=df_experiment['Clicks'].sum()

total=(ctrl_clicks+exp_clicks)

print(total,ctrl_clicks,exp_clicks)

from statsmodels.stats.proportion import proportions_ztest

zstat, pval = proportions_ztest(count=[ctrl_clicks,exp_clicks], nobs=[total,total], value=0.0, alternative='two-sided')
print(f"Two-proportion z-test p-value: {pval:.3f} , Zstat : {zstat}")

Two-proportion z-test p-value: 0.753 , Zstat : 0.314766024552368
# There is no evidence the Clicks in control and experiment groups are different
```




 

    
     
 

