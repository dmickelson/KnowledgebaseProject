# AI-Driven Knowledge Base Platform

![Alt text here](knowledgebase.svg)

## Overview

This project outlines the development of a cloud-based, serverless, microservice, scalable architected AI-driven knowledge base platform. The platform enables team members to ask questions that span multiple documents and leverage the context of prompt/chat history for extended conversations. The architectural design focuses on being interactive, serverless, modular, and scalable.

## Goal

The primary goal was to create a platform that is:

- Interactive: Remembers previous sessions to facilitate extended conversations.
- Serverless: No infrastructure engineering is required.
- Microserviced: Components are modular and replaceable.
- Scalable: Capable of scaling with the growing demand of the team.

Initially, the plan was to use a separate third-party embedding vector and store them in DynamoDB. However, Amazon Bedrock provided a fully managed solution that better aligned with our needs, leading us to pivot in that direction.

## Architecture Overview

### File Uploading Web Interface

- S3 Buckets: Used to host the static webpage, which serves as a simple UI for uploading documents into S3.
- REST API Endpoints: Protected behind Amazon API Gateway to facilitate secure document uploads.
- Amazon CloudFront: Employed to cache the website, ensuring faster load times and content delivery.
- AWS Certification Manager: Used to create an SSL certificate for secure communications.
- Route 53: For domain registration, pointing to CloudFront endpoints, and integrating with SSL.
- Security: Handled through a combination of Amazon Cognito, AWS IAM, and Amazon KMS.

### Gather Information and Import Content

- S3 Buckets: Serve as the repository for unstructured documents used within the Knowledge Base.
- Upload Process:
  - Users can upload PDF, DOC, or TXT files using the web interface to the S3 upload bucket.
  - Upon upload, an AWS Lambda function is triggered to validate the uploaded documents.
  - If validation is successful, the file is moved to the designated S3 bucket; otherwise, the file is deleted.

### Index Source and Create Knowledge Base

- Amazon Bedrock RAG Knowledge Base:
  - Securely syncs Foundation Models (FMs) to S3 data sources.
  - Ingests, retrieves, and augments queries and retrieval.
  - Offers built-in session content management for multi-turn conversations.
  - Provides automatic citations with retrievals.
  - Embedding Model: Amazon Titan Embedding G1 - Text.
  - Vector Store: Amazon OpenSearch Serverless and Pinecone.
- AWS Kendra:
  - Considered for future implementation.
  - Offers better out-of-the-box integration with various other unstructured data sources.
  - Creates an enterprise search index off of S3 content sources and metadata.

### Chat Interface

The chat interface serves as a Q&A GenAI knowledge base-powered conversation platform, built on top of imported content.

- AWS Lex: Acts as the chat interface for the Q&A knowledge base.
- Model Utilization: Leverages Anthropic Claude V2 models.
- Content Integration: Uses AWS Bedrock as the RAG content foundation.

## Conclusion

This project showcases the power of combining various AWS services to create a highly scalable, interactive, and serverless knowledge base platform. By leveraging the capabilities of Amazon Bedrock, AWS Lambda, and other cloud services, we have established a robust framework for future enhancements and integrations.
