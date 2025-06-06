---
title: Example log table queries for ACSCallAutomationIncomingOperations
description:  Example queries for ACSCallAutomationIncomingOperations log table
ms.topic: generated-reference
ms.service: azure-monitor
ms.author: edbaynash
author: EdB-MSFT
ms.date: 05/19/2025

# This file is automatically generated. Changes will be overwritten. Do not change this file directly. 

---

# Queries for the ACSCallAutomationIncomingOperations table

For information on using these queries in the Azure portal, see [Log Analytics tutorial](/azure/azure-monitor/logs/log-analytics-tutorial). For the REST API, see [Query](/rest/api/loganalytics/query).


### Call Automation operations  


Returns all distinct combinations of call automation operation and version pairs.  

```query
ACSCallAutomationIncomingOperations
| distinct OperationName, OperationVersion 
| limit 100
```



### Calculate Call Automation operation duration percentiles  


Calculates the 90th, 95th, and 99th percentiles of run duration in milliseconds for each call automation operation. It can be customized to be run for a single operation, or for other percentiles.  

```query
ACSCallAutomationIncomingOperations
// where OperationName == "<operation>" // This can be uncommented and specified to calculate only a single operation's duration percentiles
| summarize percentiles(DurationMs, 90, 95, 99) by OperationName, OperationVersion // calculate 90th, 95th, and 99th percentiles of each Operation
| limit 100
```



### Top 5 IP addresses per Call Automation operation  


For every call automation operation, fetch the 5 IP addresses that have called that operation the most.  

```query
ACSCallAutomationIncomingOperations
// | where OperationName == "<operation>" // This can be uncommented and specified to calculate only a single operation's count
| top-nested of OperationName by dummy=max(0), // For all the Operations...
  top-nested 5 of CallerIpAddress by count() // List the IP address that have called that operation the most
| project-away dummy // Remove dummy line from the result set
| limit 100
```



### Call Automation operational errors  


List every call automation error ordered by recency.  

```query
ACSCallAutomationIncomingOperations
| where ResultType == "Failed"
| project TimeGenerated, OperationName, OperationVersion, ResultSignature
| order by TimeGenerated desc
| limit 100
```



### Call Automation operation result counts  


For every call automation operation, count the types of returned results.  

```query
ACSCallAutomationIncomingOperations
| summarize Count = count() by OperationName, ResultType //, ResultSignature // This can also be uncommented to determine the count of each ResultSignature for each ResultType 
| order by OperationName asc, Count desc
| limit 100
```



### Call Automation logs for call connection ID  


Queries Call Automation logs for a particular call connection ID.  

```query
ACSCallAutomationIncomingOperations
//| where CallConnectionId == "<callConnectionId>" // This can be uncommented to filter on a specific call connection ID
| limit 100

```



### Call Automation API operations on a call  


Returns all Call Automation API operation and version pairs for a specific call (correlation ID).  

```query
ACSCallAutomationIncomingOperations
//| where CorrelationId == "<correlation ID>" // This can be uncommented to filter on a specific correlation ID
| project CorrelationId, OperationName, OperationVersion
| limit 100
```



### CallDiagnostics log for CallAutomation API call  


Queries the diagnostics log for a call which was interacted with by Call Automation API using correlation ID.  

```query
ACSCallAutomationIncomingOperations 
//| where CorrelationId == "<correlation ID>" // This can be uncommented to filter on a specific correlation ID
| join kind=inner
    (ACSCallDiagnostics)
    on CorrelationId
| limit 100

```



### CallSummary log for CallAutomation API call  


Queries the summary log for a call which was interacted with by Call Automation API using correlation ID.  

```query
ACSCallAutomationIncomingOperations 
//| where CorrelationId == "<correlation ID>" // This can be uncommented to filter on a specific correlation ID
| join kind=inner
    (ACSCallSummary)
    on CorrelationId
| limit 100

```



### Number of calls with MediaStreaming active  


Calculates the number of calls that had MediaStreaming active.  

```query
ACSCallAutomationStreamingUsage 
    | where StreamingModality contains "AudioStreaming" 
    | summarize NumCallsWithMediaStreamingActive = dcount(CallConnectionId) 
```



### MediaStreaming operation Success count  


Calculates the number of success of the MediaStreaming operation.  

```query
ACSCallAutomationIncomingOperations
// Filter OperationName to view results for each API. 
    | where OperationName in ("StartMediaStreaming", "StopMediaStreaming")
    | where ResultSignature matches regex "2.."
    | summarize MediaStreamingSuccess=count() by ResultSignature
```



### MediaStreaming operation Failure count  


Calculates the number of failures of the MediaStreaming operation.  

```query
ACSCallAutomationIncomingOperations 
// Filter OperationName to view results for each API.
    | where OperationName in ("StartMediaStreaming", "StopMediaStreaming")
    | where ResultSignature matches regex "5.."
    | summarize MediaStreamingFailures=count() by ResultSignature
```



### Number of calls with Transcription active  


Calculates the number of calls that had Transcription active.  

```query
ACSCallAutomationStreamingUsage 
    | where StreamingModality == "Transcription" 
    | summarize NumCallsWithTranscriptionActive = dcount(CallConnectionId)
```



### Transcription operation Success count  


Calculates the number of success of the Transcription operation.  

```query
ACSCallAutomationIncomingOperations
// Filter OperationName to view results for each API.
    | where OperationName in ("StartTranscription", "StopTranscription", "UpdateTranscription")
    | where ResultSignature matches regex "2.."
    | summarize TranscriptionSuccess=count()
```



### Transcription operation Failure count  


Calculates the number of failures of the Transcription operation.  

```query
ACSCallAutomationIncomingOperations 
// Filter OperationName to view results for each API.
    | where OperationName in ("StartTranscription", "StopTranscription", "UpdateTranscription")
    | where ResultSignature matches regex "5.."
    | summarize TranscriptionFailures=count()
```

