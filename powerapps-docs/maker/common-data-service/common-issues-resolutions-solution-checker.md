---
title: "Common issues and resolutions for Solution Checker | Microsoft Docs" # Intent and product brand in a unique string of 43-59 chars including spaces"
description: " A list of common issues and resolutions within Solution Checker"
keywords: ""
ms.date: 02/11/2019
ms.service:
  - powerapps
ms.custom:
  - ""
ms.topic: article
ms.assetid: caa4e3f2-9700-49b8-87ed-8a68e8878b02
author: jowells1 # GitHub ID
ms.author: jowells # MSFT alias of Microsoft employees only
manager: austinj # MSFT alias of manager or PM counterpart
ms.reviewer: 
robots: noindex,nofollow
search.audienceType: 
  - developer
search.app: 
  - PowerApps
  - D365CE
---
# Common issues and resolutions for Solution Checker

This article lists some common issues that you might encounter while using Solution Checker. Where applicable, workarounds are provided.

## Solution Checker runs fail due to PowerApps Checker version installed
Solution Checker is a feature included with the PowerApps Checker solution.  If you have a PowerApps Checker version earlier than 1.0.0.47, Solution Checker runs will fail to complete successfully. You should upgrade your PowerApps Checker version from the [!INCLUDE [pn-dyn-365-admin-center](../../includes/pn-dyn-365-admin-center.md)]. 

However, if you have a PowerApps Checker version earlier than 1.0.0.45 installed, we recommend that you delete the solution and install it again. Due to recent schema changes, upgrade of PowerApps Checker from versions earlier than 1.0.0.45 may fail.

If you want to keep the past results from Solution Checker, export the results from a previous run or export all Solution Checker data using [Export data to Excel](../../user/export-data-excel.md) to export the data from the following entities:

- Analysis Component
- Analysis Job
- Analysis Result
- Analysis Result Detail

### Delete PowerApps Checker

To delete the PowerApps Checker solution:

1. As a System Administrator or as a System Customizer, open the PowerApps portal by going to https://web.powerapps.com/environments.
2. Select **Solutions**.
3. Select **PowerApps Checker**, and then on the solutions toolbar select **Delete**.

### Add PowerApps Checker

To add PowerApps Checker back to your Common Data Service environment:

1. As a System Administrator or as a System Customizer, open up your PowerApps portal by going to https://web.powerapps.com/environments.
2. Select **Solutions**.
3. On the solution toolbar select **Solution Checker**, and then select **Install**.

## Runs on large solutions fail

There are a couple different scenarios that can occur if a solution is too large. These scenarios are further explained below. The solution to each scenario is to create smaller solutions with fewer components to be analyzed. If the large file size of the solution is due to plug-in assembly components, please see guidance to [Optimize custom assembly development](../../developer/common-data-service/best-practices/business-logic/optimize-assembly-development.md).

Solution Checker can fail to check a solution based on these scenarios:
- Hard limitation of a solution file size limit of 30MB.  
- 10-minute timeout for exporting a solution from the Common Data Service environment. Large solutions, like the Default Solution, may fail to get exported within this time, and the check will not complete successfully. Solution Checker will retry three times before it fails to process the job, so it may take over 30-minutes before you receive a failure notification.

To address these issues, check or create smaller solutions to be analyzed. To minimize false positives, ensure you add dependent customizations. When you create a solution and add these components, include the following:

- When you add plug-ins, include the SDK Message Processing Steps for the plug-in.
- When you add entity forms, include the JavaScript web resources attached to the form events.  
- When you add JavaScript web resources, include any dependent JavaScript web resources.
- When you add HTML web resources, include any dependent scripts that are defined within the HTML web resource.
- When you add custom workflows, include the assembly used within the workflow.

## Solution Checker run or download results don't complete 
Shortly after running Solution Checker the operation doesn't complete and the following message is displayed:<br />
"We weren't able to run the check on *SOLUTIONNAME* Solution. Try running it again." <br />
![Weren't able to run](media/solution-checker-werent-able-to-run.png)

This issue occurs because the organization is in the **administration mode** state and Solution Checker is unable to validate the user's permissions executing the request. To resolve this issue, disable administration mode. 

### Disable administration mode for an instance
1. Access the Dynamics 365 for Customer Engagement instance picker: https://port.crm.dynamics.com/G/Instances/InstancePicker.aspx.
2. Select the instance that has issues running Solution Checker.
3. Select **ADMIN**.<br />
![Instance Admin](media/solution-checker-instance-admin.png)

4. Clear **Enable administration mode**. <br />
![Disable Admin mode](media/solution-checker-instance-disable-admin-mode.png)

5. Run Solution Checker again.

## Solution Checker will not process patched solutions

If a solution has had a [patch](https://docs.microsoft.com/powerapps/developer/common-data-service/create-patches-simplify-solution-updates) applied, Solution Checker will fail to export the solution for analysis. When a solution has had a patch applied, the original solution becomes locked and it can’t be changed or exported as long as there are dependent patches that exist in the organization that identify the solution as the parent solution.

To address this issue, clone the solution so that all patches related to the solution are rolled into the newly-created solution. This unlocks the solution and allows the solution to be exported from the system. More information: [Clone a solution](use-segmented-solutions-patches-simplify-updates.md#clone-a-solution)

## Line number references for issues in HTML resources with embedded JavaScript are not correct 

When HTML web resources are processed within Solution Checker, the HTML web resource is processed separately than the JavaScript within the HTML web resource. Due to this, the line number of the violation found within `<script>` of the HTML web resource will not be correct.

## Web-unsupported-syntax issue for web resources

ECMAScript 6 (2015) or later versions are not currently supported for Solution Checker. When Solution Checker analyzes JavaScript using ECMAScript 6 or later, a web-supported-syntax issue for the web resource is reported.  

## Multiple violations reported for plug-ins and workflow activities based on call scope

For plug-in and workflow activity rules where the issue is only relevant in the calling context, the Solution Checker tool starts its analysis at the IPlugin interface implementation and traverses a call graph to detect issues within the scope of that implementation.  In some cases, many call paths may arrive at the same location where the issue is detected.  Since the issue is relevant to the call scope, the tool may report based on that scope to provide a better picture of impact rather than on distinct locations. As a result, multiple issues may reference a single location that should be fixed.

## See also
[Best practices and guidance for the Common Data Service](../../developer/common-data-service/best-practices/index.md)<br />
[Best practices and guidance for model-driven apps](../../developer/model-driven-apps/best-practices/index.md)<br />
