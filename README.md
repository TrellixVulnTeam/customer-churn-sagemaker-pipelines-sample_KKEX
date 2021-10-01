# Customer-Churn-SageMaker

## Description
Customer Churn Model to predict if a customer would be retained or not.
1. Store Retail Dataset (https://www.kaggle.com/uttamp/store-data)
2. Binary Classification Model using the SageMaker XGBoost Framework

## Run Code:
1. As an initial step, run through Create_Upload_Datasets_to_S3.ipynb notebook to generate and upload the required datasets to S3 Scratchpad Space.
2. The Customer_Churn_Modeling.ipynb can be used to run through each step of the model development in an interactive way.
3. SageMaker_Pipelines_Project.ipynb is to launch the SM Pipeline which would orchestrate each step of the model from preprocessing through creating model, running predictions etc..

## SageMaker Pipeline Workflow for Churn Model
![plot](img/SMPipeline_ChurnModel.png)

## Steps
1. ChurnModelProcess (SageMaker Processing Step)-  Preprocess data to build the features required and split data in train/validation/test datasets.
2. ChurnHyperParameterTuning (SageMaker Tuning Step) - Applies HyperParameterTuning based on the ranges provided with the SageMaker XGBoost Framework to give the best model which is determined based on AUC Score.
3. ChurnEvalBestModel (SageMaker Processing Step)- Evaluates the best model using the test dataset.
4. CheckAUCScoreChurnEvaluation (SageMaker Condition Step)- Check if the AUC Score is above a certain threshold.If true, run through the below steps.
5. RegisterChurnModel (SageMaker Register Model Step)- Register the trained churn model onto SageMaker Model Registry.
6. ChurnCreateModel (SageMaker Create Model Step)- Creates SageMaker Model by taking the artifacts of the best model
7. ChurnTransform (SageMaker Transform Step)- Applies Batch Transform on the given dataset by using the model created in the previous step
8. ChurnModelConfigFile (SageMaker Processing Step)- Creates the config file which includes information as to which columns to check bias on, baseline values for generating SHAPley plots etc..
9. ClarifyProcessing Step (SageMaker Processing Step)- Applies SageMaker Clarify using the config file created in the previous step to generate Model Explainability, Bias Information reports.