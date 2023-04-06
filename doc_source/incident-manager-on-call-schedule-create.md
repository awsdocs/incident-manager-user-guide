# Creating an on\-call schedule and rotation in Incident Manager<a name="incident-manager-on-call-schedule-create"></a>

Create an on\-call schedule with one or more rotations of contacts to engage to respond to incidents during their shifts\.

Before you create an on\-call schedule, ensure that you previously created the contacts you want to add to the rotations in the schedule\. For information, see [Working with contacts in Incident Manager](contacts.md)\.

**To create on on\-call schedule**

1. Open the [Incident Manager console](https://console.aws.amazon.com/systems-manager/incidents/home)\. 

1. In the left navigation, choose **On\-call schedules**\. 

1. Choose **Create on\-call schedule**\.

1. For **Schedule name**, enter a name to help you identify the schedule, such as **MyApp Primary On\-callSchedule**\.

1. For **Schedule alias**, enter an alias for this schedule that is unique in the current AWS Region, such as **my\-app\-primary\-on\-call\-schedule**\.

1. \(Optional\) In the **Tags** area, apply one or more tag key name and value pairs to the on\-call schedule\.

   Tags are optional metadata that you assign to a resource\. Tags allow you to categorize a resource in different ways, such as by purpose, owner, or environment\. For example, you can tag a schedule to identify the period of time in which it runs, the types of operators it contains, or the escalation plan it supports\. For more information about tagging Incident Manager resources, see [Tagging resources in Incident Manager](tagging.md)\.

1. Continue by [adding one or more rotations to the on\-call schedule](#on-call-schedule-rotation-times)\.

## Creating a rotation for an on\-call schedule in Incident Manager<a name="on-call-schedule-rotation-times"></a>

A rotation in an on\-call schedule specifies when the shift is in effect\. It also specifies the contacts that shifts rotate through\. You can include up to eight rotations in a single on\-call schedule\.

You can add any individuals you created as a contact in Incident Manager to a rotation\. For information about managing your contacts, see [Working with contacts in Incident Manager](contacts.md)\.

As you configure your rotation, you can see how the overall schedule looks in a **Preview** calendar on the right side of the page\.

**To create a rotation for an on\-call schedule**

1. In the **Rotation 1** section of the **Create on\-call schedule** page, for **Rotation name**, enter a name that identifies the rotation, such as **00:00 \- 7:59 Support**, or **Dublin Support Group**\.

1. For **Start date**, enter the date when this rotation becomes active in `YYYY/MM/DD` format, such as `2023/07/14`\.

1. For **Time zone**, select the global time zone that serves as the basis for shift coverage times and dates you specify for this rotation\. Note that you can base each rotation on its own time zone\.

   You can use any time zone defined by the Internet Assigned Numbers Authority \(IANA\)\. For example: "America/Los\_Angeles", "UTC", "Asia/Seoul"\. For more information, see the [Time Zone Database](https://www.iana.org/time-zones) on the IANA website\.

1. For **Rotation start time**, enter the time when this rotation's shift begins in 24\-hour `hh:mm` format, such as `16:00`\.

   Note the differences in local time for contacts in time zones different from the one you specified\. For example, if you choose `America/Los_Angeles` as the time zone and `00:00` as the rotation start time, this equals 08:00 in Dublin, Ireland, and 13:30 in Mumbai, India\.

1. For **Rotation end time**, enter the time when this rotation's shift ends in 24\-hour `hh:mm` format, such as `23:59`\.
**Note**  
The length of time between the start and end of a rotation must be at least 30 minutes\.

1. \(Optional\) To set the rotation length to 24 hours, select **24\-hour coverage** and enter the start time for this rotation in the **Rotation start time** field\. The **Rotation end time** value updates automatically\.

   For example, if you want your on\-call to have 24\-hour coverage with the shift change at 11 AM, choose **24\-hour coverage** and enter **11:00** as the start time\.

1. For **Active days**, select the days of the week that this rotation is active\. If your on\-call plan excludes weekend coverage for example, select all the days except **Sunday** and **Saturday**\.

1. Continue by [adding contacts to the rotation](#on-call-schedule-rotation-contacts)\.

## Adding contacts to a rotation in an on\-call schedule in Incident Manager<a name="on-call-schedule-rotation-contacts"></a>

For each rotation in your on\-call schedule, you can add one or more contacts, up to a total of 30\. You choose from contacts who are set up in your Incident Manager configuration\. 

When you add a contact to a rotation, the contact may receive notifications as part of their on\-call duties\. Notifications may be sent by email, SMS, or voice call as specified in a contact's details\. 

For information about managing your contacts and contacts notification options, see [Working with contacts in Incident Manager](contacts.md)\.

**To add contacts to a rotation in an on\-call schedule**

1. On the **Create on\-call schedule** page, in the **Contacts** section for the rotation, choose **Add or remove contacts**\.

1. In the **Add or remove contacts** dialog box, select the aliases of the contacts to include in the rotation\.

   The order that you select the contacts in is the order that they are first listed in the rotation schedule\. You can change the order after you add contacts\.

1. Choose **Confirm**\.

1. To change a contact's position in the order, select the radio button for that user and use the Up \(![\[)The Up button\]](http://docs.aws.amazon.com/incident-manager/latest/userguide/images/on-call-Up.png)\) and Down \(![\[)The Down button\]](http://docs.aws.amazon.com/incident-manager/latest/userguide/images/on-call-Down.png)\) buttons to update the contact order\.

1. Continue by [specifying individual shift recurrence and length](#on-call-schedule-rotation-recurrence-and-tags) for the rotation\.

## Specifying shift recurrence and length and adding tags to a rotation in Incident Manager<a name="on-call-schedule-rotation-recurrence-and-tags"></a>

Shift recurrence specifies how frequently the contacts in a rotation rotate in and out of being on call\. Recurrence lengths can be specified in a number of days, weeks, or months\.

**To specify shift recurrence and length and add tags to a rotation**

1. On the **Create on\-call schedule** page, in the **Recurrence settings** section for the rotation, do the following:
   + For **Shift recurrence type**, specify whether each on\-call's shift lasts a number of days, weeks, or months by choosing from `Daily`, `Weekly`, and `Monthly`\.
   + For **Shift length**, enter how many days, weeks, or months a shift lasts\.

     For example, if you chose `Daily` and enter **1**, each contact's on\-call shift lasts one day\. If you chose `Weekly` and enter **3**, each contact's on\-call shift lasts three weeks\.

1. \(Optional\) In the **Tags** area, apply one or more tag key name and value pairs to the rotation\.

   Tags are optional metadata that you assign to a resource\. Tags allow you to categorize a resource in different ways, such as by purpose, owner, or environment\. For example, you can tag a rotation to identify the location of the contacts assigned to it, the type of coverage it's meant to provide, or the escalation plan it will support\. For more information about tagging Incident Manager resources, see [Tagging resources in Incident Manager](tagging.md)\.

1. \(Recommended\) Use the calendar preview to ensure there are no unintended gaps in coverage for your on\-call schedule\.

1. Choose **Create**\.

You can now add the on\-call schedule as an escalation channel in an escalation plan\. For information, see [Create an escalation plan](escalation.md#escalation-create)\.