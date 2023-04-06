# Troubleshooting AWS Systems Manager Incident Manager<a name="troubleshooting"></a>

If you encounter issues while using AWS Systems Manager Incident Manager, you can use the following information to resolve them according to our best practices\. If the issues you encounter are outside the scope of the following information, or if they persist after you've tried to resolve them, contact [AWS Support](http://aws.amazon.com/premiumsupport/)\.

**Topics**
+ [Error message: `ValidationException – We were unable to validate the AWS Secrets Manager secret`](#troubleshooting-response-plans-pagerduty)
+ [Other troubleshooting issues](#troubleshooting-problem3)

## Error message: `ValidationException – We were unable to validate the AWS Secrets Manager secret`<a name="troubleshooting-response-plans-pagerduty"></a>

**Problem 1**: The AWS Identity and Access Management \(IAM\) identity \(user, role, or group\) that creates the response plan doesn't have the `secretsmanager:GetSecretValue` IAM permission\. IAM identities must have this permission to validate Secrets Manager secrets\.
+ **Solution**: Add the missing `secretsmanager:GetSecretValue` permission to the IAM policy for the IAM identity that creates the response plan\. For information, see [Adding IAM identity permissions \(console\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-attach-detach.html#add-policies-console) or [Adding IAM policies \(AWS CLI\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-attach-detach.html#add-policy-cli) in the *IAM User Guide*\.

**Problem 2**: The secret doesn't have a resource\-based policy attached that allows the IAM identity to run the [https://docs.aws.amazon.com/secretsmanager/latest/apireference/API_GetSecretValue.html](https://docs.aws.amazon.com/secretsmanager/latest/apireference/API_GetSecretValue.html) action, or the resource\-based policy denies permission to the identity\.
+ **Solution**: Create or add an `Allow` statement to the secret's resource\-based policy that grants permission for `secrets:GetSecretValue` to the IAM identity\. Or, if you use a `Deny` statement that includes the IAM identity, update the policy so the identity can run the action\. For information, see [Attach a permissions policy to an AWS Secrets Manager secret](https://docs.aws.amazon.com/secretsmanager/latest/userguide/auth-and-access_resource-policies.html) in the *AWS Secrets Manager User Guide*\.

**Problem 3**: The secrets doesn't have a resource\-based policy attached that allows access to the Incident Manager service principal, `ssm-incidents.amazonaws.com`\.
+ **Solution**: Create or update the resource\-based policy for the secret and include the following permission:

  ```
  {
      "Effect": "Allow",
      "Principal": {
          "Service": ["ssm-incidents.amazonaws.com"]
      },
      "Action": "secretsmanager:GetSecretValue",
      "Resource": "*"
  }
  ```

**Problem 4**: The AWS KMS key selected to encrypt the secret isn't a customer managed key, or the selected customer managed key doesn't provide the IAM permissions `kms:Decrypt` and `kms:GenerateDataKey*` to the Incident Manager service principal\. Alternately, the IAM identity that creates the response plan may not have the IAM permission [https://docs.aws.amazon.com/secretsmanager/latest/apireference/API_GetSecretValue.html](https://docs.aws.amazon.com/secretsmanager/latest/apireference/API_GetSecretValue.html)\.
+ **Solution**: Ensure that you meet the requirements described under **Prerequisites** in the topic [Storing PagerDuty access credentials in an AWS Secrets Manager secret](integrations-pagerduty-secret.md)\.

**Problem 5**: The ID of the secret that contains the General Access REST API Key or User Token REST API Key isn't valid\.
+ **Solution**: Ensure that you entered the ID of the Secrets Manager secret accurately, with no trailing space\. You must work in the same AWS Region that stores the secret you want to use\. You can't use a deleted secret\.

**Problem 6**: In rare cases, the Secrets Manager service may experience an issue, or Incident Manager might have trouble communicating with it\.
+ **Solution**: Wait a few minutes, then try again\. Check the [AWS Health Dashboard](https://phd.aws.amazon.com/) for any issues that might affect either service\.

## Other troubleshooting issues<a name="troubleshooting-problem3"></a>

If the previous steps didn't resolve your issue, you can find additional help from the following resources:
+ For IAM issues specific to Incident Manager when you access the [Incident Manager console](https://console.aws.amazon.com/systems-manager/incidents/home), see [Troubleshooting AWS Systems Manager Incident Manager identity and access](security_iam_troubleshoot.md)\.
+ For general authentication and authorization issues when you access the AWS Management Console, see [Troubleshooting IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/troubleshoot.html) in the *IAM User Guide*