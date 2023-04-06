# Working with escalation plans in Incident Manager<a name="escalation"></a>

AWS Systems Manager Incident Manager provides escalation paths through your defined contacts or on\-call schedules, collectively known as *escalation channels*\. You can pull multiple escalation channels into an incident at the same time\. If the designated contacts in the escalation channel don't respond, Incident Manager escalates to the next set of contacts\. You can also choose if a plan stops escalating once a user acknowledges the engagement\. You can add escalation plans to a response plan so escalation automatically starts at the beginning of an incident\. You can also add escalation plans to an active incident\.

**Topics**
+ [Stages](#escalation-stages)
+ [Create an escalation plan](#escalation-create)

## Stages<a name="escalation-stages"></a>

Escalation plans use stages where each stage lasts a defined number of minutes\. Each stage has the following information:
+ **Duration** – The amount of time the plan waits until beginning the next stage\. The first stage of the escalation plan begins once the engagement starts\.
+ **Escalation channel** – An escalation channel is either a single contact or an on\-call schedule composed of multiple contacts who rotate responsibilities on a defined schedule\. The escalation plan engages each channel using its defined engagement plan\. You can set up each escalation channel to stop the progression of the escalation plan before it continues to the next stage\. Each stage can have multiple escalation channels\. 

  For information about setting up individual contacts, see [Working with contacts in Incident Manager](contacts.md)\. For information about creating on\-call schedules, see [Working with on\-call schedules in Incident Manager](incident-manager-on-call-schedule.md)\.

## Create an escalation plan<a name="escalation-create"></a>

1. Open the [Incident Manager console](https://console.aws.amazon.com/systems-manager/incidents/home) and choose **Escalation plans** from the left navigation\.

1. Choose **Create escalation plan**\.

1. For **Name**, enter a unique name for the escalation plan, such as **My Escalation Plan**\.

1. For **Alias**, enter an alias to help you identify the plan, such as **my\-escalation\-plan**\.

1. For **Stage duration**, enter the number of minutes for Incident Manager to wait until it continues to the next stage\.

1. For **Escalation channel**, choose eone or more contacts or on\-call schedules to engage during this stage\.

1. \(Optional\) To let a contact stop the escalation plan once they acknowledge the engagement, select **Acknowledgment stops plan progression**\.

1. To add another channel to this stage, choose **Add escalation channel**\.

1. To add another stage to the escalation plan, choose **Add stage**\.

1. Repeat steps 5 through 9 until you finish adding the escalation channels and stages you want for this escalation plan\.

1. \(Optional\) In the **Tags** area, apply one or more tag key name and value pairs to the escalation plan\.

   Tags are optional metadata that you assign to a resource\. Tags allow you to categorize a resource in different ways, such as by purpose, owner, or environment\. For example, you can tag an escalation plan to identify the type of incidents to use it for, the types of escalation channels it contains, or the escalation plan it supports\. For more information about tagging Incident Manager resources, see [Tagging resources in Incident Manager](tagging.md)\.

1. Choose **Create escalation plan**\.