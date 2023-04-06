# Working with Systems Manager Automation runbooks in Incident Manager<a name="runbooks"></a>

You can use runbooks in [AWS Systems Manager Automation](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-automation.html) to automate common application and infrastructure tasks\. For example, you can use runbooks to automate the maintenance, deployment, and remediation of your AWS resources\. In Incident Manager, a runbook drives incident response and mitigation\.

You can choose from dozens of pre\-configured runbooks for commonly automated tasks, such as restarting Amazon Elastic Compute Cloud \(Amazon EC2\) instances, or you can create your own\. If you specify a runbook in a response plan definition, the system can automatically initiate the runbook when an incident starts\.

This topic describes how to create and share a runbook, and how to locate and use the Incident Manager runbook template\. For more information about Systems Manager Automation, runbooks, and using runbooks with Incident Manager, see the following topics:
+ To learn more about runbooks, see [AWS Systems Manager Automation](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-automation.html) in the *AWS Systems Manager User Guide*
+ To learn about the cost of using runbooks, see [Systems Manager pricing](http://aws.amazon.com/systems-manager/pricing/)\.
+ To learn how to add a runbook to a response plan, see [Working with response plans in Incident Manager](response-plans.md)\.
+ To learn more about automatically invoking runbooks when an incident is created by a Amazon CloudWatch alarm or an Amazon EventBridge event, see [Tutorial: Using Systems Manager Automation runbooks with Incident Manager](https://docs.aws.amazon.com/incident-manager/latest/userguide/tutorials-runbooks.html)\.

**Important**  
Incidents created by a cross\-Region failover don't invoke runbooks specified in response plans\.

## Working with runbook parameters<a name="runbooks-parameters"></a>

When you add a runbook to a response plan, you can specify the parameters the runbook should use at runtime\. Response plans support parameters with both static and dynamic values\. For static values, you enter the value when you define the parameter in the response plan\. For dynamic values, the system determines the correct parameter value by collecting information from the incident\. Incident Manager supports the following dynamic parameters:

`Incident ARN`  
When Incident Manager creates an incident, the system captures the Amazon Resource Name \(ARN\) of the corresponding incident record and enters it for this parameter in the runbook\.  
This value can only be assigned to parameters of type `String`\. If assigned to a parameter of any other type, the runbook fails to run\.

`Involved resources`  
When Incident Manager creates an incident, the system captures the ARNs of the resources involved in the incident\. These resource ARNs are then assigned to this parameter in the runbook\.



### About associated resources<a name="runbooks-parameters-involved-resources"></a>

Incident Manager can populate runbook parameter values with the ARNs of AWS resources specified in CloudWatch alarms, EventBridge events, and manually\-created incidents\. This section describes the different types of resources for which Incident Manager can capture ARNs when populating this parameter\.

**CloudWatch alarms**  
When an incident is created from a CloudWatch alarm action, Incident Manager automatically extracts the following types of resources from the associated metrics\. It then populates the chosen parameters with the involved resources described below:


****  

| AWS service | Resource type | 
| --- | --- | 
|  Amazon DynamoDB  |  Global secondary indexes Streams Tables  | 
|  Amazon EC2  |  Images Instances  | 
|  AWS Lambda  |  Function aliases Function versions Functions  | 
|  Amazon Relational Database Service \(Amazon RDS\)  |  Clusters Database instances  | 
|  Amazon Simple Storage Service \(Amazon S3\)  |  Buckets  | 

**EventBridge rules**  
When the system creates an incident from an EventBridge event, Incident Manager populates the chosen parameters with the `Resources` property in the event\. For more information, see [Amazon EventBridge events](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-events.html) in the *Amazon EventBridge User Guide*\. 

**Manually created incidents**  
When you create an incident by using the [StartIncident](https://docs.aws.amazon.com/incident-manager/latest/APIReference/API_StartIncident.html) API action, Incident Manager populates the chosen parameters by using information in the API call\. Specifically, it populates parameters by using items of the type `INVOLVED_RESOURCE` that are passed in the `relatedItems` parameter\.

**Note**  
The `INVOLVED_RESOURCES` value can only be assigned to parameters of type `StringList`\. If assigned to a parameter of any other type, the runbook fails to run\.

## Define a runbook<a name="runbook-create"></a>

When creating a runbook, you can follow the steps provided here, or you can follow the more detailed guide provided in the [Working with runbooks](https://docs.aws.amazon.com/systems-manager/latest/userguide/automation-documents.html) section in the *Systems Manager User Guide*\. If you're creating a multi\-account, multi\-Region runbook, see [Running automations in multiple AWS Regions and accounts](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-automation-multiple-accounts-and-regions.html) in the *Systems Manager User Guide*\. 

**Define a runbook**

1. Open the Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\. 

1. In the navigation pane, choose **Documents**\.

1. Choose **Create automation**\.

1. Enter a unique and identifiable runbook name\.

1. Enter a description of the runbook\.

1. Provide an IAM role for the automation document to assume\. This allows the runbook to run commands automatically\. For more information, see [Configuring a service role access for Automation workflows](https://docs.aws.amazon.com/systems-manager/latest/userguide/automation-setup.html#automation-setup-configure-role)\.

1. \(Optional\) Add any input parameters that the runbook starts with\. You can use dynamic or static parameters when starting a runbook\. Dynamic parameters use values from the incident that the runbook is started in\. Static parameters use the value you provide\.

1. \(Optional\) Add a **Target** type\.

1. \(Optional\) Add tags\.

1. Fill in the steps that the runbook will take when it runs\. Each step requires:
   + A name\.
   + A description of the purpose of the step\.
   + The action to run during the step\. Runbooks use the **Pause** action type to describe a manual step\.
   + \(Optional\) Command properties\.

1. After adding all required runbook steps, choose **Create Automation**\.

To enable cross\-account functionality, share the runbook in your management account with all application accounts that use the runbook during an incident\. 

**Share a runbook**

1. Open the Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Documents**\.

1. In the documents list, choose the document you want to share and then choose **View details**\. On the **Permissions** tab, verify that you're the document owner\. Only a document owner can share a document\.

1. Choose **Edit**\.

1. To share the command publicly, choose **Public** and then choose **Save**\. To share the command privately, choose **Private**, enter the AWS account ID, choose **Add permission**, and then choose **Save**\. 

## Incident Manager runbook template<a name="runbooks-template"></a>

Incident Manager provides the following runbook template to help your team start authoring runbooks in Systems Manager automation\. You can use this template as is, or edit it to include details specific to your application and resources\. 

**Find the Incident Manager runbook template**

1. Open the Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Documents**\.

1. In the documents list, use the search field to find the text *AWSIncidents\-*\. This displays all Incident Manager runbooks\.

**Using a template**

1. Open the Systems Manager console at [https://console\.aws\.amazon\.com/systems\-manager/](https://console.aws.amazon.com/systems-manager/)\.

1. In the navigation pane, choose **Documents**\.

1. Choose the template you want to update from the documents list\.

1. Choose the **Content** tab, and then copy the content of the document\.

1. In the navigation pane, choose **Documents**\.

1. Choose **Create automation**\.

1. Enter a unique and identifiable name\.

1. Choose the **Editor** tab\. 

1. Choose **Edit**\.

1. Paste or enter the copied details in the **Document editor** area\. 

1. Choose **Create automation**\.

### `AWSIncidents-CriticalIncidentRunbookTemplate`<a name="runbooks-template-critical"></a>

The `AWSIncidents-CriticalIncidentRunbookTemplate` is a template that provides the Incident Manager incident lifecycle in manual steps\. These steps are generic enough to use in most applications, but detailed enough for responders to get started with incident resolution\. 