# Working with contacts in Incident Manager<a name="contacts"></a>

AWS Systems Manager Incident Manager contacts are responders to incidents\. A contact can have multiple channels that Incident Manager can engage during an incident\. You can define a contact's engagement plan to describe how and when Incident Manager engages the contact\.

**Topics**
+ [Contact channels](#contacts-channels)
+ [Engagement plans](#contacts-engage)
+ [Create a contact](#contacts-define)
+ [Import contact details to your address book](#contacts-details-file)

## Contact channels<a name="contacts-channels"></a>

Contact channels are the various methods Incident Manager uses to engage a contact\.

Incident Manager supports the following contact channels:
+ Email
+ Short Message Service \(SMS\) 
+ Voice

**Contact channel activation**  
To protect your privacy and security, Incident Manager sends a device activation code to you when you create contacts\. To engage your devices during an incident, you must first activate them\. To do so, enter the device activation code on the create contact page\.

Certain features of Incident Manager include functionality that send notifications to a contact channel\. By using these features, you consent to this service sending notifications about service disruptions or other events to the contact channels included in the specified workflow\. This includes notifications sent to a contact as part of an on\-call schedule rotation\. Notifications may be sent by email, SMS message, or voice call as specified in a contact's details\. You confirm by using these features that you're authorized to add the contact channels you provide to Incident Manager\.

**Opting out**  
You can cancel these notifications at any time by removing a mobile device as a contact channel\. Individual notification recipients may also cancel notifications at any time by removing the device from their contact\.

**To remove a contact channel from a contact**

1. Navigate to the [Incident Manager console](https://console.aws.amazon.com/systems-manager/incidents/home) and choose **Contacts** from the left navigation\.

1. Select the contact with the contact channel that you are removing and choose **Edit**\.

1. Choose **Remove** next to the contact channel that you would like to remove\.

1. Choose **Update**\.

**Contact channel deactivation**  
To deactivate a device, reply **UNSUBSCRIBE**\. Replying **UNSUBSCRIBE** stops Incident Manager from engaging your device\.

**Contact channel reactivation**

1. Reply **START** to the message from Incident Manager\.

1. Navigate to the [Incident Manager console](https://console.aws.amazon.com/systems-manager/incidents/home) and choose **Contacts** from the left navigation\.

1. Select the contact with the contact channel that you are removing and choose **Edit**\.

1. Choose **Activate devices**\.

1. Enter the **Activation code** sent to the device by Incident Manager\.

1. Choose **Activate**\.

## Engagement plans<a name="contacts-engage"></a>

Engagement plans define when Incident Manager engages the contact channels\. You can engage contact channels multiple times at different stages from the start of an engagement\. You can use engagement plans in an escalation plan or response plan\. To learn more about escalation plans, see [Working with escalation plans in Incident Manager](escalation.md)\. 

## Create a contact<a name="contacts-define"></a>

To create a contact, use the following steps\.

1. Open the [Incident Manager console](https://console.aws.amazon.com/systems-manager/incidents/home) and choose **Contacts** from the left navigation\.

1. Choose **Create Contact**\.

1. Type the full name of the contact and provide a unique and identifiable alias\.

1. Define a **Contact channel**\. We recommend having two or more different types of contact channels\.

   1. Choose the type: email, SMS, or voice\.

   1. Enter an identifiable name for the contact channel\.

   1. Provide the contact channel details, such as email: arosalez@example\.com

1. To define more than one contact channel, choose **Add contact channel**\. Repeat step 4 for each new contact channel added\.

1. Define an engagement plan\.
**Important**  
To engage a contact, you must define an engagement plan\.

   1. Choose a **Contact channel name**\.

   1. Define how many minutes from the start of the engagement to wait until Incident Manager engages this contact channel\.

   1. To add another contact channel, choose **Add engagement**\.

1. After defining your engagement plan, choose **Create**\. Incident Manager sends an activation code to each of the defined contact channels\. 

1. \(Optional\) To activate the contact channels, enter the activation code that Incident Manager sent to each defined contact channel\. 

1. \(Optional\) To send a new activation code, choose **Send new code**\.

1. Choose **Finish**\.

After you define a contact and activate its contact channels, you can add contacts to escalation plans to form a chain of escalation\. To learn more about escalation plans, see [Working with escalation plans in Incident Manager](escalation.md)\. You can add contacts to a response plan for direct engagement\. To learn more about creating response plans, see [Working with response plans in Incident Manager](response-plans.md)\.

## Import contact details to your address book<a name="contacts-details-file"></a>

When an incident is created, Incident Manager can notify responders by using voice or SMS notifications\. To ensure that responders see that the call or SMS notification is from Incident Manager, we recommend that all responders download the Incident Manager [virtual card format \(\.vcf\)](https://docs.fileformat.com/email/vcf/) file to the address book on their mobile devices\. The file is hosted in Amazon CloudFront and is available in the AWS commercial partition\. 

**To download the Incident Manager \.vcf file**

1. On your mobile device, either choose or enter the following URL: [https://d26vhuvd5b89k2\.cloudfront\.net/aws\-incident\-manager\.vcf](https://d26vhuvd5b89k2.cloudfront.net/aws-incident-manager.vcf)\. 

1. Save or import the file to the address book on your mobile device\.