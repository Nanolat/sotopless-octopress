---
layout: page
title: "A leaderboard server without any cheating players on top scores."
date: 2013-08-12 18:16
comments: true
sharing: true
footer: true
---
What?
-----
This is a problem.

{% img /images/gamecenter.jpg %}

SoTopless is a leaderboard server(ranking server) that excludes cheating players with its own genuine player checking algorithm. 

It supports C++/Objective C/JavaScript client SDK for Android, iOS, Linux, and Windows platform. 

Following is the list of features :

*   Check if a score was posted by a genuine player.
*   Post a new score of a user.
*   Get top N users with scores from a leaderboard.  
*   Get rank and score of a user from a leaderboard. 
*   Add different kinds of leaderboards such as weekly, monthly, Asia, Europe, etc. 

Why?
----
No more scores posted by cheating players. We provide automatic cheating player filter with our own algorithm.

Simple and easy APIs save your time. Focus on your game/app development, instead of designing your ranking database and server.  
SoTopless fits well with the leaderboard requirements both for your game, and your gamified app.

How?
----
To run our algorithm to check if the player's score is genuine, we need more computing power and efficient server S/W processing. SoTopless uses ASIO library of Boost for supporting huge number of requests leveraging asynchronous I/O on the server side, and uses carefully designed database engine. SoTopless server is written in C++ supporting major operating systems such as Windows and Linux.

Open Source?
------------
SoTopless is an open source software with AGPL v3 license. It is open source except for the genuine player checking algorithm. A commercial license which includes an Apache v2 license is available for project owners not willing to open their source codes.

The source code is available at [https://github.com/nanolat/sotopless](https://github.com/nanolat/sotopless).

Tutorial?
---------
To check out the tutorial on deploying SoTopless on Amazon EC2 node, posting scores, getting top N users, getting the rank of a user, click [here](tutorials/ios.html) for a sample code of iOS SDK, [here](tutorials/javascript.html) for JavaScript Tutorial, [here](tutorials/cpp.html) for C++ Tutorial, [here](tutorials/cpp-api.html) for C++ API.

Price?
------

All SoTopless products have an unconditional 60-day money back satisfaction guarantee.

{% img /images/60days.jpg %}

1. Community Edition
:= Free, but under AGPL v3 license requiring to open source your projects. 
2. Start-up Subscription
:= Includes commercial license. This is for start-up companies or indie developers having less than 3 developers. The price is $2,000 per game(or app) per year.
3. Enterprise Subscription, Enterprise Edition
:= Send us an email ( support [at] nanolat.com ).

When?
-----
The first closed alpha testing starts on 30th Sep. To join the alpha testing, click [here](join-alpha-test).
