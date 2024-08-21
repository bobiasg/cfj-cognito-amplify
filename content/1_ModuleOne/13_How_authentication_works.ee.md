---
title: "How authentication works" # MODIFY THIS TITLE
chapter: true
weight: 3 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES IF APPLICABLE
---

# How authentication works with Amazon Cognito user pools and identity pools

When your customer signs in to an Amazon Cognito user pool, your application receives JSON web tokens (JWTs).

When your customer signs in to an identity pool, either with a user pool token or another provider, your application receives temporary AWS credentials.

With user pool sign-in, you can implement authentication and authorization entirely with an AWS SDK. If you don't want to build your own user interface (UI) components, you can invoke a prebuilt web UI (the hosted UI) or the sign-in page for your third-party identity provider (IdP).