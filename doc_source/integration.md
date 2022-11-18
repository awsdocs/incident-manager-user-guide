# Product and service integrations with Incident Manager<a name="integration"></a>

Incident Manager, a component of AWS Systems Manager, integrates with the following products and services\.

You can integrate Incident Manager, a component of AWS Systems Manager, with multiple products and services\. After you configure integration, when you create a new incident in Incident Manager, the integration creates the incident in the integrated product or service as well\. If you update an incident in Incident Manager, it makes these updates to the corresponding incident in the integrated product or service\. If you resolve an incident in either Incident Manager or the connected product or service, the integration resolves the incident in both services based on which preferences you configure\.

The following products and services can integrate with Incident Manager:
+  **[ServiceNow](https://www.servicenow.com/)** – For more information, see [Integrating AWS Systems Manager Incident Manager in ServiceNow](https://docs.aws.amazon.com/smc/latest/ag/sn-im.html)\.
+  **[Jira Service Management](https://www.atlassian.com/software/jira/service-management)** – For more information, see [Configuring Jira Service Management](https://docs.aws.amazon.com/smc/latest/ag/jsd-integration-configure-jsd.html) in the *AWS Service Management Connector Administrator Guide*\.
+  **[Jira Cloud](https://www.atlassian.com/enterprise/cloud)** – For more information, see [Integrating AWS Systems Manager Incident Manager](https://docs.aws.amazon.com/smc/latest/ag/jsmcloud-im.html) in the *AWS Service Management Connector Administrator Guide*\.<a name="integration-PagerDuty"></a>
+ **[PagerDuty](https://www.pagerduty.com)** – Turning on integration with [PagerDuty](https://www.pagerduty.com) makes it possible to add a PagerDuty service to your response plan\. To integrate Incident Manager with PagerDuty, you must first create a secret in AWS Secrets Manager that contains your PagerDuty credentials\.

  For information about adding a PagerDuty REST API Key and other required details to a secret in AWS Secrets Manager, see [Store PagerDuty access credentials in an AWS Secrets Manager secret](integrations-pagerduty-secret.md)\.

  For information about adding a PagerDuty service from your PagerDuty account to a response plan in Incident Manager, see the steps for [Integrate a PagerDuty service into the response plan](response-plans.md#anchor-pagerduty) in the topic [Create a response plan](response-plans.md#response-plans-create)\.