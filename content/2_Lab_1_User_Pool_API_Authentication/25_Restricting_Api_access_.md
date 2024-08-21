---
title: "Restricting Api Access" # MODIFY THIS TITLE
chapter: true
weight: 5 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES
---

# Deploy a mock API
We will now deploy a Mock API and configure access to the read resource of that API.

In the AWS Console, in the unified search type API Gateway and click on the API Gateway Service icon.
![Postman](/images/40-mockapi-01.png)

If this is your first visit to API Gateway you will see the welcome page, click Get Started
![Postman](/images/41-mockapi-02.png)

You will be prompted with a dialog to deploy a prepopulated mock API, click OK
![Postman](/images/42-mockapi-03.png)

You can peruse the Example API response, when you are satisfied, leave all settings as defaults and click Import
![Postman](/images/43-mockapi-04.png)


If you have used API Gateway before, click Create API
![Postman](/images/44-mockapi-05.png)

......


# Create Cognito Authorizer
In API Gateway on the left menu click Authorizers , then click + Create New Authorizer input the name as Builder-Class, select Cognito and select your Cognito user pool in the drop down ( this should pre-populate when you click in the input field ).

Additionally add Authorization as the Token Source, ( note the spelling is Authorization ). Finally, click Create
![Cognito Authorizer](/images/50-cognito_authorizer-01.png)

Your new Authorizer is ready, but before we attach this to the API let’s explore the test functionality. Click Test
![Cognito Authorizer](/images/51-cognito_authorizer-02.png)

This will bring up a dialog, this is expecting what we would send to the API in an Authorization header. We will send a Bearer token that was generated earlier in Postman. Leave this dialog open.
![Cognito Authorizer](/images/52-cognito_authorizer-03.png)

Return to Postman and return to the original Tab, Click Auth then click on the Available Tokens combo box, click Manage Tokens.
![Cognito Authorizer](/images/53-cognito_authorizer-04.png)

From here scroll down until you see Token Type Bearer select and copy this token into your clipboard. If you double click the token it will select it all, do this carefully as the token is long. If you have left this idle for too long you may need to click Get New Access Token to retrieve a new token.
![Cognito Authorizer](/images/54-cognito_authorizer-05.png)

Return to the AWS Console and in the input box type Bearer with one space and then paste the token and click test. Your Authorizer works end to end, now lets test this against the actual API.
![Cognito Authorizer](/images/55-cognito_authorizer-06.png)

# Attach Authorzier to the API
We now have all the parts to add authentication to the API. In this section we will restrict access to the /pets end point.

To confirm this is all working as expected return to Postman and open a new tab, take the API URL ( you should still have this in a browser tab, if not navigate back to API Gateway, click Stages in the left menu and copy the URL from the highlighted section at the top next to Invoke URL ) and paste this in, click send to see the result.
![Cognito Authorizer](/images/56-cognito_authorizer-07.png)

Return to the AWS Console, Navigate to API Gateway and select your API. In the left hand menu click Resources, in the Resources column, click/expand /pets then click on GET we will add authentication to only this resource which is the http GET verb on the sample API resource. Note: You may have to refresh the webpage for this new resource to appear.
![Cognito Authorizer](/images/57-cognito_authorizer-08.png)

Click on the Authorization combo box
![Cognito Authorizer](/images/58-cognito_authorizer-09.png)

select Builder-class (You may have refresh the page if you do not see it), then click the little tick to save.
![Cognito Authorizer](/images/59-cognito_authorizer-10.png)

This will deploy a new option OAuth Scopes, click next to the default value which is NONE and type in petstore/read this is the OAuth scope we defined in Cognito earlier. Click the small tick to save.
![Cognito Authorizer](/images/60-cognito_authorizer-11.png)

You screen should look like below:
![Cognito Authorizer](/images/61-cognito_authorizer-12.png)

The settings are now ready but we have to deploy our API changes, click on Actions then Deploy API
![Cognito Authorizer](/images/62-cognito_authorizer-13.png)

for the deployment stage select Prod and click deploy
![Cognito Authorizer](/images/63-cognito_authorizer-14.png)

## Test access api in postman
To confirm this is all working as expected return to Postman, the same tab from earlier will be open, click Send to call the api again. This time the response will be:

![Cognito Authorizer](/images/64-cognito_authorizer-15.png)

This is expected, the API now require authorisation!

The API is expecting a new http header, the header required is:
![Cognito Authorizer](/images/65-cognito_authorizer-16.png)
`
Key:  Authorization
Value:  Bearer token

`

You can manually add this by extracting the token as in earlier steps when we tested the Authorizer in the API Gateway console or you can click on the Authorization tab, for type select OAuth 2.0 This should re-populate the settings we entered. Available tokens will be empty, scroll down and click Get New Token, put in the username and password of the user created. Click Proceed, this time once the Manage Access Tokens dialog appears click Use Token.
![Cognito Authorizer](/images/66-cognito_authorizer-17.png)

Click on the Headers tab and notice the additional header key and value.
![Cognito Authorizer](/images/67-cognito_authorizer-18.png)

Click Send and the API will again return data!
![Cognito Authorizer](/images/68-cognito_authorizer-19.png)

<!-- MORE SUBMODULES CAN BE ADDED TO DIVIDE UP THE SETUP INTO SMALLER SECTIONS -->
<!-- COPY AND PASTE THIS SUBMODULE FILE, RENAME, AND CHANGE THE CONTENTS AS NECESSARY -->

# Create an IAM Role for your workspace <!-- MODIFY THIS SUBHEADING -->

## Submodule Three Heading <!-- MODIFY THIS SUBHEADING -->

This paragraph block can be used to explain how to create a workspace if necessary. Example content guidance can be found at the bottom of the page.

{{% notice info %}}
<p style='text-align: left;'>
**REMOVE:** With the exception of _index.md, the module folders and filenames should be changed to better reflect their content, i.e. 1_Planning as the folder and 11_HowToBegin as the first submodule. Changing the "weight" value of the header is ultimately what reflects the order the modules are presented.
</p>
{{% /notice %}}

### Next Section Heading <!-- MODIFY THIS HEADING -->
This paragraph block can optionally be utilized to lead into the next section of the workshop.

#### Example Content Guidance

### Create an IAM Role for your workspace <!-- MODIFY THIS SUBHEADING -->

Info: Starting from here, when you see command to be entered such as below, you will enter these commands into Cloud9 IDE. You can use the Copy to clipboard feature (right hand upper corner) to simply copy and paste into Cloud9. In order to paste, you can use Ctrl + V for Windows or Command + V for Mac.

    Follow this deep link to create an IAM role with Administrator access.
    Confirm that AWS service and EC2 are selected, then click Next to view permissions.
    Confirm that AdministratorAccess is checked, then click Next to review.
    Enter CircleCI-Workshop-Admin for the Name, and select Create Role createrole
