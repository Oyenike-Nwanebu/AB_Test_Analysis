This project aims to analyse the result of an A/B test conducted on the subscription service cancellation page of a company.

## Background Information
An A/B test on the cancellation page of the subscription service was recently ran. Before running the test, members were able to cancel using a simple web form. The experiment aims to measure the impact of forcing members to phone-in to the customer service line in order to cancel.

Information about the A/B test:
- control group (0) can cancel using a web form
- test group (1) can only cancel by calling into the customer service line
- Users were randomly assigned to a group when they go to the website's cancellation page for the first-time
- The distribution probabilty between both groups is uneven
- Additional transactions generated after users were randomized were recorded
- REBILLs are Transactions recurring payments that were processed
- CHARGEBACKs or REFUNDs transactions represent payments that were cancelled

## Results

### What is the approximate probability distribution between the test group and the control group?
After cleaning and analyzing the data, it was discovered that the subsription service members were distributed in approximately 75.2% to 24.8% control group to test group ratio as shown in the bar chart below. This implies that a user has a 75.2% chance of being selected to the control group.

![Probability Distribution](https://github.com/Oyenike-Nwanebu/AB_Test_Analysis/blob/master/Data/Prob_distribution.png?raw=true)

### Is a user that must call-in to cancel more likely to generate more revenues?
Null hypothesis: A user that must call-in to cancel is less or equally likely to generate more revenue
Alternative hypothesis: A user that must call-in to cancel is more likely to generate more revenue

To determine whether a user belonging to the test group was more likely to generate more revenues, one-tailed Welch's t-test was used to determine statistical significance since there is an imbalance between the two sample groups. Although the transaction amounts for both the control and test groups were not from a normal distribution (see data analysis file), since the sample size was large, Central limit theorem can be applied as if repeated sampling is done, the distribution will converge to a normal distribution.

The p-value obtained from the one-tailed Welch's t-test was less than 0.05 significance level (p_value < 0.05). This implies that we can reject the null hypothesis. Thus, there is a 95% confidence level that users that must call-in to cancel will likely generate more revenue.

### Is a user that must call-in is more likely to produce a higher chargeback rate(CHARGEBACKs/REBILLs)?
Null hypothesis: a user that must call-in is less or equally likely to produce a higher chargeback rate
Alternative hypothesis: a user that must call-in is more likely to produce a higher chargeback rate

The chargeback rate for each group was computed using the formula below:

chargeback rate = number of chargeback transactions/total transactions

Using the above formula, 

Chargeback rate for control group = 0.073
Chargeback rate for test group = 0.052

To test if the difference in the chargeback rate for both groups is significant, the p-value was computed from the z-score. p-value was calculated as 1.0 (p_value > 0.05). One-tailed Mann-Whitney U test was also used to compute the p-value and the same value was obtained. This implies that we can accept the null hypothesis. Thus, it can be concluded with a 95% confidence level that a user that must call-in is less or equally likely to produce a higher chargeback rate.

### Is a user that must call-in to cancel more likely to generate at least 1 addition REBILL?
Null hypothesis: User that must call-in to cancel is less or equally likely to generate at least 1 addition rebill
Alternative hypothesis: User that must call-in to cancel is more likely to generate at least 1 addition rebill

From the transaction dataset, the control group generated 3756 rebill transactions while the test group generated 3205 rebill transactions.

To test the statistical significance of the difference between the number of rebill transactions generated, one-tailed Mann-Whitney U test was used. The p-value was calculated as 1.0 (p_value > 0.05). This implies that we can accept the null hypothesis. Thus, it can be concluded with a 95% confidence level that a user that must call-in is less or equally likely to generate at least 1 addition rebill.


## Information About the Data
The required data-sets for this analysis are in Data Folder. They are  split into the following 2 csv files:
- [testSamples.csv](testSamples.csv)
- [transData.csv](transData.csv)

In testSamples.csv, there is a list of unique users that were randomized in the A/B test.
* sample_id : is the unique identifier for the sample
* test_group : is the group in which the sample was placed, 0= control group, 1=test group

In transData.csv, there is a list of transactions generated by randomized users after their randomization:
* transaction_id : is the unique identifier for the transaction
* sample_id : is a foreign key that links transactions to test samples
* transaction_type : is the transaction type for a transaction, can be REBILL, CHARGEBACK or REFUND
* transaction_amount : is the amount generated for a transaction, this can be a negative value
