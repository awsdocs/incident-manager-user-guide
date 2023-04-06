# Working with chat channels in Incident Manager<a name="chat"></a>

Incident Manager, a capability of AWS Systems Manager, provides your incident responders with the ability to directly communicate through *chat channels* during an incident\. A chat channel is a chat room you set up in [AWS Chatbot](https://docs.aws.amazon.com/chatbot/latest/adminguide/) and then connect to a response plan in Incident Manager\.

During an incident, responders can use the chat channel to communicate with one another about the incident\. Incident Manager also pushes updates and notifications about the incident directly to the chat channel to keep all responders informed\. These notifications are sent using one or more Amazon Simple Notification Service \(Amazon SNS\) topics that you specify in your chat room configuration\. 

AWS Chatbot and Incident Manager support chat channels in the following applications:
+ Slack
+ Microsoft Teams
+ Amazon Chime

The process for setting up a chat channel for use in your incidents consists of tasks in three different Amazon Web Services services\.

**Topics**
+ [Task 1: Create or update Amazon SNS topics for your chat channel](#sns-topic)
+ [Task 2: Create a chat channel in AWS Chatbot](#chat-create)
+ [Task 3: Add the chat channel to a response plan in Incident Manager](#response-plan)
+ [Interacting through the chat channel](#chat-interact)

## Task 1: Create or update Amazon SNS topics for your chat channel<a name="sns-topic"></a>

Amazon SNS is a managed service that provides message delivery from publishers to subscribers \(also known as *producers* and *consumers*\)\. Publishers communicate asynchronously with subscribers by sending messages to a *topic*, which is a logical access point and communication channel\. Incident Manager uses one or more topics you associate with a response plan to send notifications about an incident to the incident responders\.

In a response plan, you can include one or more Amazon SNS topics to incident notifications\. As a best practice, you should create an SNS topic in each AWS Region you have added to your replication set\.

**Tip**  
For a more linear setup workflow, we recommend that you configure your Amazon SNS topics for use with Incident Manager before you create a chat channel\.

**To create or update Amazon SNS topics for your chat channel**

1. Follow the steps in the [Creating an Amazon SNS topic](https://docs.aws.amazon.com/sns/latest/dg/sns-create-topic.html) in the *Amazon Simple Notification Service Developer Guide*\.
**Note**  
After you create the topic, you edit it to update its access policy\.

1. Select the topic you created, and note or copy the Amazon Resource Name \(ARN\) of the topic, in a format such as `arn:aws:sns:us-east-2:111122223333:My_SNS_topic`\.

1. Choose **Edit**, and then expand the **Access policy** section to configure additional access permissions beyond the defaults\.

1. Add the following statement to the policy's **Statement** array:

   ```
   {
       "Sid": "IncidentManagerSNSPublishingPermissions",
       "Effect": "Allow",
       "Principal": {
           "Service": "ssm-incidents.amazonaws.com"
       },
       "Action": "SNS:Publish",
       "Resource": "sns-topic-arn",
       "Condition": {
           "StringEqualsIfExists": {
               "AWS:SourceAccount": "account-id"
           }
       }
   }
   ```

   Replace the *placeholder values* as follows:
   + *sns\-topic\-arn* is the Amazon Resource Name \(ARN\) of the topic you created for this Region, in the format `arn:aws:sns:us-east-2:111122223333:My_SNS_topic`\.
   + *account\-id* is the ID of the AWS account you are working in, such as `111122223333`\.

1. Choose **Save changes**\.

1. Repeat the process in each Region included in your replication set\.

## Task 2: Create a chat channel in AWS Chatbot<a name="chat-create"></a>

You can create a chat channel in Slack, Microsoft Teams, or Amazon Chime\. You need only one chat channel for each response plan\.

For your chat channels, we recommend following the principal of least privilege \(not providing users with more permissions than needed to complete their tasks\)\. You should also regularly review the membership of your AWS Chatbot chat channels to ensure that only the appropriate responders and other stakeholders have access to them\.

In AWS Chatbot\-enabled Slack channels and Microsoft Teams channels, incident responders can run a number of Incident Manager CLI commands directly from the Slack or Microsoft Teams application\. For more information, see [Interacting through the chat channel](#chat-interact)\.

**Important**  
Be sure to add the same users to your Slack channel, Microsoft Teams channel, or Amazon Chime room that you add as contacts to your escalation plan or response plan\. You can also add any additional users to chat channels such as stakeholders and incident observers\.

For general information about AWS Chatbot, see [What is AWS Chatbot](https://docs.aws.amazon.com/chatbot/latest/adminguide/what-is.html) in the *AWS Chatbot Administrator Guide*\.

Choose from the following applications to create your channel in:

------
#### [ Slack ]

The steps in this procedure provide the recommended permission settings to allow all channel users to use chat commands with Incident Manager\. Using supported chat commands, your incident responders can update and interact with the incident directly from the Slack chat channel\. For information, see [Interacting through the chat channel](#chat-interact)\.

**To create a chat channel in Slack**
+ Follow the steps in [Tutorial: Get started with Slack](https://docs.aws.amazon.com/chatbot/latest/adminguide/slack-setup.html) in the *AWS Chatbot Administrator Guide* and include the following in your configuration
  + In step 10, for **Role settings**, choose **Channel role**\.
  + In step 10d, for **Policy templates**, select **Incident Manager permissions**\.
  + In step 11, for **Channel guardrail policies**, for **Policy name**, choose [https://console.aws.amazon.com/iam/home#/policies/arn:aws:iam::aws:policy/AWSIncidentManagerResolverAccess$jsonEditor](https://console.aws.amazon.com/iam/home#/policies/arn:aws:iam::aws:policy/AWSIncidentManagerResolverAccess$jsonEditor)\.
  + In step 12, in the **SNS topics** section, do the following:
    + For **Region 1**, select an AWS Region that is included in your replication set\.
    + For **Topics 1**, select the SNS topic you created in that Region to use to send incident notifications to the chat channel\.
    + For each additional Region in your replication set, choose **Add another Region** and add the additional Regions and SNS topics\.

------
#### [ Microsoft Teams ]

The steps in this procedure provide the recommended permission settings to allow all channel users to use chat commands with Incident Manager\. Using supported chat commands, your incident responders can update and interact with the incident directly from the Microsoft Teams chat channel\. For information, see [Interacting through the chat channel](#chat-interact)\.

**To create a chat channel in Microsoft Teams**
+ Follow the steps in [Tutorial: Get started with Microsoft Teams](https://docs.aws.amazon.com/chatbot/latest/adminguide/teams-setup.html) in the *AWS Chatbot Administrator Guide* and include the following in your configuration:
  + In step 10, for **Role settings**, choose **Channel role**\.
  + In step 10d, for **Policy templates**, select **Incident Manager permissions**\.
  + In step 11, for **Channel guardrail policies**, for **Policy name**, choose [https://console.aws.amazon.com/iam/home#/policies/arn:aws:iam::aws:policy/AWSIncidentManagerResolverAccess$jsonEditor](https://console.aws.amazon.com/iam/home#/policies/arn:aws:iam::aws:policy/AWSIncidentManagerResolverAccess$jsonEditor)\.
  + In step 12, in the **SNS topics** section, do the following:
    + For **Region 1**, select an AWS Region that is included in your replication set\.
    + For **Topics 1**, select the SNS topic you created in that Region to use to send incident notifications to the chat channel\.
    + For each additional Region in your replication set, choose **Add another Region** and add the additional Regions and SNS topics\.

------
#### [ Amazon Chime ]

**To create a chat channel in Amazon Chime**
+ Follow the steps in [Tutorial: Get started with Amazon Chime](https://docs.aws.amazon.com/chatbot/latest/adminguide/chime-setup.html) in the *AWS Chatbot Administrator Guide* and include the following in your configuration:
  + In step 11, for **Policy templates**, select **Incident Manager permissions**\.
  + In step 12, in the **SNS topics** section, select the SNS topics that will send notifications to the Amazon Chime webhook:
    + For **Region 1**, select an AWS Region that is included in your replication set\.
    + For **Topics 1**, select the SNS topic you created in that Region to use to send incident notifications to the chat channel\.
    + For each additional Region in your replication set, choose **Add another Region** and add the additional Regions and SNS topics\.

**Note**  
Chat commands, which incident responders can use in Slack and Microsoft Teams chat channels, are not supported in Amazon Chime\.

------

## Task 3: Add the chat channel to a response plan in Incident Manager<a name="response-plan"></a>

When you create or update a response plan in Incident Manager, you can add the chat channel you have created for responders to communicate through and receive incident updates\. 

When following the steps in [Create a response plan](response-plans.md#response-plans-create), for the section **\(Optional\) Chat channel**, select the channel you want to use for incidents related to this response plan\.

## Interacting through the chat channel<a name="chat-interact"></a>

For channels in Slack and Microsoft Teams, Incident Manager enables responders to interact with incidents directly from the chat channel using the following `ssm-incidents` commands:
+ [start\-incident](https://docs.aws.amazon.com/cli/latest/reference/ssm-incidents/start-incident.html)
+ [list\-response\-plan](https://docs.aws.amazon.com/cli/latest/reference/ssm-incidents/list-response-plan.html)
+ [get\-response\-plan](https://docs.aws.amazon.com/cli/latest/reference/ssm-incidents/get-response-plan.html)
+ [create\-timeline\-event](https://docs.aws.amazon.com/cli/latest/reference/ssm-incidents/create-timeline-event.html)
+ [delete\-timeline\-event](https://docs.aws.amazon.com/cli/latest/reference/ssm-incidents/delete-timeline-event.html)
+ [get\-incident\-record](https://docs.aws.amazon.com/cli/latest/reference/ssm-incidents/get-incident-record.html)
+ [get\-timeline\-event](https://docs.aws.amazon.com/cli/latest/reference/ssm-incidents/get-timeline-event.html)
+ [list\-incident\-records](https://docs.aws.amazon.com/cli/latest/reference/ssm-incidents/list-incident-records.html)
+ [list\-timeline\-events](https://docs.aws.amazon.com/cli/latest/reference/ssm-incidents/list-timeline-events.html)
+ [list\-related\-items](https://docs.aws.amazon.com/cli/latest/reference/ssm-incidents/list-related-items.html)
+ [update\-related\-items](https://docs.aws.amazon.com/cli/latest/reference/ssm-incidents/update-related-items.html)
+ [update\-incident\-record](https://docs.aws.amazon.com/cli/latest/reference/ssm-incidents/update-incident-record.html)
+ [update\-timeline\-event](https://docs.aws.amazon.com/cli/latest/reference/ssm-incidents/update-timeline-event.html)

To run commands in an active incident's chat channel, use the following format\. Replace *cli\-options* with any options to be included for a command\.

```
@aws ssm-incidents cli-options
```

For example:

```
@aws ssm-incidents start-incident --response-plan-arn arn:aws:ssm-incidents::111122223333:response-plan/test-response-plan-chat --region us-east-2
```

```
@aws ssm-incidents create-timeline-event --event-data "\"example timeline event"\" --event-time 2023-03-31 T20:30:00.000  --event-type Custom Event --incident-record-arn arn:aws:ssm-incidents::111122223333:incident-record/MyResponsePlanChat/98c397e6-7c10-aa10-9b86-f199aEXAMPLE
```

```
@aws ssm-incidents list-incident-records
```