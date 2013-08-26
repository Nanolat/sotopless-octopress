---
layout: page
title: "JavaScript Tutorial"
date: 2013-08-12 18:19
comments: true
sharing: true
footer: true
---

Introduction
============
This tutorial provides examples of using SoTopless JavaScript APIs.

User Management
===============
To post scores of a user, or get scores and ranks of a user, you need to create the user on SoTopless server. You need to create the user only once, like you sign-up on a web site only once.
After the user is created, you can use the name of the user to post scores, to get scores and ranks of the user on a leaderboard.

Create a User
-------------
Creating a user is simple. Specify the identiy of the user such as e-mail, device-id, or GUID. The user_create function creates a user.

{% codeblock %}
// Create a user with an e-mail. 
user_create("billg@gmail.com");

// Create a user with a device-id or GUID.
user_create("2b6f0cc904d137be2e1730235f5664094b831186"); 
user_create("759bfd0a-2c91-4e45-a35c-779740ac5e77");  

{% endcodeblock %}

Leaderboard Management
======================
You can create multiple leaderboards, post scores of users based on different criteria. For example, you may want to create leaderboards based on users' countries such as USA, Canada, or Germany. Creating weekly, monthly, and daily leaderboards also requires you to create multiple leaderboards.

Leaderboard management APIs help you to create, get, drop, and purge leaderboards. After creating a leaderboard, you can get the handle of the created leaderboard. The handle will be used to purge, drop a leaderboard. Also it will be used to get scores and ranks of a user, or to get the top N users on a leaderboard.

In case you have monthly, weekly, or daily leaderboards, you can purge those leaderboards regularly to keep the rankings up-to-date. 

Create a Leaderboard
--------------------
Following code snippet creates monthly, weekly, and daily leaderboards. After you create the leaderboard, scores posted to the leaderboard is safely stored so that you do not lose any data even on catastropic situations such as power-off or OS crash.

{% codeblock %}

leaderboard_create("monthly");
leaderboard_create("weekly");
leaderboard_create("daily");

{% endcodeblock %}

Get the List of Leaderboards
----------------------------
You can get the list of all created leaderboards by using leaderboard_list function.

{% codeblock %}

var boards = leaderboard_list();

// The boards is Array of strings containing the list of leaderboard names.
for ( var board in boards ) {
    console.log(board);
}

{% endcodeblock %}

Purge Scores on a Leaderboard
-----------------------------
You can purge leaderboards such as monthly, weekly, and daily ones to keep the rankings up-to-date.

{% codeblock %}

// 24 hours converted to milliseconds.
var one_day_ms = 24 * 60 * 60 * 1000;

var a_day_ago   = util_now() - one_day_ms;
var a_week_ago  = util_now() - 7 * one_day_ms;
var a_month_ago = util_now() - 30 * one_day_ms;

leaderboard_purge("monthly", a_day_ago  );
leaderboard_purge("weekly",  a_week_ago );
leaderboard_purge("daily",   a_month_ago);

{% endcodeblock %}

Drop a Leaderboard
------------------
Dropping a leaderboard is like dropping a table on a database. You will lose all data on the leaderboard to be dropped.

{% codeblock %}

leaderboard_drop("monthly"); 
leaderboard_drop("weekly" ); 
leaderboard_drop("daily" ); 

{% endcodeblock %}


Posting and Getting Scores
==========================
Whenever a user achieves a new high score, you post the user's score with the identity of the user and the name of the leaderboard. While you post the user's score, you can provide a string containing the situation of the user such as game items the user used to clear the stage. 

You can get the ranking and score on the leaderboard where you posted the user's score. Also you can get a range of ranking on a leaderboard. For example, you can get the list of users and scores of the top 10 scoreboard by quering the rankings from 1st to 10th. 

Also, you may want to remove a score posted by a user. Removing the score posted by a user is supported.

Post a User Score
-----------------
{% codeblock %}

var score = 30290;
var situation = "Black King Sword, White Devil Shield";
var now = util_now();    

score_post("monthly", "billg@gmail.com", score, situation, now );
score_post("weekly",  "billg@gmail.com", score, situation, now );
score_post("daily",   "billg@gmail.com", score, situation, now );

{% endcodeblock %}

Get User Score and Ranking
---------------------------
Following code snippet gets a score and a ranking on the weekly leaderboard.

{% codeblock %}

var rank_desc = score::get("weekly", "billg@gmail.com");

console.log( "rank      : " + rank_desc.rank );
console.log( "user name : " + rank_desc.user_name );
console.log( "score     : " + rank_desc.score );
console.log( "situation : " + rank_desc.situation );
console.log( "time      : " + rank_desc.when );

{% endcodeblock %}

Remove a User Score
-------------------
Following code snippet removes the score posted by a user on monthly, weekly, daily leaderboard.

{% codeblock %}

score_remove("monthly", "billg@gmail.com" );
score_remove("weekly",  "billg@gmail.com" );
score_remove("daily",   "billg@gmail.com" );

{% endcodeblock %}

Get Top N Users
---------------
The following code snippet prints the top 10 scores and users on the weekly leaderboard.

{% codeblock %}

var top10_scores = 
score::list("weekly", 
            1,    // The starting ranking. Start from 1st ranking. 
            10 ); // The number of users from the 1st ranking.  

for( var score in top10_scores )
{
    console.log( "Score : " + rank.score + ", Name : " + rank.user_name );
}

{% endcodeblock %}
