---
layout: page
title: "iOS SDK Sample"
date: 2013-08-12 18:19
comments: true
sharing: true
footer: true
---

Introduction
============
This sample shows a working code for using SoTopless iOS SDK.

GameKit Compatibility
=====================
SoTopless iOS SDK provides compatible API for major features of Leaderboard API in Apple GameKit.

The only different part is user authentication. You need to pass playerID, playerAlias, and a password. 

A Sample Code 
-----------
SoTopless iOS SDK supports asynchronous API calls similar to Apple GameKit. The sample code shows following steps.

1. Player Authentication.
2. Posting Player Score.
3. Listing top N Scores and getting player's score and ranking.

{% codeblock %}
////////////////////////////////////////////////////////////////////////////////
// Login ( User Authentication )
NSString * identifier = [ NSString stringWithFormat:@"unique identifier for a the user or phone"];

NLLocalPlayer * localPlayer = [ NLLocalPlayer localPlayer:uuid playerAlias:@"Kangdori" password:uuid ];

localPlayer.authenticateHandler = ^(NSError * error) {
    if (error != NULL) {
        NSLog( @"Error : while authenticating : %@", error.description );
        return;
    }
    NSLog( @"Success : User authenticated.");

    ////////////////////////////////////////////////////////////////////////////////
    // Report Score
    NLScore * myScore = [[NLScore alloc] initWithCategory:@"all"];
    myScore.value = 300; // score

    [myScore reportScoreWithCompletionHandler:^(NSError * error) {
        if (error != NULL) {
            NSLog( @"Error : while authenticating : %@", error.description );
            return;
        }
        NSLog( @"Success : Score reported.");
    
        ////////////////////////////////////////////////////////////////////////////////
        // List top 10 Scores
        NLLeaderboard * leaderboard = [[NLLeaderboard alloc] init];
        leaderboard.category = @"all";
        leaderboard.range = NSMakeRange(1,10); // 10 users from the 1st user.
    
        [leaderboard loadScoresWithCompletionHandler:^(NSArray* scores, NSError* error) {
            if (error != NULL) {
                NSLog( @"Error : while loading scores : %@", error.description );
                return;
            }
            NSLog( @"Success : Scores loaded.");

            NSLog( @"Player's ranking : %lld score : %lld", 
                   leaderboard.localPlayerScore.rank, 
                   leaderboard.localPlayerScore.value );

            NSLog( @"Listing scores." );
	
            for (NLScore * score in scores) {
                NSString *dateString = [NSDateFormatter localizedStringFromDate:score.date
                                             dateStyle:NSDateFormatterShortStyle
                                             timeStyle:NSDateFormatterFullStyle];
    
                NSLog( @"Rank: %lld, Display Alias : %@, Score : %lld, Time : %@",
                       score.rank, score.playerAlias, score.value, dateString );
            }
        }]; // [leaderboard loadScoresWithCompletionHandler:...]
    
    }]; // [myScore reportScoreWithCompletionHandler:...]

}; // localPlayer.authenticateHandler

{% endcodeblock %}

Warning
-------
The iOS SDK was built during TechCrunch 2013 Hackathon from 7th to 8th Sep. It took about 20 hours for coding and testing. More work needs to be done and the iOS SDK code is *experimental*.

Full source code of SoTopless iOS SDK is available at following github URL.

[https://github.com/Nanolat/sotopless/tree/master/src/ios/objc](https://github.com/Nanolat/sotopless/tree/master/src/ios/objc)
