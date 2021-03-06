# Optimizing an ML Pipeline in Azure

## Overview
This project is part of the Udacity Azure ML Nanodegree.
In this project, we build and optimize an Azure ML pipeline using the Python SDK and a provided Scikit-learn model.
This model is then compared to an Azure AutoML run.

## Summary
**In 1-2 sentences, explain the problem statement: e.g "This dataset contains data about... we seek to predict..."**
This dataset contains data about Bank Marketing which is aimed at identifying potential customers willing to subscribe to a fixed term deposit with a financial institution. We seek to predict if clients will or will not subscribe to a fixed term deposit with a given financial institution.
**In 1-2 sentences, explain the solution: e.g. "The best performing model was a ..."**
The best performing model was MaxAbsScaler, XGBoostClassifier at an accuracy value of 0.91737. This was achieved by running an AutoML config with random values for various parameters.
## Scikit-learn Pipeline
**Explain the pipeline architecture, including data, hyperparameter tuning, and classification algorithm.**
![alt text](https://github.com/vikkyfama/An_Azure_ML_Pipeline/blob/toria/Scikit-Learn%20Pipeline%20Architecture.png)
The scikit-learn pipeline consisted of a data which was retrieved from a .csv file and then converted to a tabular dataset using the TabularDatasetFactory. Because the data remains in its existing location, no extra storage cost was incured and the integrity of your data sources was secured.
The Parameter sampling method of choice was the RandomParameterSampler with the following Hyperparameter tuning:
1. The "learning rate" was specified with a normal(10, 3)which returns a real value that's normally distributed with mean 10 and standard deviation 3.
2. The "keep probability" was specified with a uninform(0.05, 0.1) which returns a value uniformly distributed between 0.05 and 0.1.
3. The "Regularization Strength" was set to its default value of 1.0.
4. The "Max iterations" was set to a value of 1000.
5. The "Accuracy" was specified with a choice(0.906, 0.908, 0.910) which is a list of values.
The classification Algorithm used was the LogisticRegression which basically measures the relationship between the categorical dependent variable and one or more independent variables by estimating the probability of occurrence of an event using its logistics function. It was defined with the following parameters:
1. The C with default value = 1.0
2. The Max_Iter value = 1000
**What are the benefits of the parameter sampler you chose?**
Benefits of the RandomParameterSampler include:
1. Support of both discrete and continuous hyperparameters. 
2. It supports early termination of low-performance runs. 
3. Ability to perform a pre-search with random sampling and then refining the search space to improve results.
**What are the benefits of the early stopping policy you chose?**
The early stopping policy of choice was the Bandit policy which is based on slack factor/slack amount and evaluation interval.
1. Ability to specify the frequency for applying the policy in terms of the evaluation_interval.
2. Ability to specify the  delay of the first policy evaluation for a specified number of intervals.
3. Bandit policy terminates runs where the primary metric is not within the specified slack factor/slack amount compared to the best performing run.
## AutoML
**In 1-2 sentences, describe the model and hyperparameters generated by AutoML.**
The best model generated by the AutoML was the MaxAbsScaler, XGBoostClassifier with an accuracy of 0.91737. Parameters specifications were as follows: experiment_timeout_minutes=30, compute_target = "vikkycompute", task="classification", primary_metric="accuracy", training_data= training_data, label_column_name= "y", n_cross_validations=5.
## Pipeline comparison
**Compare the two models and their performance. What are the differences in accuracy? In architecture? If there was a difference, why do you think there was one?**
The Hyperdrive Config Model had an Accuracy of 0.9153, while the AutoML Model had an Accuracy of 0.9173 with a difference of 0.0020. The difference in their various architecture is that in the hyperdrive config in addition to being required to manually specify parameters such as an estimator, policy, primary_metric_goal, max_total_runs and max_concurrent_runs we also need to fit the train data manually with a chosen algorithm. AutoML only require us to specify the task, training_data, label_column_name,n_cross_validations and automatically does all the data splitting, cross validation, model fitting and model selection in the background.
## Future work
**What are some areas of improvement for future experiments? Why might these improvements help the model?**
1. Adding more data. Presence of more data results in better and accurate models.
2. Treat missing and Outlier values which often reduces the accuracy of a model or leads to a biased model.
3. Handling imbalanced data so as to decrease model bias.
4. Tuning the algorithm. The objective of parameter tuning is to find the optimum value for each parameter to improve the accuracy of the model
## Proof of cluster clean up
**If you did not delete your compute cluster in the code, please complete this section. Otherwise, delete this section.**
**Image of cluster marked for deletion**
![alt text](https://github.com/vikkyfama/An_Azure_ML_Pipeline/blob/toria/ClusterCleanUp.png)
