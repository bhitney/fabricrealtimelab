# Microsoft Fabric End-to-End Real-Time Analytics Workshop

Welcome to the Microsoft Fabric end-to-end workshop!

In this workshop, we'll start by rapidly generating real-time data using Fabric Real-time Intelligence (RTI) and understanding how that data can be processed and visualized in Microsoft Fabric. With the initial reporting in place, multiple modules are available that explore data warehousing, data lakehouse architecture, data activator, data science, and of course, real-time intelligence leveraging KQL. The modules are designed to be cohesive but flexible -- they all involve the same core scenario, but have limited dependencies so you can consume the modules that make the most sense for you. 

The basic architecture of our solution is illustrated below. The app deployed in the beginning of this workshop (either as a docker container or running in Jupyter notebook) will publish events to our Fabric environment. The data is ingested into a KQL database for real-time reporting in Power BI. 

![Data Lakehouse with Azure Synapse Analytics](./images/readme/ArchitectureSlide1.png)

## About the scenario

In this workshop, participants will get hands-on with a fictitious financial company "AbboCost." AbboCost would like to set up a stock monitoring platform to monitor price fluctuations and report on historical data. Throughout the workshop, we'll look at how every aspect of Microsoft Fabric can be incorporated as part of a larger solution -- by having everything in an integrated solution, we'll be able to quickly and securely integrate data, build reports, create data warehouses and lakehouses, forecast using ML models, and more.

## :thinking: Prerequisites

* An [Azure account](https://azure.microsoft.com/free/) with an active subscription.
  * If you are working through this content as part of a proctored workshop, your proctor may be able to provide you with an Azure Pass or preconfigured lab environment. 
  * If you are a [Visual Studio subscriber](https://azure.microsoft.com/en-us/pricing/member-offers/credit-for-visual-studio-subscribers/), you may leverage this benefit to host the data in your account.
  * If you don't have access to an Azure subscription, you may be able to sign up for a [free account](https://www.azure.com/free).
* You must have the necessary privileges within your Azure subscription to create resources, create Microsoft Fabric capacity (if not using a trial or other environment), register resource providers (if required), etc. Note that some organizational accounts may have administrative restrictions on Power BI features, which may limit functionality. 

## :books: Learning Modules

Core Modules for real-time analytics:

0. [Environment Setup](./modules/module00.md)
1. [KQL Database Configuration and Ingestion](./modules/module01.md)
2. [Exploring the Data](./modules/module02.md)
3. [Reporting in Power BI](./modules/module03.md)

Additional modules for end-to-end solution:

4. [Data Activator](./modules/module04.md)
5. [Data Warehousing: Synapse Data Warehouse and Data Pipelines](./modules/module05a.md)
6. [Data Lakehouse: Fabric Lakehouse, Notebooks, and Data Wrangler](./modules/module06a.md)
7. [Data Science: Using the Lakehouse and Notebooks to Forecast](./modules/module07a.md)

Continued Learning & Additional Modules

Extras: [Additional Content and Continued Learning](./modules/moduleex00.md)
   * Ex-01: [Advanced KQL](./modules/moduleex01.md)
   * Ex-02: [Additional Real-time Dashboard](./modules/moduleex02.md)
   * Ex-03: [Predicted vs Actual Reporting](./modules/moduleex03.md)
   * Ex-04: [Generate Custom Historical Data](./modules/moduleex04.md)
   * Ex-05: [Delta File Maintenance](../modules/moduleex05.md)

## :laughing: Some comedic relief

Names for the stocks and scenario were inspired by the iconic skit "Who's on First?" by Abbott and Costello. View this skit here: [Who's on First by Abbott and Costello (6:35)](https://www.youtube.com/watch?v=sYOUFGfK4bU)

<div align="right"><a href="#fabric-real-time-workshop">↥ back to top</a></div>

