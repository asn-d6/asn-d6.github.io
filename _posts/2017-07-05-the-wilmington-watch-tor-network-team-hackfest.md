---
layout: post
title: "The Wilmington Watch: A Tor Network Team Hackfest"
---
*[Post originally appeared on the [Tor blog](https://blog.torproject.org/blog/network-team-hackfest-wilmington-watch/)]*

{:refdef: style="text-align: center;"}
[![](https://blog.torproject.org/sites/default/files/image/tor-hackfest-mandela.jpg)](https://blog.torproject.org/sites/default/files/image/tor-hackfest-mandela.jpg)
{: refdef}

The [Tor network team](https://trac.torproject.org/projects/tor/wiki/org/teams/NetworkTeam) is a small team responsible for developing the core Tor daemon. We're located around the globe, so we periodically meet in person for team hackfests to keep our team fresh and up to date with all things Tor, and to fast-track features and improvements. Previously, we've met for hackfests in [Arlington](https://blog.torproject.org/blog/hidden-service-hackfest-arlington-accords) and [Montreal](https://blog.torproject.org/blog/mission-montreal-building-next-generation-onion-services), and for our latest meeting, we met in Wilmington, Delaware, a town revered for its yearly Italian Festival.

[![](https://extra.torproject.org/blog/2017-07-02-wilmington-hackfest/delaware_river.png)](https://extra.torproject.org/blog/2017-07-02-wilmington-hackfest/delaware_river.png)

To better understand the geographical setting of this meeting, go to Trenton, New Jersey, USA, hop into a Delaware river riverboat, then follow the flow south; after about 60 miles you will eventually see a town named Wilmington on your right. This small town hosted us for a few days while we worked on making Tor stronger and safer.

### What went down 

We worked intensely for several days and nights, researching, planning, and cooking meals for each other. Here is a small fraction of the topics we worked on:

*   We schemed on about the [Tor modularization project](https://trac.torproject.org/projects/tor/query?status=!closed&keywords=~tor-modularity) which aims to clean up and organize [our codebase](https://gitweb.torproject.org/tor.git) into nice and tidy abstract modules. Cleaning and modularizing our code not only reduces technical debt, but also allows us to eventually rewrite those submodules into higher-level languages such as [Rust](https://en.wikipedia.org/wiki/Rust_(programming_language)), [D](https://en.wikipedia.org/wiki/D_(programming_language)) or [APL](https://en.wikipedia.org/wiki/APL_(programming_language)).


![Tor network team hackfest stickies](https://blog.torproject.org/sites/default/files/inline-images/tor-hackfest-wilmington-stickies.jpg)

*   That last part is not science-fiction; as of a month ago, Tor's build system [is capable](https://blog.torproject.org/blog/tor-0312-alpha-out-notes-about-0311-alpha) of building and linking code modules written in Rust. We [are now actively working](https://trac.torproject.org/projects/tor/ticket/22106) on implementing the [protover](https://gitweb.torproject.org/torspec.git/tree/proposals/264-subprotocol-versions.txt) [subsystem](https://gitweb.torproject.org/tor.git/tree/src/or/protover.c) into Rust as our first prototype module.
*   As part of the security discussion, we talked about the [new padding defenses](https://gitweb.torproject.org/torspec.git/tree/proposals/251-netflow-padding.txt) that were recently [added to Tor](https://trac.torproject.org/projects/tor/ticket/16861) and provide cover to Tor circuits against traffic analysis. We made plans for future padding techniques and defenses.
*   We also briefed up the whole team on how our [new entry guard picking algorithm](https://gitweb.torproject.org/torspec.git/tree/guard-spec.txt) works to enhance the security of Tor clients and protects them against local network attacks. We planned various defenses against [hidden service guard discovery attacks](https://gitweb.torproject.org/torspec.git/tree/proposals/247-hs-guard-discovery.txt), as well as [alternative onion routing path algorithms](https://www.freehaven.net/anonbib/papers/pets2013/paper_65.pdf). Our next step for improving guard security is to simulate alternative path construction algorithms and evaluate their performance and security guarantees.
*   We discussed KIST, an alternative network scheduler and congestion management logic for Tor which offers improved circuit performance and cleaner network tubes. KIST is currently [under active development](https://trac.torproject.org/projects/tor/ticket/12541), so we roadmapped and planned for how to get it included upstream.
*   We talked about our code testing techniques and how to improve them. We discussed the necessity of [regression tests](https://trac.torproject.org/projects/tor/ticket/22745), the need to improve our integration tests, and also the future of our [fuzzing framework](https://gitweb.torproject.org/tor.git/tree/doc/HACKING/Fuzzing.md). (Feel free to [get in touch](https://www.torproject.org/about/contact.html.en#irc) if you want to help improve our tests!)
*   We all shared our experiences and thoughts on our beloved tool for ticket tracking and project management: [Trac](https://trac.torproject.org/). We discussed ways we could improve our Trac workflows and also alternative tools we could potentially try (e.g. Gitlab). Transitioning to another tool is not so easy, though; since our Trac instance contains 10+ years of Tor history, we need to make sure we don't lose any information.

### [![](https://extra.torproject.org/blog/2017-07-02-wilmington-hackfest/alienabduction.jpg)](https://extra.torproject.org/blog/2017-07-02-wilmington-hackfest/alienabduction.jpg)

We stayed for about 5 days in town doing all these things and more. Then on a Friday afternoon as the Wilmington Italian Festival was setting up for yet another day, we jumped on trains, planes, and buses and moved on to other places and stories. Life goes on, and the same goes for Tor development.

We're committed to being open and transparent about our work, and we hope you enjoyed this post. Keep on hacking.

