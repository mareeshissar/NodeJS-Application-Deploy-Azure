# Deploying NodeJS application using Microsoft Azure☁️


Initial setup
1. Install Heroku Command Line Interface (CLI) 
2. Setup your MongoDB Atlas account (make sure to set the IP access setting in mongodb atlas such that everyone can acess that)
3. Create a new user, database and collection

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

Initiating a Heroku session (from the applicaiton folder)
~~~
heroku login
~~~

Creating framework for production
~~~
git create --remote production
~~~

Configuring the environment variable in Heroku (GUI based option also present)
~~~
heroku config:set MONGO_URL=<MongoDB Atlas connection string> --remote production
heroku config:set NG_CMD=prod --remote production
~~~

Setting remote and pushing  
~~~
heroku git:remote -a <your unique heroku website name ex. guarded-plains-08242>
git push heroku master
~~~

Congratulations, your website should be [live](https://deploy-demo-node-app.azurewebsites.net/) now🎉!!

Reference:
[Node.js: Deploying Applications](https://www.linkedin.com/learning/node-js-deploying-applications/welcome)




