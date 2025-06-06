---
title: Example log table queries for ASRJobs
description:  Example queries for ASRJobs log table
ms.topic: generated-reference
ms.service: azure-monitor
ms.author: edbaynash
author: EdB-MSFT
ms.date: 04/14/2025

# This file is automatically generated. Changes will be overwritten. Do not change this file directly. 

---

# Queries for the ASRJobs table

For information on using these queries in the Azure portal, see [Log Analytics tutorial](/azure/azure-monitor/logs/log-analytics-tutorial). For the REST API, see [Query](/rest/api/loganalytics/query).


### Get all test failover jobs run  


Get all test failover jobs run for your ASR protected items to verify if recoverability is being tested regularly for all your important resources.  

```query
ASRJobs
//| where TimeGenerated >= ago(30d) // uncomment this line to view last 30 days
| summarize arg_max(TimeGenerated,*) by JobUniqueId
| where OperationName == "Test failover"
| project StartTime, EndTime, SourceResourceId, SourceFriendlyName, DurationMs, ResultDescription
```

