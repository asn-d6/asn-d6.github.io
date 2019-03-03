---
layout: post
title: "Mission: Montreal! (Building the Next Generation of Onion Services)"
---
*[Post originally appeared on the [Tor blog](https://blog.torproject.org/mission-montreal-building-next-generation-onion-services)]*

[![](https://extra.torproject.org/blog/2016-05-23-montreal/hs_montreal_masterplan.png)](https://extra.torproject.org/blog/2016-05-23-montreal/hs_montreal_masterplan.png)

* * *


A few weeks ago, a small group of Tor developers got together in Montreal and worked on onion services for a full week. The event was very rewarding and we wrote this blog post to share with you how we spent our week! For the record, it was our second onion service hackfest, following the [legendary Arlington Accords of July 2015](https://blog.torproject.org/blog/hidden-service-hackfest-arlington-accords). Our main goal with this meeting was to accelerate the development of the Next Generation Onion Services project (aka proposal 224). We have been working on this project for the past several months and have made great progress. However, it's a huge project! Because of its volume and complexity, it has been extremely helpful to meet and work together in the same physical space as we hammer out open issues, review each other's code, and quickly make development decisions that would take days to coordinate through mailing lists. During the hackfest, we did tons of work. Here is an incomplete list of the things we did:

*   [In our previous hidden service hackfest](https://blog.torproject.org/blog/hidden-service-hackfest-arlington-accords), we started designing [a system for _distributed random number_ generation on the Tor network](https://gitweb.torproject.org/torspec.git/tree/proposals/250-commit-reveal-consensus.txt). A _"distributed random number generator"_ is a system where multiple computers collaborate and generate a single random number in a way that nobody could have predicted in advance (not even themselves). Such a system will be used by next generation onion services to inject unpredictability into the system and enhance their security.

    Tor developers finished implementing the protocol several months ago, and since then we've been reviewing, auditing, and testing the code.

    As far as we know, a distributed random generation system like this has never been deployed before on the Internet. It's a complex system with multiple protocol phases that involves many computers working together in perfect synergy. To give you an idea of the complexity, here are the hackfest notes of a developer suggesting a design improvement to the system:

     [![](https://extra.torproject.org/blog/2016-05-23-montreal/hs_montreal_4.png)](https://extra.torproject.org/blog/2016-05-23-montreal/hs_montreal_4.png)



    Complicated protocols require lots of testing! So far, onion service developers have been testing this system by creating fake small virtual Tor networks on their laptops and doing basic tests to ensure that it works as intended. However, that's not enough to properly test such an advanced feature. To really test something like this, we need to make a Tor network that works **exactly** like the real Tor network. It should be a real distributed network over the Internet, and not a virtual thing that lives on a single laptop!

    And that's exactly what we did during the Montreal hackfest! Each Tor developer set up their own Tor node and enabled the _"distributed random number generation"_ feature. We had Tor nodes in countries all around the world, just like the real Tor network, but this was a network just for ourselves! This resulted in a "testing Tor network" with 11 nodes, all performing the random number generation protocol for a whole week.

    This allowed us to test scenarios that could make the protocol burp and fail in unpredictable ways. For example, we instructed our testing Tor nodes to abort at crucial protocol moments, and come back in the worst time possible ways, just to stress test the system. We had our nodes run ancient Tor versions, perform random chaotic behaviors, disappear and never come back, etc.

    This helped us detect various bugs and edge cases. We also confirmed that our system can survive network failures that can happen on the real Internet. All in all, it was a great educational experience! We plan to keep our testing network live, and potentially recruit more people to join it, to test even more features and edge cases!

    For what it's worth, here is a picture of the two first historic random values that our Tor test network generated. The number "5" means that 5 Tor nodes contributed randomness in generating the final random value:

     [![](https://extra.torproject.org/blog/2016-05-23-montreal/hs_montreal_3.png)](https://extra.torproject.org/blog/2016-05-23-montreal/hs_montreal_3.png)


*   We also worked to improve the design of next generation onion services in other ways. We improved the clarity of the specification of proposal 224 and fixed inconsistencies and errors in the text (see [latest prop224 commits](https://gitweb.torproject.org/torspec.git/log/)).

    We designed various improvements to the onion service descriptor download logic of proposal 224 as well as ways to improve the handling of clients with skewed clocks. We also brainstormed ways we can improve the shared randomness protocol in the future.

     [![](https://extra.torproject.org/blog/2016-05-23-montreal/hs_montreal_2.png)](https://extra.torproject.org/blog/2016-05-23-montreal/hs_montreal_2.png)

    We discussed ways to improve the user experience of the 55-character-long onion addresses for next generation onion services (compared to the 16-character-long onion addresses used currently). While no concrete design has been specified yet, we identified the need for a checksum and version field on them. We also discussed modifications to the Tor Browser Bundle that could improve the user experience of long onion addresses.


*   We don't plan to throw away the current onion service system just yet! When proposal 224 first gets deployed, the Tor network will be able to handle both types of onion services: the current version and the next generation version.

    For this reason, while writing the code for proposal 224, we've been facing the choice of whether to refactor a particular piece of code or just rewrite it completely from scratch. The Montreal hackfest allowed us to make quick decisions about these, saving tons of time we would have spent trying to decide over mailing lists and bug trackers.


 [![](https://extra.torproject.org/blog/2016-05-23-montreal/hs_montreal_7.png)](https://extra.torproject.org/blog/2016-05-23-montreal/hs_montreal_7.png)

*   We also worked on breaking down further the implementation plan for proposal 224. We split each task into smaller subtasks and decided how to approach them. Take a look at [our notes](https://extra.torproject.org/blog/2016-05-23-montreal/).

* * *

Be seeing you!
