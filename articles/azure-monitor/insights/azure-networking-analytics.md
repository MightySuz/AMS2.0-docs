---
title: Azure Networking Analytics solution in Azure Monitor | Microsoft Docs
description: You can use the Azure Networking Analytics solution in Azure Monitor to review Azure network security group logs and Azure Application Gateway logs.
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 06/21/2018 
ms.custom: devx-track-azurepowershell

---

# Azure networking monitoring solutions in Azure Monitor

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Azure Monitor offers the following solutions for monitoring your networks:
* Network Performance Monitor (NPM) to
    * Monitor the health of your network
* Azure Application Gateway analytics to review
    * Azure Application Gateway logs
    * Azure Application Gateway metrics
* Solutions to monitor and audit network activity on your cloud network
    * [Traffic Analytics](../../networking/network-monitoring-overview.md#traffic-analytics) 

## Network Performance Monitor (NPM)

The [Network Performance Monitor](../../networking/network-monitoring-overview.md) management solution is a network monitoring solution, that monitors the health, availability and reachability of networks.  It is used to monitor connectivity between:

* Public cloud and on-premises
* Data centers and user locations (branch offices)
* Subnets hosting various tiers of a multi-tiered application.

For more information, see [Network Performance Monitor](../../networking/network-monitoring-overview.md).


## Azure Application Gateway analytics

1. Enable diagnostics to direct the diagnostics to a Log Analytics workspace in Azure Monitor.
2. Consume the detailed summary for your resource using the workbook template for Application Gateway.

If diagnostic logs are not enabled for Application Gateway, only the default metric data would be populated within the workbook.


## Review Azure networking data collection details
The Azure Application Gateway analytics and the Network Security Group analytics management solutions collect diagnostics logs directly from Azure Application Gateways and Network Security Groups. It is not necessary to write the logs to Azure Blob storage and no agent is required for data collection.

The following table shows data collection methods and other details about how data is collected for Azure Application Gateway analytics and the Network Security Group analytics.

| Platform | Direct agent | Systems Center Operations Manager agent | Azure | Operations Manager required? | Operations Manager agent data sent via management group | Collection frequency |
| --- | --- | --- | --- | --- | --- | --- |
| Azure |  |  |&#8226; |  |  |when logged |


### Enable Azure Application Gateway diagnostics in the portal

1. In the Azure portal, navigate to the Application Gateway resource to monitor.
2. Select *Diagnostics Settings* to open the following page.

   ![Screenshot of the Diagnostics Settings config for Application Gateway resource.](media/azure-networking-analytics/diagnostic-settings-1.png)

   [ ![Screenshot of the page for configuring Diagnostics settings.](media/azure-networking-analytics/diagnostic-settings-2.png)](media/azure-networking-analytics/application-gateway-diagnostics-2.png#lightbox)

5. Click the checkbox for *Send to Log Analytics*.
6. Select an existing Log Analytics workspace, or create a workspace.
7. Click the checkbox under **Log** for each of the log types to collect.
8. Click *Save* to enable the logging of diagnostics to Azure Monitor.

#### Enable Azure network diagnostics using PowerShell

The following PowerShell script provides an example of how to enable resource logging for application gateways.

```powershell
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$gateway = Get-AzApplicationGateway -Name 'ContosoGateway'

Set-AzDiagnosticSetting -ResourceId $gateway.ResourceId  -WorkspaceId $workspaceId -Enabled $true
```

#### Accessing Azure Application Gateway analytics via Azure Monitor Network insights

Application insights can be accessed via the insights tab within your Application Gateway resource.

![Screenshot of Application Gateway insights ](media/azure-networking-analytics/azure-appgw-insights.png
)

The "view detailed metrics" tab will open up the pre-populated workbook summarizing the data from your Application Gateway.

[ ![Screenshot of Application Gateway workbook ](media/azure-networking-analytics/azure-appgw-workbook.png)](media/azure-networking-analytics/application-gateway-workbook.png#lightbox)

### New capabilities with Azure Monitor Network Insights workbook

> [!NOTE]
> There are no additional costs associated with Azure Monitor Insights workbook. Log Analytics workspace will continue to be billed as per usage.

The Network Insights workbook allows you to take advantage of the latest capabilities of Azure Monitor and Log Analytics including:

* Centralized console for monitoring and troubleshooting with both [metric](../insights/network-insights-overview.md#resource-health-and-metrics) and log data.

* Flexible canvas to support creation of custom rich [visualizations](../visualize/workbooks-overview.md#visualizations).

* Ability to consume and [share workbook templates](../visualize/workbooks-overview.md#workbooks-versus-workbook-templates) with wider community.

To find more information about the capabilities of the new workbook solution check out [Workbooks-overview](../visualize/workbooks-overview.md)

## Migrating from Azure Gateway analytics solution to Azure Monitor workbooks

> [!NOTE]
> Azure Monitor Network Insights workbook is the recommended solution for accessing metric and log analytics for your Application Gateway resources.

1. Ensure [diagnostics settings are enabled](#enable-azure-application-gateway-diagnostics-in-the-portal) to store logs into a Log Analytics workspace. If it is already configured, Azure Monitor Network Insights workbook will be able to consume data from the same location and no additional changes are required.

> [!NOTE]
> All past data is already available within the workbook from the point diagnostic settings were originally enabled. There is no data transfer required.

2. Access the [default insights workbook](#accessing-azure-application-gateway-analytics-via-azure-monitor-network-insights) for your Application Gateway resource. All existing insights supported by the Application Gateway analytics solution will be already present in the workbook. You can extend this by adding custom [visualizations](../visualize/workbooks-overview.md#visualizations) based on metric & log data.

3. After you are able to see all your metric and log insights, to clean up the Azure Gateway analytics solution from your workspace, you can delete the solution from the solution resource page.

[ ![Screenshot of the delete option for Azure Application Gateway analytics solution.](media/azure-networking-analytics/azure-appgw-analytics-delete.png)](media/azure-networking-analytics/application-gateway-analytics-delete.png#lightbox)


## Troubleshooting
[!INCLUDE [log-analytics-troubleshoot-azure-diagnostics](../../../includes/log-analytics-troubleshoot-azure-diagnostics.md)]

## Next steps
* Use [Log queries in Azure Monitor](../logs/log-query-overview.md) to view detailed Azure diagnostics data.

