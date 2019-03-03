---
layout: post
title: "Announcing the Vanguards Add-On for Onion Services"
---
*[Post originally appeared on the [Tor blog](https://blog.torproject.org/announcing-vanguards-add-onion-services)]*

An Intro to Onion Service Security
==================================

Earlier this year, the Tor Project released its first stable Tor and Tor Browser releases with [the new v3 onion service protocol](https://blog.torproject.org/tors-fall-harvest-next-generation-onion-services). The protocol features many improvements, including longer and more secure onion addresses, service enumeration resistance, improved authentication, and upgraded cryptography.

However, while this new protocol closes off some attacks (particularly enumeration and related targeted DoS attacks), it does not solve any attacks that could lead to service deanonymization.

We believe that the most serious threat that v3 onion services currently face is _guard discovery_. A guard discovery attack enables an adversary to determine the guard node(s) that are in use by a Tor client and/or Tor onion service. Once the guard node is known, traffic analysis attacks that can deanonymize an onion service (or onion service user) become easier.

The most basic form of this attack is to make many connections to a Tor onion service, in order to force it to create circuits until one of the adversary's nodes is chosen for the middle hop next to the guard. That is possible because middle hops for rendezvous circuits are picked from the set of all relays:

[![](https://raw.githubusercontent.com/asn-d6/vanguard_simulator/illustrations/illustrations/current_system.jpg)](https://raw.githubusercontent.com/asn-d6/vanguard_simulator/illustrations/illustrations/current_system.jpg)

A traffic analysis side channel can be used to confirm that the malicious node is in fact part of the rendezvous circuit, leading to the discovery of that onion service's guard node. From that point, the guard node can be compromised, coerced, or surveilled to determine the actual IP address of the onion service or client.

The Vanguards Control Port Add-On
=================================

Fixing the guard discovery problem in Tor itself is an immense project -- primarily because it involves many trade-offs between performance and scalability versus path security, which makes it very hard to pick good defaults for every onion service.

Because of this, we [have created an add-on](https://github.com/mikeperry-tor/vanguards) that can be used in conjunction with a Tor onion service server or a Tor client that accesses Tor onion services.

The add-on uses [our Control Port Protocol](https://gitweb.torproject.org/torspec.git/tree/control-spec.txt) and the corresponding [Stem Library](https://stem.torproject.org/) to defend against these attacks. The hope is that it will we will be able to study the performance and functionality of this feature and gather feedback before we deploy these changes in Tor for all onion services and clients.

Vanguards, Bandguards, and a Rendguard, oh my!
==============================================

Our add-on has three components:

### Vanguards

The core functionality is provided by the **Vanguards** **component** which implements the [Mesh Vanguards (Proposal 292)](https://gitweb.torproject.org/torspec.git/tree/proposals/292-mesh-vanguards.txt). This ensures that all onion service circuits are restricted to a set of second and third layer guards, which have randomized rotation times as defined in that proposal. Basically, now all the hops of onion service circuits are pinned to specific nodes instead of sampling random ones from the whole network every time.

This change to fixed nodes for the second and third layer guards is designed to force the adversary to have to run many more nodes, and to execute both an active sybil attack, as well as a node compromise attack. In particular, the addition of second layer guard nodes means that the adversary goes from being able to discover your guard in minutes by running just one middle node, to requiring them to sustain the attack for weeks or even months, even if they run 5% of the network.

The analysis behind our choice for the number of guards at each layer, and for rotation duration parameters is [available on GitHub](https://github.com/asn-d6/vanguard_simulator/wiki/Optimizing-vanguard-topologies). Here is how our current vanguard _2-3-8_ topology looks like:

[![](https://raw.githubusercontent.com/asn-d6/vanguard_simulator/illustrations/illustrations/vanguard_system.jpg)](https://raw.githubusercontent.com/asn-d6/vanguard_simulator/illustrations/illustrations/vanguard_system.jpg)

Furthermore, to better protect the identity of these new pinned guard nodes the circuit lengths have been altered for rendezous point circuits, hidden service directory circuits, and introduction point circuits. You can see them here (where L1 is the first layer guard, L2 is second layer guard, L3 is third layer guard, M is random middle): 

[![](https://raw.githubusercontent.com/asn-d6/vanguard_simulator/illustrations/illustrations/new_paths.jpg)](https://raw.githubusercontent.com/asn-d6/vanguard_simulator/illustrations/illustrations/new_paths.jpg)

### Bandguards

Additionally, the **Bandguards component** of the add-on also checks for evidence of bandwidth side channel attacks, which may be used by the adversary to aid/amplify traffic analysis attacks.

When these attacks are detected, the circuit is (optionally) closed.

Note that the Bandguards component also closes any circuit older than 24 hours (the `circ_max_age_hours` setting), and has an option (off-by-default) to close circuits that transmit more than a certain number of megabytes (the `circ_max_megabytes` option).

**If your service requires large file uploads, or very long-lived circuits, set these options to 0 in your [vanguards.conf](https://github.com/mikeperry-tor/vanguards/blob/master/vanguards-example.conf#L68)**.

### Rendguards

Finally, the **Rendguards component** of the add-on performs analysis on the prevalence of rendezvous points on the onion service side. The rendezvous point is chosen by the client when it connects to an onion service, and some attacks rely on the use of a malicious rendezvous point to aid in traffic analysis.

This component tracks the frequency of rendezvous point use, and when it finds overuse, it optionally closes circuits from that rendezvous point and emits a log message.

Each of these components is **configurable**. Please see [the README](https://github.com/mikeperry-tor/vanguards/blob/master/README.md) for more information.

Requirements, Installation, Usage, and Caveats
==============================================

The Vanguards add-on is primarily for high-risk onion service operators at this point. In order for the Bandguards side channel detection features to be enabled, Tor 0.3.4.4 or above is required, but the script will run with Tor 0.3.3.x+. Earlier Tors do not have sufficient Control Port support for the script, however.

Additionally, while they have been thoroughly tested by us, the parameters for the various detection mechanisms of the Bandguards and Rendgaurd components are still experimental and may need fine tuning for your service or scenario, especially if it differs from our testing environment.

**_If you notice log messages or alarms from these components, it does not necessarily mean that you are under attack._** If you can, please report frequent log messages to the [GitHub issue tracker](https://github.com/mikeperry-tor/vanguards/issues).

Thank you and let us know if you have any questions or concerns! :)
