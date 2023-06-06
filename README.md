# ML Ops AWS Native
AWS MLOps using Native AWS Services

Please kindly refer to the original guide here:
1. [SageMaker Pipelines](https://aws.amazon.com/getting-started/hands-on/machine-learning-tutorial-mlops-automate-ml-workflows/)
2. [Step Functions](https://github.com/aws/amazon-sagemaker-examples/blob/main/step-functions-data-science-sdk/step_functions_mlworkflow_processing/step_functions_mlworkflow_scikit_learn_data_processing_and_model_evaluation.ipynb)

## Prerequisites
1. Require to have an AWS Account, with SageMaker Studio Access.
2. This guidance is using Singapore Region (ap-southeast-1)
3. This guidance is using SageMaker Studio Notebook to run
4. If you haven't set up the SageMaker Studio, [Click this link to help you set it up](https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/quickcreate?templateURL=https://sagemaker-sample-files.s3.amazonaws.com/libraries/sagemaker-user-journey-tutorials/CFN-SM-IM-Lambda-catalog.yaml&stackName=CFN-SM-IM-Lambda-catalog)
5. If you have the SageMaker Studio ready, please use the existing one, and make sure to have an apppropriate role (SageMaker, Lambda, and S3 bucket access are required)
4. Please clone this github repository on your SageMaker Studio. [Click here for Guidance to clone](https://docs.aws.amazon.com/sagemaker/latest/dg/studio-tasks-git.html)

## Agenda
1. [MLOps using SageMaker Pipelines](/Guide/SMPipelines.md)
3. [MLOps using Step Functions](/Guide/StepFunction.md)