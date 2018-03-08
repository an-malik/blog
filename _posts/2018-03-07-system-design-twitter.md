---
layout: post
title:  "System Design - Twitter"
author: "Anurag"
---
## Rough Draft

1. Tweeting - Sending a tweet/ retweet
2. Timeline - A homefeed of tweets on your profile
- User Timeline - your own tweets
- Home Timeline - tweets from people your follow.

3. Following - ability to follow someone elses tweets on your 'home timeline'.

Solution 1 -
Relational Database -
1. SQL - TWEETS & USER Tables
TWEETS.user --many_to_one--> USER.id (pk)
TWEETS.tweets                  USER.name
TWEETS.id (pk)

Issues - How to display homefeed -
select query to get all people I follow and get their tweets.
NOT SO SCALABLE

* READ HEAVY SYSTEM
* AVAILABILITY is of more importance compared to CONSISTENCY (all users see the same homefeed - tweet content)

Better Design:
1. In-memory FANNING OUT (put tweet in an in-memory database - precompute homefeeds of users)

2. LOAD BALANCER - pushes tweets to a REDIS cluster.
Each tweet gets replicated thrice in redis i.e. replication factor = 3
Identical eventualy consistent native redis lists on leader and replicas.

Bob follows Alice:
Alice has one hunded followers.
Alice tweet.
Each tweet goes to 100 users * replication factor = 100 * 3

What if a celebrity with 60 million followers send a tweet -
60 million * 3 tweet copies on redis? huge performance overhead?

3. Mix of 1 and 2.

TRADE OFFS -
* FAST READS
* SPACE - 140 char limit on tweet size.

Homefeed copes in Redis is space expensice but improves the read time.

Action -
Access Timeline -
Browser - GET - Load Balancer - Pick one redis instance to serve request.


Design - BROWSER --> LB --> REDIS
How does LB know with Redis machine to get a user's timeline - hash lookup

Twitter search - another branch on fanout step.

