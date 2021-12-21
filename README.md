# Udacity-Projects

Optmizing ML Pipeline in Azure
# Optimizing an ML Pipeline in Azure

## Overview
This project is part of the Udacity Azure ML Nanodegree.
In this project, we build and optimize an Azure ML pipeline using the Python SDK and a provided Scikit-learn model.
This model is then compared to an Azure AutoML run. 

As shown in the below flow , Logistic regression model was given as a custom coded model , in this project I will exploring the AzureML in Python SDK of Hyperparameter tuning and Automl to check which has the better accuracy and share more details of how to do the same.   

![image](https://user-images.githubusercontent.com/92014201/146714044-bf997ced-149d-4a1b-a706-8cd8c0dcf72c.png)


## Summary
The data is related with direct marketing campaigns of a bank , there were 16 columns which describes more about the client and the Target column was whether a client
will subscribe a term deposit or not (variable y). This is a classification problem. 


**In 1-2 sentences, explain the solution: e.g. "The best performing model from the Auto Ml which had a Accuracy of 91.66% using XG Boost Classifier and with Hyper patameter tuning the best accuracy was 91.08% " , clearly AUTOML is the champion model here.


## Scikit-learn Pipeline
**Explain the pipeline architecture, including data, hyperparameter tuning, and classification algorithm.**

#### Data Load : 

Bank Marketing Dataset was loaded as URL and it later converted into the Datastore as explained earlier it had 21 columns with a Target columns as y and has 32950 rows.  
Once the data was loaded all the categorical columns was passed into the  one hot encoding and converted into binary columns and label encoding was done for columns related to time. The Model was also split into Training and Testing model with 77 % of the data goes into Training and 33% goes into Testing the model.   

#### Model Training : 

##### Only Logistic Model 

Only Model has a Accuracy of 91.08% , below is the screenshot showing accuracy with the arguments of C: 1 and Max Iter : 100

![image](https://user-images.githubusercontent.com/92014201/146740531-e9476b40-e326-4d23-91b9-fb3746b98051.png)

A Custom Coded Model (Logistic Regression) was used for the classification prediction with the below parameters where tuned for best accuracy : 

####  a) C controls the regularisation level in the ML model. The sample space is uniform distribution between 0.1 and 1 
####  b) max iter controls then number of maximum iterations. The sample space is random value from a list of 50 and 100 
####  c) logistic regression 
####  d) random parameter sampling is chosen to find the best hyperparameter values 
####  e) early stopping policy is chosen 

Hyperparameter tuning : 

Parameters choosen for Hyperparameter tuning :     

![image](https://user-images.githubusercontent.com/92014201/146743319-e8ce5992-5ffd-4093-8edb-e71cdbd305c9.png)

Hyperparameters showing Accuracy : 

![image](https://user-images.githubusercontent.com/92014201/146743120-155c09cc-d7f1-40e6-b1c1-9443ce86efcf.png)  

Best Model in Hyper parameter tuning with C : 0.557 and Max Iter: 150 

![image](https://user-images.githubusercontent.com/92014201/146878856-84221f88-f764-46e0-8a76-6ce7ddeca5f4.png)


**What are the benefits of the parameter sampler you chose?**

RandomParameterSampling is fast and support early termination. If budget is not an issue, we can use GridParameterSampling for an exhaustive solution. The accuracy of the model may be better than values that are found by RandomParameterSampling. Additionally, BayesianParameterSampling is another option that we can utilise.

**What are the benefits of the early stopping policy you chose?**

I also have Bandit policy which basically checks the whether the accuracy falls within the Top 10% range for every iteration , it automatically removes the iterations where it is beyond the 10% 
and this helps in using the resources optimaly and also saves time. 

## AutoML
**In 1-2 sentences, describe the model and hyperparameters generated by AutoML.**

AutoML Config is away of leveraging the SDK to automate the Machine learning , from the doing feature engineering to the selecting the best algorithm for the 
data everything will be automated in the Auto ML. In my case , the best model was XG Boost Classifier with a Accuracy of 0.9174.

![image](https://user-images.githubusercontent.com/92014201/146748539-7560c797-48c3-45a0-8258-d7d0f9396735.png)

##### a) experiment_timeout_minutes=30
This parameter is used to define in minutes, how long the experiment will run. The experiment will be stopped in this case after 15 minutes

##### b) task=classification
This parameter defines the task involved. In this case we are doing classification. Hence, classification is set as a value

#### c) primary_metric=accuracy
This parameter sets the peformance metrics chosen. In this case accuracy is used.

#### d) compute_target
This parameter defines the compute cluster used for the training process

#### e) training_data
This parameter defines the traning data

#### f) label_column_name=y
This parameter defines the groud truth column

#### g) n_cross_validations=3
This parameter defines the number of cross validation performed during training process.

### Screenshot showing the different Iterations tried by the Auto ML Model 

![image](https://user-images.githubusercontent.com/92014201/146871300-49705838-5a7c-420b-b4e4-fc05344dcbb6.png)

### Best Model parameters from the Auto ML model 

"param_kwargs": {
        "booster": "gbtree",
        "colsample_bytree": 1,
        "eta": 0.3,
        "gamma": 0,
        "max_depth": 10,
        "max_leaves": 511,
        "n_estimators": 10,
        "objective": 
        
        
## Pipeline comparison
**Compare the two models and their performance. What are the differences in accuracy? In architecture? If there was a difference, why do you think there was one?**

Using Logistics regression and the HyperParameter tuning the best model accuracy was 91.08% but from AutoML the best accuracy was 91.66%. In terms of Hyperparameter 
tuning , we had to set the configurations in  Hyperdrive but for the AutoMl everything was automatically created by the Model. 

![image](https://user-images.githubusercontent.com/92014201/146880482-0e9c06da-b73d-4149-b96c-14cd71fca9e5.png)

AutoML Model was an ensemble model which means multiple diverse models are created to predict an outcome, whereas in the Hyper Parameter tuning only one model logistic regression is been trained and optimized to bring out the best accuracy. Hence we see very sligh improvement in the accuracy of the Auto Ml and Hyperparameter tuning model. 
 

## Future work
**What are some areas of improvement for future experiments? Why might these improvements help the model?**


   1) Only Logistic model without any hyper parameter tuning had an accuracy of 91.08% and Hyper Parameter tuning model also has the same accuracy which means that there are more scope to improve the accuracy. Due to the given time constraints and cluster constraints , we could not do the iterate the model further  we could tune the logistics model with more parameters to see , if we get a better accuracy than what we have now. 

   2) Getting to know more about features and how it is caluculated would be very usefull for the Model Interpretation and business. Sometimes using a Automl and 
Optimizing the function to have the best fit can have lot of Mathematical transformations of the variabels but may not be useful for the business. 
Hence need more visibility of the calcualtion of features might improve the performance of the model. 

  3) Model should not be focused only on improving the Overall Accuracy , in this problem False Positives are where the Customers are not going to buy the FD but the model recommends them as positives and False Negatives are where Model predicts the customers not going to buy the FD , but in actual they would be buying it. So considering the business case Model should be focused on reducing the False Negatives , hence the Primary metric should be Recall and not only overall Accuracy. 



