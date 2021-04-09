# Building a simple Microservices App with Express Gateway





One of the interesting programming problems I have encountered on my programming journey is building a microservice architecture application. What made it interesting was my discovery of how it promotes collaboration among team members working on a product amongst other numerous benefits that it has. It made working in a team easier and flawless bringing about an increase in delivery speed. A large team with a large codebase would likely have conflicts along the way. This can be mitigated by breaking down the codebase into smaller services and separated by a clear interface, this is what the microservice architecture is about. It makes it easier to scale a product.


This article is intended to be a quick-start guide for developing a simple microservice application with Node.js and Express Gateway.

## Getting Started
===============

## Prerequisites
-------------

This tutorial requires the following;

-   Basic understanding of javascript
-   Ability to run commands on the terminal
-   Node v10 and above

For multiple versions of node, follow this link to setup node js using nvm <https://github.com/nvm-sh/nvm>

NPM comes with Node which is what will be used to install all the needed dependencies.

## Setting up Express Gateway
--------------------------

Open your terminal and navigate to the location in which you will like to create your app.

Install Express Gateway

$ npm install -g express-gateway

Create an Express Gateway application.

$ eg gateway create

Follow the prompts and choose the *Getting Started with Express Gateway*

➜ eg gateway create? What is the name of your Express Gateway? microservice-app? Where would you like to install your Express Gateway? microservices? What type of Express Gateway do you want to create? (Use arrow keys)❯ Getting Started with Express GatewayBasic (default pipeline with proxy)

This should create an Express Gateway application template which we will build upon.

Open the app

$ cd microservices

Before we get started, let's install other dependencies and setup, Babel, as we will be writing our code in ES6.

## Installing Dependencies And Setting Up Babel
============================================

Install express.js, a framework of Javascript

$ npm install express --save

Setup Babel

$ npm install @babel/core @babel/node @babel/preset-env --save-dev

At the root of the app, we'll create a .babelrc file.

$ touch .babelrc

In the .babelrc file add the following to set up the preset.

Start the app

$ npm start

The API gateway starts on the port defined in `config/gateway.config.yml` which by default is 8080. It has a hot reload config that monitors any changes and restarts the server automatically. Now that we have set up the Express Gateway and we have it running, it's time to create our endpoints.

Create Endpoints
================

We are going to be creating two simple endpoints, a GET user endpoint and GET music endpoint. The main point of focus in this tutorial is how to integrate an Express Gateway to a microservice app.

Create the User Endpoint
------------------------

Create a file for users in the root folder

$ touch user.js

To set up the user server, add the following lines of code to the user.js file

To enable the ES6 code to run, add a start script in the package.json file.

Credits 

[Bolaji](https://medium.com/u/5d97add8fae6?source=post_page-----a8fd49d81aeb--------------------------------)

Run the app

npm start user

Open another terminal and make a GET request with curl.

curl <http://localhost:3000/users>

Or open <http://localhost:3000/users> on the browser. You can also use Postman

The response should look like this

["Tony","Lisa","Michael","Ginger","Food"]

Create the Music Endpoint
-------------------------

Create a file for music in the root folder

$ touch music.js

To set up the music server, add the following lines of code to the music.js file

Run the app to ensure that the endpoint works as expected.

npm start music

Open another terminal and make a GET request with curl.

curl [http://localhost:8000/musics](http://localhost:3000/users)

The response should look like this

["Jesus","I love to praise your name","My Story","Days of Elijah"]

## Exposing the APIs
=================

The Express Gateway accepts requests from the clients and directs them to the microservice in charge of the particular request. For example, if a request is made through the gateway, [*http://localhost:8080/users*](http://localhost:3000/users)*, *the request is directed to the user microservice and a request such as [*http://localhost:8080/musics*](http://localhost:3000/users) is directed to the music microservice. In summary, each request is accepted through a server and directed to their various host. Let's set up our app to implement this.

Expose the endpoints to the gateway
-----------------------------------

Navigate into config folder and open gateway.config.yml file. In the *apiEndpoint*s section, we create an API endpoint named *"user"*, define the host and path. The path defined is an array of paths we would like to match. This is to cover for all URLs that match the path pattern. Repeat the same for the music endpoint. For more details about matching patterns for hostname and paths, check out the Express Gateway endpoint [documentation](https://www.express-gateway.io/docs/configuration/gateway.config.yml/apiEndpoints/)

With these, the Express gateway can accept external APIs(request from the client) of these formats '[*http://localhost:8080*](http://localhost:3000/users)*/users'* or *'*[*http://localhost:8080*](http://localhost:3000/users)*/users/*'*


## Declare the Service Endpoints to the gateway
--------------------------------------------

The service endpoints are the endpoints of our microservices, in this case, the user and the music microservices. The external request from the API endpoints is directed to the service endpoints. In the serviceEndpoints section still in the same gateway.config.yml file, we create services and define their URLs. For users, we call its service endpoints *"userService"* and add its endpoints *'*[*http://localhost:3000*](http://localhost:3000/users)*'*. Repeat the same for the music service endpoint.


Tying the Knot
--------------

Now let's wire the API endpoints and the service endpoints. This is done in the Pipelines section. It is where we connect the API endpoints and the service endpoints.

You will notice that a default has been defined already, so we just define ours. You can copy and paste the default pipeline and then change some of the details. We name our pipeline *"userPipeline"*, add the endpoint name which is *"user"*. Find the serviceEndpoint in the proxy policy of the userPipeline pipeline and add our service endpoint name which is *"userService"*. Repeat the procedure for the music pipeline.

Ensure that the user, music and the Express Gateway server are all running in the background. Run the gateway server using this command

npm start server

Now we can test our URLs. curl [http://localhost:8080/musics](http://localhost:3000/users) and curl [http://localhost:8080/musics](http://localhost:3000/users). Note that in the URLs, both the user and the music have the same localhost. Don't you just love the Express Gateway?

## Stress Reduction
================

To have all the servers up and running on *"npm start"* without having to start them individually, we import them in the server.js file.

Stop all running servers. Open the server.js file in the root folder and import the user and the music servers. The server.js file generated by Express is in ES5 but let's rewrite it in ES6.

Edit the start script in the package.json

"start": "babel-node server.js"

*"npm start"* should work properly now. Finally, we have our microservice app up and running.

For hot-reload of the user and music server whenever there are changes, we can install nodemon, this is because the Express Gateway hot-reload only affects changes made to its gateway.

To install nodemon, run the following in your terminal.

$ npm install nodemon --- save-dev

Edit the start script in the package.json

"start": "nodemon --- exec babel-node server.js"

Assuming there are no errors in the application, *"npm start" *should start the microservice application.

This is the end of a simple microservice application tutorial. If there are any errors while building yours, or you have suggestions, feel free to comment below. For more details about Express Gateway, its [documentation](https://www.express-gateway.io/getting-started/) is a great place to start. 