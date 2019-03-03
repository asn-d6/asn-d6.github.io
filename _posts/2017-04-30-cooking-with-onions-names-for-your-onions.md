---
layout: post
title: "Cooking With Onions: Names for your onions"
---
*[Post originally appeared on the [Tor blog](https://blog.torproject.org/cooking-onions-names-your-onions)]*

Hello again, this blog post is the second issue of the _Cooking with Onions_ series which aims to highlight interesting aspects of the onion space. Check-out our [first issue](https://blog.torproject.org/blog/cooking-onions-finding-onionbalance) as well!

Onion addresses are weird...
----------------------------

This post is about onion addresses being weird and the approaches that can be taken to improve onion service usability. In particular, if you've cruised around the onionspace, you must have noticed that onion services typically have random-looking addresses that look like these:

*   3g2upl4pq6kufc4m.onion
*   33y6fjyhs3phzfjj.onion
*   propub3r6espa33w.onion

So for example, if you wanted to visit the Tor website onion service, you would have to use the address [http://expyuzz4wqqyqhjn.onion/](http://expyuzz4wqqyqhjn.onion/) instead of the usual [https://www.torproject.org](www.torproject.org). To better understand why onion addresses are so strange, it helps to remember that onion services don't use the insecure Domain Name System (DNS), which means there is no organization like [ICANN](https://en.wikipedia.org/wiki/ICANN) to oversee a single root registry of onion addresses or to handle ownership dispute resolution of onion addresses. Instead, onion services get **strong authentication** from using _self-authenticating_ addresses: the address itself is [a cryptographic proof of the identity](https://tools.ietf.org/html/rfc7686#section-1) of the onion service. When a client visits an onion service, Tor verifies its identity by using the address as ground truth. In other words, onion services have such absurd names because of all the cryptography that's used to protect them. Cryptographic material are basically **huge numbers** that look meaningless to most humans, and that's the reason onion addresses tend to look random as well. To motivate this subject further, Tor developers have medium-term future plans for [upgrading the cryptography of onion services](https://gitweb.torproject.org/torspec.git/tree/proposals/224-rend-spec-ng.txt), which has the side-effect of increasing onion address length to 54 characters! This means that in the future onion addresses will look like this:

*   llamanymityx4fi3l6x2gyzmtmgxjyqyorj9qsb5r543izcwymlead.onion
*   lfels7g3rbceenuuqmpsz45z3lswakqf56n5i3bvqhc22d5rrszzwd.onion
*   odmmeotgcfx65l5hn6ejkaruvai222vs7o7tmtllszqk5xbysolfdd.onion

Remembering onions
------------------

Over the years the Tor community has come up with various ways of handling these large and non-human-memorable onion addresses. Some people memorize them entirely or scribe them into secret notebooks, others use tattoos, [third-party centralized directories](https://ahmia.fi/) or just google them everytime. We've heard of people using decks of cards to remember their favorite onion sites, and others who memorize them using the position of stars and the moon. We believe that the UX problem of onion addresses is not actually solved with the above ad-hoc solutions and remains a critical usability barrier that prevents onion services from being used by a wider audience. The onion world never had a system like DNS. Even though we are well aware that DNS is far from the perfect solution, it's clear that human memorable domain names play a fundamental role in the user experience of the Internet. In this blog post we present you a few techniques that we have devised to improve the usability of onion addresses. All of these ideas are experimental and come with various fun open questions, so we are still in exploration mode. We [appreciate any help](https://lists.torproject.org/pipermail/tor-dev) in prototyping, analyzing and finding flaws in these ideas.

 [![](https://extra.torproject.org/blog/2017-03-31-cooking-for-onions-names/onionnames.jpg)](https://extra.torproject.org/blog/2017-03-31-cooking-for-onions-names/onionnames.jpg)



### Idea 1) A modular name system API for Tor onion services


During the past years, many research groups have experimented and designed various secure name systems (e.g. [GNS](https://gnunet.org/gns), [Namecoin](https://namecoin.org/), [Blockstack](https://blockstack.org/)). Each of these systems has its own strengths and weaknesses, as well as different user models and total user experience. We are not sure which one works best for the onion space, so ideally we'd like to **try them all** and let the community and the sands of time decide for us. We believe that by integrating these experimental systems into Tor, we can greatly strengthen and improve the whole scientific field by exposing name systems to the real world and an active and demanding userbase. For this reason and based on our experience [with modular anti-censorship techniques](https://gitweb.torproject.org/torspec.git/tree/pt-spec.txt), we designed a generic & modular scheme through which any name system can be integrated to Tor: [Proposal 279 defines _A Name System API for Tor Onion Services_](https://gitweb.torproject.org/torspec.git/tree/proposals/279-naming-layer-api.txt) which can be used to integrate any complex name system (e.g. Namecoin) or even simple silly naming schemes (e.g. a local /etc/tor-hosts file). Here is a graphical depiction of the _Name System API_ with a _Namecoin_ module enabled and resolving the domain _sailing.tor_ for a user:

[![](https://extra.torproject.org/blog/2017-03-31-cooking-for-onions-names/tor_ns_api.jpg) ](https://extra.torproject.org/blog/2017-03-31-cooking-for-onions-names/tor_ns_api.jpg)

It's worth pointing out that proposal 279 is in draft status and we still need to incorporate feedback [received](https://lists.torproject.org/pipermail/tor-dev/2016-October/011516.html) [in the mailing list](https://lists.torproject.org/pipermail/tor-dev/2017-March/012077.html). Furthermore, [people have pointed out](https://lists.torproject.org/pipermail/tor-dev/2016-October/011517.html) [simple ways through which we can fast-track and prototype the proposal faster](https://lists.torproject.org/pipermail/tor-dev/2017-March/012020.html). Help in implementing this proposal is greatly appreciated (find us [in IRC](https://www.torproject.org/about/contact.html.en#irc)!).

### Idea 2) Using browser extensions to improve usability


Other approaches for improving the usability of onion addresses use the Tor Browser as a framework: think of browser extensions that map human memorable names to onion addresses. There are many variants here so let's walk through them:

##### Idea 2.1) Browser Extension + New pseudo-tld + Local onion registry


A browser extension like [HTTPS-everywhere](https://www.eff.org/https-everywhere), uses an _onion registry_ to map human-memorable addresses from a new pseudo-tld (e.g. ".tor") to onion addresses. For example, it maps "watchtower.tor" to "fixurqfuekpsiqaf.onion" and "globaleconomy.tor" to "froqh6bdgoda6yiz.onion". Such an onion registry could be local (like HTTPS-everywhere) or remote (e.g. a trusted append-only database). Even an extension with a local onion registry would be a very effective improvement to the current situation since it would be pretty usable and its security model is easy to understand: an audited local database seems to work well for HTTPS-everywhere. However, there are social issues here: how would the onion registry be operated and how should name registrations be handled? I can see people fighting for who will get _bitcoin.tor_ first. That said, this idea can be beneficial even with a small onion database (e.g. 50 popular domains). Here is a graphical depiction of a browser extension with a local onion registry resolving the domain _sailing.tor_ for a user: 

[![](https://extra.torproject.org/blog/2017-03-31-cooking-for-onions-names/local_onion_registry.jpg)](https://extra.torproject.org/blog/2017-03-31-cooking-for-onions-names/local_onion_registry.jpg)

##### Idea 2.2) Browser extension + New pseudo-tld + Remote onion registries


A more dynamic alternative here involves multiple _trusted remote onion registries_ that the user can add to their torrc. Imagine a web-of-trust based system where you add your friend's _Alice_ onion registry and then you can visit _facebook_ using _facebook.alice.onion_. A similar more decentralized alternative could be a browser addon that uses multiple _remote onion registries/notaries_ to resolve a name, employing a majority or supermajority rule to decide the resolution results. Such a system could involve notary nodes similar to SSL schemes like [Convergence](https://en.wikipedia.org/wiki/Convergence_(SSL)).

##### Idea 2.3) Browser extension redirects existing DNS names


An easier but less effective approach would be for the browser extension to only map DNS domain names to onion names. So for example, it would map "duckduckgo.com" to "3g2upl4pq6kufc4m.onion". That makes the job of the name registrar easier, but it also heavily restricts users only to services with a registered DNS domain name. [Some attempts have already been made in this area](https://github.com/chris-barry/darkweb-everywhere) but unfortunately they never really took off.

##### Idea 2.4) Automatic Redirection using HTTP


The _[Alt-Svc HTTP header](http://httpwg.org/http-extensions/alt-svc.html)_ defines a way for a website to say "I'm facebook.com but you should talk to me using fbcdn.com." If we replace that _fbcdn.com_ address with _facebookcorewwi.onion_ - then when you typed in Facebook, the browser would, under the covers, use the .onion address. And this can be done without any browser extension whatsoever. One problem is that the browser has to remember this mapping, and in Tor Browser that mapping could be used to track or correlate you. Preloading the mapping would solve this, but how to preload the mapping probably brings us back into the realm of a browser extension.

##### Idea 2.5) Smart browser bookmarks for onion addresses


Talking about random addresses, it's funny how people seem to be pretty happy handling phone numbers (big meaningless random numbers) using a phone book and contacts on their devices. On the same note, an easier but less usable approach would be to enhance Tor Browser with some sort of smart bookmark/[petname system](http://www.skyhunter.com/marcs/petnames/IntroPetNames.html) which allows users to register custom names for onion sites, and allows them to trust them or share them with friends. Unfortunately, it' unclear whether the user experience of this feature would make it useful to anyone but power users. Of course it's important to realize that any approach that relies on a browser extension will only work for the web, and you wouldn't be able to use it for arbitrary TCP services (e.g. visiting an IRC server)

### Idea 3) Embed onion addresses in SSL certificates


So let's shift back to non-browser approaches! [Let's Encrypt](https://letsencrypt.org/) is an innovative project which issues **free** SSL certificates in an **automated** fashion. It has greatly improved Internet security since now anyone can freely acquire an SSL certificate for their service and provide link security to their users. Now let's imagine that Let's Encrypt embedded onion address information into the certificates it issues, for clients with both a normal service and an onion service. For example, the onion address could be embedded into a custom certificate extension or in the _C/ST/L/O_ fields. Then Tor Browser, when visiting such an SSL-enabled website, would parse and validate the certificate and if an onion address is included, the browser would automagically redirect the user. [Take a look at this paper](https://www.nrl.navy.mil/itd/chacs/sites/www.nrl.navy.mil.itd.chacs/files/pdfs/15-1231-0478.pdf) for some more neat ideas on this area.

### Idea 4) Embed onion addresses in DNS/DNSSEC records


A similar approach could use the DNS system instead of the SSL CA system. For example, site owners could add their onion address into their TXT or SRV DNS records and Tor could learn to redirect users to the onion address. Of course this approach only applies to operators that can afford a DNS domain. Oh yeah DNS also has zero security...

Conclusion
----------

As you can see there are many approaches that we should explore to improve usability in this area. Each of them comes with its own tradeoffs and applies to different users, so it's important that we allow users to experiment with various systems and let each community decide which approach works best for them. It's also worth pointing out that some of these approaches are not that hard to implement technically, but they still require lots of effort and community building to really take off and become effective. Involving and pairing with other friendly Internet privacy organizations is essential to achieve our goals. Furthermore, we should think carefully of [unintended usability and security consequences](https://lists.torproject.org/pipermail/tor-dev/2016-October/011516.html) that come with using these systems. For example, _people are not used_ to their browser automagically redirecting them from one domain to another: this can seriously freak people out. It's also not clear _how Tor Browser should handle these special names_ to avoid SSL certificate verification issues and hostname leaks. One thing is for sure: even though onion services are used daily by thousand of people, the random addresses confuse casual users and prevent the ecosystem from maturing and achieving widespread adoption. We hope that this blog post inspires researchers and developers to toy around with naming systems and take the initiative in building and experimenting with the various approaches. Please join the [\[tor-dev\] mailing list](https://lists.torproject.org/pipermail/tor-dev/) and share your thoughts and projects with us!

* * *


And this brings us to the end of this post. Hope you enjoyed this issue of _Cooking With Onions_! We will be back soon, always with the finest produce and the greatest cooking tips! What would you like us to cook next? _


*[Thanks to Philipp Winter and Tom Ritter for the feedback on this blog post, as well as to everyone who has discussed and helped develop these ideas.]*
