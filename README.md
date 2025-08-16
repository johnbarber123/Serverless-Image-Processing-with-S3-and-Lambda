# Serverless-Image-Processing-with-S3-and-Lambda
serverless image processing application where users upload images to an S3 bucket, triggering an AWS Lambda function that processes and resizes the images before storing them in another S3 bucket.

Architecture: Serverless


# Table of Contents
- [Solution Overview](#solution-overview)
- [Architecture Diagram](#architecture-diagram)
- [Deployment](#Deployment)

---

***

## Solution Overview

This project is a serverless image processing application designed to automate the transformation of images. It provides a simple and cost-effective way to resize and process images as soon as they are uploaded. This solution is ideal for web applications and mobile apps that require dynamic image manipulation and a scalable back-end.

The core of this solution leverages an event-driven architecture. Images are uploaded, triggering a processing workflow to resize and manage them efficiently.

This approach offers several key advantages:

* **Scalability**: The system automatically scales to handle varying levels of image uploads.
* **Cost-Efficiency**: You only pay for the resources consumed during processing.
* **Simplified Management**: Focus on your application logic without managing underlying infrastructure.

For a detailed architectural breakdown and deployment guide, refer to the following sections in this document.

***

## Architecture Diagram

This solution utilizes the following AWS services to process images:

* **Client**: Users upload images through an interface.
* **Amazon API Gateway**: (Optional) Provides an API endpoint to receive image uploads from clients.
* **Amazon Simple Storage Service (Amazon S3)**: Stores the original uploaded images. Uploading an image to this bucket triggers the processing workflow.
* **AWS Step Functions**: Coordinates the image processing workflow, potentially involving multiple steps.
* **AWS Lambda**: Executes the image processing logic (e.g., resizing).
* **Amazon DynamoDB**: (Optional) Stores metadata about the processed images.
* **Amazon Simple Storage Service (Amazon S3)**: Stores the processed images in a separate bucket.

* ## Architecture Diagram
![Serverless Image Processing Architecture Diagram](docs/Blank%20diagram.png)

***
## Deployment

This section provides the steps to deploy the serverless application to your AWS account. It uses the **AWS Serverless Application Model (SAM) CLI** to build and deploy the project.

### Prerequisites

* **AWS Account**: You must have an active AWS account.
* **AWS CLI**: Ensure the AWS CLI is installed and configured with your credentials.
* **AWS SAM CLI**: Install the SAM CLI to build and deploy the application.
    ```bash
    pip install aws-sam-cli
    ```
* **Docker**: Docker is required to build the Lambda functions locally.

### Step-by-Step Deployment

1.  **Build the Project**: From the root directory of your project, run the `sam build` command. This command will compile your Lambda function code and prepare it for deployment.
    ```bash
    sam build
    ```

2.  **Deploy the Stack**: After the build is successful, run `sam deploy --guided`. This command will prompt you for a series of configuration options, such as the stack name and AWS Region.
    ```bash
    sam deploy --guided
    ```
    Follow the on-screen prompts to complete the deployment. SAM will automatically provision all necessary resources (S3 buckets, Lambda functions, etc.) as defined in your project's template file (`template.yaml`).

### GitHub Actions for Automated Deployment

To automate the deployment process, you can set up a GitHub Actions workflow. This workflow will automatically build and deploy your application every time you push code to the `main` branch.

1.  **Configure AWS Credentials**: In your GitHub repository settings, go to **Settings > Secrets > Actions** and add the following repository secrets:
    * `AWS_ACCESS_KEY_ID`
    * `AWS_SECRET_ACCESS_KEY`
    * `AWS_REGION` (e.g., `us-east-1`)

2.  **Create the Workflow File**: In your repository, create a new directory and file: `.github/workflows/deploy.yml`.

3.  **Add the Workflow Code**: Copy the following code into `deploy.yml`. This workflow will check out your code, configure AWS credentials, and then build and deploy your application using the SAM CLI.

    ```yaml
    name: Deploy to AWS

    on:
      push:
        branches:
          - main

    jobs:
      deploy:
        runs-on: ubuntu-latest
        steps:
          - name: Checkout code
            uses: actions/checkout@v2

          - name: Set up Python
            uses: actions/setup-python@v2
            with:
              python-version: '3.9'

          - name: Install dependencies
            run: |
              python -m pip install --upgrade pip
              pip install aws-sam-cli

          - name: Configure AWS Credentials
            uses: aws-actions/configure-aws-credentials@v1
            with:
              aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
              aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
              aws-region: ${{ secrets.AWS_REGION }}

          - name: SAM Build
            run: sam build --use-container

          - name: SAM Deploy
            run: sam deploy --no-confirm-changeset --no-fail-on-empty-changeset
    ```
