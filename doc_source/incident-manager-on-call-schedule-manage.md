# Managing an existing on\-call schedule in Incident Manager<a name="incident-manager-on-call-schedule-manage"></a>

Use the content in this section to help you work with on\-call schedules you have already created\.

**Topics**
+ [Viewing on\-call schedule details](#on-call-schedule-details)
+ [Editing an on\-call schedule](#on-call-schedule-edit)
+ [Copying an on\-call schedule](#on-call-schedule-copy)
+ [Creating an override for an on\-call schedule rotation](#on-call-schedule-override)
+ [Deleting an on\-call schedule](#on-call-schedule-delete)

## Viewing on\-call schedule details<a name="on-call-schedule-details"></a>

You can access an at\-a\-glance summary of an on\-call schedule on the **View on\-call schedule details** page\. This page also contains information about who is currently on call and who is on call next\. The page includes a calendar view that shows which contacts are on call at any specific time\.

**To view on\-call schedule details**

1. Open the [Incident Manager console](https://console.aws.amazon.com/systems-manager/incidents/home)\. 

1. In the left navigation, choose **On\-call schedules**\. 

1. In the row for the on\-call schedule to view, do one of the following:
   + To open a summary view of the calendar, choose the schedule alias\.

     \-or\-

     Select the radio button for the row, and then choose **View**\.
   + To open a calendar view of the schedule, choose **View calendar** ![\[)The View calendar button\]](http://docs.aws.amazon.com/incident-manager/latest/userguide/images/on-call-calendar.png)

     In calendar view, choose the name of a contact on a specific date in the schedule to see details about the assigned shift or create an override,\.
   + To turn the display of a specific rotation in the calendar on or off choose the switch next to the rotation's name\.  
![\[Toggle buttons for an on-call calendar preview.\]](http://docs.aws.amazon.com/incident-manager/latest/userguide/images/on-call-calendar-toggles.png)

## Editing an on\-call schedule<a name="on-call-schedule-edit"></a>

You can update the configuration for an on\-call schedule and its rotations, except the following details:
+ The schedule alias
+ Rotation names
+ Rotation start dates

To use an existing calendar as the basis for a new calendar with the ability to change these values, you can copy the calendar instead\. For information, see [Copying an on\-call schedule](#on-call-schedule-copy)\.

**To edit an on\-call schedule**

1. Open the [Incident Manager console](https://console.aws.amazon.com/systems-manager/incidents/home)\. 

1. In the left navigation, choose **On\-call schedules**\. 

1. Do one of the following:
   + Select the radio button in the row for the on\-call schedule to edit, and then choose **Edit**\.
   + Choose the schedule alias for the on\-call schedule to open the **View on\-call schedule details** page, and then choose **Edit**\.

1. Make any modifications needed to the on\-call schedule and its rotations\. You can change rotation configuration options such as the start and end times, contacts, and recurrence\. You can add or remove rotations from the schedule as needed\. The calendar preview reflects your changes as you make them\.

   For information about working with the options on the page, see [Creating an on\-call schedule and rotation in Incident Manager](incident-manager-on-call-schedule-create.md)\.

1. Choose **Update**\.

**Important**  
If you edit a schedule that contains overrides, your changes can affect the overrides\. To ensure that your overrides remain configured as expected, we recommend reviewing your shift overrides closely after you update the schedule\.

## Copying an on\-call schedule<a name="on-call-schedule-copy"></a>

To use the configuration of an existing on\-call schedule as the starting point for a new schedule, you can create a copy of the calendar and modify it as needed\.

**To copy an on\-call schedule**

1. Open the [Incident Manager console](https://console.aws.amazon.com/systems-manager/incidents/home)\. 

1. In the left navigation, choose **On\-call schedules**\. 

1. Select the radio button in the row for the on\-call schedule to copy\.

1. Choose **Copy**\.

1. Make any modifications you need to the calendar and its rotations\. You can change, add, or remove rotations as needed\.
**Note**  
When you copy an existing schedule, you must specify new start dates for each rotation\. Copied schedules don't support rotations with start dates in the past\.

   For information about working with the options on the page, see [Creating an on\-call schedule and rotation in Incident Manager](incident-manager-on-call-schedule-create.md)\.

1. Choose **Create copy**\.

## Creating an override for an on\-call schedule rotation<a name="on-call-schedule-override"></a>

If you need to make one\-off changes to an existing rotation schedule, you can create an override\. An override lets you replace all or part of a contact's shift with another contact\. You can also create an override that spans across multiple shifts\.

You can only assign contacts to an override that are already assigned to the rotation\.

In the calendar preview, overridden shifts are shown with a striped background instead of a solid background\. In the following image, we can see that the contact Zhang Wei is on call in an override that include parts of the shifts for John Doe and Martha Rivera, starting May 5th and ending May 11th\.

![\[Illustration of an override shift covering parts of two other shifts\]](http://docs.aws.amazon.com/incident-manager/latest/userguide/images/on-call-rotation-override-example.png)

**To create an override for an on\-call schedule**

1. Open the [Incident Manager console](https://console.aws.amazon.com/systems-manager/incidents/home)\. 

1. In the left navigation, choose **On\-call schedules**\. 

1. In the row for the on\-call schedule to view, do one of the following:
   + Choose the schedule alias, then choose the **Schedule calendar** tab\.
   + Choose **View calendar** ![\[)The Calendar button\]](http://docs.aws.amazon.com/incident-manager/latest/userguide/images/on-call-calendar.png)\.

1. Do one of the following:
   + Choose **Create override**\.
   + Choose the name of a contact in the calendar preview, and then choose **Override shift**\.

1. In the **Create shift override** dialog box, do the following:
**Note**  
An override must be at least 30 minutes in length\. You can only specify an override for shifts that occur no more than six months in the future\.

   1. For **Select rotation**, select the name of the rotation to create an override in\.

   1. For **Start date**, select or enter the date when the override begins\.

   1. For **Start time**, enter the time when the override begins in `hh:mm` format\. 

   1. For **End date**, select or enter the date when the override ends\.

   1. For **End time**, enter the time when the override ends, in `hh:mm` format\. 

   1. For **Select override contact**, select the name of the rotation contact who is on call during the override period\. 

1. Choose **Create override**\.

After you create an override, you can identify it by its striped background\. When you choose the contact name for an overridden shift, an information box identifies it as an overridden shift\. You can choose **Delete override** to remove it and restore the original on\-call assignment\.

## Deleting an on\-call schedule<a name="on-call-schedule-delete"></a>

When you no longer need a particular on\-call schedule, you can delete it from Incident Manager\.

If any escalation plans or response plans currently use the on\-call schedule as an escalation channel, you should remove it from those plans before you delete the schedule\. 

**To delete an on\-call schedule**

1. Open the [Incident Manager console](https://console.aws.amazon.com/systems-manager/incidents/home)\. 

1. In the left navigation, choose **On\-call schedules**\. 

1. Select the radio button in the row for the on\-call schedule to delete\.

1. Choose **Delete**\.

1. In the **Delete on\-call schedule? **dialog box, enter **confirm** in the text box\.

1. Choose **Delete**\.