# Deploying NodeJS application using Microsoft Azure☁️


Initial setup
1. Sign up for [Azure](https://azure.microsoft.com/en-us/) account (credit card details required)
2. Install [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli-windows?view=azure-cli-latest&tabs=azure-cli)
3. Setup your MongoDB Atlas account (make sure to set the IP access setting in mongodb atlas such that everyone can access)
4. Create a new user, database and collection

Connecting to MongoDB Atlas cluster (via command line)
1. Click the connect button (after selecting your cluster), set Node.js version 2.2.12 or later
2. Copy the connection string and fill in the required parameter values (while entering parameters, make sure that they are [URL encoded](https://docs.atlas.mongodb.com/troubleshoot-connection/#special-characters-in-connection-string-password))

Setting up the environment variables
~~~
MONGO_URL=<MongoDB Atlas connection string>
NG_CMD=prod
~~~

Importing data into MongoDB Atlas
~~~
mongoimport --host <your primary cluster name ending in 27017> --db <database name> --collection <collection name> --type tsv --file <filename> --authenticationDatabase admin --ssl --username <your username> --password <your password> --headerline
~~~

Install all dependencies 
~~~
npm install
~~~

Deploying application on localhost (port:8080)
~~~
node app.js
~~~

Login into azure account
~~~
az login
~~~

Setting up new user credentials (Username should be unique in Azure) (Password should have atleast an uppercase, a lowercase alphabet and a number)
~~~
az webapp deployment user set --user-name <your application name> --password <password>
~~~~

Listing all possible locations
~~~
az appservice list-locations --sku F1
~~~

Creating new group directory
~~~
az group create --name <your group name> --location "East US"
~~~

Set group as default
~~~
az configure --defaults group=<your group name>
~~~

Setting up a free-tier billing plan
~~~
az appservice plan create --name <your plan name> --sku FREE
~~~

Setting your plan as default
~~~
az configure --defaults plan=<your plan name>
~~~

~~~
az webapp create --name deploy-demo-node-app --deployment-local-git --plan deployPlan
az webapp create --name deploy-demo-node-app-staging --deployment-local-git --plan deployPlan
~~~

~~~
git remote add az_staging https://deploy-emojifier@deployemojifierstaging.scm.azurewebsites.net/deployEmojifierStaging.git
git remote add az_prod https://deploy-emojifier@deployemojifier.scm.azurewebsites.net/deployEmojifier.git
~~~

Configuring the environment variable in Heroku (GUI based option also present)
~~~
az webapp config appsettings set --name deploy-demo-node-app --settings MONGO_URL="<MongoDB Atlas connection string>"
az webapp config appsettings set --name deploy-demo-node-app --settings NG_CMD="prod"
~~~

~~~
git add.
git commit -m "final version"
git push az_prod master
~~~

Browse to the website
~~~
az webapp browse --name <your application name>
~~~

Setting different targets for git (optional)
~~~
git config push.default upstream
git branch --set-upstream-to az_prod/master master
git branch --set-upstream-to az_staging/master staging
~~~

Congratulations, your website should be [live](https://deploy-demo-node-app.azurewebsites.net/) now🎉!!

Reference:
[Node.js: Deploying Applications](https://www.linkedin.com/learning/node-js-deploying-applications/welcome)




