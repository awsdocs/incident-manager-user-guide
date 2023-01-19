# Incident Manager CloudWatch metrics<a name="incident-manager-metrics"></a>

Incident Manager provides aggregate metrics that you can monitor in CloudWatch\. You can use these metrics to identify incident and response plan trends\.

**These metrics include:**
+ Number of incidents created over a given period of time
+ The time to respond to and resolve those incidents
+ Number of incidents resolved

You can monitor Incident Manager metrics to better understand your operational health, and take meaningful actions to drive the operational excellence of your incident response\. Incident Manager metrics are available in all Incident Manager Regions\. Your metrics will be available to view in Amazon CloudWatch for all the Regions you specified in your replication set when on\-boarding to Incident Manager\. You can view the published metrics in the Region that actions for the incident were taken\. There is no additional charge for these metrics\.

**On the CloudWatch console, you can build dashboards with these metrics to:**
+ Measure and review your existing incident load
+ Track whether your incident load is increasing, decreasing, or remaining the same
+ More effectively use Incident Manager to reduce the frequency, duration, and impact of your incidents

This page describes the Incident Manager metrics available on the CloudWatch console\.

Incident Manager sends the following metrics to CloudWatch\.


| Metric | Description | 
| --- | --- | 
|  `NumberOfCreateIncidents` |  Number of incidents created\. Valid Dimensions: \[\]\(Empty dimension\), \[ResponsePlan\], \[Impact\], \[Source\], \[ResponsePlan, Impact\], \[ResponsePlan, Source\] Unit: Count  | 
|  `NumberOfResolveIncidents` |  Number of incidents resolved\. Valid Dimensions: \[\]\(Empty dimension\), \[ResponsePlan\], \[Impact\], \[Source\], \[ResponsePlan, Impact\], \[ResponsePlan, Source\] Unit: Count  | 
|  `TimeToFirstAcknowledgement` |  Time difference between the incident create time and the time the first acknowledgment was made to the incident\. Valid Dimensions: \[\]\(Empty dimension\), \[ResponsePlan\], \[Impact\], \[Source\], \[ResponsePlan, Impact\], \[ResponsePlan, Source\] Unit: Seconds  | 
|  `TimeToResolveIncident` |  Time difference between when the incident was created and when it was resolved\. Valid Dimensions: \]\(Empty dimension\), \[ResponsePlan\], \[Impact\], \[Source\], \[ResponsePlan, Impact\], \[ResponsePlan, Source\] Unit: Seconds  | 

## Viewing Incident Manager metrics on the CloudWatch console<a name="Viewing-metrics"></a>

**To view Incident Manager metrics in the CloudWatch console**

1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

1. In the navigation pane, choose **Metrics**\.

1. Select the `IncidentManager` namespace\.

1. On the **Metrics** tab, choose a dimension, and then choose a metric\.

For more information about working with CloudWatch metrics, see the following topics in the *Amazon CloudWatch User Guide*:
+ [Metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_concepts.html#Metric)
+ [Using Amazon CloudWatch metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/working_with_metrics.html)

## Dimensions for Metrics<a name="YourService-metricdimensions"></a>

Incident Manager metrics use the `IncidentManager` namespace and provide metrics for the following dimension\(s\):


| Dimension | Description | 
| --- | --- | 
|  `By Response Plan`  |  View aggregate metrics by response plan\.  | 
|  `By Impact Level`  |  View aggregate metrics by the level of severity\.  | 
|  `By Source`  |  View metrics for incidents created manually, by CloudWatch alarm, or EventBridge event\.  | 
|  `Across All Incidents`  |  View aggregate metrics for all incidents in the current AWS Region\.  | 
|  `Response Plan name and Source`  |  View aggregate metrics for each combination of response plan and source\.  | 
|  `Response Plan Name and Impact Level`  |  View aggregate metrics for each combination of response plan and level of severity\.  | 