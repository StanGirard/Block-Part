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
For example, most of soccer games happen during the same time window. Therefore, it is imperative to have a scalable infrastructure.

### Calculation
#### Beforehand
We can easily calculate the quotes for each game before hand. We know that they are so many games each day. Therefore we can easily scale our infrastructure to be able to calculate the quote of the game in advance. Even if a game is in a week, we can schedule calculations. The calculation of the quote can be scheduled, and therefore we don't have to be able to scale up and down.

#### Live quotes
Live quoting calculation varies depending on the games ongoing. If there are a lot of events in a game, we'll have to recalculate and adjust the quote every minute. But if nothing happens, we can change the quote every 5-6 minutes based on a predetermined calculation.
