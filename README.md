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
Has an oracle on the Ethereum's Blockchain we need to provide accurate and validated information. 
We could either have a self-hosted node, or we could use a third-party service to interact with the blockchain.
Self-hosted node: We'll have to take care of it, make sure it is always running. Monitor the server it is running on, etc ...
Third-Party service: We'll have to pay a fee, but we won't have to take care of the maintenance of the node. 