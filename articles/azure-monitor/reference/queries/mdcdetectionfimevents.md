---
title: Example log table queries for MDCDetectionFimEvents
description:  Example queries for MDCDetectionFimEvents log table
ms.topic: generated-reference
ms.service: azure-monitor
ms.author: edbaynash
author: EdB-MSFT
ms.date: 04/14/2025

# This file is automatically generated. Changes will be overwritten. Do not change this file directly. 

---

# Queries for the MDCDetectionFimEvents table

For information on using these queries in the Azure portal, see [Log Analytics tutorial](/azure/azure-monitor/logs/log-analytics-tutorial). For the REST API, see [Query](/rest/api/loganalytics/query).


### All FIM events for directories  


Get all FIM events against directories of the host.  

```query
MDCDetectionFimEvents
| where IsDir == "True"
| order by TimeGenerated
| limit 100
```

