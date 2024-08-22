---
title: "Enrich user token with additional claims" # MODIFY THIS TITLE
chapter: true
weight: 3 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES
---

# Enrich user token with additional claims <!-- MODIFY THIS HEADING -->

## Add Claim with Lambda

In this activity, we will enrich users token with additional claims using Pre Token Generation lambda trigger. This trigger will add “department” claim to the token as users sign-in. In real-world scenario, this could be based on a backend check to verify user’s department or it could be a custom claim in user’s profile that can be only modified by administrator.

Navigate to the Lambda console and click on the Create Function button. Select Author from scratch and give the lambda function a name. Select Node.js runtime.
![Api Access](/images/430-api_access-00.png)

In the lambda window, click on the index.js to select it and replace the code with the following code. Click on Deploy when done.
```js
    exports.handler = (event, context, callback) => {
    event.response = {
        claimsOverrideDetails: {
        claimsToAddOrOverride: {
            department: 'Legal',
        },
        },
    };

    callback(null, event);
    };
```
![Api Access](/images/431-api_access-01.png)

Navigate back to the Cognito console, select your "CFJ--01" user pool, in User pool properties tab, Add Lambda Trigger
Select Authentication, Pre Token Generation trigger, select lambda function has just created. Click on the Save Changes button.
![Api Access](/images/432-api_access-02.png)

Go back to postman to get new token, get idtoken and use https://jwt.io to decode it.
![Api Access](/images/433-api_access-03.png)

## Adding fine-grained access control

We will add fine-grained access control to allow users access to S3 resources based on their department.

Navigate to the Cognito console and select Manage Identity providers. Click on Edit Identity Pool. Expand the Authentication providers section and click on the dropdown button for Attribute access control Select the custom mappings item. Type department into both fields and then click on the save changes button.
![Api Access](/images/440-api_access-10.png)

Edit the trust policy of IAM role mapped to your identity pool to allow “sts:TagSession”. Replace identity-pool-id below with your Identity Pool ID
```js
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Federated": "cognito-identity.amazonaws.com"
      },
      "Action": ["sts:AssumeRoleWithWebIdentity", "sts:TagSession"],
      "Condition": {
        "StringEquals": {
          "cognito-identity.amazonaws.com:aud": "identity-pool-id"
        },
        "ForAnyValue:StringLike": {
          "cognito-identity.amazonaws.com:amr": "authenticated"
        }
      }
    }
  ]
}
```

Edit the IAM role mapped to your identity pool to allow listing objects under user’s department only as below
```js
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": ["s3:List*"],
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "s3:prefix": "${aws:PrincipalTag/department}"
        }
      }
    }
  ]
}

```  


Now test accessing S3 once again by sign-out, sign-in again and then navigate to “Access S3” tab. Try to list files under the bucket directly (with empty prefix) or under any sub-directory other than the one defined in user’s department claim. You should see 403 error returning from this call indicating that access is denied.

Try listing files under “Engineering” sub-directory by providing “Engineering” in prefix. This call should succeed since you are trying to list files under the department that exists in token.

What happened here? Why can I list files under “Engineering” prefix but not under “Legal”?

Because user’s token has a “department” claim with the value “Engineering” (we added this claim from Pre Token Generation trigger).

And we configured the identity pool to pass this claim as session tag with the name “department”, then we modified the IAM policy to allow access only if the requested prefix matches the “department” session tag.