# cceteam-pipeline

# CloudFormation Pipeline

This repository contains a CloudFormation template that creates a pipeline for deploying CloudFormation templates.

## Table of Contents
- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Deployment](#deployment)
  - [Clone the repository](#clone-the-repository)
  - [Configure AWS credentials](#configure-aws-credentials)
  - [Deploy the CloudFormation template](#deploy-the-cloudformation-template)
- [Usage](#usage)
  - [Manually start the pipeline (AWS Console)](#manually-start-the-pipeline-aws-console)
  - [Automatically start the pipeline (GitHub commit)](#automatically-start-the-pipeline-github-commit)

## Overview

The CloudFormation template in this repository creates the following resources:

1. **CodeCommit Repository**: A CodeCommit repository named `cf-templates` to store CloudFormation templates.
2. **CodeBuild Project**: A CodeBuild project named `cf-templates-builder` that validates, packages, and deploys the CloudFormation templates.
3. **CodePipeline Pipeline**: A CodePipeline pipeline that monitors a GitHub repository (specified by the parameters) for new commits, runs the CodeBuild project, and deploys the CloudFormation template.
4. **IAM Roles**: Two IAM roles, one for CodeBuild and one for CodePipeline, with administrator permissions.
5. **S3 Bucket**: An S3 bucket for storing the pipeline artifacts.

## Prerequisites

- An AWS account with the necessary permissions to create the resources in the CloudFormation template.
- AWS CLI installed and configured on your local machine.
- A GitHub account and a personal access token with the necessary permissions.

## Deployment

### Clone the repository

1. Open a terminal or command prompt.
2. Navigate to the directory where you want to clone the repository.
3. Run the following command to clone the repository:

   ```
   git clone https://github.com/your-username/your-repo.git
   ```

   Replace `your-username` and `your-repo` with the appropriate values.

### Configure AWS credentials

Before deploying the CloudFormation template, you need to configure your AWS credentials. You can do this using the AWS CLI:

1. Run the following command to configure your AWS credentials:

   ```
   aws configure
   ```

2. Follow the prompts to enter your AWS Access Key ID, AWS Secret Access Key, default region, and default output format.

### Deploy the CloudFormation template

1. Navigate to the cloned repository directory in your terminal or command prompt.
2. Run the following command to deploy the CloudFormation template:

   ```
   aws cloudformation create-stack --stack-name CFNPipeline --template-body file://path/to/cloudformation-template.yaml --parameters ParameterKey=GitHubOwner,ParameterValue=your-github-owner ParameterKey=GitHubRepo,ParameterValue=your-github-repo ParameterKey=GitHubToken,ParameterValue=your-github-token ParameterKey=CloudFormationStackName,ParameterValue=my-cf-stack
   ```

   Replace the following:
   - `path/to/cloudformation-template.yaml` with the actual path to the CloudFormation template file in your local repository.
   - `your-github-owner` with the owner or organization of your GitHub repository.
   - `your-github-repo` with the name of your GitHub repository.
   - `your-github-token` with your GitHub personal access token.
   - `my-cf-stack` with the name of the CloudFormation stack you want to deploy.

3. The CloudFormation stack creation may take a few minutes to complete. You can monitor the progress in the AWS CloudFormation console.

## Usage

After the CloudFormation stack has been successfully created, you can start the pipeline in the following ways:

### Manually start the pipeline (AWS Console)

1. Open the AWS Management Console and navigate to the AWS CodePipeline service.
2. Locate the pipeline created by the CloudFormation template and click on it.
3. In the pipeline page, click the "Release change" button to manually start the pipeline.

### Automatically start the pipeline (GitHub commit)

1. Make a commit to the "CRUD-API.yaml" file in your GitHub repository.
2. The pipeline will be automatically triggered after the new commit is detected.
3. You can monitor the progress of the pipeline in the AWS CodePipeline console.

Note that the pipeline requires manual approval before the CloudFormation stack is deployed. You will receive a notification via the SNS topic specified in the pipeline, and you can approve the deployment through the AWS CodePipeline console.

Ensure that your AWS user has the necessary permissions to access CodePipeline, CodeBuild, CloudFormation, and other AWS services used by the pipeline.
