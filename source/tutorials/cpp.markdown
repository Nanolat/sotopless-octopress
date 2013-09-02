---
layout: page
title: "C++ Tutorial"
date: 2013-08-12 18:19
comments: true
sharing: true
footer: true
---

Introduction
============
This tutorial provides examples of using SoTopless APIs.
For each function call, you need to check the return value to make sure it is topl::success. However, this kind of error checking is not shown in this tutorial to keep it short and simple. 


Setup SoTopless on EC2
======================
You can deploy and start SoTopless server in your EC2 node running Linux by issuing a command in your laptop.

Prerequisites
-------------
SoTopless supports Windows, Linux, and OSX platform.
Java 6 is required for Java SDK and REST service.

Unzip the tarball
-----------------
{% codeblock %}
$ tar xvfz sotopless-1.0.tar.gz
$ cd sotopless-1.0
$
{% endcodeblock %}

Deploy SoTopless on EC2 (Linux)
-------------------------------
Run deploy-ec2.sh to deploy SoTopless on your EC2 node.
{% codeblock %}
$ ./deploy-ec2.sh -node ec2-123-123-123-123.compute-1.amazonaws.com
SoTopless version 1.0 deployed on ec2-123-123-123-123.compute-1.amazonaws.com.

$
{% endcodeblock %}

Deploy SoTopless on Windows
---------------------------
Unzip the tarball and run bin/sotopless.bat to run the SoTopless server.

Deploy SoTopless on OSX and Linux
---------------------------------
Unzip the tarball and run bin/sotopless.sh to run the SoTopless server.

User Management
===============
To post scores of a user, or get scores and ranks of a user, you need to create the user on SoTopless server. You need to create the user only once, like you sign-up on a web site only once.
After the user is created, you can get the user handle of the created user. The user handle will be used to post scores, to get scores and ranks of the user on a leaderboard.

Create a User
-------------
Creating a user is simple. Specify an identity of the user such as e-mail, device-id, or GUID. The user::create API gives you a user handle of the created user.

{% codeblock %}

using namespace topl;

user::handle_t user_handle;

// Create a user with an e-mail. 
user::create("foo.bar@gmail.com", & user_handle);

// Create a user with a device-id or GUID.
user::create("2b6f0cc904d137be2e1730235f5664094b831186", & user_handle); 
user::create("759bfd0a-2c91-4e45-a35c-779740ac5e77",     & user_handle);  

{% endcodeblock %}

Get a User Handle
-----------------
Once a user is created, you can get the handle of the user by calling user::get function. 

{% codeblock %}
using namespace topl;

user::handle_t user_handle;

// Get the user handle by e-mail.
user::get("foo.bar@gmail.com", & user_handle); 

// Get the user handle by device-id or GUID. 
user::get("2b6f0cc904d137be2e1730235f5664094b831186", & user_handle); 
user::get("759bfd0a-2c91-4e45-a35c-779740ac5e77",     & user_handle);

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
using namespace topl;

leaderboard::handle_t monthly_handle;
leaderboard::handle_t weekly_handle ;
leaderboard::handle_t daily_handle  ;

leaderboard::create("monthly", & monthly_handle);
leaderboard::create("weekly",  & weekly_handle );
leaderboard::create("daily",   & daily_handle  );

{% endcodeblock %}

Get a Leaderboard Handle
------------------------
Once your leaderboard is created, you can use leaderboard::get function to get the handle of the created leaderboard. 

{% codeblock %}
using namespace topl;

leaderboard::handle_t monthly_handle;
leaderboard::handle_t weekly_handle ;
leaderboard::handle_t daily_handle  ;

leaderboard::get("monthly", & monthly_handle);
leaderboard::get("weekly",  & weekly_handle );
leaderboard::get("daily",   & daily_handle  );

{% endcodeblock %}

Get the List of Leaderboards
----------------------------
You can get the list of all created leaderboards by using leaderboard::list function.

{% codeblock %}
using namespace topl;
using namespace std;

std::vector<leaderboard::leaderboard_desc_t> leaderboards;

leaderboard::list( & leaderboards);

// The leaderboards vector contains an instance of leaderboard_desc_t for each leaderboard.
for( const leaderboard::leaderboard_desc_t & leaderboard : leaderboards )
{
    cout << "leaderboard name : " << leaderboard.identity << endl;
    cout << "leaderboard handle : " << leaderboard.handle << endl;
} 

{% endcodeblock %}

Purge Scores on a Leaderboard
-----------------------------
You can purge leaderboards such as monthly, weekly, and daily ones to keep the rankings up-to-date.

{% codeblock %}
using namespace topl;

// 24 hours converted to milliseconds.
util::timestamp_t one_day_ms = 24 * 60 * 60 * 1000;

util::timestamp_t a_day_ago   = util::now() - one_day_ms;
util::timestamp_t a_week_ago  = util::now() - 7 * one_day_ms;
util::timestamp_t a_month_ago = util::now() - 30 * one_day_ms;

leaderboard::purge(monthly_handle, a_day_ago  );
leaderboard::purge(weekly_handle,  a_week_ago );
leaderboard::purge(daily_handle,   a_month_ago);

{% endcodeblock %}

Drop a Leaderboard
------------------
Dropping a leaderboard is like dropping a table on a database. You will lose all data on the leaderboard to be dropped.

{% codeblock %}
using namespace topl;

leaderboard::drop(monthly_handle); 
leaderboard::drop(weekly_handle ); 
leaderboard::drop(daily_handle  ); 

{% endcodeblock %}


Posting and Getting Scores
==========================
Whenever a user achieves a new high score, you post the user's score with the handle of the user and the handle of the leaderboard. While you post the user's score, you can provide a string containing the situation of the user such as game items the user used to clear the stage. 

You can get the ranking and score on the leaderboard where you posted the user's score. Also you can get a range of ranking on a leaderboard. For example, you can get the list of users and scores of the top 10 scoreboard by quering the rankings from 1st to 10th. 

Also, you may want to remove a score posted by a user. Removing the score posted by a user is supported.

Post a User Score
-----------------
Following code posts a user's score to SoTopless server. SoTopless server runs a quick scrutinization, and includes the score to the given leaderboard only if the score is from a genuine player.

{% codeblock %}
using namespace topl;

const int score = 30290;
const std::string situation = "Black King Sword, White Devil Shield";
const util::timestamp_t now = util::now();    

score::post(monthly_handle, user_handle, score, situation, now );
score::post(weekly_handle,  user_handle, score, situation, now );
score::post(daily_handle,   user_handle, score, situation, now );

{% endcodeblock %}

Get User Score and Ranking
---------------------------
Following code snippet gets a score and a ranking on the weekly leaderboard.

{% codeblock %}
using namespace topl;
using namespace std;

score::rank_desc_t rank_desc;

score::get(weekly_handle, user_handle, & rank_desc);

cout << "ranking : "      << rank_desc.rank          << endl;
cout << "user handle : "  << rank_desc.user_handle   << endl;
cout << "user identity: " << rank_desc.user_identity << endl;
cout << "score : "        << rank_desc.score         << endl;
cout << "situation : "    << rank_desc.situation     << endl;
cout << "time : "         << rank_desc.when          << endl;

{% endcodeblock %}

Remove a User Score
-------------------
Following code snippet removes the score posted by a user on monthly, weekly, daily leaderboard.

{% codeblock %}
using namespace topl;

score::remove(monthly_handle, user_handle );
score::remove(weekly_handle,  user_handle );
score::remove(daily_handle,   user_handle );

{% endcodeblock %}

Get Top N Users
---------------
The following code snippet prints the top 10 scores and users on the weekly leaderboard.

{% codeblock %}
using namespace topl;
using namespace std;

std::vector<score::rank_desc_t> top10_scores;

score::list(weekly_handle, 
            1,  // The starting ranking. Start from 1st ranking. 
            10, // The number of users from the 1st ranking.  
            & top10_scores) );

for( const score::rank_desc_t & rank : top10_scores )
{
    cout << "Score : " << rank.score << ", Name : " << rank.user_identity << endl;
}

{% endcodeblock %}
