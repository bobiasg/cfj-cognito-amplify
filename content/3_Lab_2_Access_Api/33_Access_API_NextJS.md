---
title: "Access API in NextJS" # MODIFY THIS TITLE
chapter: true
weight: 3 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES
---

# Access API in NextJS <!-- MODIFY THIS HEADING -->

## Postman
To confirm this is all working as expected return to Postman, the same tab from earlier will be open, click Send to call the api again. This time the response will be:
![Api Access](images/340-api_access-00.png)

This is expected, the API now require authorisation! 

You can manually add this by extracting the token as in earlier steps when we tested the Authorizer in the API Gateway console or you can click on the Authorization tab, for type select OAuth 2.0 This should re-populate the settings we entered. Available tokens will be empty, scroll down and click Get New Token, put in the username and password of the user created. Click Proceed, this time once the Manage Access Tokens dialog appears click Use Token.

Click on the Headers tab and notice the additional header key and value. Click Send and the API will again return data!
![Api Access](images/344-api_access-04.png)

## NextJS App
After sign-in and getting tokens, you can use these tokens to authorize access to protected APIs on API Gateway.

This exercise is based on the APIs that has been created in 2. User Pool API Authentication.

Add button in dashboard page with click handler:
```js
const makeRequestWithToken = async () => {
    try {
      const response = await fetch('https://06gfqqzl55.execute-api.ap-southeast-1.amazonaws.com/prod/pets',{
        method: 'GET',
        headers: {
          'Content-Type': 'application/json',
          'Authorization': 'Bearer ' + session?.accessToken,
        },
      })
      const data = await response.json()
      setApiResponse(JSON.stringify(data, null, 2))
    } catch (error) {
      setApiResponse("Failed to fetch data: " + error)
    }
  }
```

You can now test invoking the API, after you sign-in with "_**Google**_" and get tokens from Cognito, click the **Make API Request** button.
![Api Access](images/350-api_access-14.png)

Notice the access-token is added to the authorization header and the endpoint responds with the expected results:
![Api Access](images/351-api_access-15.png)

Currently, when you sign in by username/password, you will get error because in crendentials process, we does not process to request scope: _**cfj/read**_
![Api Access](images/352-api_access-16.png)