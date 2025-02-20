# Analysis-of-Youth-Drug-Use-A-Decision-Tree-Approach
[Abstract](#Abstract) /
[Introduction](#Introduction) /
[Background](#Background) /
[Methodology](#Methodology) /
[Results](#Results) /
[Discussion](#Discussion) /
[Conclusions](#Conclusions) /
[References](#References)




## Abstract
This project utilizes decision trees to analyze youth drug use using data from the 2020 National Survey on Drug Use and Health (NSDUH), focusing on youth experiences, demographics, and substance use. Our classification models achieve accuracies above 0.9, indicating effective data analysis. We discover patterns showing how the use of one substance may relate to another and how peer perceptions influence drug use behaviors. By employing decision trees and ensemble methods, we aim to uncover hidden patterns and drive evidence-based interventions to mitigate youth drug use, enhancing our understanding of the complex factors influencing these behaviors.

[Back to Top](#Analysis-of-Youth-Drug-Use-A-Decision-Tree-Approach)

## Introduction
Youth drug use is a multifaceted issue that encompasses a range of substances, including tobacco, alcohol, marijuana, and other illegal drugs.The patterns of use among youths can be influenced by various factors such as economic status, peer pressure, family dynamics, health conditions, and the availability of substances. Our project aims to delve into the underlying factors associated with youth drug use.

We leverage decision trees model —a form of machine learning—  to analysis data from the 2020 National Survey on Drug Use and Health (NSDUH). The dataset provides a comprehensive of data and categories available. Our analysis is structured into three distinct domains: youth experiences, demographic factors, and substance use.Through careful selection and processing of the data, we aim to identify underlying patterns and also to provide evidence-based recommendations that can inform strategies to address youth drug use issues.

[Back to Top](#Analysis-of-Youth-Drug-Use-A-Decision-Tree-Approach)

## Background
### Decision Tree

In our project, we use decision trees for data analysis. These are non-parametric, supervised learning methods ideal for classification and regression tasks. The goal is to build models that predict target values by learning decision rules from data features. 

Decision trees initiate from a root node and branch out based on decision rules applied to input features. This branching continues until it meets certain criteria such as minimum node size or maximum tree depth, which are adjustable parameters during model training. The leaves of the tree denote outcomes—class labels for classification tasks and continuous values for regression.

The construction of a decision tree involves selecting the best attributes to split the data, guided by metrics like classification error, the Gini index, or entropy, aiming to create homogenous subsets for accurate predictions. And these indicators have been calculated with the following formula:

<img width="791" alt="image" src="https://github.com/user-attachments/assets/6afea562-ade2-43c6-82b0-6ea45ffd52fe" />

### Decision Tree Ensemble Methods

Decision tree ensembles use multiple decision trees to enhance prediction accuracy and robustness. Bagging involves running multiple models on different random subsets of the training data, where each subset is created with replacement. By aggregating results from several trees, these ensembles help reducing the overfitting which  commonly seen with single decision trees.  The final output is either averaged (for regression) or determined by majority vote (for classification).

Random Forests enhance the bagging approach by introducing feature randomness. Instead of selecting the best feature from all available features for each split, the algorithm picks a random subset and chooses the best feature from it. This method, which involves building each tree by sampling ''m' predictors from the total 'p' available predictors, allows for a broader exploration of potential trees. To run the Random Forest algorithm, you must provide the predictor variables, the response variables, and specify 'm', the number of predictors to sample for each tree.

Boosting is another ensemble technique we'll use in our project,  where each new tree is developed to correct the errors from the previous trees. These models focus on improving accuracy by specifically addressing challenging cases misclassified in earlier iterations. During training, we can adjust the shrinkage (α) to regulate the learning rate, thereby impacting training speed and overall model performance.

[Back to Top](#Analysis-of-Youth-Drug-Use-A-Decision-Tree-Approach)

## Methodology

### Data Preparation

To prepare the data for our models, we first selected the variables of interest and categorize them into youth experiences, demographics, and substance use. This categorization will be helpful for model building later on.

Next, we converted some categorical data to factors, including binary and ordered where appropriate. In some variable columns, certain values like 991, 993, 91, 93, 5, and 6 lack numerical significance; they likely represent "Never used" or "Not used in the past period." Therefore, we convert them to 0. Additionally, values such as 94, 97, 98, and 99 in the "eduskpcom" column do not have analytical meaning, so we converted them to NA.

Furthermore, to ensure interpretability, we removed data with missing values before proceeding with the analysis.

### Models
For each model, the data was split into 70% for training and the remaining 30% was reserved for testing model accuracy or MSE. In binary classification, "mrjflag" (whether marijuana had been used or not) was chosen as the response variable, and "alcflag"  (whether alcohol had been used or not), "tobflag" (whether tobacco had been used or not), along with variables in demographic_cols and youth_experience_cols were used as predictors, to predict marijuana use. A single decision tree was first used for prediction, followed by a bagging approach by adjusting the number of trees to reduce variance and achieve optimal accuracy. 

In multi-class classification, "mrjmdays" (frequency of marijuana use in the past month) was selected as the response variable, and variables in demographic_cols and youth_experience_cols were used as predictors, to predict the frequency of marijuana use in the past month. A single decision tree was initially used for prediction, followed by random forest by adjusting the number of variables sampled at each split (mtry) to improve accuracy. 

For regression, "irmjfy" (frequency of marijuana use in the past year) was chosen as the response variable, and variables in demographic_cols and youth_experience_cols were used as predictors, to predict the number of days of marijuana use in the past year. A single decision tree was first used for prediction, followed by a boosting approach by adjusting the learning rate to obtain the optimal model.

[Back to Top](#top)

## Results 
### Binary classification
This classification tree model, shown in Figure 1, was built using training data to predict the "mrjflag" variable. It incorporated variables such as "alcflag," "yflmjmo," "stndsmj," "frdmjmon," and "tobflag" in its construction. With 8 terminal nodes, the model achieved a residual mean deviance of 0.4593, indicative of a good fit. It has a misclassification error rate of 0.09796, meaning that around 9.8% of the samples were incorrectly classified. The model's predictions on the test data, presented in Table 1(a), result in an accuracy of approximately 0.9174. 

<img width="804" alt="image" src="https://github.com/user-attachments/assets/b48175fa-15de-4f14-80bb-692818759d65" />
<img width="806" alt="image" src="https://github.com/user-attachments/assets/9015f262-6e2d-4999-8446-d173baafb0f7" />

To obtain the most suitable bagging approach, we tuned the models for the values of ntrees (number of trees). The results for the tuning can be seen in Figure 3. We observed that with different values of ntrees, there is not much difference. We were still able to identify the tree number with the lowest error rate, enabling us to proceed with the random forest model using bagging approach and obtain the optimal model. We got the result with each decision tree considering 62 variables at every split. The model yields an out-of-bag (OOB) error estimate of 10.21%, indicating an expected prediction error rate of approximately 10.21% on unseen data. The model's predictions on the test data, presented in Figure 2(b), result in an accuracy of approximately 0.9291, indicating a slight improvement compared to the single decision tree model. Through Figure 4, the Gini Index reflects how each variable contributes to the homogeneity of the nodes and splits in the tree. Variables "alcflag" and "tobflag" show high values, indicating their importance in creating pure nodes in the tree that effectively distinguish between users and non-users of marijuana. This result is similar to that of the previous single decision tree model.

<img width="538" alt="image" src="https://github.com/user-attachments/assets/de6ac160-b4ff-424d-9f31-a125b12f2da3" />
<img width="576" alt="image" src="https://github.com/user-attachments/assets/cd0dab57-fd73-429c-a35b-9f0530311689" />

### Multi-class classification
The single tree model for multi-class classification was constructed using the training data to predict the "mrjflag" variable. It utilized 8 variables and comprised 8 terminal nodes. The model attained a residual mean deviance of 0.4474 and a misclassification error rate of 0.06503. Upon evaluating the model's predictions on the test data, it achieved an accuracy of approximately 0.9291.

To improve the model, we utilized the random forest approach and adjusted the models for various values of mtry. The tuning outcomes are illustrated in Figure 5. The best-performing model was determined with an mtry value of 21. This model yields an out-of-bag (OOB) error estimate of 7%. Furthermore, it achieved an accuracy of 0.9300, slightly exceeding that of the single decision tree model. Through Figure 6, it's evident that variables "EDUSCHGRD2" (current or expected grade level) and "frdmjmon" exhibit high importance values. This highlights their significance in creating pure nodes within the tree, thus aiding in effectively distinguishing different levels of monthly marijuana usage frequency.

<img width="528" alt="image" src="https://github.com/user-attachments/assets/74f5b68c-eabd-4c60-80f5-f2862ddec161" />
<img width="783" alt="image" src="https://github.com/user-attachments/assets/b02e7dd3-b9cd-4016-bd16-b3e44114918d" />

### Regression 
The single tree model for regression was built using the training data to predict the "irmjfy" variable. It employed 6 variables and consisted of 7 terminal nodes. Upon evaluating the model's predictions on the test data, it resulted in a test MSE of approximately 1358.83.

To enhance the model, we applied the boosting approach and adjusted the models for various learning rate values. The optimal model was identified with a learning rate of 0.03 and a maximum depth of 3 for tree growth. This model achieved a test MSE of approximately 1243.07, lower than the single decision tree model.

Through Figure 7, it's evident that variables "prmjmo" (parents' feelings about youth marijuana use) and "stndsmj" exhibit high importance values. This underscores their significance in creating pure nodes within the tree, thus aiding in effectively predicting the number of days of marijuana use in the past year.

<img width="811" alt="image" src="https://github.com/user-attachments/assets/4270073c-2612-4aee-b97d-26be5ab51c01" />

[Back to Top](#top)

## Discussion
### Flow and Interpretation of a Tree Model
In the flow of our tree model as depicted in Figure 1,  begins at the root node with an assessment of "alcohol ever used." As the tree branches, a notable split occurs at "any tobacco ever used" , which strongly suggests the influence of alcohol and tobacco use behaviors. The diagram shows that on the left side—where no alcohol use is reported—the analysis tends to predict a lower likelihood of marijuana use. Conversely, on the right side of the tree, where alcohol use is confirmed, and when factoring in friends who neither approve nor disapprove of monthly marijuana use, the likelihood of marijuana use markedly increases.
### Data Types and Predictive Changes
The model incorporates variables in different formats: binary, ordinal, and numerical. Our analysis shows that binary variables reveal distinct behavior patterns, while ordinal variables offer insights into drug use frequency and severity. Numerical variables, such as the number of days marijuana was used, provide direct quantification and enable precise regression modeling. The appropriate use of each data type depends on the specific analysis goal—binary and ordinal variables are beneficial for classification tasks, while numerical variables are essential for regression.
### Importance of Variables
Variables such as "alcflag" and "tobflag" play a crucial role in predicting marijuana use, indicating a pattern where the use of one substance may be related to the use of another. "prmjmo" and "frdmjmon" are also significant factors, suggesting that perceptions of drug use by people around can have a certain degree of influence.
### Ethical Consideration in Communication
As data scientists, it is our responsibility to communicate findings ethically. We must present data objectively, without inferring causation from correlation. Our communication should aim to inform, support public health efforts, and contribute to a comprehensive understanding of youth drug use.

[Back to Top](#top)

## Conclusions
Our study provided valuable insights into the predictors of youth drug use through decision trees and ensemble methods. The variables related to alcohol and tobacco use, along with perceptions of drug use among peers, emerged as significant predictors. Our findings underscore the interrelated nature of substance use and the social factors surrounding youth. It is important to approach these findings with caution and ethical consideration, recognizing the difference between correlation and causation. The knowledge gained from our models can inform public health strategies aimed at understanding and reducing drug use among youth.

[Back to Top](#top)

## References
National Survey on Drug Use and Health (NSDUH), https://www.datafiles.samhsa.gov/dataset/national-survey-drug-use-and-health-2020-nsduh-2020-ds0001

[Back to Top](#top)







