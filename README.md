# Lead Conversion Prediction System for World Plus Bank

## Table of Contents
- [Introduction](#introduction)
- [Project Objective](#project-objective)
- [Methodology (CRISP-DM)](#methodology-crisp-dm)
    - [Data Understanding](#data-understanding)
    - [Data Preparation](#data-preparation)
    - [Modelling](#modelling)
    - [Model Evaluation](#model-evaluation)
- [Outcome and Key Findings](#outcome-and-key-findings)
- [Limitations](#limitation)
- [Conclusion and Future Work](#conclusion-and-future-work)
- [References](#references)

## Introduction
This project focuses on addressing a critical challenge for World Plus, a mid-size private bank: accurately identifying potential leads for cross-selling a new term deposit product to existing customers. Effective cross-selling is known to enhance customer lifetime value and loyalty. The goal was to develop a robust lead prediction system that maximizes profit from cross-selling while minimizing marketing expenses.

This technical report details the application of the CRISP-DM methodology, employing various machine learning algorithms to achieve this objective. The project aims to provide a data-driven solution for World Plus to strategically target customers.

**Note:** Artificial Intelligence (AI) tools were **not** used in any part of this assignment.

## Project Objective
The primary objective of this project was to build a machine learning algorithm that accurately predicts lead conversion for World Plus's new term deposit product, thereby maximizing net profit for the bank.

## Methodology (CRISP-DM)

The project strictly adhered to the **CRISP-DM (Cross-Industry Standard Process for Data Mining)** methodology, ensuring a structured and comprehensive approach to problem-solving.

### Data Understanding
World Plus provided 220,000 historical customer records. **It is important to note that this dataset is a simulated one, provided for the purpose of this analysis.** The dataset comprised 15 independent variables and one dependent variable, "Target" (indicating product purchase). Key variables included:

* **Categorical:** ID, Gender, Dependent, Marital_Status, Region_Code, Occupation, Channel_Code, Credit_Product, Account_Type, Active, Registration, Target.
* **Numerical:** Age, Years_at_Residence, Vintage, Avg_Account_Balance.

### Data Preparation
Several crucial steps were undertaken to prepare the data for machine learning algorithms:
1.  **Feature Removal:** The "ID" variable was removed due to its lack of informative value.
2.  **Encoding Categorical Variables:**
    * Binary categorical variables were converted to numerical representations.
    * Categorical variables with more than three levels (`Marital_Status`, `Occupation`, `Channel_Code`, `Account_Type`) were processed using **one-hot encoding**.
    * `Region_Code` (35 levels) was handled with **target encoding** (Singh, 2018).
3.  **Addressing Data Imbalance:** The dependent variable was highly imbalanced, with only 14.8% of total leads converted. To mitigate this, the **Random Over Sampling Examples (ROSE)** method was applied to the training data. Various sampling methods (oversampling, undersampling, both-sampling) were trialed, with oversampling proving most effective.
4.  **Handling Missing Values:** A significant portion (8.3%) of missing values existed for the `Credit_Product` variable. A comparison between models built from data with missing values removed, untouched, or replaced with the mode value was conducted. Replacing missing values with the mode value in the oversampled training data yielded the highest performance.

### Modelling
With the prepared training data (25 variables), six distinct classification models were utilized to predict lead conversion. These models were selected due to their proven high performance in predicting binary dependent variables, especially within the banking industry (Karvana et al., 2019; Mohammed et al, 2019):

* **Decision Tree**
* **Support Vector Machine (SVM)**
* **Random Forest**
* **Logistic Regression**
* **Gradient Boosting**
* **Naive Bayes**

The **Information Gain method** (Provost & Fawcett, 2013, p.52) was applied to assess the informativeness of attributes for predicting the "Target" variable. However, removing attributes with minimal information gain did not significantly improve model performance, leading to the retention of all attributes in the final model versions to avoid overfitting.

### Model Evaluation
Model performance was rigorously assessed from both technical and business perspectives.

**Technical Evaluation:**
* **Confusion Matrix:** Used to obtain True Positives (TP), False Positives (FP), True Negatives (TN), and False Negatives (FN).
* **Receiver Operating Characteristic (ROC) Graph:** Plotted to compare the performance of models against each other and a random baseline.
* **Key Metrics:**
    * **Accuracy:** Overall correctness of predictions.
    * **Recall:** The percentage of actual positive cases (customers who bought the product) that were correctly identified by the model. This was a critical business metric for World Plus.
    * **Precision:** The proportion of positive predictions made by the model that were actually correct. Crucial for calculating losses from misguided targeting.
    * **Area Under the Curve (AUC):** A measure of the model's ability to distinguish between classes.

**Model Performance Summary:**

| Model             | Accuracy | Recall  | Precision | AUC    |
| :---------------- | :------- | :------ | :-------- | :----- |
| Decision Tree     | 83.74%   | 65.31%  | 46.41%    | 0.8211 |
| SVM               | 89.69%   | 59.85%  | 66.86%    | 0.8462 |
| Random Forest     | 90.38%   | 53.99%  | 73.82%    | 0.8712 |
| Logistic Regression | 88.19%   | 60.06%  | 60.00%    | 0.8566 |
| Gradient Boosting | 89.19%   | 61.96%  | 63.81%    | 0.8809 |
| Naïve Bayes       | 80.10%   | **68.21%** | 39.85%    | 0.8391 |

**Business Evaluation (Expected Profit Calculation):**
To align with the project's objective of maximizing profit, an expected value method (Provost & Fawcett, 2013, p.198-203) was applied, considering the following assumptions:

* **Revenue per Correct Lead:** Assumed to be the average balance of customers’ bank accounts over the last 12 months, which was GBP 1,115,099 per customer in the validation data.
* **Loss from False Positives:** Equaled the probability of False Positives multiplied by the cost per lead (sales and marketing expenses). Average cost per lead for the financial services sector was forecasted at USD 658 (GBP 522) in 2024 (Bailyn, 2023).
* **Opportunity Cost from False Negatives:** Equaled the foregone revenue (average bank account balance) for mistakenly ignored potential customers.

**Estimated Net Profit per Customer:**

| Model             | TP    | FP     | TN     | FN    | Expected Profit (GBP) |
| :---------------- | :---- | :----- | :----- | :---- | :-------------------- |
| Decision Tree     | 6,362 | 7,347  | 48,875 | 3,380 | 50,295                |
| SVM               | 5,831 | 2,890  | 53,332 | 3,911 | 32,383                |
| Random Forest     | 5,260 | 1,865  | 54,357 | 4,482 | 13,092                |
| Logistic Regression | 5,851 | 3,901  | 52,321 | 3,891 | 33,052                |
| Gradient Boosting | 6,036 | 3,424  | 52,798 | 3,706 | 39,309                |
| Naïve Bayes       | 6,645 | 10,030 | 46,192 | 3,097 | **59,838** |

## Outcome and Key Findings
Based on the comprehensive evaluation, the **Naïve Bayes model** was selected as the most profitable lead prediction model for World Plus. It exhibited the highest recall rate (68.21%), signifying its strong ability to identify potential customers who would actually purchase the product. Furthermore, it demonstrated the highest estimated net profit per customer, reaching **GBP 59,838**.

While Random Forest had the highest precision, its lower recall rate made it less suitable for the bank's primary objective of identifying as many potential buyers as possible. The ROC graph also indicated no significant visual variances in performance among the models.

## Limitations
* **Scalability & Performance:** The project necessitated downsizing the original dataset of over 2 million recipes due to substantial computational demands and cost considerations.
* **Contextual Understanding:** The system's ability to interpret nuanced or ambiguous queries might be constrained, potentially leading to irrelevant responses.
* **Response Generation:** The model's response generation is limited to the data it was trained on, preventing it from integrating new information or adapting to evolving trends without retraining.

## Conclusion and Future Work
This project successfully demonstrated the application of machine learning algorithms within the CRISP-DM framework to build an effective lead prediction system for World Plus Bank. The Naïve Bayes model, identified as the optimal choice, significantly aids in maximizing profit from cross-selling efforts.

For future enhancements, the model's performance could be further improved by:
* Collecting additional data dimensions, such as customers' banking habits and the bank's specific marketing efforts (Kilinc & Rohrhirsch, 2022).
* Incorporating customer satisfaction levels (Karvana, et al., 2019).
* Obtaining precise data on customer lifetime value and actual cost per lead to refine the expected profit and loss estimations.

This project underscores the transformative potential of data-driven insights in optimizing business strategies within the financial sector.

## References
* Bailyn, E. (2023). *FirstPageSage*. [Online] Available at: https://firstpagesage.com/reports/average-cost-per-lead-by-industry/
* Blattberg, R. C., Malthouse, E. C. & Neslin, S. A. (2009). Customer Lifetime Value: Empirical Generalizations and Some Conceptual Questions. *Journal of Interactive Marketing, 15*(23).
* Dahana, W. D., Miwa, Y., Baumann, C. & Morisada, M. (2019). Relative importance of motivation, store patronage, and marketing efforts in driving cross-buying behaviors. *Journal of Strategic Marketing, 28 8, 30*(5), pp. 481-509.
* Karvana, K. G. M., Yazid, S., Syalim, A. & Mursanto, P. (2019). Customer Churn Analysis and Prediction Using Data Mining Models in Banking Industry. Bali, Indonesia, IEEE.
* Kilinc, M. S. & Rohrhirsch, R. (2022). Predicting customers’ cross-buying decisions: a two-stage machine learning approach. *Journal of Business Analytics, 9, 6*, pp. 1-8.
* Madaan, M. et al. (2021). Loan default prediction using decision trees and random forest: A comparative study. Rajpura, India, IOP Publishing Ltd.
* Mohammed, A. A., Olalere, M. & Mohammed, A. I. (2019). Comparative Analysis of Performance of Different Machine Learning Algorithms for Prediction of Success of Bank Telemarketing. s.l., Federal University of Technology, Minna Institutional Repository.
* Provost, F. & Fawcett, T. (2013). *Data Science for Business : What You Need to Know about Data Mining and Data-Analytic Thinking*. s.l.:O'Reilly Media, pp. 51 - 61, 194-203, 214-222.
* Sadikin, M. & Alfiandi, F. (2018). Comparative Study of Classification Method on Customer Candidate Data to Predict its Potential Risk. *International Journal of Electrical and Computer Engineering, 8*(6).
* Singh, N. (2018). [Online] Available at: https://www.datacamp.com/tutorial/encoding-methodologies [Accessed 1 Dec. 2023].
* Vafeiadis, T., Diamantaras, K., Sarigiannidis, G. & Chatzisavvas, K. (2015). A comparison of machine learning techniques for customer churn prediction. *Simulation Modelling Practice and Theory, 55*, pp. 1-9.
