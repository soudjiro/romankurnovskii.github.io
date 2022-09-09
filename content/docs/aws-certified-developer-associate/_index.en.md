---
title: AWS Certified Developer
description: My plan for preparing for and taking the AWS Certified Developer exam
toc: true
authors: [roman-kurnovskii]
tags: [aws]
categories: [aws]
series:
date: 2022-09-08
draft: false
featuredImage: "images/aws-cda-intro.en.png"
---


## Criteria

In order to pass the exam, you must score more than 70 (unspecified) points. Criterion will be a minimum threshold of **70/100** points, unless conditions change in preparation.

### Draft plan

1. Find out what the exam requirements are
2. Have a list of topics that will be on the exam
3. Practice each service for comprehension
4. Read extra theory that will not be covered during practice
5. Go through the test general questions
6. Repeat 3-5 repeat until the result of *failed* block greater than 80 points

**Entrypoint:**

- [AWS Certified Developer Exam Information](https://aws.amazon.com/certification/certified-developer-associate/)

## Prepare

The AWS website has [Exam Preparation Guide](https://d1.awsstatic.com/training-and-certification/docs-dev-associate/AWS-Certified-Developer-Associate_Exam-Guide.pdf)

To pass the exam, you need to know certain services from the 5 domains: `Deployment`, `Security`, `Development with AWS Services`, `Refactoring`, `Monitoring and Troubleshooting`

### List of services on the exam

> Version 2.1 DVA-C01

Analytics:

- Amazon OpenSearch Service (Amazon Elasticsearch Service)
- Amazon Kinesis

Application Integration:

- Amazon EventBridge (Amazon CloudWatch Events)
- Amazon Simple Notification Service (Amazon SNS)
- Amazon Simple Queue Service (Amazon SQS)
- AWS Step Functions

Compute:

- Amazon EC2
- AWS Elastic Beanstalk
- AWS Lambda

Containers:

- Amazon Elastic Container Registry (Amazon ECR)
- Amazon Elastic Container Service (Amazon ECS)
- Amazon Elastic Kubernetes Services (Amazon EKS)

Database:

- Amazon DynamoDB
- Amazon ElastiCache
- Amazon RDS

Developer Tools:

- AWS CodeArtifact
- AWS CodeBuild

AWS CodeCommit

- AWS CodeDeploy
- Amazon CodeGuru
- AWS CodePipeline
- AWS CodeStar
- AWS Fault Injection Simulator
- AWS X-Ray

Management and Governance:

- AWS CloudFormation
- Amazon CloudWatch

Networking and Content Delivery:

- Amazon API Gateway
- Amazon CloudFront
- Elastic Load Balancing

Security, Identity, and Compliance:

- Amazon Cognito
- AWS Identity and Access Management (IAM)
- AWS Key Management Service (AWS KMS)

Storage:

- Amazon S3

### Training plan

Opened a training plan for any tutorial to understand where to start learning. Chose **cloudacademy** service (but for example FreeCodeCamp has a free course with content). 

Another option is to use free [AWS Workshops](https://workshops.aws/)

[AWS Developer - Associate (DVA-C01) Certification Preparation](https://cloudacademy.com/learning-paths/aws-developer-associate-dva-c01-certification-preparation-4364/)

Don't see coverage of the following services, so I add them to the block when related topics are covered:

**Analytics**:

- Amazon Elasticsearch Service (Amazon ES) -> OpenSearch Service

**Developer Tools**:

- AWS CodeArtifact
- AWS Fault Injection Simulator

## My roadmap

The following is my roadmap for the study. There may be adjustments.

<!-- 1. 

2. [Amazon EC2](ec2)
3. [AWS Elastic Beanstalk](elasticbeanstalk)
   
4. [AWS Lambda](lambda)
5. [Amazon S3](s3)
   
6. [Amazon DynamoDB](dynamodb)
7. [Amazon ElastiCache](elasticache)
8. [Amazon RDS](rds)

10. [Amazon API Gateway](api-gateway)
11. [Amazon CloudFront](cloudfront)
12. [Elastic Load Balancing (ELB)](elasticloadbalancing)
    
13. [Amazon Kinesis](kinesis)
14. [Amazon OpenSearch Service (Amazon Elasticsearch Service)](opensearch-service)


16. [Amazon CloudWatch](cloudwatch)
17. [AWS CloudFormation](cloudformation)

18. [AWS CodeCommit](codecommit)
19. [AWS CodeDeploy](codedeploy)
21. [AWS CodeBuild](codebuild)
22. [AWS CodePipeline](codepipeline)


23. [Amazon CodeGuru](codeguru)
24. [AWS CodeStar](codestar)

25. [AWS CodeArtifact](codeartifact)


26. [AWS X-Ray](xray)
27. [AWS Fault Injection Simulator](fis)

29. [Amazon Elastic Container Registry (Amazon ECR)](ecr)
30. [Amazon Elastic Container Service (Amazon ECS)](ecs)

32. [AWS Fargate](fargate)


34. [Amazon Elastic Kubernetes Services (Amazon EKS)](eks)

36. [Amazon Cognito](cognito)

38. [AWS Key Management Service (AWS KMS)](kms)

40. [Amazon EventBridge (Amazon CloudWatch Events)](eventbridge)
    
41. [Amazon Simple Notification Service (Amazon SNS)](sns)
42. [Amazon Simple Queue Service (Amazon SQS)](sqs)


44. [AWS Step Functions](step-functions) 
-->


1. [AWS Identity and Access Management (IAM)](iam)
46. Amazon EC2
47. AWS Elastic Beanstalk
48. AWS Lambda
49. Amazon S3
50. Amazon DynamoDB
51. Amazon ElastiCache
52. Amazon RDS
53. Amazon API Gateway
54. Amazon CloudFront
55. Elastic Load Balancing (ELB)
56. Amazon Kinesis
57. Amazon OpenSearch Service (Amazon Elasticsearch Service)
58. Amazon CloudWatch
59. AWS CloudFormation
60. AWS CodeCommit
61. AWS CodeDeploy
62. AWS CodeBuild
63. AWS CodePipeline
64. Amazon CodeGuru
65. AWS CodeStar
66. AWS CodeArtifact
67. AWS X-Ray
68. AWS Fault Injection Simulator
69. Amazon Elastic Container Registry (Amazon ECR)
70. Amazon Elastic Container Service (Amazon ECS)
71. AWS Fargate
72. Amazon Elastic Kubernetes Services (Amazon EKS)
73. Amazon Cognito
74. AWS Key Management Service (AWS KMS)
75. Amazon EventBridge (Amazon CloudWatch Events)
76. Amazon Simple Notification Service (Amazon SNS)
77. Amazon Simple Queue Service (Amazon SQS)
78. AWS Step Functions


## Resources

- [AWS Certified Developer](https://aws.amazon.com/certification/certified-developer-associate/)
- [A brief overview of the official documentation](https://docs.aws.amazon.com/index.html)
- [Exam Preparation Guide](https://d1.awsstatic.com/training-and-certification/docs-dev-associate/AWS-Certified-Developer-Associate_Exam-Guide.pdf)
- [Sample Exam Questions](https://d1.awsstatic.com/training-and-certification/docs-dev-associate/AWS-Certified-Developer-Associate_Sample-Questions.pdf)
- [https://github.com/itsmostafa/certified-aws-developer-associate-notes](https://github.com/itsmostafa/certified-aws-developer-associate-notes)
- [https://github.com/arnaudj/mooc-aws-certified-developer-associate-2020-notes](https://github.com/arnaudj/mooc-aws-certified-developer-associate-2020-notes)
- [FreeCodeCamp Youtube - AWS Certified Developer - Associate 2020](https://www.youtube.com/watch?v=RrKRN9zRBWs)
- [How-To Labs from AWS](https://aws.amazon.com/getting-started/hands-on/?getting-started-all.sort-by=item.additionalFields.sortOrder&getting-started-all.sort-order=asc&awsf.getting-started-category=*all&awsf.getting-started-level=*all&awsf.getting-started-content-type=*all)
- https://amazon.qwiklabs.com/catalog
- https://workshops.aws
- https://wellarchitectedlabs.com/
- https://testseries.edugorilla.com/tests/1359/aws-certified-developer-associate

### Community posts

- https://dev.to/romankurnovskii/aws-certified-developer-associate-prepare-2np
- https://www.reddit.com/user/romankurnovskii/comments/x8rgig/what_is_the_topics_order_to_cover_to_get_prepared/?utm_source=share&utm_medium=web2x&context=3
- https://twitter.com/romankurnovskii/status/1567746601136832512