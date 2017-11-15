---
layout: post
title: how we're using Twine at DoSomething.org
---
Twine is an awesome open-source platform for hosting text-based "interactive fiction" web games. Anyone can use it to create a game and put it on a site, and many Twine games deal with themes of identity, justice, and personal experience--the most famous perhaps being "Depression Quest", a beautifully heartbreaking game giving a first-person view into the darkness of depression. ([Laura Hudson](http://www.nytimes.com/2014/11/23/magazine/twine-the-video-game-technology-for-all.html) writing about Twine, Depresion Quest, and Gamergate for the New York Times Magazine.)

At DoSomething.org, we're now using a modification of Twine--something we call Intertwine--to build [SMS games]({{ site.baseurl }}/sms-games). And we're proud of this. We're honored that we're able to benefit from and contribute to a development environment that has built so many games which encourage empathy and understanding of deeply personal experiences aside from your own--very much along the lines of DoSomething.org's mission, as well. 

###THE PROBLEM WE'RE TRYING TO SOLVE
I've previously written about some of the [challenges]({{ site.baseurl }}/testing-challenges) in the game creation and testing process--one of the major challenges we face is ensuring that each game's configuration file (the big JSON object which determines which text message should be sent when) is correct. 

While our configuration file was previously written by hand, all we need to do now is set up the game within Intertwine and it'll generate this file for us automatically. 

###PLAY-TESTING WITH THE TWINE UI
Now, we'll be able to playtest our games before they're even deployed, and more quickly  identify pain points within the user experience. Additionally, our business development and campaign teams can use Intertwine to demo gameplay when pitching SMS games to potential corporate partners. 

###VISUAL VALIDATIONS ON THE FRONT-END
The newest version of Twine is built entirely with JavaScript as a front-end web app. Using Backbone.js, a front-end framework, Twine has validations written into its model objects. For instance, if you make a story passage that doesn't have any connections to another story passage, the passage object in your browser turns red. We plan on building in similar validations, so that anyone setting up an SMS game with Intertwine can easily confirm visually that the game configuration file is valid. 

###NON-DEVELOPERS CAN CREATE GAME CONFIGS 
We want to get to the point where we can say that no code deploys--no developer intervention--is necessary in order to get a game up and running. With the right validations in place (code making sure that everything is set up correctly), Intertwine may very well be able to bring us to that place. 

A related benefit is that we can create and deploy games much faster. Instead of engineering being the rate limiting step, now we can publish games as fast as we can write them. Which brings us to our next advantage:

###THE POTENTIAL OF CROWDSOURCING GAME CREATION
At DoSomething.org, some of [our best campaign ideas](https://www.dosomething.org/campaigns/hunt) have come from members. In much of the same way that Twine has democratized game creation by allowing anyone to create a game, we hope that Intertwine will allow DoSomething.org members to create SMS games we publish. 

When it comes to issues our SMS games are attempting to tackle--bullying, spreading awareness among girls about STEM careers--our members are subject matter experts. They know exactly what stories resonate with their peer group and how to keep their friends engaged. Eventually, we want to be able to bring their narratives and expertise to a wider audience. 

*Many thanks to the developers of Twine.js: Chris Klimas, Leon Arnott, Daithi O Crualaoich, Ingrid Cheung, Thomas Michael Edwards, Micah Fitch, Juhana Leinonen, and Ross Smith.*