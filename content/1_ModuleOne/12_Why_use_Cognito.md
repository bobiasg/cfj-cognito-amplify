---
title: "Why use Cognito" # MODIFY THIS TITLE
chapter: true
weight: 2 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES IF APPLICABLE
---

# Why use Cognito <!-- MODIFY THIS HEADING TO REFLECT THE PROBLEM THE WORKSHOP IS ADDRESSING -->

## Integration with Other AWS Services
Currently, there are many user management platforms, such as Clerk, Auth0, etc, that offer authentication features. Each of these services has unique advantages compared to AWS Cognito. However, when an application requires access to AWS resources, it’s essential to implement proper authorization and use Developer credentials/SDK to interact with those resources. 

Cognito integrates seamlessly with other AWS services like API Gateway, Lambda, S3, and DynamoDB. This allows you to build comprehensive, robust, scalable, serverless applications where authentication and authorization are tightly coupled with your backend services.

![Cognito Diagram](images/01-CognitoDiagram.png)

[Source Image](https://aws.amazon.com/blogs/compute/secure-api-access-with-amazon-cognito-federated-identities-amazon-cognito-user-pools-and-amazon-api-gateway/): Amazon Blog

## Benefits as cloud service 
- Scalability and Reliability
- Security
- Customizable User Management
- Cost-Effectiveness
- Ease of Use and Quick Setup
- User Experience