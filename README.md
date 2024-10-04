
# Improving constituent experience using AWS-powered generative AI chatbots

Detailed documentation of this solution is published as blog and available at the following link: 
https://aws.amazon.com/blogs/publicsector/improving-constituent-experience-generative-artificial-intelligence-chatbot/

## Prerequisites

- AWS account
- Understanding of Amazon Bedrock, AWS Lambda, Amazon Connect, Amazon Lex, and IAM
- Permissions to create/modify Lambda functions and launch CloudFormation templates

## Solution walkthrough

### 1. Deploy CloudFormation stack for OpenSearch Serverless and S3 bucket

1. Download `opensearchserverlesscollection.yaml` from GitHub
2. Create CloudFormation stack using the file
3. Enter stack details (AOSSCollectionName, IAMUserArn)
	For AOSSCollectionName, the default is rag-kb.
	For IAMUserArn, use the Amazon Resource Name (ARN) of the user or role running this stack
4. Acknowledge IAM resource creation and submit
5. Note S3BucketName from outputs

### 2. Create vector index in OpenSearch Serverless

1. Navigate to OpenSearch Service console, choose Collections
2. Select rag-kb collection, create vector index
3. Name it rag-readthedocs-io, add vector field (name: vector, dimensions: 1536)
4. Choose FAISS engine and Euclidean distance metric

### 3. Deploy CloudFormation stack for Amazon Bedrock resources

1. Download `amazonbedrockknowledgebase.yaml` from GitHub
2. Create CloudFormation stack using the file
3. Use default values for AOSSIndexName, KnowledgeBaseDescription, and KnowledgeBaseName
4. Acknowledge IAM resource creation and submit

### 4. Upload documents to S3 bucket

Upload documents to the S3 bucket created in step 1

### 5. Create Amazon Lex bot

1. Create bot named medicaidchatbot
2. Configure language settings
3. Import Lex configuration
4. Add AMAZON.QnAIntent – GenAI feature
5. Configure QnA, disable closing response, and build

### 6. Deploy CloudFormation stack for Amazon Connect resources

1. Download `amazonconnectresources.yaml` from GitHub
2. Create CloudFormation stack using the file
3. Enter Environment, LexBotAliasID, and LexBotID
4. Acknowledge IAM resource creation and submit

### 7. Deploy static website

1. Follow amazon-cloudfront-secure-static-site repo instructions
2. Create chat UI snippet using Amazon Connect documentation
3. Update index.html with new snippet
4. Replace S3 bucket content with updated html

### 8. Testing

1. Access static website URL and test chat assistant
2. Initiate phone call through Amazon Connect
