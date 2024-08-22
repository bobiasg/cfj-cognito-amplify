---
title: "Access S3 in NextJS" # MODIFY THIS TITLE
chapter: true
weight: 2 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES
---

# Access S3 Bucket <!-- MODIFY THIS HEADING -->

## NextJS App
After sign-in and getting tokens, you need to get credentials from Identity Pool and use it to access S3 bucket.

This exercise is based on the APIs that has been created in 2. User Pool API Authentication.

Important: Make sure that you have an API that uses Cognito authorizer for authorization using id_token. Remove Oauth Scopes from authorizer settings. Deploy to prod stage once done.

Add new api in /api/aws/s3/route.ts :
```js
import { auth } from "@/libs/auth";
import { CognitoIdentityClient, GetCredentialsForIdentityCommand, GetIdCommand } from "@aws-sdk/client-cognito-identity";
import {
    ListObjectsV2Command,
    S3Client,
  } from "@aws-sdk/client-s3"

export async function GET(req: Request) {
    const session = await   auth()
    if (!session)
        return Response.json({error: 'Method Not Allowed'}, {status: 405})

    //get credentials aws
    
    try {
        const credentials = await GetCredentials(session?.idToken)

        if (!credentials)
            return Response.json({error: 'Id Token Not Allowed'}, {status: 405})


        const client = new S3Client({
            region: process.env.COGNITO_REGION as string,
            credentials: {
                accessKeyId: credentials?.AccessKeyId as string,
                secretAccessKey: credentials?.SecretKey as string,
                sessionToken: credentials?.SessionToken as string
            }
        })

        const { Contents } = await client.send(
            new ListObjectsV2Command ({
              Bucket: 'cfj-ws-01',
            //   Prefix: `${credentials.identityId}/`,
            })
          )

        return Response.json(Contents);

    }catch(err) {
        return Response.json({error: err}, {status: 500})
    }

}


async function GetCredentials(idToken: string) {

    const client = new CognitoIdentityClient({region: process.env.COGNITO_REGION});
    const providerName = `cognito-idp.${process.env.COGNITO_REGION}.amazonaws.com/${process.env.COGNITO_USER_POOL_ID}` as string

    const getIdCommand = new GetIdCommand({
        IdentityPoolId: process.env.COGNITO_IDENTITY_POOL_ID,
        Logins: {
            [providerName]: idToken,
        },
    });

    const idResponse = await client.send(getIdCommand);

    const credentialCommand = new GetCredentialsForIdentityCommand(
        { // GetCredentialsForIdentityInput
            IdentityId:  idResponse.IdentityId,
            Logins: { // LoginsMap
                [providerName]: idToken// idToken,
            },
            }
    );

    const response = await client.send(credentialCommand);

    return response.Credentials;
    
}
```

You can now test invoking the API, after you sign-in with "Google" and get tokens from Cognito, click the Make S3 Request button.
![Api Access](/images/400-api_access-14.png)
