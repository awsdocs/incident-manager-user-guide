# Working with on\-call schedules in Incident Manager<a name="incident-manager-on-call-schedule"></a>

An on\-call schedule in Incident Manager defines who is notified when an incident occurs that requires operator intervention\. An on\-call schedule consists of one or more rotations you create for the schedule\. Each rotation can include up to 30 contacts\.

After you create an on\-call schedule, you can include it as an escalation in your escalation plan\. When an incident associated with that escalation plan occurs, Incident Manager notifies the operator \(or operators\) who are on call according to the schedule\. This contact can then acknowledge the engagement\. In your escalation plan, you can designate one or more on\-call schedules, as well as one or more individual contacts, across multiple stages of escalation\. For more information, see [Working with escalation plans in Incident Manager](escalation.md)\.

**Tip**  
As a best practice, we recommend adding contacts and on\-call schedules as the escalation channels in an escalation plan\. You should then choose an escalation plan as the engagement in a response plan\. This approach provides the fullest coverage for incident response in your organization\.

Each on\-call schedule supports up to eight rotations\. Rotations can overlap or run concurrently\. This increases the number of operators notified to respond when an incident occurs\. You can also create rotations that run consecutively\. This supports scenarios like "follow the sun" incident management where you have groups around the world that support the same service\.

Use the topics in this section to help you create and manage on\-call schedules for your incident response operations\.

**Topics**
+ [Creating an on\-call schedule and rotation in Incident Manager](incident-manager-on-call-schedule-create.md)
+ [Managing an existing on\-call schedule in Incident Manager](incident-manager-on-call-schedule-manage.md)