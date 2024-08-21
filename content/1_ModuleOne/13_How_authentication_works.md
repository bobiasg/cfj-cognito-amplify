---
title: "How authentication works" # MODIFY THIS TITLE
chapter: true
weight: 3 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES IF APPLICABLE
---

# How authentication works with Amazon Cognito user pools and identity pools

When your customer signs in to an Amazon Cognito user pool, your application receives JSON web tokens (JWTs).

When your customer signs in to an identity pool, either with a user pool token or another provider, your application receives temporary AWS credentials.

With user pool sign-in, you can implement authentication and authorization entirely with an AWS SDK. If you don't want to build your own user interface (UI) components, you can invoke a prebuilt web UI (the hosted UI) or the sign-in page for your third-party identity provider (IdP).

## User pool API authentication and authorization with an AWS SDK
![User pool API Authentication](/images/05-authentication-api.png)

#### API authentication flow

1. A user accesses your application.
2. They select a "Sign in" link.
3. They enter their username and password.
4. The application invokes the method that makes an InitiateAuth API request. The request passes the user's credentials to a user pool.
5. The user pool validates the user's credentials and determines that the user has activated multi-factor authentication (MFA).
6. The user pool responds with a challenge that requests an MFA code.
7. The application generates a prompt that collects the MFA code from the user.
8. The application invokes the method that makes a RespondToAuthChallenge API request. The request passes the user's MFA code.
9. The user pool validates the user's MFA code.
10. The user pool responds with the user's JWTs.
11. The application decodes, validates, and stores or caches the user's JWTs.
12. The application displays the requested access-controlled component.
13. The user views their content.
14. Later, the user's access token has expired, and they request to view an access-controlled component.
15. The application determines that the user's session should persist. It invokes the InitiateAuth method again with the refresh token and retrieves new tokens.

## User pool authentication with the hosted UI
![User pool Hosted UI](/images/06-authentication-hosted-ui.png)

#### Hosted UI authentication flow
1. A user accesses your application.
2. They select a "Sign in" link.
3. The application directs the user to a hosted UI sign-in prompt.
4. They enter their username and password.
5. The user pool validates the user's credentials and determines that the user has activated multi-factor authentication (MFA).
6. The hosted UI prompts the user to enter an MFA code.
7. The user enters their MFA code.
8. The hosted UI redirects the user to the application.
9. The application collects the authorization code from the URL request parameter that the hosted UI appended to the callback URL.
10. The application requests tokens with the authorization code.
11. The token endpoint returns JWTs to the application.
12. The application decodes, validates, and stores or caches the user's JWTs.
13. The application displays the requested access-controlled component.
14. The user views their content.
15. Later, the user's access token has expired, and they request to view an access-controlled component.
16. The application determines that the user's session should persist. It requests new tokens from the token endpoint with the refresh token.

## User pool authentication with a third-party identity provider
![User pool Authentication federated](/images/07-authentication-federated.png)

#### Federated authentication flow
1. A user accesses your application.
2. They select a "Sign in" link.
3. The application directs the user to a sign-in prompt with their IdP.
4. They enter their username and password.
5. The IdP validates the user's credentials and determines that the user has activated multi-factor authentication (MFA).
6. The IdP prompts the user to enter an MFA code.
7. The user enters their MFA code.
8. The IdP redirects the user to the user pool with a SAML response or an authorization code.
9. If the user passed an authorization code, the user pool silently exchanges the code for IdP tokens. The user pool validates the IdP tokens and redirects the user to the application with a new authorization code.
10. The application collects the authorization code from the URL request parameter that the user pool appended to the callback URL.
11. The application requests tokens with the authorization code.
12. The token endpoint returns JWTs to the application.
13. The application decodes, validates, and stores or caches the user's JWTs.
14. The application displays the requested access-controlled component.
15. The user views their content.
16. Later, the user's access token has expired, and they request to view an access-controlled component.
17. The application determines that the user's session should persist. It requests new tokens from the token endpoint with the refresh token.

## Identity pool authentication
![User pool Authentication federated](/images/08-authentication-identity-pool.png)

#### Federated authentication flow
1. A user accesses your application.
2. They select a "Sign in" link.
3. The application directs the user to a sign-in prompt with their IdP.
4. They enter their username and password.
5. The IdP validates the user's credentials.
6. The IdP redirects the user to the application with a SAML response or an authorization code.
7. If the user passed an authorization code, the application exchanges the code for IdP tokens.
8. The application decodes, validates, and stores or caches the user's JWTs or assertion.
9. The application invokes the method that makes a GetId API request. It passes the user's token or assertion and requests an identity ID.
10. The identity pool validates the token or assertion against configured identity providers.
11. The identity pool returns an identity ID.
12. The application invokes the method that makes a GetCredentialsForIdentity API request. It passes the user's token or assertions and requests an IAM role.
13. The identity pool generates a new JWT. The new JWT contains claims that request an IAM role. The identity pool determines the role based on the user's request and the role-selection criteria in the identity pool configuration for the IdP.
14. AWS Security Token Service (AWS STS) responds to the AssumeRoleWithWebIdentity request from the identity pool. The response contains API credentials for a temporary session with an IAM role.
15. The application stores the session credentials.
16. The user takes an action in the app that requires access-protected resources in AWS.
17. The application applies the temporary credentials as signatures to API requests for the required AWS services.
18. IAM evaluates the policies attached to the role in the credentials. It compares them to the request.
19. The AWS service returns the requested data.
20. The application renders the data in the user's interface.
21. The user views the data.

