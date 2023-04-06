# Getting started with Incident Manager<a name="getting-started"></a>

This section walks through **Get prepared** in the Incident Manager console\. You're required to complete **Get prepared** in the console before you can use it for incident management\. The wizard walks you through setting up your replication set, at least one contact and one escalation plan, and your first response plan\. The following guides will help you understand Incident Manager and the incident lifecycle:
+ [What Is AWS Systems Manager Incident Manager?](what-is-incident-manager.md)
+ [The incident lifecycle in Incident Manager](incident-lifecycle.md)

## Prerequisites<a name="getting-started-prereq"></a>

If you're using Incident Manager for the first time, see the [Setting up AWS Systems Manager Incident Manager](setting-up.md)\. We recommend setting up Incident Manager in the account that you use to manage your operations\.

We recommend that you complete the Systems Manager quick setup before beginning the Incident Manager **Get prepared** wizard\. Use Systems Manager [Quick Setup](https://console.aws.amazon.com/systems-manager/quick-setup) to configure frequently used AWS services and features with recommended best practices\. Incident Manager uses Systems Manager features to manage incidents associated with your AWS accounts and benefits from having Systems Manager configured first\. 

## Get prepared wizard<a name="getting-started-wizard"></a>

The first time you use Incident Manager, you can access the **Get prepared** wizard from the Incident Manager service homepage\. To access the **Get prepared** wizard after you first complete setup, choose **Prepare** on the **Incidents** list page\.

1. Open the [Incident Manager console](https://console.aws.amazon.com/systems-manager/incidents/home)\. 

1. On the Incident Manager service homepage, choose **Get prepared**\. 

**General settings**

1. Under **General settings**, choose **Set up**\.

1. Read the terms and conditions\. If you agree to Incident Manager's terms and conditions, select **I have read and agree to the Incident Manager terms and conditions**, then choose **Next**\.

1. In the **Regions** area, your current AWS Region appears as the first Region in your replication set\. To add more Regions to your replication set, choose them from the list of Regions\. 

1. To set up encryption for your replication set, do one of the following:
**Note**  
All Incident Manager resources are encrypted\. To learn more about how your data is encrypted, see [Data protection in Incident Manager](data-protection.md)\. For more information about your Incident Manager replication set, see [Using the Incident Manager replication set](disaster-recovery-resiliency.md#replication)\.
   + To use an AWS owned key, choose **Use AWS owned key**\.
   + To use your own AWS KMS key, choose **Choose an existing AWS KMS key**\. For each Region you selected in step 3, choose an AWS KMS key, or enter an AWS KMS Amazon Resource Name \(ARN\)\.
**Tip**  
If you don't have an available AWS KMS key, choose **Create an AWS KMS key**\.

1. \(Optional\) In the **Tags** area, add one or more tags to the replication set\. A tag includes a key and, optionally, a value\.

   Tags are optional metadata that you assign to a resource\. Tags allow you to categorize a resource in different ways, such as by purpose, owner, or environment\. For more information, see [Tagging resources in Incident Manager](tagging.md)\.

1. Choose **Create**\.

   To learn more about replication sets and resiliency, see [Resilience in AWS Systems Manager Incident Manager](disaster-recovery-resiliency.md)\.

**Note**  
Creating the replication set creates the `AWSServiceRoleforIncidentManager` service\-linked role in your account\. To learn more about this role, see [Using service\-linked roles for Incident Manager](using-service-linked-roles.md) 

**Contacts \(optional\)**

1. Choose **Create contact**\. 

   Incident Manager engages contacts during an incident\. For more information about contacts, see [Working with contacts in Incident Manager](contacts.md)\.

1. Enter the contact's name\.

1. Enter a unique and identifiable alias for this contact\.

1. Add contact channel to your contact in the **Contact channel** section\.

   1. Choose the contact channel's **Type**\. Incident Manager supports **Email**, **SMS**, or **Voice** as contact channels\.

   1. Enter a unique and identifiable **Name**\.

   1. Enter the channel details based on the chosen channel type, such as an email address for **Email**\.

   1. To create another contact channel, choose **Add a new contact channel**\. 

1. Create the contacts **Engagement plan**\. We recommend defining at least two devices in the engagement plan\. We recommend that you select at least one device to engage at the beginning of an engagement\.

   1. Choose a **Contact channel name**\.

   1. Enter the number of minutes to wait before engaging the contact channel\. 

   1. To add more contact channels to the engagement plan, choose **Add engagement**\.

1. To create the contact and send activation codes to the defined contact channels, choose **Next**\.

1. \(Optional\) Enter the activation code sent to each device\.

1. Repeat step four until you have added all of your contacts to Incident Manager\.

1. Once all contacts are entered, choose **Finish**\.

**\(Optional\) Escalation plans**

1. Choose **Create escalation plan**\. An escalation plan escalates through your contacts during an incident, ensuring that Incident Manager engages the correct responders during an incident\. For more information about escalation plans, see [Working with escalation plans in Incident Manager](escalation.md)\.

1. Enter a unique and identifiable **Escalation plan name**\.

1. Enter the number of minutes the first stage of the escalation plan should last before starting the next stage\.

1. Add contacts to the first stage by choosing them from the **Contact** search bar\. 

1. If you want a contact to be able to halt the progression of escalation plan stages, select **Acknowledgment stops plan progression**\.

1. To add more contacts to a stage, choose **Add contact**\.

1. To create a new stage in the escalation plan, choose **Add stage**\.

1. After you've finished adding stages and contacts, choose **Create escalation plan**\.

**Response plan**

1. Choose **Create response plan**\. Use the response plan to put together contacts and escalation plans you created\. During this **Getting started** wizard, the following sections are optional, especially if this is your first time setting up a response plan:
   + **Chat channel**
   + **Runbooks** 
   + **Engagements**
   +  **Third\-party integrations**

   For information about adding these elements to response plans later, see [Preparing for incidents in Incident Manager](incident-response.md)\.

1. Enter a unique, identifiable **Name**\.

1. Enter an **Incident title prefix**\. The incident title prefix is appended to the start of the incident's title\. The alarm or event that started the incident is also added to the title\.

1. Select the expected **Impact** of the incident\.

1. Enter a brief **Summary** of the incident to inform responders of the incident's details\. Incident Manager automatically populates relevant information into the summary during an incident\.

1. \(Optional\) Enter a dedupe string\. The dedupe string removes duplicate incidents created in the same account\.

1. Select the contacts and escalation plans to apply to the incident from the **Engagements** dropdown\.

1. Choose **Create response plan**\. 

   After you choose **Create response plan**, the **Response plans** list console page opens\. 

After you've created a response plan, you can associate Amazon CloudWatch alarms or Amazon EventBridge events with the response plan\. This will automatically create an incident based on an alarm or event\. To learn more about incident creation, see [Creating incidents in Incident Manager](incident-creation.md)\.