# Step Functions MLOps

Before Opening the notebook, we need to setup the IAM permission, in order to give the access to Step Functions

1. Go to the [IAM console](https://console.aws.amazon.com/iam/).
2. Select **Roles** and then **Create role**.

![IAM Roles](/Images/StepFunctions/Guide/2.2a.png)

![IAM Roles 2](/Images/StepFunctions/Guide/2.2b.png)


3. Under **Choose the service that will use this role** select **Step Functions**.

![IAM Step Function](/Images/StepFunctions/Guide/2.3a.png)

![IAM Step Function 2](/Images/StepFunctions/Guide/2.3b.png)

![IAM Step Function 3](/Images/StepFunctions/Guide/2.3c.png)


4. Choose **Next** until you can enter a **Role name**.

![Role Details](/Images/StepFunctions/Guide/2.4.png)


5. Enter a name such as `AmazonSageMaker-StepFunctionsWorkflowExecutionRole` and then select **Create role**.

Next, attach a AWS Managed IAM policy to the role you created as per below steps.

6. Go to the [IAM console](https://console.aws.amazon.com/iam/).
7. Select **Roles**
8. Search for `AmazonSageMaker-StepFunctionsWorkflowExecutionRole` IAM Role

![Role](/Images/StepFunctions/Guide/2.8.png)


9. Under the **Permissions** tab, click **Add permissions**, and click **Attach policies**. Then search for `CloudWatchEventsFullAccess` IAM Policy managed by AWS (type under search box, and click enter).

![IAM Policy](/Images/StepFunctions/Guide/2.9a.png)

![IAM Policy 2](/Images/StepFunctions/Guide/2.9b.png)


10. Click on `Attach Policy`

![Attach Policy](/Images/StepFunctions/Guide/2.10.png)


11. Copy the IAM Roles ARN, we are going to use it again later.

![IAM Role ARN](/Images/StepFunctions/Guide/2.11.png)


Next, create and attach another new policy to the role you created. As a best practice, the following steps will attach a policy that only provides access to the specific resources and actions needed for this solution.

12. Click **Policies** in IAM Console, click **Create Policy**.

![IAM Policy](/Images/StepFunctions/Guide/2.12a.png)

![IAM Policy 2](/Images/StepFunctions/Guide/2.12b.png)


13. Enter the following in the **JSON** tab:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "events:PutTargets",
                "events:DescribeRule",
                "events:PutRule"
            ],
            "Resource": [
                "arn:aws:events:*:*:rule/StepFunctionsGetEventsForSageMakerTrainingJobsRule",
                "arn:aws:events:*:*:rule/StepFunctionsGetEventsForSageMakerTransformJobsRule",
                "arn:aws:events:*:*:rule/StepFunctionsGetEventsForSageMakerTuningJobsRule",
                "arn:aws:events:*:*:rule/StepFunctionsGetEventsForECSTaskRule",
                "arn:aws:events:*:*:rule/StepFunctionsGetEventsForBatchJobsRule"
            ]
        },
        {
            "Sid": "VisualEditor1",
            "Effect": "Allow",
            "Action": "iam:PassRole",
            "Resource": "NOTEBOOK_ROLE_ARN",
            "Condition": {
                "StringEquals": {
                    "iam:PassedToService": "sagemaker.amazonaws.com"
                }
            }
        },
        {
            "Sid": "VisualEditor2",
            "Effect": "Allow",
            "Action": [
                "batch:DescribeJobs",
                "batch:SubmitJob",
                "batch:TerminateJob",
                "dynamodb:DeleteItem",
                "dynamodb:GetItem",
                "dynamodb:PutItem",
                "dynamodb:UpdateItem",
                "ecs:DescribeTasks",
                "ecs:RunTask",
                "ecs:StopTask",
                "glue:BatchStopJobRun",
                "glue:GetJobRun",
                "glue:GetJobRuns",
                "glue:StartJobRun",
                "lambda:InvokeFunction",
                "sagemaker:CreateEndpoint",
                "sagemaker:CreateEndpointConfig",
                "sagemaker:CreateHyperParameterTuningJob",
                "sagemaker:CreateModel",
                "sagemaker:CreateProcessingJob",
                "sagemaker:CreateTrainingJob",
                "sagemaker:CreateTransformJob",
                "sagemaker:DeleteEndpoint",
                "sagemaker:DeleteEndpointConfig",
                "sagemaker:DescribeHyperParameterTuningJob",
                "sagemaker:DescribeProcessingJob",
                "sagemaker:DescribeTrainingJob",
                "sagemaker:DescribeTransformJob",
                "sagemaker:ListProcessingJobs",
                "sagemaker:ListTags",
                "sagemaker:StopHyperParameterTuningJob",
                "sagemaker:StopProcessingJob",
                "sagemaker:StopTrainingJob",
                "sagemaker:StopTransformJob",
                "sagemaker:UpdateEndpoint",
                "sagemaker:AddTags",
                "sns:Publish",
                "sqs:SendMessage"
            ],
            "Resource": "*"
        }
    ]
}
```

14. Replace **NOTEBOOK_ROLE_ARN** with the IAM Role ARN that previously has been copied in step 11. Click `Next`

![Notebook ARN](/Images/StepFunctions/Guide/2.14.png)


15. In **Review and Create**, give the policy name as `AmazonSageMaker-StepFunctionsWorkflowExecutionPolicy`.

![Review IAM Policy](/Images/StepFunctions/Guide/2.15.png)


16. Scroll down, and Choose **Create policy**.

We need to attach the policy back to the role that previously we created.

17. Select **Roles** and search for your `AmazonSageMaker-StepFunctionsWorkflowExecutionRole` role.

![IAM Role Search](/Images/StepFunctions/Guide/2.17.png)


18. Under the **Permissions** tab, click **Add permissions**, and click **Attach policies**.

![Permission](/Images/StepFunctions/Guide/2.18.png)


19. Search for your newly created `AmazonSageMaker-StepFunctionsWorkflowExecutionPolicy` policy (type, and press enter) and select the check box next to it.

![IAM SF Policy](/Images/StepFunctions/Guide/2.19a.png)

![IAM SF Policy 2](/Images/StepFunctions/Guide/2.19b.png)


20. Choose **Attach policy**. You will then be redirected to the details page for the role.
21. Copy the `AmazonSageMaker-StepFunctionsWorkflowExecutionRole` **Role ARN** at the top of the Summary.

![IAM Role ARN](/Images/StepFunctions/Guide/2.21.png)


Now, we need to allow this role to access the default sagemaker role that has been created

22. Select **Roles** and search for your `<yourstackname>SageMakerExecutionRole-<randomstring>` role.

![Roles](/Images/StepFunctions/Guide/2.22.png)


23. Copy the ARN of this role

![Copy ARN](/Images/StepFunctions/Guide/2.23.png)


We need to allow to assume this role to the first role we have created.

24. Select **Roles** and search for your `AmazonSageMaker-StepFunctionsWorkflowExecutionRole` role.

![Roles](/Images/StepFunctions/Guide/2.24.png)


25. In Permission, Click **Add permissions**, and click **Create inline policy**

![Inline Policy](/Images/StepFunctions/Guide/2.25.png)


26. Copy and paste this JSON

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "iam:PassRole",
      "Resource": "<your_ARN_Role_for_SageMakerExecutionRole>"
    }
  ]
}
```


27. Click `Review Policy`

![Review Policy](/Images/StepFunctions/Guide/2.27.png)


28. In Policy Name, write the name as `AssumeRoleSMforStepFunctionPolicy`
29. Click `Create Policy`

![Create Policy](/Images/StepFunctions/Guide/2.29.png)


Please continue to run the SageMaker Notebook.