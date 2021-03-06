# Introduction
This repository contains a collection of Azure functions to collect data and send to Sumo Logic cloud service, and a library called sumo-function-utils for these functions.

| TLS Deprecation Notice |
| --- |
| In keeping with industry standard security best practices, as of May 31, 2018, the Sumo Logic service will only support TLS version 1.2 going forward. Verify that all connections to Sumo Logic endpoints are made from software that supports TLS 1.2. |

## Deploying via ARM
Go to Resource Groups Service and create a Resource Group.
Click on + icon on left and search for template deployment.Click on it.Then in new window click on create.
Now choose build your own template in the editor option and in new window upload azuredeploy.json file and click on save.
In new window check the T&C and click on Purchase.

If you get any error it may be due to multiple deployments of this function so delete old deployments before creating new one.

This will most of the resources and configurations. Some specific Azure integration require extra configuration.

EventHubs:
*  Click on Storage Account Service and select sumoazureauditfaildata(for EventHubs function) or sumometricsfaildata(for EventHubs_metrics function) storage account.
*  Then select container under Blob and create a container by clicking on + button.
*  Input azureaudit-failover(for EventHubs function) or sumomet-failover(for EventHubs_metrics function) as name and choose private in public access level

## Exporting Logs to EventHub
*Metrics*
* Login to Azure Portal
* Click on all services in left panel -> search monitor and click on it -> on left side under settings you will find diagnostic settings -> List of resources -> Select a resource
* In the new window select ALL Metrics option and click on configure eventhub
* Select SumoMetricsNamespace as Eventhub namespace, insights-metrics-pt1m as Eventhub name and  RootManageSharedAccessKey as eventhub policy name. Click ok.
* Save the setting

*Acitivity Logs*
* Login to Azure Portal
* Click on all services in left panel -> search activity logs and click on it
* Click on export button and in the new window check the export to eventhub checkbox and select SumoAzureAudit as Eventhub namespace and RootManageSharedAccessKey as eventhub policy name.Click ok.
* Save the setting


## Building the function
Currently ARM template is integrated with github and for each functions
logs_build/EventHubs - Function for ingesting Activity Logs
metrics_build/EventHubs_Metrics - Function for ingesting Metrics Data

## For Developers
`npm run build`
This command copies required files in two directories logs_build(used for activity logs ingestions) and metrics_build(used for metrics data(in diagnostic settings) ingestion)

Integrations tests are in tests folder and unit tests are in sumo-function-utils/tests folder

