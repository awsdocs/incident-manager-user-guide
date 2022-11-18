# Store PagerDuty access credentials in an AWS Secrets Manager secret<a name="integrations-pagerduty-secret"></a>

After you turn on integration with PagerDuty, Incident Manager can create a corresponding incident in PagerDuty when your create a new incident in Incident Manager\. The paging structure and escalation policies you created in PagerDuty are used in the PagerDuty environment\. However, Incident Manager doesn't import your PagerDuty configuration\. You also have the option of automatically resolving the PagerDuty incident when you resolve the related incident in Incident Manager\. 

To integrate Incident Manager with PagerDuty, you must first create a secret in AWS Secrets Manager that contains your PagerDuty credentials\. These allow Incident Manager to communicate with your PagerDuty service\. You can then include a PagerDuty service in response plans that you create in Incident Manager\.

This secret you create in Secrets Manager must contain, in the proper JSON format, the following:
+ An API key from your PagerDuty account\. You can use either a General Access REST API Key or a User Token REST API Key\.
+ A valid user email address from your PagerDuty subdomain\.
+ The PagerDuty service region where you deployed your subdomain\. 
**Note**  
All services in a PagerDuty subdomain are deployed to the same service region\.

**Prerequisites**  
Before creating the secret in Secrets Manager, ensure that you meet the following requirements\.

**KMS key**  
You must encrypt the secret you create with a *customer managed key* that you have created in AWS Key Management Service \(AWS KMS\)\. You specify this key when you create the secret that stores you PagerDuty credentials\.   
Secrets Manager provides the option of encrypting the secret with an AWS managed key, but this encryption mode is not supported\.
The customer managed key must meet the following requirements:  
+ **Key type**: Choose **Symmetric**\.
+  **Key usage**: Choose **Encrypt and decrypt**\.
+ **Regionality**: If you want to replicate your response plan to multiple AWS Regions, ensure that you select **Multi\-Region key**\.
+ **Key policy**: The IAM user that is configuring the response plan must have permission for `kms:GenerateDataKey` and `kms:Decrypt` in the key's resource\-based policy\. The `ssm-incidents.amazonaws.com` service principal must have permission for `kms:GenerateDataKey` and `kms:Decrypt` in the key's resource based policy\.

  The following policy demonstrates these permissions, which you add to the `Statement` section:

  ```
  {
      "Version": "2012-10-17",
      "Id": "key-consolepolicy-3",
      "Statement": [
          {
              "Sid": "Allow creator of response plan to use the key",
              "Effect": "Allow",
              "Principal": {
                  "AWS": "iam_arn_of_principal_creating_response_plan"
              },
              "Action": [
                  "kms:Decrypt",
                  "kms:GenerateDataKey*"
              ],
              "Resource": "*"
          },
          {
              "Sid": "Allow Incident Manager to use the key",
              "Effect": "Allow",
              "Principal": {
                  "Service": "ssm-incidents.amazonaws.com"
              },
              "Action": [
                  "kms:Decrypt",
                  "kms:GenerateDataKey*"
              ],
              "Resource": "*"
          }
      ]
  }
  ```

  For information about creating a new customer managed key, see [Creating symmetric encryption KMS keys](https://docs.aws.amazon.com/kms/latest/developerguide/create-keys.html#create-symmetric-cmk) in the *AWS Key Management Service Developer Guide*\. For more information about AWS KMS keys, see [AWS KMS concepts](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html)\.

  If an existing customer managed key meets all the previous requirements, you can edit its policy to add these permissions\. For information about updating the policy in a customer managed key, see [Changing a key policy](https://docs.aws.amazon.com/kms/latest/developerguide/key-policy-modifying.html) in the *AWS Key Management Service Developer Guide*\.
**Tip**  
You can specify a condition key to limit access even further\. For example, the following policy allows access through Secrets Manager in the US East \(Ohio\) Region \(us\-east\-2\) only:  

  ```
  {
              "Sid": "Enable IM Permissions",
              "Effect": "Allow",
              "Principal": {
                  "Service": "ssm-incidents.amazonaws.com"
              },
              "Action": ["kms:Decrypt", "kms:GenerateDataKey*"],
              "Resource": "*",
              "Condition": {
                  "StringEquals": {
                      "kms:ViaService": "secretsmanager.us-east-2.amazonaws.com"
                  }
              }
          }
  ```

**`GetSecretValue` permission**  
The IAM identity \(user, role, or group\) that creates the response plan must have the IAM permission `secretsmanager:GetSecretValue`\. 

**To store PagerDuty access credentials in an AWS Secrets Manager secret**

1. Follow the steps through Step 3a in [Create an AWS Secrets Manager secret](https://docs.aws.amazon.com/secretsmanager/latest/userguide/create_secret.html) in the *AWS Secrets Manager User Guide*\.

1. For Step 3b, for **Key/value pairs**, do the following:
   + Choose the **Plaintext** tab\.
   + Replace the default contents of the box with the following JSON structure:

     ```
     {
         "pagerDutyToken": "pagerduty-token",
         "pagerDutyServiceRegion": "pagerduty-region",
         "pagerDutyFromEmail": "pagerduty-email"
     }
     ```
   + In the JSON sample you pasted, replace the *placeholder values* as follows:
     + *pagerduty\-token*: The value of a General Access REST API Key or a User Token REST API Key from your PagerDuty account\.

       For related information, see [API Access Keys](https://support.pagerduty.com/docs/api-access-keys) in the *PagerDuty Knowledge Base*\.
     + *pagerduty\-region*: The service region of the PagerDuty data center that hosts your PagerDuty subdomain\.

       For related information, see [Service Regions](https://support.pagerduty.com/docs/service-regions) in the *PagerDuty Knowledge Base*\.
     + *pagerduty\-email*: The valid email address for a user that belongs to your PagerDuty subdomain\.

       For related information, see [Manage Users](https://support.pagerduty.com/docs/users) in the *PagerDuty Knowledge Base*\.

     The following example shows a completed JSON secret containing the required PagerDuty credentials:

     ```
     {
         "pagerDutyToken": "y_NbAkKc66ryYEXAMPLE",
         "pagerDutyServiceRegion": "US",
         "pagerDutyFromEmail": "JohnDoe@example.com"
     }
     ```

1. On Step 3c, for **Encryption key**, choose a customer managed key you created that meets the requirements listed under the previous **Prerequisites** section\.

1. On Step 4c, for **Resource permissions**, do the following:
   + Expand **Resource permissions**\.
   + Choose **Edit permissions**\.
   + Replace the default contents of the policy box with the following JSON structure:

     ```
     {
         "Effect": "Allow",
         "Principal": {
             "Service": "ssm-incidents.amazonaws.com"
         },
         "Action": "secretsmanager:GetSecretValue",
         "Resource": "*"
     }
     ```
   + Choose **Save**\.

1. On Step 4d, for **Replicate secret**, do the following if you replicated your response plan to more than one AWS Region:
   + Expand **Replicate secret**\.
   + For **AWS Region**, select the Region where you replicated your response plan to\.
   + For **Encryption key**, choose a customer managed key you created in, or replicated to, this Region that meets the requirements listed under the **Prerequisites** section\. 
   + For each additional AWS Region, choose **Add Region** and select the Region name and customer managed key\.

1. Complete the remaining steps in [Create an AWS Secrets Manager secret](https://docs.aws.amazon.com/secretsmanager/latest/userguide/create_secret.html) in the *AWS Secrets Manager User Guide*\. 

For information about how to add a PagerDuty service to a Incident Manager incident workflow, see [Integrate a PagerDuty service into the response plan](response-plans.md#anchor-pagerduty) in the topic [Create a response plan](response-plans.md#response-plans-create)\.

**Related information**

[Secret encryption in AWS Secrets Manager](https://docs.aws.amazon.com/secretsmanager/latest/userguide/security-encryption.html) in the *AWS Secrets Manager User Guide*\.