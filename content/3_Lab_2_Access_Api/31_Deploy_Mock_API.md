---
title: "Deploy Mock API" # MODIFY THIS TITLE
chapter: true
weight: 1 # MODIFY THIS VALUE TO REFLECT THE ORDERING OF THE MODULES
---

# Deploy Mock API <!-- MODIFY THIS HEADING -->
In the AWS Console, in the unified search type API Gateway and click on the API Gateway Service icon.
![Api Access](/images/300-api_access-01.png)

Select **REST API** by clicking Build
![Api Access](/images/305-api_access-06.png)

Then select Example API and click **Create API**.
![Api Access](/images/306-api_access-07.png)

The Mock API and endpoints are ready
![Api Access](/images/307-api_access-08.png)

configure **Enable CORS**
![Api Access](/images/312-api_access-12.png)

And deploy. We will now define a stage, click the deployment stage combobox and select [New Stage]. Provide a Stage name*, for this example use Prod, then click deploy.
![Api Access](/images/308-api_access-09.png)

You will see the API endpoint that has been deployed, you can click on this and see the result of a get request in the browser,
![Api Access](/images/310-api_access-11.png)

Change the URL and append /pets to the end and press enter you will see an example JSON response. An example URL will look like so: https://{random}.execute-api.{region}.amazonaws.com/prop/pets
![Api Access](/images/311-api_access-12.png)

We now have a mock API the provides static pet data, next we will add authentication restricted by OAuth scope.