---
layout: post
title: "Flattor: A practical crowdfunded Flattr-like incentive scheme for Tor relays"
---
*(Post originally appeared on the [[tor-talk] mailing list](https://lists.torproject.org/pipermail/tor-talk/2013-August/029419.html))*

## tl;dr

Where will Tor's bandwidth come from in 20 years? Will solo volunteers
still exist, or will all the bandwidth come from Tor-friendly
organizations?

Tor incentive schemes are interesting. There are many proposed schemes
but their crypto needs to be reviewed and lots of code/spec needs to
be written before they can be deployed.

This document describes the idea of a Flattr-like crowdfunding model
for tor relays.

## Intro (skip if you know why Tor incentive schemes might be useful)

One of the goals of Tor is to increase its reach and get tens of
millions of Tor users. This makes sense from an anonymity point of
view, since an increase on the number of users is also an increase on
Tor's anonymity set.

One of the problems of scaling Tor to tens of millions of users is
that Tor's bandwidth capacity is finite. The [current total relay](https://metrics.torproject.org/network.html#bandwidth)
bandwidth is about 4GB/s, and it's donated by [kind volunteers and
various organizations](https://metrics.torproject.org/bubbles.html#contact). As the number of users increases, Tor's
bandwidth must also increase.

Lately the bandwidth coming out of Tor-friendly organizations (like
torservers.net, universities, etc.) seems to increase. Currently,
there is 50% chance of exiting from an org-controlled exit node, as
can be seen in slide 30 of http://freehaven.net/~arma/slides-dimacs13.pdf

If this trend continues, Tor might end up looking like the Bitcoin
network -- where a number of organizations (mining pools) drive the
network. Unfortunately, in contrast with the coin minting of Bitcoin,
there is no incentives for organizations to contribute to the Tor
network.

At the moment, organizations and solo volunteers pay out of their own
pocket (or accept donations) to maintain their Tor relays. There must
be ways to help reimburse their costs.

## Incentive schemes (feel free to skip if you know this stuff)

Incentive schemes for anonymous networks have been extensively
researched and [there are](http://www-users.cs.umn.edu/~jansen/papers/lira-ndss2013.pdf) [multiple papers](https://blog.torproject.org/blog/two-incentive-designs-tor) for systems that apply
specifically to Tor [2].

Most of those systems require to modify the code of the Tor network,
to be able to give "contribution tokens" to relay operators. Then
those tokens can be exchanged to get "premium Tor service" or other
goods.

These systems have a few issues that make them hard to implement and
deploy:
* Baking anything inside Tor is a lengthy procedure. Secure designs and code must be written, time must pass for the new code to be deployed and used by the majority of the network, etc.
* These schemes might cause anonymity issues, since the set of people who have "contribution tokens" is smaller than the set of Tor users. The proposed incentive schemes try to fix these issues; for example, LIRA solves it by creating a lottery system (yes, on top of Tor) that rewards "contribution tokens" to random relays.
* Alternative currencies, like "contribution tokens", are not easy to get right. Baking them inside the Tor network is not a trivial task

While these proposed "complex" schemes might be The Right Thing for
the long-term, we might be able to create an incentive scheme based on
already existing technologies; like the bandwidth authorities and
Bitcoin.

## Flattor: A simple incentive scheme

Flattor is (fictional) software (or a website) that given a Bitcoin
wallet and a number of Bitcoins that the user is willing to spend,
splits those Bitcoins in chunks and sends them to contributing relay
operators.

The idea assumes that there is some way to find the Bitcoin address of
relay operators. There is no standard way to do so, but operators who
are interested in getting reimbursed can put their address in the
Contact field of their relay.

Flattor uses the bandwidth estimations of Tor to find the contribution
factor of each relay. We will assume that these estimates are
accurate, since the security of Tor depends on them anyway.

As a simplified example, if the Tor network has 4 relays with
bandwidth contribution 0.05, 0.05, 0.3 and 0.6 respectively, and the
user is willing to spend 1 bitcoin, Flattor will send 0.05, 0.05, 0.3
and 0.6 bitcoins to each relay operator respectively.

Of course, this gets more complicated as the number of relays
increases, or when only a subset of the relays have a registered
bitcoin address, etc.

# Why this might be a good idea

It's simple to implement and easy to understand, it doesn't require
any Tor code to be written and it can even be started as an unofficial
project.

Furthermore, it doesn't cause anonymity issues, there are no
middlemen, and it doesn't centralize bandwidth to a single relay
operator.

# Why this might be a bad idea

(This concern is based on a discussion with gmaxwell.)

Incentivising Tor relay operators with money is not a good way to run
an anonymity network

Currently, (we want to believe that) the Tor network is run by a bunch
of cypherpunks that are contributing bandwidth because they believe in
the Cause.

If relay operators start getting money for their bandwidth, we might
end up with relay operators that are just in for the money. It might
then be easier for a three-letter org to persuade those relay
operators to snoop on their users (by giving them double the money
they are currently getting).

While I agree that this concern is legitimate, I would say that it's
pretty far off at the moment: I doubt that anything like Flattor will
ever generate a considerable income for anyone. Still, it's something
that we should have in mind.

(Furthermore, since the Bitcoin blockchain is public you can see how
much money each relay operator has gotten so far. Maybe there should
be some kind of limit on the number of money each operator should get
per time period.)

# Final thoughts

There are many details that must be sorted out before Flattr can be
implemented. There are also multiple improvements that can be applied
on top of the simple model described above. Also, there are ethical
issues that spawn up when real money is given to relay operators.

My plan was to expand on all these issues in this paragraph, but it
seems like I've already spent too many hours writing this document.

I'm not planning to implement this system before I hear some opinions
from other people. To be honest, I'm not even sure if such an
incentive scheme is a good idea, but posting bad ideas to mailing
lists is what the Internet is for, right?

Thoughts?

