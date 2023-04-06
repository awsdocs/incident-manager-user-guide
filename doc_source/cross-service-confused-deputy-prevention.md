# Cross\-service confused deputy prevention in Incident Manager<a name="cross-service-confused-deputy-prevention"></a>

The confused deputy problem is an information security issue that occurs when an entity without permission to perform an action calls a more\-privileged entity to perform the action\. This can allow malicious actors to run commands or modify resources they otherwise would not have permission to run or access\.

In AWS, cross\-service impersonation can lead to a confused deputy scenario\. Cross\-service impersonation is when one service \(the *calling service*\) calls another service \(the *called service*\)\. A malicious actor can use the calling service to alter resources in another service using permissions that they normally would not have\.

AWS provides service principals with managed access to resources on your account to help you protect your resources' security\. We recommend using the [https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-sourcearn](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-sourcearn) and [https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-sourceaccount](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-sourceaccount) global condition context keys in your resource policies\. These keys limit the permissions that AWS Systems Manager Incident Manager gives another service to that resource\. If you use both global condition context keys, the `aws:SourceAccount` value and the account referenced in the `aws:SourceArn` value must use the same account ID when used in the same policy statement\.

The value of `aws:SourceArn` must be the ARN of the affected incident record\. If you don't know the full ARN of the resource, or if you are specifying multiple resources, use the `aws:SourceArn` global context condition key with the `*` wildcard for the unknown portions of the ARN\. For example, you can set `aws:SourceArn` to `arn:aws:ssm-incidents::111122223333:*`\. 

In the following trust policy example, we use the `aws:SourceArn` condition key to restrict access to the service role based on the incident record's ARN\. Only incident records created from the response plan `myresponseplan` are able to use this role\.

```
{
  "Version": "2012-10-17",
  "Statement": {
    "Effect": "Allow",
    "Principal": { "Service": "ssm-incidents.amazonaws.com" },
    "Action": "sts:AssumeRole",
    "Condition": {
      "ArnLike": {
        "aws:SourceArn": "arn:aws:ssm-incidents:*:111122223333:incident-record/myresponseplan/*"
      }
    }
  }
}
```