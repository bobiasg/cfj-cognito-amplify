---
title: "Create Identity Pool" # MODIFY THIS TITLE
chapter: true
weight: 1 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES
---

# Create Identity Pool <!-- MODIFY THIS HEADING -->
From main Cognito page in the AWS console, Select _Grant access to AWS services_ and then select **Create identity pool**
![Identity Pool](images/400-api_access-00.png)

Select "_Authenticated access_" and "_Amazon Cognito user pool_". Click "**Next**"
![Identity Pool](images/401-api_access-01.png)

Select "_Create a new IAM role_", enter "_**cfj-01**_" in role name
![Identity Pool](images/402-api_access-02.png)

Select User pool and App Client that was created in Lab 2
![Identity Pool](images/403-api_access-03.png)

Enter pool name, and Click Next and Create identity pool
![Identity Pool](images/404-api_access-04.png)

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
