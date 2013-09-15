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
SoTopless iOS SDK provides simple and easy to use APIs like Apple GameKit. In case you have experience in developing leaderboards using GameKit, you will feel familiar with iOS SDK as well.

However, there are some different parts such as user authentication. You need to pass playerID, playerAlias, and a password. 

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
    [myScore increaseValue:300 reason:@"Ate Apple"]; // Increase the score with a description on how the user got the score. 
    [myScore increaseValue:500 reason:@"Ate Orange"];  
    [myScore increaseValue:100 reason:@"Ate Water"];  

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

History & Source Code
---------------------
The first iOS SDK was built during TechCrunch Disrupt 2013 Hackathon from 7th to 8th Sep. It took about 20 hours for coding and testing. Source code : 
[https://github.com/Nanolat/sotopless/tree/master/src/ios-legacy/objc](https://github.com/Nanolat/sotopless/tree/master/src/ios-legacy/objc)

After the Hackathon, both server code and client SDK was completely rewritten. The new version of SDK implementation eliminates unnecessary round trips between the server and client improving the time user needs to wait for posting scores and getting top scores as well as the user's score. 
 
Full source code of the new SoTopless iOS SDK is available at following Github URL.
[https://github.com/Nanolat/sotopless/tree/master/src/ios-client/objc](https://github.com/Nanolat/sotopless/tree/master/src/ios-client/objc)
