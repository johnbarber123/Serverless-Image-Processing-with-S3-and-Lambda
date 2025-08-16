# Serverless-Image-Processing-with-S3-and-Lambda
serverless image processing application where users upload images to an S3 bucket, triggering an AWS Lambda function that processes and resizes the images before storing them in another S3 bucket.

Architecture: Serverless


# Table of Contents
- [Solution Overview](#solution-overview)
- [Architecture Diagram](#architecture-diagram)

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

