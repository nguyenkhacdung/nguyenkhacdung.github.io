---
layout: post
title: using Mocha to test games with complex branching paths
---

When we consider testing [DoSomething.org's SMS games]({{ site.baseurl }}/sms-games), we face [challenges]({{ site.baseurl }}/testing-challenges) which run the gamut from debugging problems across mobile networks to figuring out a method of testing thousands of unique user choice paths. 

But given these constraints, how have we actually implemented testing? We split our concerns, separating tests for MobileCommons functionality from automated tests of responder functionality. 

###Testing interactions with the MobileCommons API
MobileCommons sends users text messages in response to POST requests made to its API. What's the best way to test that the API is working, given that its outputs are sent to a mobile phone number and are't web-based? This is a question we haven't yet answered--our suspicion is that we may be able to set up a Google Voice account, which then can programmatically collect text messages and confirm their receipt via [API](https://code.google.com/p/google-voice-java/).

To test our responder's interactions to MobileCommons API calls, we currently simply use Postman, [a web service](https://chrome.google.com/webstore/detail/postman-rest-client/fdmmgilgnpjigdojojpjoooidkmcomcm?hl=en) which allows easy creation and sending of HTTP requests. 

If anyone has any suggestions on how we can easily automate our manual MobileCommons API tests, please get in otuch! 

###Testing the ds-mdata-responder
The parts of our responder which deal with the MobileComons API have been manually tested separately, and now we're able to automate tests for the majority of the remaining code. 

What's the scope of the rest of the application? Our MongoDB database exists on one end of the app, which can query to confirm that the appropriate state changes have been made to the gameplay documents (docs which store information about where users are in a game and what choices they've made.) 

On the other end, our app makes requests to the MobileCommons API. (Our automated tests simply confirm that the requests have been made by detecting events emitted; they do not confirm that MobileCommons has sent the appropriate texts.)

###Running automated tests with Mocha
Our automated tests are built with [Mocha](http://mochajs.org/)--its advantages, from the documentation: 

*Mocha is a feature-rich JavaScript test framework running on node.js and the browser, making asynchronous testing simple and fun. Mocha tests run serially, allowing for flexible and accurate reporting, while mapping uncaught exceptions to the correct test cases.*

We like Mocha because like a lot of contemporary testing frameworks, has syntax that's very readable--you describe your code's functionality with Mocha functions that resemble read language, like `describe` and `expect`. 

How are we using Mocha? We require our game logic controller files within our test files, and then simulate a user texting their game choices to MobileCommons by creating fake request objects mimicking those requests which actually come from MobileCommons by user action. Then, we call game logic controller functions--taking in these mocked request objects as arguments--and then query our database to make sure that the correct state changes have been made. And since there's lot a repetition in the game (i.e., user acts by texting back 'A'), we've extracted a lot of the testing functionality into a custom helper function module which is called throughout our tests. The first part of our Mocha tests for BullyText functionality is below. 

<img src="{{ site.baseurl }}/images/mochaBullyText.png" style="width: 700px; display: block; margin: auto;"/>  

###Other test coverage tools
We also wanted to give ourselves feedback on how much of our code was covered by tests. Since our tests were automatically running upon code pushes through our continuous integration platform [Wercker](http://wercker.com/), we incorporated an additional code coverage reporting step with [Istanbul](https://github.com/gotwarlost/istanbul)--which calculates code coverage, [Mocha-lcov-reporter](https://github.com/StevenLooman/mocha-lcov-reporter), which outputs test coverage reporting files in the standard LCOV format, and [Coveralls](https://coveralls.io/), a web platform which collects LCOV file data and displays it, linked to specific Github repos, in a clean UI.

<img src="{{ site.baseurl }}/images/coveralls.png" style="width: 500px; display: block; margin: auto;"/> 

To see how we've implemented this Mocha > Istanbul > Mocha-lcov-reporter > Wercker > Coveralls testing coverage flow, feel free to check out our [Wercker configuration file](https://github.com/DoSomething/ds-mdata-responder/blob/master/wercker.yml).

###shortcomings, and things to keep in mind for the future
There are a lot of deficiencies in our testing coverage. Most obviously, we wrote our game-specific tests to simulate games where [every player texts 'A'](https://github.com/DoSomething/ds-mdata-responder/blob/master/test/BullyText.js) at every possible choice. (We really considered this a sort of testing triage--at that moment, the need for testing our game logic fundamentals outweighed our need to ensure that every possible game combination produced the correct endgame result.)