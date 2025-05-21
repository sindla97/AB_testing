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
    The net conversion should see an Increase, as we expect students to entroll and continue past 14 day free trail increase over number of unique cookies to click the "Start free trial" button which is same for both groups

### 2. Calculate the Standard error of metrics:
 Based on the given baseline estimates for each of this metrics we can analytically calculate the standard error.  
 we can use Binomial proportion formula to estimate standard error because they are fundamentally binary outcomes (success/failure) with fixed trial counts, satisfying the core assumptions of binomial distributions.  

 $$ SE = \sqrt{\frac{p(1-p)}{n}} $$

 where:  
 - \( p \) = Baseline probability of success (e.g., 0.20 for 20% conversion)  
 - \( n \) = Sample size (e.g., number of clicks or enrollments)

 ### Standard Error Calculations for Evaluation Metrics
 
 | Metric          | Baseline Rate (p) | Sample Size (n) | Calculation                      | Standard Error |
 |-----------------|-------------------|-----------------|----------------------------------|----------------|
 | **Gross Conversion** | 0.20625          | 3,200 clicks    | $$\sqrt{\frac{0.20625 \times 0.79375}{3200}}$$ | 0.0071         |
 | **Net Conversion**   | 0.1093125        | 3,200 clicks    | $$\sqrt{\frac{0.1093125 \times 0.8906875}{3200}}$$ | 0.0055         |
 | **Retention**        | 0.53             | 660 enrollments | $$\sqrt{\frac{0.53 \times 0.47}{660}}$$         | 0.0194         |



    
     
 

