---
title: "User Pool Configuration" # MODIFY THIS TITLE
chapter: true
weight: 2 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES
---

<!-- MORE SUBMODULES CAN BE ADDED TO DIVIDE UP THE SETUP INTO SMALLER SECTIONS -->
<!-- COPY AND PASTE THIS SUBMODULE FILE, RENAME, AND CHANGE THE CONTENTS AS NECESSARY -->

# Resouce Server

We are now ready to further configure our user pool, we will begin by adding a resource server. As this lab will grant access to a Mock API at this stage we will create the resource and scope that will provide authenticated access to the get method of the API.

Select the App Integration tab and click on Create resource server
![User Pool Configuration](/images/19-user-pool-configuration-01.png)

Next we will enter as above the Resource server name, server identifier and Scope name then select Create resource server.
![User Pool Configuration](/images/20-user-pool-configuration-02.png)

# App Client Settings

We will now configure the call back URLs, OAuth flows and OAuth scopes.

Scroll down while still in the App integration tab to App client list and click on cfj-client”
![User Pool Configuration](/images/21-user-pool-configuration-03.png)

Then under Hosted UI select Edit
![User Pool Configuration](/images/22-user-pool-configuration-04.png)

There are a number of options to enable, ensure yours look the same as the image.

First we add the Allowed sign-out URL to https://localhost

Under OAuth 2.0 Grant Types select

- Authorization code grant and
- Implicit grant


{{% notice note %}}
The implicit grant flow exposes OAuth tokens in the url. Although we are using it here for testing purposes only, we strongly reocmmend you only use the authroization code flow with PKCE for public clients.
{{% /notice %}}

Under OpenID Connect Scopes select

- OpenID 
- Profile
- Email

Under Custom Scopes select

- cfj-01/read

Finally click Save changes

![User Pool Configuration](/images/23-user-pool-configuration-05.png)