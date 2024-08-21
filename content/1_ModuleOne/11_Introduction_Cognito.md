---
title: "Introduction to Cognito" # MODIFY THIS TITLE IF APPLICABLE
chapter: true
weight: 1 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES IF APPLICABLE
---

# Introduction to Cognito <!-- MODIFY THIS HEADING IF APPLICABLE -->

Amazon Cognito lets you easily add user sign-up and authentication to your mobile and web apps. Amazon Cognito also enables you to authenticate users through an external identity provider and provides temporary security credentials to access your app’s backend resources in AWS or any service behind Amazon API Gateway. Amazon Cognito works with external identity providers that support SAML or OpenID Connect, social identity providers (such as Facebook, Apple, Amazon) and you can also integrate your own identity provider.

![Cognito Overview](/images/00-overview.png)   

[Image Source](https://www.awsgeek.com/Amazon-Cognito/): Jerry Hargrove, awsgeek.com. [License](https://creativecommons.org/licenses/by-sa/4.0/legalcode)




The two components that follow make up Amazon Cognito. They operate independently or in tandem, based on your access needs for your users.

## User pools
![Cognito User pool](/images/03-user-pools-overview.png)

[Image Source](https://docs.aws.amazon.com/cognito/latest/developerguide/what-is-amazon-cognito.html): Amazon Docs

Create a user pool when you want to authenticate and authorize users to your app or API. User pools are a user directory with both self-service and administrator-driven user creation, management, and authentication. Your user pool can be an independent directory and OIDC identity provider (IdP), and an intermediate service provider (SP) to third-party providers of workforce and customer identities. You can provide single sign-on (SSO) in your app for your organization's workforce identities in SAML 2.0 and OIDC IdPs with user pools. You can also provide SSO in your app for your organization's customer identities in the public OAuth 2.0 identity stores Amazon, Google, Apple and Facebook. For more information about customer identity and access management (CIAM), see What is CIAM?.

User pools don’t require integration with an identity pool. From a user pool, you can issue authenticated JSON web tokens (JWTs) directly to an app, a web server, or an API.

## Identity pools
![Cognito User pool](/images/04-identity-pools-overview.png)
[Image Source](https://docs.aws.amazon.com/cognito/latest/developerguide/what-is-amazon-cognito.html#:~:text=Amazon%20Cognito%20is%20an%20identity,access%20tokens%20and%20AWS%20credentials.): Amazon Docs

Set up an Amazon Cognito identity pool when you want to authorize authenticated or anonymous users to access your AWS resources. An identity pool issues AWS credentials for your app to serve resources to users. You can authenticate users with a trusted identity provider, like a user pool or a SAML 2.0 service. It can also optionally issue credentials for guest users. Identity pools use both role-based and attribute-based access control to manage your users’ authorization to access your AWS resources.

Identity pools don’t require integration with a user pool. An identity pool can accept authenticated claims directly from both workforce and consumer identity providers.
