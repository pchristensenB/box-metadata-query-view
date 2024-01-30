# box-metadata-query-viewo
Shows how a simple integration can be used to add a metadata query view in your Box.com app. This is part of the a blog post on Box Developer Blog

Steps to setup

1. Create AWS lambda function for authentication

2. Deploy HTML and javascript to website

3. Configure Box Application and Web App Integration





## Create AWS Lambda function
For this you need a AWS account. https://aws.amazon.com/free/. AWS Lambda free tier is quite generous and allows 400K seconds of compute per month. See https://aws.amazon.com/lambda/pricing/

1. When you get to the AWS console, locate the Lambda page (type lambda in the search at the top) and 'Create function' - call it box-token-generator or similar. 

<img src="/img/lambda1.png" width="90%" height="90%">

2. In the repo folder 'box-token-generator' is both a zip file with the code and the actual code. If you want to use as is, upload the zip to the Lambda, if not you can change the code and zip and upload

<img src="/img/lambda2.png" width="90%" height="90%">

3. Add an API gateway trigger as below and copy the API endpoint as you will need it later

<img src="/img/lambda3.png" width="90%" height="90%">

<img src="/img/lambda4.png" width="90%" height="90%">

## Deploy to website
In this example I use GitHub Pages which allows you (free of charge) to serve HTML directly from a GitHub repo. https://pages.github.com/. Just setup the GitHub pages repo and add the files in the root of this repository to the pages repo and it will be available. You can also use a simple local http server if you want, the URL here does not have to be public if only yourself is accessing it.

You will need to make a couple of changes to the file to make it work for your Box environment
1. Replace <URL TO TOKEN EXCHANGE FUNCTION> in the file with the URL for the token generator function create in previous step
2. Replace <eid> with the EID of your Box instance. 
3. Replace <template-key> with your template key. You can use this cURL command to create the template if you want to use same as in the sample in which case the template key would be "boxImpactFundApplication"


## Configure Box Application and Web App Integration
For this you will need a Box account. If you have not already got one, sign up here https://account.box.com/signup/personal?tc=annual

1. Once signed in goto https://developer.box.com and click the 'My Apps' in the upper right hand corner
2. This takes you to the developer console where you can create new app

<img src="/img/box1.png" width="90%" height="90%">

3. The app needs to be custom app, call it 'Box Impact Fund Application overview' or similar and fill out the details
4. Choose User Authentication (OAuth 2.0) as the authentication method
5. Make sure it has read and write scope

<img src="/img/box2.png" width="90%" height="90%">

6. Copy the client ID as you will need it later, save changes
7. Copy the client secret and add it to the environment variable in your AWS Lamdba

<img src="/img/env1.png" width="90%" height="90%">

<img src="/img/env2.png" width="90%" height="90%">

Configure the Web App Integration
1. Click the 'Integrations' tab and create new integration
2. Call it 'Box Impact Fund View' or similar (this will be the name that will appear in the menu)
3. Set it to work on files and requiring full permissions. You can also limit to only video extensions such as mp4, mov etc
4. Set the callback URL to be your website  (eg. https://mygithubusername.github.io) and add '?clientId=THE_CLIENT_ID_YOU_CREATED_IN_SETTING_UP_THE_BOX_APP
5. Add call back parameters as below

<img src="/img/box3.png" width="90%" height="90%">

You are now good to go. 