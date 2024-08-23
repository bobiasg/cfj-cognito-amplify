---
title: "Create Cognito Authorizer" # MODIFY THIS TITLE
chapter: true
weight: 2 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES
---

# Cognito Authorizer <!-- MODIFY THIS HEADING -->

## Create Cognito Authorizer
In API Gateway on the left menu click Authorizers , then click + Create New Authorizer input the name as _cfj-ws-01_, select Cognito and select your Cognito user pool in the drop down ( this should pre-populate when you click in the input field ).

Additionally add Authorization as the Token Source, ( note the spelling is Authorization ). Finally, click **Create authorizer**
![Api Access](/images/320-api_access-00.png)

Your new Authorizer is ready, but before we attach this to the API let’s explore the test functionality. Click Test authorizer. This will bring up a dialog, this is expecting what we would send to the API in an Authorization header. We will send a Bearer token that was generated earlier in Postman. Leave this dialog open.
![Api Access](/images/321-api_access-01.png)

Return to Postman, click Get New Access Token to retrieve a new token.
![Api Access](/images/324-api_access-04.png)

Return to the AWS Console and in the input box type Bearer with one space and then paste the token and click Test authorizer. Your Authorizer works end to end, now lets test this against the actual API.
![Api Access](/images/325-api_access-05.png)

## Attach Authorizer to the API
We now have all the parts to add authentication to the API. In this section we will restrict access to the _/pets_ end point.

To confirm this is all working as expected return to Postman and open a new tab, take the API URL ( you should still have this in a browser tab, if not navigate back to API Gateway, click Stages in the left menu and copy the URL from the highlighted section at the top next to Invoke URL ) and paste this in, click send to see the result
![Api Access](/images/326-api_access-06.png)

Return to the AWS Console, Navigate to API Gateway and select your API. In the left hand menu click Resources, in the Resources column, click/expand **/pets** then click on GET we will add authentication to only this resource which is the http GET verb on the sample API resource. Note: You may have to refresh the webpage for this new
![Api Access](/images/327-api_access-07.png)

Click on the Authorization combo box, select **_cfj-ws-01_** (You may have refresh the page if you do not see it), add a new option OAuth Scopes: **_cfj/read_**, this is the OAuth scope we defined in Cognito earlier. Click the small tick to save.
![Api Access](/images/331-api_access-11.png)

You screen should look like below:
![Api Access](/images/332-api_access-12.png)

The settings are now ready but we have to deploy our API changes, click on Actions then Deploy API, for the deployment stage select prod and click deploy
![Api Access](/images/334-api_access-14.png)