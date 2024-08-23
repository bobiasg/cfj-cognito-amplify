---
title: "Third Party Identity Provider" # MODIFY THIS TITLE
chapter: true
weight: 7 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES
---

<!-- MORE SUBMODULES CAN BE ADDED TO DIVIDE UP THE SETUP INTO SMALLER SECTIONS -->
<!-- COPY AND PASTE THIS SUBMODULE FILE, RENAME, AND CHANGE THE CONTENTS AS NECESSARY -->

# Third Party Identity Provider
Cognito user pools also allow you to set up social login for your users. This is very convenient in a world where most people have become accustomed to authentication via existing accounts such as Google, Facebook, Apple, etc.
![Social](/images/130-social-12.png)

Source Image: https://awskarthik82.medium.com/how-to-integrate-aws-cognito-with-google-social-login-fd379ff644cc

## Social login setup

To add google for authentication, we need to create OAuth client ID in Google API console. Open browser to access https://console.cloud.google.com/, select a project or create new project if not exists, and select "API & Service" in overview of project. Select "Credentials" tab and click "Create Crendentials" to create new OAuth Client
![Social](/images/101-social-02.png)

Select Applicatio Type is _Web Application_, Enter name and set redirect Url. Redirect URI should be set to https://<your-user-pool-domain>/oauth2/idpresponse. 
![Social](/images/102-social-03.png)

Click "**Create**" to create new crendentials. Store Client Id and Client Secret for setup in AWS Cognito.
![Social](/images/103-social-04.png)

Go to "_CFJ-01_" User Pool, tab "_Sign-in experience_", click "**Add identity provider**"
![Social](/images/110-social-10.png)

Select "_Google_" and provider Client Id and Client Secret that has created above. Click "**Add identity provider**" to create Federated identity provider sign-in
![Social](/images/111-social-11.png)

## Login with Hosted UI

We need to edit Hosted UI for using Google Authentication. In "_CFJ-01_" User pool, Select "_cfj-client_", Click "**Edit**" on _Hosted UI_ tab, add "**Google**" in "Identity provider"
![Social](/images/112-social-12.png)

Click "**Save**", it return to app client overview. On Hosted UI tab, Click "_View Hosted UI_" to see new Sign in with Google.
![Social](/images/113-social-13.png)
![Social](/images/114-social-14.png)

Now you can authenticate with google in intergrated AWS Cognito. 

## Seamless auth experience
Now, we can authenticate users with Google, but they need to go to the Hosted UI site to continue with Google authentication. We will improve this by bypassing the Cognito-hosted UI for a seamless authentication experience for the user.

Add new provider in Next Auth configuration in _libs/auth.ts_
```js
....
providers: [
    CognitoProvider({
      clientId: process.env.COGNITO_CLIENT_ID,
      clientSecret: process.env.COGNITO_CLIENT_SECRET,
      issuer: process.env.COGNITO_ISSUER,
      authorization: {
        params: { scope: "email profile openid cfj/read" }
      }

    }),
    ...(["Google"] as TProvider[]).map((providerName: TProvider) => {
      const provider: Provider = getProvider(providerName);

      (provider as OAuth2Config<Profile>).authorization.params.scope = "email profile openid cfj/read";

      return provider;
    })
  ],
....  
```

Update redirect Url in Hosted UI setting
![Social](/images/120-social-20.png)

Open browser at https://localhost:3000, click "Sign In" and select "**Sign in with CognitoGoogle**"
![Social](/images/121-social-21.png)

In user's view, browser redirect to social page (Google), after alowed by user, it redirect to nextjs, bypass Hosted UI site.

For username/password login, we use NextAuth credentials provider with the AWS Cognito library

Add Credentials provider to '_libs/auth.ts_'
```js
Credentials({
      id: 'credentials',
      name: 'credentials',
      credentials: {
        username: { label: 'username', type: 'text' },
          password: { label: 'password', type: 'password' }
      },
      async authorize(credentials) {

        if (!credentials) return null;

        const {username, password} = credentials;

        const secretHash = computeSecretHash(username as string, process.env.COGNITO_CLIENT_ID as string, process.env.COGNITO_CLIENT_SECRET as string);
        const cognito = new CognitoIdentityProviderClient({region: process.env.COGNITO_REGION});


        const command = new InitiateAuthCommand({
          AuthFlow: 'USER_PASSWORD_AUTH',
          ClientId: process.env.COGNITO_CLIENT_ID as string,
          AuthParameters: {
              USERNAME: username as string,
              PASSWORD: password as string,
              SECRET_HASH: secretHash,
          },
        });
        
        try {
          const response = await cognito.send(command);

         // You can access the ID, Access, and Refresh tokens here
          const idToken = response.AuthenticationResult?.IdToken;
          const accessToken = response.AuthenticationResult?.AccessToken;
          const refreshToken = response.AuthenticationResult?.RefreshToken;
          const expiresIn = response.AuthenticationResult?.ExpiresIn;

          let user: User = {}
          if (accessToken) {
            const userCommand = new GetUserCommand({
              AccessToken: accessToken,
            });

            const userResponse = await cognito.send(userCommand);
            user.id = userResponse.UserAttributes?.find(item => item.Name === 'sub')?.Value as string | undefined;
            user.email = userResponse.UserAttributes?.find(item => item.Name === 'email')?.Value as string | undefined;
            user.name = userResponse.Username;
          }

          return { ...user,  token: {idToken, accessToken, refreshToken, expiresIn}  } as User;
        } catch (error) {
            console.error(error);
            return null;
        }
      },
  })
```

Now, user authentication avoids redirection to external pages (Hosted UI site), thereby enhancing the user experience and interface (UX/UI).
![Social](/images/125-social-25.png)


#### Reference:
https://kelvinmwinuka.com/social-login-with-cognito-and-nextauth

https://awskarthik82.medium.com/how-to-integrate-aws-cognito-with-google-social-login-fd379ff644cc

Source code: https://github.com/bobiasg/cfj-cognito
