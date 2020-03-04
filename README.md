# Architecture

## Objectives

- Make bets for coming games
- Make bets live
- Calculate quotes live
- KYC Clients
- Oracle

## Issues

### Heavy fluctuation in traffic
Betting online fluctuates heavily depending on the hour of the day. 
For example, most soccer games happen during the same time window. Therefore, it is imperative to have a scalable infrastructure.

### Quotes
#### Beforehand
We can easily calculate the quotes for each game beforehand. We know that they are so many games each day. Therefore we can easily scale our infrastructure to be able to calculate the quote of the game in advance. Even if a game is in a week, we can schedule calculations. The calculation of the quote can be scheduled, and therefore we don't have to be able to scale up and down.


#### Live quotes
Live quoting calculation varies depending on the games ongoing. If there are a lot of events in a game, we'll have to recalculate and adjust the quote every minute. But if nothing happens, we can change the quote every 5-6 minutes based on a predetermined calculation.


#### KYC
Depending on which country this project is for. We could either use FranceConnect or another KYC Provider. 
- FranceConnect is a platform that guarantees the identity of a user by relying on existing accounts for which his identity has already been verified. This is as simple as connecting to their API and asking the user to connect with an officially approved account such as (Impots.gouv, ameli, etc.). It is free.
- Other KYC Providers will probably charge us a fee, but we'll be able to select the information we want to test more precisely.

#### Oracle
As an Oracle on the Ethereum's Blockchain, we need to provide accurate and validated information. 
We could either have a self-hosted node, or we could use a third-party service to interact with the blockchain.
Self-hosted node: We'll have to take care of it, make sure it is always running. Monitor the server it is running on, etc ...
Third-Party service: We'll have to pay a fee, but we won't have to take care of the maintenance of the node. 

## Architecture Proposition

### Bets

#### Coming Games

We can use a standard server to calculate all the quotes. We'll have to see how much computational power we need to be able to calculate all the quotes per week.
We could have one server per sport. Therefore if one server is down, we only lose the calculation of one sport and not all the others. Because our business heavily depends on the information that comes out of these servers, we should have multiple servers doing the same calculation to make sure there aren't any mistakes. It is an increased cost but we could be dealing with millions of euros in bets, which is a lot compared to a few hundred euros more each month for redundancy.

#### Live Games

Because the live games quotes calculation depends so heavily on what happens during those game we should move our calculation to the Cloud.
On AWS we could use a mix of Lambda functions and/or EC2 with scaling up.
- Lambda Functions: Higher cost but hypothetical infinite scaling up. Limited to a few languages. Higher cost but pay as you use.
- EC2: Harder to implement a scaling up & down model but we can use it with any languages we like.

At first, we should use lambda functions and when we achieve a critical mass we should move to EC2. Lambda functions will allow us to easily create a model that responds to our needs without having to worry about scaling.

### KYC 

Assuming that the project is only for the French population. FranceConnect should answer all of our needs. It provides us an API to easily authenticate & verify the identity of a user. It doesn't require any passport validation or other. The user just uses an account he already has that is validated to connect to our service. It could reduce the time for signup significantly. Also, we wouldn't need to have a server running for KYC Validation, or pay a third party for validation, or recruit people to validate the passport.

### Oracle

[Infura](https://infura.io/) seems to be a good choice for our needs. The Free Tier is fairly generous and allows us to easily interact with the blockchain via API.

### API For Sports Dataprovider

We'll have to look at different data providers for information on the games.

### Webserver

Our webserver should be hosted on AWS. Because the quotes beforehand and live calculations are done on AWS, to avoid additional cost we should host our webserver on AWS. 

We could use:
- AWS EC2: For hosting the front & the back
- AWS Cloudfront: CDN to ensure that every user has fast access to our website
- AWS RDS: For our SQL Database for storing quotes
- AWS Cognito: For user authentification
- AWS S3: For stocking files such as images
- AWS DynamoDB: For a NoSQL Database for any information that doesn't require a SQL format - TO BE DEFINED

#### Front

The front should be done with ReactJS. It is one of the most mainstream frameworks for web development and it a perfect match for this kind of websites

We can bootstrap the project with this boilerplate: [https://github.com/StanGirard/ReactBoilerplate](https://github.com/StanGirard/ReactBoilerplate)

#### Back

The back should be created in either Go or NodeJS.
- Go: Good choice because AWS Lambda functions can also use go. Unify our stacks for the back
- NodeJS - Express: Unify front and back.

We'll use NodeJS because I have more technical knowledge with it than with Go.

We can also bootstrap the project with this boilerplate: [https://github.com/StanGirard/ExpressBoilerplate](https://github.com/StanGirard/ExpressBoilerplate)

#### Database

AWS Aurora allows us to run a managed SQL database. Ensuring maximum availability and easy scaling.

#### Authentification

Accounts on the platform are very sensible, some user could have thousands of euros in currency at one time. Therefore using a validated and secure authentification such as AWS Cognito will ensure maximum security for our users.

#### Storage

If we need to store any images, documents, etc AWS S3 is the best choice.

#### Cron Worker

A worker that will look in the database for the results of the games and send it to the Smart Contract via Infura












