# Deploying NodeJS application using Microsoft Azure☁️


Initial setup
1. Sign up for [Azure](https://azure.microsoft.com/en-us/) account (credit card details required)
2. Download and install [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli-windows?view=azure-cli-latest&tabs=azure-cli)
3. Setup your MongoDB Atlas account (make sure to set the IP access setting such that anyone can access)
4. Create a new user, database and collection

Connecting to MongoDB Atlas cluster (via command line)
1. Click the connect button (after selecting your cluster) and set Node.js version 2.2.12 or later
2. Copy the connection string and fill in the required parameter values, making sure they are [URL encoded](https://docs.atlas.mongodb.com/troubleshoot-connection/#special-characters-in-connection-string-password)

Setting up environment variables
~~~
MONGO_URL=<MongoDB Atlas connection string>
NG_CMD=prod
~~~

Importing data into MongoDB Atlas
~~~
mongoimport --host <your primary cluster name ending in 27017> --db <database name> --collection <collection name> --type tsv --file <filename> --authenticationDatabase admin --ssl --username <your username> --password <your password> --headerline
~~~

Installing dependencies 
~~~
npm install
~~~

Deploying application on localhost:8080
~~~
node app.js
~~~

Login azure account
~~~
az login
~~~

Setting up new user credentials 
- Username should be unique in Azure 
- Password should have an uppercase alphabet, a lowercase alphabet and a number)
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

Setting group as default
~~~
az configure --defaults group=<your group name>
~~~

Setting up free-tier billing plan
~~~
az appservice plan create --name <your plan name> --sku FREE
~~~

Setting plan as default
~~~
az configure --defaults plan=<your plan name>
~~~

Creating web application
~~~
az webapp create --name deploy-demo-node-app --deployment-local-git --plan <your plan name>
~~~

Adding remotes
~~~
git remote add az_prod <URL for .git file>
~~~

Configuring environment variable in Azure (GUI based option also available)
~~~
az webapp config appsettings set --name <your application name> --settings MONGO_URL="<MongoDB Atlas connection string>"
az webapp config appsettings set --name <your application name> --settings NG_CMD="prod"
~~~

Pushing to Azure
~~~
git push az_prod master
~~~

Opening the website
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




