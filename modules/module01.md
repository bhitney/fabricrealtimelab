# Module 01 - KQL Database (Eventhouse) Configuration and Ingestion

[< Previous Module](./module00.md) - **[Home](../README.md)** - [Next Module >](./module02.md)

## :stopwatch: Estimated Duration

30 minutes

## :thinking: Prerequisites

- [x] Lab environment deployed from [setup](../modules/module00.md)

## :loudspeaker: Introduction

With our environment setup complete, we will complete the ingestion of the Eventstream so the data is ingested into a KQL database. All KQL databases are stored within an *eventhouse*; the eventhouse acts as a workspace for one or more KQL databases. Multiple KQL databases can be stored within the same eventhouse, but in this case, we'll have only a single KQL database. This data will also be stored in Fabric OneLake. 

Prefer video content? These videos illustrate the content in this module:
* [Getting Started with Real-time Analytics in Microsoft Fabric](https://youtu.be/wGox1lf0ve0)

## Table of Contents

1. [Create Eventhouse](#1-create-eventhouse)
2. [Send data from the Eventstream to the KQL database](#2-send-data-from-the-eventstream-to-the-kql-database)

## 1. Create Eventhouse

Kusto Query Language (KQL) is the query language used by an Eventhouse/KQL databases within Fabric (along with several other solutions, like Azure Data Explorer, Log Analytics, Microsoft 365 Defender, and others). Similar to Structured Query Language (SQL), KQL is optimized for ad-hoc queries over big data, time series data, and data transformation. 

To work with the data, we'll create a KQL database and stream data from the Eventstream into the KQL DB. 

In the Fabric portal, switch to the Real-Time Intelligence workload by using the workload switcher in the bottom left. This helps contextualize the menus for the features most often used for the current tasks:

![Fabric Workload](../images/module01/workload.png)

Select the Home icon on the left nav, then click *Eventhouse*, and name the eventhouse *StockDB*. This will create both the eventhouse and the KQL database with the name StockDB. 

![New KQL Database](../images/module01/createkqldb.png)

The Eventhouse main page will be displayed; click on the StockDB database under KQL databases:

![New Eventhouse](../images/module01/eventhousemainpage.png)

When the eventhouse and KQL database is created, we should see the KQL details. An important setting for us to change immediately is enabling *OneLake folders*, which is inactive by default. While this can be changed later, only tables created after the setting is activated will be included in OneLake. Click on the pencil to change the setting and enable OneLake access:

![Enable OneLake](../images/module01/kqlenableonelake.png)

After enabling OneLake, the OneLake folder should show as active. This will now make the data available in OneLake for other processing (notebooks, pipelines, dataflows...); however, we can also add an endpoint to the eventstream as we'll see later on.

![OneLake Active](../images/module01/kqlonelakeactive.png)

## 2. Send data from the Eventstream to the KQL database

Navigate back to the Eventstream created in the previous module. Our data should be arriving into our Eventstream, and we'll now configure the data to be ingested into the KQL database we created above. On the Eventstream, click *New destination* and select KQL Database:

![Eventstream](../images/module01/eventstream-kql.png)

On the KQL settings, select *Direct ingestion*. While we have the opportunity to process event data at this stage, for our purposes, we will ingest the data directly into the KQL database. Set the destination name to *KQL*, then select your workspace and KQL database created above:

![Eventstream KQL Settings](../images/module01/eventstream-kqlsettings.png)

On the first settings page, enter the name *StockPrice* for the table to hold the data in StockDB:

![Eventstream KQL Settings Page 1](../images/module01/eventstream-kqlconfig1.png)

The next page allows us to inspect and configure the schema. Be sure to change the format from TXT to JSON, if necessary. The default columns of *symbol*, *price*, and *timestamp* should be formatted like the image below; then click *Finish*:

![Eventstream KQL Settings Page 2](../images/module01/eventstream-kqlconfig2.png)

On the final page, verify the settings show a green checkmark and if there are no errors, click *Close* to complete the configuration:

![Eventstream KQL Settings Page 3](../images/module01/eventstream-kqlconfig3.png)

## :tada: Summary

With the above steps completed, we have created a KQL database, and configured the database to ingest data from the Eventstream. 

## :white_check_mark: Results

- [x] Created the Eventhouse/KQL Database
- [x] Completed the ingestion process from the Eventstream to the KQL database

[Continue >](./module02.md)
