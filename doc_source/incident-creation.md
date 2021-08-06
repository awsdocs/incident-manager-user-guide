# Incident creation<a name="incident-creation"></a>

AWS Systems Manager Incident Manager tracks incidents\. Using Amazon CloudWatch or Amazon EventBridge, Incident Manager can automatically start incidents\. You can also create incidents manually on the incident list page\. This section describes automatic and manual incident creation\. When you create an incident, Incident Manager creates a parent OpsItem in OpsCenter to track related work and future incident analyses\. Calls to OpsCenter incur costs in Systems Manager\. For more information about OpsCenter pricing, see [Systems Manager pricing](http://aws.amazon.com/systems-manager/pricing/)\.

## Automatically create incidents with CloudWatch alarms<a name="incident-tracking-auto-alarms"></a>

CloudWatch uses your CloudWatch metrics to alert you about changes in your environment and to automatically perform the start incident action\. CloudWatch works with Systems Manager and Incident Manager to create an incident from a response plan template when an alarm goes into alarm state\. This requires the following prerequisites:
+ Incident Manager configured and replication set created\. This step creates the Incident Manager service linked role in your account, providing the necessary permissions\.
+ A configured Incident Manager response plan\. To learn how to configure Incident Manager response plans, see [Response plans](response-plans.md) in the *Incident preparation* section of this guide\.
+ Configured CloudWatch metrics monitoring your application\. For monitoring best practices, see [Monitoring](incident-response.md#incident-response-monitoring) in the *Incident preparation* section of this guide\.

**To create an alarm with a **Start incident** action**

1. Decide what type of alarm you're creating and follow the steps found in the following sections of the CloudWatch user guide\.
   + [Create an Alarm Based on a Static Threshold]()
   + [Creating an Alarm Based on Anomaly Detection]()
   + [Creating an Alarm Based on a Metric Math Expression]()
   + [Creating a Composite Alarm](AmazonCloudWatch/latest/monitoring/Create_Composite_Alarm.html)
   + [Creating a CPU Usage Alarm]()
   + [Creating a Load Balancer Latency Alarm]()
   + [Creating a Storage Throughput Alarm](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/US_AlarmAtThresholdEBS.html)

1. When choosing the action for the alarm to perform, select **Add Systems Manager action**\.

1. Choose **Create incident** and select the **Response plan** for this incident\.

1. Complete the remaining steps in your selected alarm type guide\.

**Tip**  
You can also add the create incident action to any existing alarm\.

## Automatically create incidents with EventBridge events<a name="incident-tracking-auto-eventbridge"></a>

EventBridge rules watch for event patterns\. If the event matches the defined pattern, Incident Manager creates an incident using the chosen response plan\. 

### Creating incidents using SaaS partners events<a name="incident-tracking-auto-eventbridge-saas"></a>

You can configure EventBridge to receive events from software as a service \(SaaS\) partner applications and services, allowing for third\-party integration\. After configuring EventBridge to receive events from third\-party partners, you can create rules that match on partner events to create incidents\. To see a list of third\-party integrations, see [Receiving events from a SaaS partner](https://docs.aws.amazon.com/eventbridge/latest/userguide/create-partner-event-bus.html)\. 

**Configure EventBridge to receive events from a SaaS integration\.**

1. Open the Amazon EventBridge console at [https://console\.aws\.amazon\.com/events/](https://console.aws.amazon.com/events/)\.

1. In the navigation pane, choose **Partner event sources**\.

1. Use the search bar to find the partner that you want and choose **Set up** for that partner\. 

1. Choose **Copy** to copy your account ID to the clipboard\.
**Note**  
To integrate with Salesforce use the steps described in the [Amazon AppFlow user guide](https://docs.aws.amazon.com/appflow/latest/userguide/EventBridge.html)\.

1. Go to the partner's website and follow the instructions to create a partner event source\. Use your account ID for this\. The event source that you create is available only on your account\. 

1. Go back to the EventBridge console and choose **Partner event sources** in the navigation pane\.

1. Select the button next to the partner event source, and choose **Associate with event bus**\.

**Create a rule that triggers on events from a SaaS partner**

1. Open the Amazon EventBridge console at [https://console\.aws\.amazon\.com/events/](https://console.aws.amazon.com/events/)\.

1. In the navigation pane, choose Rules\.

1. Choose **Create rule**\.

1. Enter a name and description for the rule\.

1. For **Define pattern**, choose **Event pattern**\.

1. Choose **Pre\-defined pattern by service**\.

1. For **Service provider**, choose **Service partners**\.

1. For **Service name**, choose the name of the partner\.

1. For **Event type**, choose **All Events** or choose the type of event to use for this rule\. If you choose **All Events**, all events emitted by this partner event source will match the rule\. 

   If you want to customize the event pattern, choose **Edit**, make your changes, and then choose **Save**\.

1. For **Service event bus**, select the event bus that corresponds to this partner\.

1. For **Select targets**, choose **Incident Manager response plan**\.

1. For **Response plan**, choose a response plan\. 
**Note**  
When selecting a response plan, all response plans that you own and have been shared with your account appear in the **Response plan** dropdown\.

1. EventBridge can create the IAM role needed for your rule to run:
   + To create an IAM role automatically, choose** Create a new role for this specific resource**\.
   + To use an IAM role that you created before, choose **Use existing role**\.

1. \(Optional\) Enter one or more tags for the rule\. 

1. Choose **Create**\.

### Creating incidents using AWS service events<a name="incident-tracking-auto-eventbridge-aws"></a>

EventBridge also receives events from the AWS services listed in [Events from Supported AWS Services](https://docs.aws.amazon.com/eventbridge/latest/userguide/event-types.html)\. Similar to how you configure rules for SaaS partners, you can configure them for AWS services\. 

**Create a rule that triggers on events from an AWS service**

1. Open the Amazon EventBridge console at [https://console\.aws\.amazon\.com/events/](https://console.aws.amazon.com/events/)\.

1. In the navigation pane, choose Rules\.

1. Choose **Create rule**\.

1. Enter a name and description for the rule\.

1. For **Define pattern**, choose **Event pattern**\.

1. Choose **Pre\-defined pattern by service**\.

1. For **Service provider**, choose **AWS**\.

1. For **Service name**, choose the service that monitors for an incident\.

1. For **Event type**, choose **All Events** or choose the type of event to use for this rule\. If you choose **All Events**, all events emitted by this partner event source will match the rule\. 

   If you want to customize the event pattern, choose **Edit**, make your changes, and then choose **Save**\.

1. For **Service event bus**, select the **AWS default event bus**\.

1. For **Select targets**, choose **Incident Manager response plan**\.

1. For **Response plan**, choose a response plan\. 
**Note**  
When selecting a response plan, all response plans that you own and have been shared with your account appear in the **Response plan** dropdown\.

1. EventBridge can create the IAM role needed for your rule to run:
   + To create an IAM role automatically, choose** Create a new role for this specific resource**\.
   + To use an IAM role that you created before, choose **Use existing role**\.

1. \(Optional\) Enter one or more tags for the rule\. 

1. Choose **Create**\.

## Manually create incidents<a name="incident-tracking-manual"></a>

Responders can manually track an incident using the Incident Manager console by using a predefined response plan\. Use the following steps to create an incident\.

1. Open the [Incident Manager console](https://console.aws.amazon.com/systems-manager/incidents/home)\.

1. Choose **Start incident**\.

1. For **Response plan**, choose a response plan from the list\.

1. \(Optional\) To override the title provided by the defined response plan, enter an **Incident title**\.

1. \(Optional\) To override the impact provided by the defined response plan, enter the **Impact** of the incident\.