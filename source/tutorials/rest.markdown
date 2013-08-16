---
layout: page
title: "REST Tutorial"
date: 2013-08-12 18:19
comments: true
sharing: true
footer: true
---

Setup SoTopless on EC2
======================
You can deploy and start SoTopless server in your EC2 node running Linux by issuing a command in your laptop.

Prerequisites
-------------
Java 6 or higher on Windows or Linux platform is the only requirement for SoTopless.

Unzip the tarball
-----------------
{% codeblock %}
tester@bigdatatools$ tar xvfz sotopless-1.0.tar.gz
tester@bigdatatools$ cd sotopless-1.0
tester@bigdatatools$
{% endcodeblock %}

Deploy SoTopless on EC2 (Linux)
-------------------------------
Run deploy-ec2.sh to deploy SoTopless on your EC2 node.
{% codeblock %}
tester@bigdatatools$ ./deploy-ec2.sh -node ec2-123-123-123-123.compute-1.amazonaws.com
SoTopless version 1.0 deployed on ec2-123-123-123-123.compute-1.amazonaws.com.

tester@bigdatatools$
{% endcodeblock %}

Deploy SoTopless on Windows
---------------------------
Unzip the tarball and run bin/sotopless.bat to run the SoTopless server.

Leaderboard Management
======================

Create a Leaderboard
--------------------
{% codeblock %}
POST leaderboards/:lname

Example:

POST leaderboards/weekly

{% endcodeblock %}

Get the list of leaderboards
----------------------------
{% codeblock %}
GET leaderboards

Example:

GET leaderboards

Response Data:
{
    [ "weekly", "monthly", "daily", "all" ]
}

{% endcodeblock %}

Purge Scores on a Leaderboard
-----------------------------
{% codeblock %}
DELETE leaderboards/:lname/before/YYYYMMDD

Example :

DELETE leaderboards/weekly/before/20130801
{% endcodeblock %}


Drop a Leaderboard
------------------
{% codeblock %}
DELETE leaderboards/:lname

Example :

DELETE leaderboards/weekly
{% endcodeblock %}


Setting and Getting Scores
===========================

Set User Score
---------------
{% codeblock %}
PUT leaderboards/:lname/:userid/:score 

Example :

PUT leaderboards/weekly/10205/203000
    
{% endcodeblock %}

Get User Score and Rank
-----------------------
{% codeblock %}
GET leaderboard/:lname/:userid

Example:

GET leaderboards/weekly/10205

Response data :
{
    "rank": 1,
    "uid": 10205,
    "score": 203000
    "date": "/Date(694224000381)/"
}
{% endcodeblock %}


Get Top N Users
---------------
{% codeblock %}
GET leaderboards/:lname/from/:K/top/:N

Example:

GET leaderboard/weekly/from/1/top/5

Response data:
{
    [
        {
            "rank": 1,
            "uid": 10205, 
            "score": 203000,
            "date": "/Date(694224000381)/"
        },
        {
            "rank": 2,
            "uid": 10301,    
            "score": 190000,
            "date": "/Date(694224009400)/"
        },
        {
            "rank": 3,
            "uid": 10391,   
            "score": 180000,
            "date": "/Date(694224004800)/"
        },
        {
            "rank": 4,
            "uid": 10189,    
            "score": 143000,
            "date": "/Date(694224000389)/"
        },
        {
            "rank": 5,
            "uid": 10197,   
            "score": 139000,
            "date": "/Date(694224003918)/"
        }
    ]
}

{% endcodeblock %}






