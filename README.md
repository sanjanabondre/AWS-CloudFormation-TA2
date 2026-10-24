# AWS CloudFormation TA-2: Event-Driven Serverless File Processing

## Project Overview

This project implements an event-driven, serverless file-processing system on AWS using AWS CloudFormation, Amazon S3, AWS Lambda, and Amazon SNS.

When a file is uploaded to a designated S3 bucket, an S3 event automatically triggers a Lambda function. The function processes the uploaded file, generates a metadata JSON file, stores the metadata in S3, and sends an email notification through Amazon SNS.

The infrastructure is provisioned using an AWS CloudFormation template.

## Objectives

- Automate AWS resource creation using CloudFormation.
- Store uploaded files securely in Amazon S3.
- Trigger AWS Lambda automatically when a file is uploaded.
- Run the Lambda function inside a private subnet.
- Store file-processing metadata in S3.
- Send processing notifications through Amazon SNS.
- Verify execution using Amazon CloudWatch Logs.

## AWS Services Used

| Service | Purpose |
|---|---|
| AWS CloudFormation | Infrastructure provisioning |
| Amazon S3 | Uploaded file and metadata storage |
| AWS Lambda | Event-driven file processing |
| Amazon SNS | Email notifications |
| Amazon VPC | Network isolation |
| Public and Private Subnets | Network segmentation |
| Internet Gateway | Internet connectivity for the public subnet |
| NAT Gateway | Outbound internet access for the private subnet |
| AWS IAM | Permissions and access control |
| Amazon CloudWatch | Lambda execution logs |

## Architecture

The workflow follows these steps:

1. A user uploads a file to the `uploads/` folder in the S3 bucket.
2. Amazon S3 detects the object-created event.
3. The event triggers the Lambda function.
4. Lambda processes the uploaded file and extracts file metadata.
5. Lambda saves the metadata as a JSON file under the `processed/` prefix in S3.
6. Lambda publishes a processing summary to an SNS topic.
7. Amazon SNS sends an email notification to the subscribed email address.
8. CloudWatch Logs records the Lambda execution details.

Refer to `architecture-diagram.png` for the system architecture.

## Repository Contents

- `template.yaml` — AWS CloudFormation infrastructure template.
- `architecture-diagram.png` — Architecture diagram of the solution.
- `README.md` — Project documentation.
- `TA2-submission-report.pdf` — Assessment report containing implementation and testing screenshots.

## Deployment

### Prerequisites

- An AWS account.
- Access to the AWS Management Console.
- A verified email address for the SNS subscription.
- The provided `template.yaml` file.

### Steps

1. Sign in to the AWS Management Console.
2. Open AWS CloudFormation.
3. Create a new stack using the `template.yaml` file.
4. Provide the required stack details and parameters, if prompted.
5. Wait until the stack status becomes `CREATE_COMPLETE`.
6. Open the S3 bucket created by the stack.
7. Upload a test file inside the `uploads/` prefix.
8. Verify that a metadata JSON file is created under the `processed/` prefix.
9. Check the subscribed email inbox for the SNS notification.
10. Review the Lambda execution logs in Amazon CloudWatch.

## Security and Networking

The CloudFormation template configures the S3 bucket with server-side encryption using S3-managed keys (SSE-S3) and S3 Block Public Access settings.

The Lambda function is associated with a private subnet in a VPC. The network configuration includes public and private subnets, an Internet Gateway, and a NAT Gateway for outbound connectivity from the private subnet.

IAM roles and permissions allow the Lambda function to access the required S3 objects, publish SNS notifications, and use VPC networking resources.

## Testing and Results

The implementation was tested by uploading a sample file to the S3 bucket.

The following results were verified:

- CloudFormation stack reached `CREATE_COMPLETE`.
- The uploaded file appeared in the S3 bucket.
- The Lambda function was triggered by the S3 object-created event.
- A metadata JSON file was generated and stored in S3.
- An SNS email notification was received.
- Lambda execution logs were available in CloudWatch.

## Conclusion

The event-driven serverless file-processing system was successfully deployed using AWS CloudFormation. The integration of Amazon S3, AWS Lambda, Amazon SNS, VPC networking, IAM, and CloudWatch demonstrates how cloud infrastructure can automate file-processing workflows, store processing metadata, and notify users of processing results.

## Author

Cloud Computing — TA-2 Assessment
