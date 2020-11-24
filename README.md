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
![alt text](https://github.com/vikkyfama/An_Azure_ML_Pipeline/blob/toria/Sklearn%20doc.png)
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
The best model generated by the AutoML was the MaxAbsScaler, XGBoostClassifier with an accuracy of 0.91737. Parameters specifications were as follows: experiment_timeout_minutes=30, compute_target = "vikkycompute", task="classification", primary_metric="accuracy", training_data= training_data, label_column_name= "y", n_cross_validations=5. Generated Ensemble iterations include VotingEnsemble: It predicts based on the weighted average of predicted class probabilities, StackEnsemble: stacking combines heterogenous models and trains a meta-model based on the output from the individual models 
## Pipeline comparison
**Compare the two models and their performance. What are the differences in accuracy? In architecture? If there was a difference, why do you think there was one?**
The Hyperdrive Config Model had an Accuracy of 0.9153, while the AutoML Model had an Accuracy of 0.9173 with a difference of 0.0020. The difference in their various architecture is that in the hyperdrive config in addition to being required to manually specify parameters such as an estimator, policy, primary_metric_goal, max_total_runs and max_concurrent_runs we also need to fit the train data manually with a chosen algorithm. AutoML only require us to specify the task, training_data, label_column_name,n_cross_validations and automatically does all the data splitting, cross validation, model fitting and model selection in the background.
## Future work
**What are some areas of improvement for future experiments? Why might these improvements help the model?**
1. Better pre-processing of the data and exploring many features which result in better performance with quality data. Pre-processing techniques such as:
a. Data cleansing routines which attempt to fill in missing values, smooth out noise while identifying outliers, and correct inconsistencies in the data.
b. Data Integration which is a data analysis task that combines data from multiple sources into a coherent data store, as in data warehousing.
c. Data Transformation such as Normalization, Smoothing for removing the noise from the data and Aggregation operations.
d. Performing data reduction techniques which are helpful in analysing the reduced representation of the data set without compromising the integrity of the original data. This      can be achieved with Principle Component Analysis. Feature engineering and feature selection are also beneficial processes of finding the best set of variables and the best data encoding and normalization for input to the model training process. By fixing the data and features before implementation coupled with good data split of 0.1 as test_size we will achieve a class balance as well as a better preformance model void of bias.

2. Using more combinations of hyper parameter tuning with hyperdrive. The objective of parameter tuning is to find the optimum value for each parameter to improve the accuracy of the model. By experimenting with various values of the following parameters: C , max_iter, learning_rate, keep_probabilty and batch_size, we can achieve the best combination of these parameter values that would result in a better performance model.

3. Exploring other parameters of AutoML. Using a performance metric that handles imbalanced data better. For instance, the AUC_weighted which is a primary metric that calculates the contribution of every class based on the relative number of samples representing that class, which makes it more robust against imbalance.

4. Using other Azure Data Science Virtual Machines tools. These are set of tools and libraries for machine learning. They include:
   a. XGBoost which is a fast, portable, and distributed gradient-boosting library for Python.It’s a highly sophisticated algorithm, powerful enough to deal with all sorts of irregularities of data. It has a very useful function called as “cv” which performs cross-validation at each boosting iteration and thus returns the optimum number of trees required. Tune tree-specific parameters ( max_depth, min_child_weight, gamma, subsample, colsample_bytree) for decided learning rate and number of trees.
   b. H2O is an open-source AI platform that supports in-memory, distributed, fast, and scalable machine learning and predictive analytics platform that allows you to build machine learning models on big data and provides easy productionalization of those models in an enterprise environment.The data is read in parallel and is distributed across the cluster and stored in memory in a columnar format in a compressed way. H2O’s data parser has built-in intelligence to guess the schema of the incoming dataset and supports data ingested from multiple sources in various formats.
   C. LightGBM is a gradient boosting framework that uses tree based learning algorithms. It is designed to be distributed and efficient with the following advantages: Faster training speed and higher efficiency. Support of parallel and GPU learning. Capable of handling large-scale data.
   
## Proof of cluster clean up
**If you did not delete your compute cluster in the code, please complete this section. Otherwise, delete this section.**
**Image of cluster marked for deletion**
![alt text](https://github.com/vikkyfama/An_Azure_ML_Pipeline/blob/toria/ClusterCleanUp.png)
