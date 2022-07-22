# Incident details<a name="tracking-details"></a>

The Incident Manager incident details page is an incident responder's point of reference for all things involved in an incident\. The incident details page automatically populates incidents as they're created\.

The top banner on every incident details page includes the **Incident title**, **Impact**, **Chat channel**, and **Duration**\. You can edit the incident title, impact, and chat channel by choosing **Edit** in the top\-right corner of the banner\. 

The incident details page has seven tabs, making it easier for responders to locate and view information during an incident\. The tabs display a counter in the tab name, which indicates the number of updates to the tab\. For more information about the contents of each tab as well as available actions, continue reading\.

**Topics**
+ [Overview](#tracking-details-overview)
+ [Metrics](#tracking-details-metrics)
+ [Timeline](#tracking-details-timeline)
+ [Runbooks](#tracking-details-runbook)
+ [Tags](#tracking-details-tags)
+ [Engagements](#tracking-details-engagements)
+ [Related items](#tracking-details-related)
+ [Properties](#tracking-details-properties)

## Overview<a name="tracking-details-overview"></a>

The **Overview** tab is the landing page for responders\. It contains the incident summary, a list of recent timeline events, and the current runbook step\.

Responders use the **Summary** to catch up on what actions have been taken, the results of any changes, possible next steps, and information about the impact of the incident\. To update the summary, choose **Edit** in the top\-right corner of the **Summary** section\.

**Important**  
If multiple responders are editing the summary field simultaneously, the responder who submits their edits last overwrites all other input\. 

The **Recent timeline events** section contains a timeline populated by Incident Manager with the five most recent events\. Use this section to understand the status of the incident and what has recently occurred\. To view a complete timeline, continue to the **Timeline** tab\. 

The overview page also displays the **Current runbook step**\. This step may be an automatic step running in your AWS environment, or it may be a set of manual instructions for responders\. To view the complete runbook, including prior and upcoming steps, choose the **Runbook** tab\.

## Metrics<a name="tracking-details-metrics"></a>

The **Metrics** tab contains vital information about your AWS hosted applications and systems\. Incident Manager uses Amazon CloudWatch to populate the metrics and alarm graphs found on this tab\. To learn more about incident management best practices for defining alarms and metrics, see [Monitoring](incident-response.md#incident-response-monitoring) in the *Incident planning* section of this user guide\.

**To add metrics**
+ Choose **Add** in the upper\-right corner of this tab\.
  + To add a metric from an existing CloudWatch dashboard, choose **From existing CloudWatch dashboard**\.

    1. Choose a **Dashboard**\. This adds all metrics and alarms that are part of the chosen dashboard\.

    1. \(Optional\) You can also **Select metrics** from the dashboard to view specific metrics\.
  + Add a single metric by selecting **From CloudWatch** and pasting a metric source\. To copy a metric source:

    1. Open the CloudWatch console at [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/)\.

    1. In the navigation pane, choose **Metrics**\. 

    1. In the **All metrics** tab, enter a search term in the search field, such as a metric name or resource name, and choose **Enter**\.

       For example, if you search for the `CPUUtilization` metric, you will see the namespaces and dimensions associated with this metric\.

    1. Choose one of the results from your search to view the metrics\.

    1. Choose the **Source** tab and copy the source\.

Metric alarm graphs can only be added to the incident details through the related response plan, or by selecting **From existing CloudWatch dashboard** when adding a metric\.

To remove metrics, choose **Remove**, and then choose the metrics you want to remove from the provided **Metrics** dropdown\.

## Timeline<a name="tracking-details-timeline"></a>

Use the **Timeline** tab to track events that occur during an incident\. Incident Manager automatically populates timeline events that identify significant occurrences during the incident\. Responders can add custom events based on occurrences that are detected manually\. During the post\-incident analysis, the timeline tab provides valuable insights into how to better prepare and respond to incidents in the future\. For more information about post\-incident analysis, see [Post\-incident analysis](analysis.md)\. 

To add a custom timeline event, choose **Add**\. Select a date using the calendar, and then enter a time\. All times are shown in your local time zone\. Provide a brief description of the event that appears in the timeline\. 

To edit an existing custom event, select the event on the timeline and choose **Edit**\. You can change the time, date, and description of *custom* events\. You can only edit custom events\.

## Runbooks<a name="tracking-details-runbook"></a>

The runbooks tab of the incident details page is where responders can view runbook steps and start new runbooks\. 

To start a new runbook, choose **Start runbook** in the **Runbooks** section\. Use the search field to find the runbook you want to start\. Provide any required **Parameters** and the **Version** of the runbook you want to use when starting the runbook\. Runbooks started during an incident from the **Runbooks** tab use the permissions of the currently signed\-in account\.

To navigate to a runbook definition in Systems Manager, choose the runbook's title under **Runbooks**\. To navigate to the running instance of the runbook in Systems Manager, choose the execution details under **Execution details**\. These pages display the template used to start the runbook and the specific details of the currently running instance of the automation document\. 

The **Runbook steps** section displays the list of steps that the selected runbook automatically takes or responders manually perform\. The steps expand as they become the current step, displaying information required to complete the step, or details about what the step does\. Automatic runbook steps resolve after the automation is complete\. Manual steps require the responders to choose **Next step** at the bottom of each step\. After a step is complete, the step output appears as a dropdown\.

To cancel a runbook execution, choose **Cancel runbook**\. This will stop the execution of the runbook and not complete any further steps in the runbook\.

## Tags<a name="tracking-details-tags"></a>

The **Tags** section displays the tag keys and values associated with the incident record\. For more information about tags in Incident Manager, see [Tagging Incident Manager resources](tagging.md)\.

## Engagements<a name="tracking-details-engagements"></a>

The **Engagements** tab of the incident details drives the engagement of responders and teams\. From this tab, you can see who has been engaged, who has responded, as well as which responders are going to be engaged as part of an escalation plan\. Responders can engage other contacts directly from this tab\. To learn more about creating contacts and escalation plans, see the [Contacts](contacts.md) and [Escalation plans](escalation.md) sections of this guide\. 

You can configure response plans with contacts and escalation plans to automatically start engagement at the beginning of an incident\. To learn more about configuring response plans, see the [Response plans](response-plans.md) section of this guide\.

You can find information about each contact in the table\. This table includes the following information:
+ **Name** – Links to the contact's details page that displays the contact's contact methods and engagement plan\.
+ **Escalation plan** – Links to the escalation plan used to engage the contact\.
+ **Engaged** – Displays when the contact was engaged or when they will be engaged as part of an escalation plan\.
+ **Acknowledged** – Displays whether the contact has acknowledged the engagement\.

To acknowledge an engagement, the responder can do one of the following:
+ Phone call – Enter **1** when prompted\.
+ SMS – Reply to the message with the provided code, or enter the provided code on the **Engagements** tab of the incident\.
+ Email – Enter the provided code on the **Engagements** tab of the incident\.

## Related items<a name="tracking-details-related"></a>

The **Related Items** tab is used to collect resources related to incident mitigation\. These resources can be ARNs, links to external resources, or files uploaded to Amazon S3 buckets\. The table displays a descriptive title and either an ARN, a link, or bucket details\. Before using S3 buckets, review [Security Best Practices for Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/security-best-practices.html)\.

When uploading files to an Amazon S3 bucket, versioning is either enabled or suspended on that bucket\. When versioning is enabled on the bucket, files uploaded with the same name as an existing file are added as a new version of the file\. If versioning is suspended, files uploaded with the same name as an existing file overwrite the existing file\. To learn more about versioning, see [Using versioning in S3 buckets](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Versioning.html)\.

When removing a file related item, the file is removed from the incident but is not removed from the Amazon S3 bucket\. To learn more about removing objects from an Amazon S3 bucket, see [Deleting Amazon S3 objects](https://docs.aws.amazon.com/AmazonS3/latest/userguide/DeletingObjects.html)\.

## Properties<a name="tracking-details-properties"></a>

The properties tab provides details about the incident\. You can view the following details:
+ **Status** – Describes the current status of the incident\. The incident can be **Open** or **Resolved**
+ **Start time** – The time when the incident was created in Incident Manager\.
+ **Resolved time** – The time that the incident was resolved in Incident Manager\.
+ **Amazon Resource Name** \(ARN\) – The ARN of the incident\. Use the ARN when referencing the incident from the chat or with AWS Command Line Interface \(AWS CLI\) commands\.
+ **Response Plan** – Identifies the response plan for the selected incident\. Choosing the response plan opens the response plan's details page\.
+ **Parent OpsItem** – Identifies the OpsItem created as the parent of the incident\. A parent OpsItem can have multiple related incidents and follow up action items\. Selecting the parent OpsItem opens the OpsItems details page in OpsCenter\.
+ **Analysis** – Identifies the analysis created from this incident\. Create an analysis from a resolved incident to improve your incident response process\. Choose the analysis to open the analysis details page\.
+ **Owner** – The account in which the incident was created\.