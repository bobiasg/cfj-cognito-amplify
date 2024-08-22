---
title: "Create Identity Pool" # MODIFY THIS TITLE
chapter: true
weight: 1 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES
---

# Create Identity Pool <!-- MODIFY THIS HEADING -->
From main Cognito page in the AWS console, Select Grant access to AWS services and then select Create identity pool
![Identity Pool](/images/400-api_access-00.png)

You will see a page saying Identify the IAM roles to use with your new pool. Click on the Allow button to accept the defaults roles.
![Identity Pool](/images/401-api_access-01.png)

You will now see the Getting Started with Amazon Cognito page. Click on the Platform dropdown button and select JavaScript. Make a note of the region and the IdentityPoolId because you will need these values for your JavaScript code.
![Identity Pool](/images/402-api_access-02.png)

Navigate to IAM, Roles and type Cognito in the search bar. You should see the roles that Cognito automatically created for you when you enabled the Identity Pool.
![Identity Pool](/images/403-api_access-03.png)

Select the IAM role with Auth in the name and edit this policy to allow listing files in S3. Don’t edit the Unauth policy as this is for unauthenticated users.
![Identity Pool](/images/404-api_access-04.png)

Create S3 Ducket For Testing

1. Create S3 bucket and create two sub-directories “Engineering”, “Legal”
2. Upload some test files in the bucket and sub-directories
3. Edit bucket CORS permissions to allow cross-origin calls

**Important**: This is for testing only, in production or real-world scenario you need to have proper CORS settings that allow only trusted origin.

    [
        {
            "AllowedHeaders": ["*"],
            "AllowedMethods": ["GET"],
            "AllowedOrigins": ["*"],
            "ExposeHeaders": []
        }
    ]
