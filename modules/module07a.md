# Module 07a - Data Science: Building and storing an ML model

[< Previous Module](./module06c.md) - **[Home](../README.md)** - [Next Module >](./module07b.md)

## :stopwatch: Estimated Duration

* 40 minutes for 07a
* 2 hours overall

## :thinking: Prerequisites

- [x] Access to a Fabric environment

Recommended modules:

- [x] Completed [Module 01 - KQL Database](../modules/module01.md)
- [x] Completed [Module 02 - KQL Queries](../modules/module02.md)
- [x] Completed [Module 03 - Reporting](../modules/module03.md)
- [x] Completed [Module 06 - Lakehouse](../modules/module06a.md)

We've designed module to be stand-alone with limited dependencies on earlier modules. However, [Module 07c - Solution in practice](./module07c.md) requires [Module 06 - Lakehouse](./module06a.md) to be completed. 

[Module 07a](./module07a.md) and [Module 07b](./module07b.md) demonstrate building a model, storing the model in MLflow, retrieving the model, and generating predictions. [Module 07c](./module07c.md) shows how these principles can be applied to the solution as a whole, while [Module 07d](./module07d.md) illustrate building a semantic model and including the data in a Power BI report.

If you have not created any Fabric resources and consuming this module as a stand-alone exercise, you can follow the instructions in [Module 00 - Create Fabric Capacity](./module00.md#2-create-fabric-capacity-and-workspace) to get a Fabric environment up and running.

## :book: Sections

This module is broken down into 4 sections:

* [Module 07a - Building and storing an ML model](./module07a.md)
* [Module 07b - Using models, saving to the lakehouse](./module07b.md)
* [Module 07c - Solution in practice](./module07c.md)
* [Module 07d - Building a Prediction Report](./module07d.md)

## :loudspeaker: Introduction

As a refresher on the scenario, AbboCost Financial has been modernizing their stock market reporting for their financial analysts. In previous modules, we've developed the solution by creating real-time dashboards, a data warehouse, and more.

In this module, AbboCost would like to explore predictive analytics help inform their advisors. The goal is to analyze historical data for patterns that can be used to create forecasts of future values. Microsoft Fabric's Data Science capability is the ideal place to do this kind of exploration.

Prefer video content? These videos illustrate the content in this module:
* [Getting Started with Data Science in Microsoft Fabric, Part 1](https://youtu.be/kdUIUPwIy4g)
* [Getting Started with Data Science in Microsoft Fabric, Part 2](https://youtu.be/GFTDxnPDTpQ)

## :bulb: About Notebooks

Most of this lab will be done within a Jupyter notebook, an industry standard way of doing exploratory data analysis, building models, and visualizing datasets, and processing data. A notebook itself is separated into individual sections called cells which contain code or text documentation. Cells, and even sections within cells, can adapt to different languages as needed (though Python is generally the most used language). The purpose of the cells are to break tasks down into manageable chunks and make collaboration easier; cells may be run individually or as a whole depending on the purpose of the notebook. 

## :crystal_ball: About Prophet

[Prophet](https://facebook.github.io/prophet/) is a library created by Facebook's core data science team to analyze and predict time-series data. Prophet is univariate, meaning it only uses a single variable (our stock price at a given point in time). This is ideal for our scenario because this is the extent of the data that is currently available in our feed. In more complex scenarios, we'd typically use a multivariate model to find correlations -- for example, is weather impacting our sales data? This eliminates the need for feature engineering in this context.

Additionally, Prophet handles outliers and missing data well. In many data science scenarios, we'd do extensive cleaning and data preparation, such as looking for erroneous, missing, or null values. Fabric has this capability built-in with [Data Wrangler](https://learn.microsoft.com/en-us/fabric/data-science/data-wrangler), but is not needed in this circumstance.

Finally, Prophet has built-in validation. In developing most models, we'd typically divide the data into training and test datasets. The training dataset is used to create the model, and this model is then subsequently evaluated using the test dataset. This is the gold standard because a model is most accurately evaluated by using data the model has not yet seen. While this can be done manually, the built in validation routines provides functionality to perform model validation in a number of ways, as you'll see in the notebook.

## Table of Contents

1. [Download the notebook](#1-download-the-notebook)
2. [Prepare the environment](#2-prepare-the-environment)
3. [Import the notebook](#3-import-the-notebook)
4. [Explore the notebook](#4-explore-the-notebook)
5. [Run the notebook](#5-run-the-notebook)
6. [Examine the model and runs](#6-examine-the-model-and-runs)

## 1. Download the notebook

Notebook files are JSON documents with an .ipynb extension (short for Interactive Python Notebook). To view each notebook, click on the notebook link below. The notebook is presented in a readable format in GitHub -- click the download button near the upper right to download the notebook, and save the .ipynb notebook file to a convenient location. There are several notebooks referenced throughout this module. 

All resources (notebooks, scripts, etc.) for all modules can be downloaded in this zip file:

* [All Workshop Resources (resources.zip)](https://github.com/microsoft/fabricrealtimelab/raw/main/files/resources.zip)

Individually view and download:

* [Download the DS 1 - Build Model Notebook](<../resources/module07/DS 1 - Build Model.ipynb>)
* [Download the DS 2 - Predict Stock Prices Notebook](<../resources/module07/DS 2 - Predict Stock Prices.ipynb>)
* [Download the DS 3 - Forecast All](<../resources/module07/DS 3 - Forecast All.ipynb>)

![Download Notebook](../images/module07/downloadnotebook.png)

## 2. Prepare the environment

For most tasks within data science, we'll need to persist data within a Lakehouse. If you've completed the Lakehouse module, you already have a Lakehouse and can skip this step. Alternatively, if you have a Lakehouse in an existing Fabric environment, you can skip this step.

Within your Fabric workspace, select *New* > *Lakehouse*, and create a new lakehouse named *StocksLakehouse*. 

![New Lakehouse](../images/module07/newlakehouse.png)

## 3. Import the notebook

Switch to the Data Science workload using the workload switcher in the bottom left. On the Data Science homepage, click *Import notebook*, and import the notebooks listed above. Once imported, a notification should appear allowing you to switch directly to the workspace listing all of the assets.

![New Lakehouse](../images/module07/importnotebook.png)

Once imported, the notebooks should be visible in the workspace -- as your workspace grows, use filtering to narrow the types of items visible:

![Workspace](../images/module07/workspacelist.png)

Click the *DS 1 - Build Model* notebook to open. The notebook should appear like the image below:

![Notebook](../images/module07/defaultnotebook.png)

Once loaded, click *Add Lakehouse* to the left of the notebook, select *Existing Lakehouse*, and select the *StocksLakehouse* you created earlier and click *Add*. This action adds the lakehouse to the context of the existing notebook and makes it the default lakehouse. (Note: you'll need to do this for every imported notebook.) This should look similar to the image below:

![Notebook with lakehouse](../images/module07/notebookwithlakehouse.png)

If this is a new lakehouse, there should be no tables or files, but if you're continuing from another module that used a lakehouse, you may see other tables or files.

## 4. Explore the notebook

The *DS 1* notebook is documented throughout the notebook, but in short, the notebook carries out the following tasks:

* Allows us to configure a stock to analyze (such as WHO or IDGD)
* Downloads historical data to analyze in CSV format
* Reads the data into a dataframe
* Completes some basic data cleansing
* Loads [Prophet](https://facebook.github.io/prophet/), a module for conducting time series analysis and prediction
* Builds a model based on historical data
* Validates the model
* Stores the model using MLflow
* Completes a future prediction

The routine that generates the stock data is largely random, but there are some trends that should emerge. Because we don't know when you might be doing this lab, we've generated several years worth of data. The notebook will load the data and will truncate future data when building the model. As part of an overall solution, we'd then supplement the historical data with new real-time data in our lakehouse, re-training the model as necessary (daily/weekly/monthly).

> :bulb: **Notes about data prediction:**
> The purpose of this module is to demonstrate how to approach a data science solution in Fabric. Predicting stock prices (fictitious or otherwise) is challenging, and potentially impossible. The focus is on the method and concepts, not improving the accuracy of the model (as measured by root mean squared error, r2, and other metrics). Still, we'll look at various metrics but don't be discouraged if our predictions turn out to be somewhat inaccurate. 

## 5. Run the notebook

You can either run each cell manually as you follow along with the notebook, or click *Run all* in the top toolbar and follow along as the work progresses. The notebook will take roughly 15-20 minutes to execute -- some steps, like training the model and cross validation, will take the longest. If you're waiting for the results, feel free to read ahead or take a quick break!

![Run cells](../images/module07/runcell.png)

From this point, follow along with documentation in the notebook for an explanation of the steps. 

## 6. Examine the model and runs

Experiments and runs can be viewed in the workspace resource list. An experiment acts as a container for a given data science solution -- in this case, we'll create an experiment for every stock. Within each experiment, all of the runs are logged along with their metadata. From the workspace, click on the model we just created (by default, for the WHO stock):

![Model in Workspace resources](../images/module07/resourceswithmodel.png)

Metadata, in our case, includes input parameters we may tune for our model, as well as metrics on the model's accuracy, such as root mean square error (RMSE). RMSE represents the average error -- a zero would be a perfect fit between model and actual data, while higher numbers show an increase error. While lower numbers are better, a "good" number is subjective based on the scenario. 

![Model details](../images/module07/modeldetails.png)

## :thinking: Additional Learning

* [Machine Learning Experiments in Microsoft Fabric](https://learn.microsoft.com/en-us/fabric/data-science/machine-learning-experiment)
* [Data Wrangler](https://learn.microsoft.com/en-us/fabric/data-science/data-wrangler)
* [Prophet](https://facebook.github.io/prophet/)

## :tada: Summary

In this module, you learned how to approach a data science solution in Fabric by importing and executing a notebook that processed data, created a ML model of the data using Prophet, and stored the model in MLflow. Additionally, you evaluated the model using cross validation and tested the model using additional data.

## :white_check_mark: Results

- [x] Loaded a notebook into your Fabric environment, created an ML model and stored the model in MLflow, and evaluated the model performance.

[Continue >](./module07b.md)