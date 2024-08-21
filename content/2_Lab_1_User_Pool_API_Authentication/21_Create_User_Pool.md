---
title: "Create User Pool" # MODIFY THIS TITLE
chapter: true
weight: 1 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES
---

Log into the AWS Console. In the AWS Console, open Amazon Cognito . Then click “Create a user pool” in the top right
![Create User Pool](/images/09-create-user-pool-01.png)

Select “User name, Email” under Cognito user pool sign-in options and hit Next
![Create User Pool](/images/10-create-user-pool-02.png)

Select “No MFA” and hit Next
![Create User Pool](/images/11-create-user-pool-03.png)

Leave the defaults for Step 3 and hit Next

Set Send email with Cognito and hit Next
![Create User Pool](/images/13-create-user-pool-05.png)

Enter your User pool Name to “CFJ-01”, Check the Use the Cognito Hosted UI option
![Create User Pool](/images/14-create-user-pool-06.png)

You can specify a custom domain name for example auth.mycompany.com or use a subdomain of the AWS Cognito service ( amazoncognito.com ).
Pick and enter random/unique subdomain.
![Create User Pool](/images/16-create-user-pool-08.png)

Enter your App client name to "cfj-client" and enter the Allowed callbak URLs to “https://localhost”, then hit Next
![Create User Pool](/images/17-create-user-pool-09.png)

Finally in the last step choose Create user pool
![Create User Pool](/images/18-create-user-pool-10.png)