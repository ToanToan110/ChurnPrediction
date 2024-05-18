# Introduce
Imagine we are the member of a Retail/Telcos company and we wants to fuel its growth by Data-driven approach. This Series has 6 part research about 6 core problem of business:
- Part 1: [Customer Segmentation](https://github.com/ToanToan110/CustomerSegmentation)
- Part 2: [Churn Prediction](https://github.com/ToanToan110/ChurnPrediction)
- Part 3: [Customer's Life Time Value](https://github.com/ToanToan110/CustomerLifeTimeValue)
- Part 4: [Sales Prediction](https://github.com/ToanToan110/SalesPrediction)
- Part 5: [Market ResponseModel](https://github.com/ToanToan110/MarketResponseModel)
- Part 6: [A/B Testing](https://github.com/ToanToan110/A-B-Testing)

# About this Project
> ***Retention rate/Churn rate*** is an index that measures the product's suitability in the market, helping businesses better access the market.

> Controlling and predicting these indicators will help businesses be proactive and ensure customers are always satisfied with their services and always want to use their services.
## Prerequisites
Package: Pandas, Matplotlib, Numpy

Techniques: K-means

## Resources 
Dataset: This notebook use Telco Dataset (https://www.kaggle.com/datasets/vijayuv/onlineretail](https://www.kaggle.com/datasets/blastchar/telco-customer-churn) on Kaggle

References: 
[- https://towardsdatascience.com/data-driven-growth-with-python-part-1-know-your-metrics-812781e66a5b](https://towardsdatascience.com/churn-prediction-3a4a36c2129a)

# Explodatory analysis

Avarage churn rate by each categorical features:

![image](https://github.com/ToanToan110/ChurnPrediction/assets/64849001/4a10042e-464d-4df3-8390-523d6c87983b)

**From the chart above, we can figure out the factors causing the difference in customer churn rate:**

* SeniorCitizen are more than twice as likely to leave than the rest, proving that this telecommunications service does not seem suitable for the elderly.
* Customers who are not partners (partner = No) have a ~10% higher churn rate, possibly because customers do not really trust the service.
* Customers without a home (dependents = No) have a ~10% higher churn rate, which is understandable when economic problems can affect the demand for telecommunications services.
* Customers using fiber optic cable (InternetService = Fiber optic) and customers without online security (OnlineSecurity = No) have a quite high churn rate of ~40%, possibly due to problems with the company's fiber optic and internet services. is suitable for the majority of users.
* Differences in gender and PhoneService do not affect Churn rate much (diff <2%)

Avarage churn rate by each categorical features:

![image](https://github.com/ToanToan110/ChurnPrediction/assets/64849001/42ebf7a0-6121-4fc8-ad6b-c5f6a8f47146)

- Most customers with month-to-month contracts are very likely to leave ~40% (due to few constraints), a big difference compared to the other two types of contracts (>30%).
- Similar for those using Electronic Check payment method.

Avarage churn rate by each numerical features:

![image](https://github.com/ToanToan110/ChurnPrediction/assets/64849001/bbdcf8c3-e6f7-4ab8-a9f4-b6a979f63740)

- The longer the tenure, the more trust there is in the service, so it is understandable that this group has a low turnover rate.
- The **(MonthlyCharge)** for the service does not affect the service abandonment rate.

## Analysis the core impact to Churn rate:
We use the simple t-test to answer the question that: Which factors are impact to the difference of CHurn rate ?

And We got this fotures below have the t-value less than 0.05 (0.05 is the Alpha value that represent the maximum acceptable of t-value that the test is significant): 

['InternetService',
 'OnlineSecurity',
 'OnlineBackup',
 'DeviceProtection',
 'TechSupport',
 'StreamingTV',
 'StreamingMovies',
 'Contract',
 'PaymentMethod',
 'SeniorCitizen',
 'PaperlessBilling',
 'tenure',
 'TotalCharges']

So which are the features that increase the Churn rate?

![image](https://github.com/ToanToan110/ChurnPrediction/assets/64849001/a84b4ba8-2122-4c4e-b5ca-ae0ef243993b)

- Company needs to control the total chanrges of customer because it affects the Churn rate by the negative way.
- Beside that, the Fiber Optic of Internet service, Streaming Movies need to improve because these are the features with high churn rate.

And these are features that descrease the churn rate:

![image](https://github.com/ToanToan110/ChurnPrediction/assets/64849001/eb55eb02-3b13-4fab-a165-899a378f9955)

- The group of customers with high tenure, Monthly charge, 2 year contact... This groups are the company's sustainable customers.
- Services without internet connection are the company's strength.

So:

- The number of customers using fiber optic network (InternetService = 'FiberOptic') increases by 1 unit, the churn rate increases by 1.84 units if other factors do not change
- If the customer's tenure increases by 1 unit, the churn rate increases or decreases by 0.987 (=1 - 0.013002) units if other features do not change.
- **MonthlyCharges**, **TotalCharges** và **Tenure** là những yếu tố quan trọng nhất ảnh hưởng tới tỉ lệ rời bỏ.

# Predic the Churn:
We use XGBOOST for prediction task and got this result:

![image](https://github.com/ToanToan110/ChurnPrediction/assets/64849001/96ce1868-d2b1-4dee-ade8-43e1ed3dab44)

The accuracy of this model in the test set is 84%
This model is perform good in predicting customer do not churn and detect the customer do not churn (because the precision and recall are not so bad)
But it's hard to predict and detect the label 1 (this is because of the number of label 1 is to less)

Solution for this is:
- Apply balance data
- Increase the size of the label 1
