---
layout: post
title: the challenges of testing SMS games
---

Previously, I've written about [how DoSomething.org's SMS games work]({{ site.baseurl }}/sms-games). Testing our responder--the app which powers game logic--poses some unique challenges. Here, I'll talk a bit about the challenges we face in implementing an automated testing regime. In my next post, I'll show you how we've actually implementing testing, and give an overview of where we can improve. 

###what are the environmental challenges to testing the responder? 
Because our app uses a web service to send text messages, end-to-end testing in a traditional sense is difficult. (We've considered having a system of Google Voice numbers as testing endpoints sending and receiving texts, but it'd be difficult for us incorporate this into a testing system without an exposed API.)

So it's challenging to know whether or not a user didn't receive a text because of a bug on our web app's end or because of mobile carrier issues. There are a a lot of moving systems between the SMS web service we use, mobile carriers, and someone's phone. Its hard for us to confirm that a user has received a message on their phone, and to do so consistently--it's even harder for us to confirm that four users of multiplayer games have received messages. 

Our web service, MobileCommons, has implemented a highly sensible spam-filtering system which prevents someone receiving the same message more than roughly two dozen times a day before those messages are marked as spam. Finally, the messages we send all need to be hand-configured through the MobileCommons web service, which is a process prone to human error. 

###what are the app-specific challenges to testing? 
While our gameplay may seem simple, our responder handles a lot of quite complicated logic when deciding which text to send to whom when. And a great deal of this logic is stored within large JSON configuration files. 

We've developed our own logic syntax for computing gameplay outcomes, as seen below.   

<img src="{{ site.baseurl }}/images/testing-challenges-endgameLogic.png" style="width: 700px;"/>  

We use `$AND` and `$OR` operators to specify what conditions need to be met (choices the user should have made) in order for a user to get a message signifying a certain outcome. 

However, our configuration of this JSON file is performed by hand, and aside from brute-force thinking through each possibility, it's difficult to confirm whether or not each expressed outcome is actually possible, or whether or not the paths are mutually contradictory. (Eliminating the hand configuration of the JSON file is a high-priority item for us, and we're currently porting all the configuration files to a database and building a CMS layer so that no code deploys are needed for configuration changes.)

Finally, *the sheer number* of possible game outcomes makes testing each user choice path difficult. Currently, our SMS games feature 6 levels per game, with 2 choices per level for a total of 12 choices per user per game. 2 raised to the power of 12 gives us 4096 different possible user flows for a given user through a game. But performing the same calculation for a multiplayer game of four users--`(2^12)^4`--yields 281,474,980,000,000 possible game play possibilities. 

We would love to know that for every given user action input possibility, we get the right output. But implementing this test woudl be very, very difficult. Our pipe dream is to develop some sort of recursive tree-traversal function, auto-generating testing suite code, which tests **every possible combination of user choice paths.** 

###users getting creative
Our final challenge: it's so difficult for us to predict all the myriad little bizarre ways our users will play our SMS games, and just how they'll expose edge cases. 

We have super users who are invited by their friends to multiple games within a short time span, or who create a single-player and multiplayer game at the same time. We're continually discovering and fixing bugs experienced by a small fraction of our players. Maintaining these fixes also means implementing tests for them; a hefty challenge. 

So how are we working with these constraints to actually implement automated testing for SMS games? [Read on!]({{ site.baseurl }}/sms-tests)