# Managing security incidents in Incident Manager<a name="tutorials-security"></a>

You can use AWS Security Hub, Amazon EventBridge, and Incident Manager together to identify and manage security incidents in your AWS hosted\-applications\. This tutorial walks you through configuring an EventBridge rule that creates an incident based on Security Hub automatically sent findings\.

**Note**  
This tutorial uses EventBridge Security Hub\. You may incur costs from using these services\.

**Prerequisites**
+ Set up Security Hub\. For more information, see [Setting up AWS Security Hub](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-settingup.html)\.
+ Create or update findings in Security Hub\. For more information, see [Findings in AWS Security Hub](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-findings.html)\.
+ Configure a response plan that Incident Manager will use as the template when creating your security incident\. For more information, see [Preparing for incidents in Incident Manager](incident-response.md)\.

For this tutorial, we use a predefined pattern to create the EventBridge rule\. To create the rule using a custom pattern, see [Using a custom pattern to create the rule](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-cwe-all-findings.html#securityhub-cwe-all-findings-custom-pattern) in the AWS Security Hub user guide\.

**Create an EventBridge rule**

1. Open the Amazon EventBridge console at [https://console\.aws\.amazon\.com/events/](https://console.aws.amazon.com/events/)\.

1. In the navigation pane, choose **Rules**\.

1. Choose **Create rule**\.

1. Enter a **Name** and **Description** for the rule\.

   A rule can't have the same name as another rule in the same Region and on the same event bus\.

1. For **Event bus**, choose **default**\.

1. For **Rule type**, choose **Rule with an event pattern**\.

1. Choose **Next**\.

1. For **Event source**, choose **AWS events or EventBridge partner events**\.

1. For **Event pattern**, choose **Event pattern form**\.

1. For **Event source**, choose **AWS services**\.

1. For **AWS service**, choose **Security Hub**\.

1. For **Event type**, choose **Security Hub Findings \- Imported**\.

1. By default, EventBridge configures the event pattern without any filter values\. For each attribute, the **Any *attribute name*** option is selected\. Update these filters to create incidents based on the security findings that most impact your environment\. 

1. Click **Next**\.

1. For **Target types**, choose **AWS service**\.

1. For **Select a target**, choose **Incident Manager response plan**\.

1. For **Response plan**, choose a response plan to use as a template for created incidents\.

1. EventBridge can create the IAM role needed for your rule to run\.
   + To create an IAM role automatically, choose **Create a new role for the specific resource**\.
   + To use an IAM role that already exists in your account, choose **Use existing role**\.

1. \(Optional\) Enter one or more tags for the rule\.

1. Choose **Next**\.

1. Review the details of the rule and choose **Create rule**\.

Now that you've created this EventBridge rule, security findings that match the attribute values you defined will create incidents in Incident Manager\. You can triage, manage, monitor, and create post\-incident analysis from these incidents\.