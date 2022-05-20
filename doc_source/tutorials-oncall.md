# Configuring on\-call rotations<a name="tutorials-oncall"></a>

An on\-call rotation uses a schedule to rotate through a group of on\-call contacts, ensuring that there is always an contact on\-call\. On\-call contacts are tasked with monitoring and immediately responding when an incident happens\. A new contact is assigned as the on\-call contact according to the schedule you set\. Rotating the on\-call contact prevents alert fatigue\. This tutorial walks you through how to configure an on\-call rotation using Amazon S3, AWS CloudFormation, and AWS Systems Manager Incident Manager\. 

This on\-call rotation script automatically replaces the first contact in an escalation plan with the next on\-call contact from a list of provided contacts\. The frequency and timing that the contacts are rotated is customizable\. 

**Note**  
This tutorial uses Amazon S3, CloudFormation, AWS Lambda, and Amazon CloudWatch Events\. You will incur costs from these services by doing this tutorial\.

## Set up an on\-call rotation<a name="tutorials-oncall-setup"></a>

**Prerequisites**
+ A configured escalation plan with one or more contacts\. For more information about configuring an escalation plan, see the [Escalation plans](escalation.md) section of this user guide\. 
+ Two or more configured contacts\. These contacts are the group of on\-call contacts used during this set up\. For more information about configuring contacts, see the [Contacts](contacts.md) section of this user guide\. 

**Download the on\-call rotation script**  
The `OncallRotationScript` zip file contains both the CloudFormation YAML template and the python boto3 layer required for the AWS Lambda script\.

1. Download the `[oncallRotationScript\.zip](samples/oncallRotationScript.zip)` file\. 

1. Unzip the `oncallRotationScript.zip` file\.

**Create an S3 bucket and upload the boto3 layer**

1. Open the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.

1. Choose **Create bucket**\.

1. Give the bucket a unique name\.

1. Select a region\. This must be the same region that you create your AWS CloudFormation stack in later in this tutorial\.

1. Choose **Block *all* public access**\.

1. Disable **Bucket Versioning**\.

1. \(Optional\) Add tags\.

1. Disable **Server\-side encryption**\.

1. Choose **Create bucket**\.

1. Choose the bucket that you just created\.

1. Choose **Upload**\.

1. Choose **Add files**, select the `boto3_layer.zip` file, and choose **Upload**\.

**Note**  
Note the bucket name and the object key\. You will use these while configuring your CloudFormation stack\.

**Configure and deploy the CloudFormation stack**

1. Open the CloudFormation console at [https://console\.aws\.amazon\.com/cloudformation](https://console.aws.amazon.com/cloudformation/)\.

1. Choose **Stacks** from the left navigation menu\.

1. Choose **Create stack**, and then **With new resources \(standard\)**\.

1. Choose **Template Is ready**\.

1. Choose **Upload a template file**\.

1. Choose **Choose file** and select the `oncall_updater.yml` file\.

1. Choose **Next**\.

1. Enter a **Stack Name**\.

1. Set the following parameters in the **Parameters** section:

   1. **BotoLayerS3Bucket** – Enter the name of the bucket that you created in the previous section for the boto3 layer\. 

   1. **BotoLayerS3Key** – Provide the object key of the boto3 layer file that you uploaded to your Amazon S3 bucket\. The object key is the name of the boto3 layer file, boto3\_layer\.zip\. If you changed the name of the file, enter that name instead\.

   1. **ContactRotation** – Enter a comma separated list of Amazon Resource Names \(ARNs\) for each contact in the on\-call rotation\. You can find the ARN for each contact on the contact's details page\. The following string is an example of a comma separated list of contact ARNs:

      ```
      arn:aws:ssm-contacts:us-east-1:123456789101:contact/arosalez,arn:aws:ssm-contacts:us-east-1:123456789101:contact/csalazar,arn:aws:ssm-contacts:us-east-1:123456789101:contact/dramirez
      ```

   1. **CronExpression** – Provide a cron expression that defines when the on\-call contact will be rotated\. A cron expression is defined in GMT\. The following example will rotate the on\-call contact every Thursday at 17:00 GMT:

      ```
      00 17 ? * THU *
      ```

   1. **EscalationPlanArn** – Provide the ARN of the escalation plan you will use for the on\-call rotation\. The following example is the ARN of an escalation plan named test\-plan:

      ```
      arn:aws:ssm-contacts:us-east-1:123456789101:contact/test-plan
      ```

1. Choose **Next**\.

1. \(Optional\) Provide tags\.

1. Choose **Next**\. 

1. Review the details of the stack and choose **Create stack**\.

You've now completed set up of the on\-call rotation script\. This script automatically changes to the next contact in your **ContactRotation** list on the schedule you provided with the cron expression\. 

## Update an on\-call rotation<a name="tutorials-oncall-reconfigure"></a>

In case that your on\-call changes you can update the parameters of the CloudFormation stack to represent the new on\-call rotation\. Use the following steps to update any of the on\-call rotation parameters\.

1. Open the CloudFormation console at [https://console\.aws\.amazon\.com/cloudformation](https://console.aws.amazon.com/cloudformation/)\.

1. Select the stack that you created when you set up the on\-call rotation\. 

1. Choose **Update**\.

1. Choose **Use current template**, and then **Next**\.

1. Update the **Parameters** fields to update your on\-call rotation\. 
   + Change the order of the **ContactRotation**\.
   + Add or remove contacts from the **ContactRotation**\.
   + Update the schedule using the **CronExpression**\.
   + Change the escalation plan used by updating the **EscalationPlanArn**\.

1. Choose **Next** on the **Parameters** page\.

1. Choose **Next** on the **Configure stack options** page\.

1. Select **I acknowledge that AWS CloudFormation might create IAM resources\.**, and then **Update stack**\.

## Delete on\-call rotation resources<a name="tutorials-oncall-cleanup"></a>

If you no longer wish to use the on\-call rotation, you can delete the CloudFormation stack to clean up all of the resources created using CloudFormation\. Deleting the stack won't delete the contacts or escalation plan related to the on\-call rotation\.

1. Open the CloudFormation console at [https://console\.aws\.amazon\.com/cloudformation](https://console.aws.amazon.com/cloudformation/)\.

1. Select the stack that you created while setting up the on\-call rotation\.

1. Choose **Delete**\.

1. Choose **Delete stack**\.

After choosing to delete the stack, the IAM role, Lambda function, and CloudWatch Events rule are deleted according to their deletion policy\.