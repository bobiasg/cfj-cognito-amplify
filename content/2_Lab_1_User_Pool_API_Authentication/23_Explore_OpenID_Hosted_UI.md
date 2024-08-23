---
title: "Explore OpenID Config and Hosted UI" # MODIFY THIS TITLE
chapter: true
weight: 3 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES
---

<!-- MORE SUBMODULES CAN BE ADDED TO DIVIDE UP THE SETUP INTO SMALLER SECTIONS -->
<!-- COPY AND PASTE THIS SUBMODULE FILE, RENAME, AND CHANGE THE CONTENTS AS NECESSARY -->

## OpenID Config <!-- MODIFY THIS HEADING IF NECESSARY -->
Lets explore the published configuration and it’s format. This configuration follows a convention

https://cognito-idp.{region}.amazonaws.com/{userpoolid}/.well-known/openid-configuration

On the console, on the left menu click on General settings, here you will find the user pool id:
![Explore OpenID](/images/24-explore-openid-01.png)

Copy this to construct your OpenID config URL and open this in your browser. 
https://cognito-idp.ap-southeast-1.amazonaws.com/ap-southeast-1_3JmHkB1jl/.well-known/openid-configuration

![Explore OpenID](/images/25-explore-openid-02.png)

## Hosted UI

Return to the AWS Console, Under the tab App integration click on _cfj-client_. From here click on **View Hosted UI** this will open a window/tab. Link will be https://cfj-01.auth.ap-southeast-1.amazoncognito.com/oauth2/authorize?client_id=xxxx&response_type=code&scope=cfj%2Fread+email+openid+profile&redirect_uri=https%3A%2F%2Flocalhost

![Explore HostedUI](/images/25-explore-hostedui-01.png)

In the New tab click _Sign up_ we will be registering a new user. Fill out the registration form and ensure you use a real email address as a OTP ( One Time Password ) will be sent to this address to enable account confirmation. Click **Sign up**

![Explore HostedUI](/images/27-explore-hostedui-03.png)

Check your email inbox for the OTP and enter the value and click Confirm Account. The email you receive is minimal and this can all be customised if required.
![Explore HostedUI](/images/28-explore-hostedui-04.png)

Once your account is confirmed you will be redirected back to your application, as we configured that url for this exercise to localhost and we are not hosting an application your browser will display the following.
Take note of the URL it will be: _https://localhost/?code=5768f05d-9077-4acb-a4d6-4978923f1995_ though will be a different code.
![Explore HostedUI](/images/29-explore-hostedui-05.png)


Let’s now look at the response when we change to implicit flow sign in. Change the response type value to token from code 
https://cfj-01.auth.ap-southeast-1.amazoncognito.com/oauth2/authorize?client_id=xxxx&**response_type=token**&scope=cfj%2Fread+email+openid+profile&redirect_uri=https%3A%2F%2Flocalhost

And press enter to load the new URL, enter the user details you created previously and click Sign in
“https://localhost/#id_token={a-large-id-token}&access_token={a-large-access-token}&expires_in=3600&token_type=Bearer” because your browser can’t connect to the server “localhost”.


## Authorization in Postman
Once postman launches, click the + to open a new tab. Once we have a new tab, click on the _Authorization_, then change the type to _OAuth 2.0_
![Postman](/images/30-postman-06.png)

Fill out the settings as per below:
- the Callback URL will be: https://localhost 
- The Auth URL’s will be: _https://{your-cognito-domain}.auth.{your-region}.amazoncognito.com/oauth2/authorize_ 
- and _https://{your-cognito-domain}.auth.{your-region}.amazoncognito.com/oauth2/token_ 

This can be found by navigating in Cognito console to App integration tab. Then in there, look for Cognito domain. The Client ID can be found by scrolling down in the App integration page to App client and analytics section. The ID will be listed next to your App client name.

![Postman](/images/31-postman-07.png)

Then click Get New Access Token. This will pop up a mini browser requesting your credentials, use the credentials you created earlier to login and click Sign in.
![Postman](/images/33-postman-09.png)

The mini browser will close and if you have followed all the steps correctly you will see this message:
![Postman](/images/34-postman-10.png)

Click Proceed to view the tokens returned by Cognito. The token returned can be decoded at https://jwt.io for closer inspection this token is used to send to our service to authenticate and and provide course level access as defined by the scope. An example can be seen below.
![Postman](/images/35-postman-11.png)
