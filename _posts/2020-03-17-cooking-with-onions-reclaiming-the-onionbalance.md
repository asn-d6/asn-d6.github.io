---
layout: post
title: "Cooking With Onions: Reclaiming the Onionbalance"
---
*[Post originally appeared on the [Tor blog](https://blog.torproject.org/cooking-onions-reclaiming-onionbalance)]*

{:refdef: style="text-align: center;"}
[![](https://raw.githubusercontent.com/asn-d6/onionbalance/master/obv3_logo.jpg)](https://raw.githubusercontent.com/asn-d6/onionbalance/master/obv3_logo.jpg)
{: refdef}

Welcome to another issue of _[Cooking with Onions](https://blog.torproject.org/category/tags/cooking-onions)_!

[Onionbalance](https://blog.torproject.org/cooking-onions-finding-onionbalance) is one of the standard ways onion service administrators can load balance onion services, but it didn't work for v3 onions. Until now. **We just [released a new version](https://gitlab.torproject.org/asn/onionbalance) of Onionbalance that supports v3 onion services.**

The core functionality remains the same: Onionbalance allows onion service operators to achieve the property of _high availability_ by allowing multiple machines to handle requests for an onion service.

If you are already familiar with configuring Tor onion services, setting up Onionbalance is simple. Check the precise setup instructions on our [documentation page](https://onionbalance-v3.readthedocs.io/en/latest/v3/tutorial-v3.html#tutorial-v3).

What took you so long?
----------------------

We deployed [next generation v3 onions](https://blog.torproject.org/tors-fall-harvest-next-generation-onion-services") two years ago, but we just added v3 support for Onionbalance. This took a while...

Introducing v3 functionality to Onionbalance wasn't easy. The v3 protocol is more secure and robust than v2, but those benefits come with a more complicated protocol. That made Onionbalance integration harder.

To implement v3 support for Onionbalance, we had to jump [through various](https://trac.torproject.org/projects/tor/ticket/29583) [hoops](https://trac.torproject.org/projects/tor/ticket/29583), and in some cases, we had to take the more complicated scenic route.

In particular, some of the [advanced v3 crypto](https://trac.torproject.org/projects/tor/ticket/32709) was not allowing us to pull the classic Onionbalance trick of making a user think they are going to the frontend service but in reality visiting a backend service. To work around this, we were forced to make the OnionBalance setup procedure a bit more complicated: **operators now need to explicitly configure their backend services to act as Onionbalance instances via the torrc.**

Finally, as part of our work on Onionbalance v3, we also had to implement [](https://stem.torproject.org/api/descriptor/hidden_service.html#stem.descriptor.hidden_service.HiddenServiceDescriptorV3) [v3 descriptor support](https://stem.torproject.org/api/descriptor/hidden_service.html#stem.descriptor.hidden_service.HiddenServiceDescriptorV3) for [Stem](https://stem.torproject.org), a Python controller library for Tor.

What's next?
------------

We're still missing Onionbalance packages for Debian and pip. We want to conduct more tests before replacing the existing packages with the new Onionbalance, so we expect these to be ready in April. The current version of Onionbalance is 0.1.9, and our plan is to create the packages when 0.2.0 is released.

Additionally, these features are still to come:

*   Support for v3 ["distinct descriptor" mode](https://onionbalance-v3.readthedocs.io/en/latest/v2/design.html#choice-of-introduction-points). This mode allows Onionbalance v2 to load-balance more than 10 backend instances, whereas currently Onionbalance v3 has a limit of 8 backend instances. In theory, Onionbalance could load-balance hundreds of backend instances by publishing descriptors at small time intervals that contain introduction points from a different subset of those instances each time.
*   Minimize the differences between both v3 and other descriptors. Currently Onionbalance v3 descriptors can look different from other descriptors, which makes it possible for clients and HSDirs to learn that a service is using Onionbalance. This can be an issue for more [advanced onion service threat models](https://github.com/mikeperry-tor/vanguards/blob/master/README_SECURITY.md#how-to-onionbalance). 
*   Enable client authorization on the frontend service. This may be needed in specialized use cases. Adding this feature would first require implementing client authorization support to Stem v3 descriptors and then using that feature in Onionbalance.
*   Allow the ability to transfer your existing v3 onion service to Onionbalance.

Right now, any sort of testing and feedback would be really helpful for us. Also, if you are in the mood for coding or fixing bugs, it would be great if you could provide patches for any of the above missing features or any other feature you might need: We also have a [GitHub mirror](https://github.com/asn-d6/onionbalance) for people who feel more comfortable there.

We hope this new flavor gives the transition to v3 onion services a boost. And now that Onionbalance is back in active development, maybe you'll join us in kitchen and write some code?

Until next time! :)
