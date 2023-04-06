# Working with response plans in Incident Manager<a name="response-plans"></a>

 Response plans let you plan for how to respond to an incident that impacts your users\. A response plan works as a template that includes information about who to engage, the expected severity of the event, automatic runbooks to initiate, and metrics to monitor\. 

## Best practices<a name="response-plan-best-practices"></a>

You can reduce the impact on incidents on your teams when you plan for incidents ahead of time\. Teams should consider the following best practices when you design a response plan\.
+ **Streamlined engagement** – Identify the most appropriate team for an incident\. If you engage too wide a distribution list, or if you engage the wrong teams, you can cause confusion and waste responder time during an incident\. 
+ **Reliable escalation** – For your engagements in a response plan, we recommend selecting an engagement plan instead of contacts or on\-call schedules\. The engagement plan should specify the individual contacts or on\-call schedules \(which contain multiple rotating contacts\) to engage during incidents\. Because responders specified in your engagement plan can be unreachable at times, you should configure backup responders in your response plan to cover these scenarios\. With backup contacts, if the primary and secondary contacts are unavailable or there are other unplanned gaps in coverage, Incident Manager still notifies a contact about the incident\.
+ **Runbooks** – Use runbooks to provide repeatable, understandable steps that reduce the stress a responder experiences during an incident\.
+ **Collaboration** – Use chat channels to streamline communication during incidents\. Chat channels help responders stay up to date with information\. They can also share information with other responders through these channels\. 

## Create a response plan<a name="response-plans-create"></a>

Use the following procedure to create a response plan and automate incident response\.

**Response plan details**

1. Open the [Incident Manager console](https://console.aws.amazon.com/systems-manager/incidents/home), and in the navigation pane, choose **Response plans**\.

1. Choose **Create response plan**\.

1. For **Name**, enter a unique and identifiable response plan name to use in the Amazon Resource Name \(ARN\) for the response plan\.

1. \(Optional\) For **Display name**, enter a more human readable name to help identify the response plan when you create incidents\.

**Incident defaults**

1. For **Title**, enter a title for this incident to help you identify it on the Incident Manager home page\.

1. For **Impact**, choose an impact level to indicate the potential scope of an incidents created from this response plan, such as **Critical** or **Low**\. For information about impact ratings in Incident Manager, see [Triage](incident-lifecycle.md#triage)\.

1. \(Optional\) For **Summary**, enter a brief summary the type of incidents created from this response plan\.

1. \(Optional\) For **Dedupe string**, enter a dedupe string that Incident Manager uses to prevent the same root cause from creating multiple incidents in the same account\. For example, Incident Manager deduplicates incidents created from the same CloudWatch alarm or EventBridge event into a single incident\.

1. \(Optional\) Under **Tags**, add tag keys and values to assign to incidents created from this response plan\. You must have the `TagResource` permission for the incident record resource to set incident tags within the response plan\.

**\(Optional\) Chat channel**

1. For **Chat channel**, select a channel where responders can communicate during an incident\. For more information about chat channels, see [Working with chat channels in Incident Manager](chat.md)\. 
**Important**  
Incident Manager must have permissions to publish to the chat channel's simple notification service \(SNS\) topic\. Without permissions to publish to that SNS topic, you can't add it to the response plan\. Incident Manager publishes a test notification to the SNS topic to verify permissions\.

1. For Chat channel SNS topics, choose additional SNS topics to publish to during the incident\. Adding SNS topics in multiple AWS Regions increases redundancy in case a Region is down at the time of the incident\.

**\(Optional\) Engagements**
+ For **Engagements**, choose any number of escalations plans, on\-call schedules, and individual contacts\. 

  As a best practice, we recommend the following:

  1. Add contacts and on\-call schedules as the escalation channels in an escalation plan\.

  1. Choose an escalation plan as the engagement in a response plan\.

  For more information contacts and escalation plans, see [Working with contacts in Incident Manager](contacts.md) and [Working with escalation plans in Incident Manager](escalation.md)\.

**\(Optional\) Runbook**

1. To select a **Runbook**, do one of the following:
   + Choose **Clone runbook from template**\. For **Runbook name**, enter a descriptive name for the new runbook\. 
   + Choose **Select an existing runbook**\. Select the **Owner**, **Runbook**, and **Version** to use\. For information about runbook creation, see [Working with Systems Manager Automation runbooks in Incident Manager](runbooks.md)\.

1. For **Role name**, select a role that contains the permissions needed to run the Automation runbook you selected\. 

   At minimum, the role must allow the `ssm:StartAutomationExecution` action for your specific runbook\. For the runbook to work across accounts, the role must also allow the `sts:AssumeRole` action for the `AWS-SystemsManager-AutomationExecutionRole` role that you created during [Cross\-Region and cross\-account incident management in Incident Manager](incident-manager-cross-account-cross-region.md)\. 

   Depending on the runbook you selected, the role might require other permissions\. For information, see the [AWS Systems Manager Automation runbook reference](https://docs.aws.amazon.com/systems-manager-automation-runbooks/latest/userguide/automation-runbook-reference.html)\.

   If you need to create a role to use with your runbook, do the following

   1. Open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

   1. Choose **Roles** from the left navigation, then choose **Create role**\.

   1. For **Select trusted entity**, do the following:

      1. For **Trusted entity type**, choose **AWS service**

      1. For **Use cases for other AWS services**, select **Incident Manager**

      1. Select **Incident Manager**, as shown in the following image\.  
![\[Illustrating the Incident Manager option selected as a use case.\]](http://docs.aws.amazon.com/incident-manager/latest/userguide/images/iam_use_cases_for_response-plans.png)

   1. Choose **Next**\.

   1. Choose **Create policy**, and then choose the **JSON** tab\. 
**Tip**  
The **Create policy** page opens in a new tab\.

   1. Replace the default contents of the JSON editor with the following policy\. Replace the placeholder account number \(*111122223333*\) and runbook name \(*DocumentName*\) in the runbook's ARN with your own values\.

      ```
      {
         "Version": "2012-10-17",
         "Statement": [
           {
             "Effect": "Allow",
             "Resource": "arn:aws:ssm:*:111122223333:automation-definition/DocumentName:*",
             "Action": "ssm:StartAutomationExecution"
           },
           {
             "Effect": "Allow",
             "Resource": "arn:aws:iam::*:role/AWS-SystemsManager-AutomationExecutionRole",
             "Action": "sts:AssumeRole"
           },
           {
             "Effect": "Allow",
             "Resource": "arn:aws:ssm-incidents:*:*:*",
             "Action": "ssm-incidents:*"
           },
           {
             "Effect": "Allow",
             "Resource": "arn:aws:ssm-contacts:*:*:*",
             "Action": "ssm-contacts:*"
           }
         ]
       }
      ```

   1. Choose **Next: Tags**\.

   1. \(Optional\) For **Tags**, add one or more tag key\-value pairs to your policy\.

   1. Choose **Next: Review**\.

   1. \(Optional\) For **Name** and **Description** enter a human readable name and description for the policy\.

   1. Choose **Create policy**\.

   1. Return to the browser tab for creating a role and search for the policy you created\. You might need to choose the refresh button first\.

   1. Select the policy you created, and then choose **Next**\.

   1. For **Role name** and **Description**, provide a human readable name and \(optional\) description for the role\.

   1. \(Optional\) Under **Tags**, and one or more tag key\-value pairs to your role\.

   1. Choose **Create role**\.

1. Return to the response plan you created and refresh the **Role name** dropdown\.

1. Select the role you created\.

1. Expand **Additional options** and choose one of the following:
   + **Response plan owner's account** – Start the runbook operation in the AWS account that created it\.
   + **Impacted account** – Start the runbook operation in the account that began or reported an incident\. 

**\(Optional\) Integrate a PagerDuty service into the response plan**

When you integrate Incident Manager with PagerDuty, PagerDuty creates a corresponding incident whenever Incident Manager creates an incident\. The incident in PagerDuty uses the paging workflow and escalation policies you've defined there in addition to those in Incident Manager\. PagerDuty attaches timeline events from Incident Manager as notes on your incident\.

1. Expand **Third\-party integrations**, then choose the **Enable PagerDuty integration** check box\.

1. For **Select secret**, select the secret in AWS Secrets Manager where you store the credentials to access your PagerDuty account\.

   For information about storing your PagerDuty credentials in a Secrets Manager secret, see [Storing PagerDuty access credentials in an AWS Secrets Manager secret](integrations-pagerduty-secret.md)\.

1. For **PagerDuty service**, select the service from your PagerDuty account where you want to create the PagerDuty incident\.

**Add tags and create the response plan**

1. \(Optional\) Expand the **Tags** section and add one or more tag key\-value pairs to your response plan\.

1. Choose **Create response plan**\.

    