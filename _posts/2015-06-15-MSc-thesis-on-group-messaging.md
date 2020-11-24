---
layout: post
title: "My MSc thesis on group messaging and group key exchange protocols"
---
*(Post originally appeared on the [[messaging] mailing list](https://moderncrypto.org/mail-archive/messaging/2015/001743.html))*

I just finished my MSc thesis which you can find [here](https://github.com/asn-d6/thesis-rhul).

It all started as an attempt to design a secure messaging protocol, but because
of the nature of my degree it became more theoretical and cryptographic so I
ended up looking at group key exchange protocols and provable security.

Still, the document contains some information about group messaging and of
previous work on this space. However the content is incomplete and a bit
outdated; if you are looking for a robust survey on messaging protocols I would
suggest [the recent paper by Bonneau et al.](http://cacr.uwaterloo.ca/techreports/2015/cacr2015-02.pdf)

We also present a key integrity attack on the group key exchange protocol from
the paper "Flexible group key exchange with on-demand computation of subgroup
keys". That's the paper that the (n+1)sec key exchange protocol is based on,
however the (n+1)sec protocol itself is *not* affected.

Finally, we present a toy new key exchange protocol based on a paper by Bresson
et al., slightly modified to make it nicer, and then prove it secure using a
sequence-of-games proof. Finally, in Appendix B, there is some *research-level*
simulation source code of the "Deniable Group Key Agreement" by Bohli et al.

It's worth mentioning that halfway into writing my thesis, I got convinced that
key exchanges might not be the best way to do cryptography in the group
messaging setting. Discussions with Trevor and Ximin helped me realize that
pairwise crypto and some kind of group key (à la sender-keys) might be the way
forward since it's more asynchronous, making it more useful for mobile
platforms.

That said, it was a fun thesis to write and I learned lots about provable
security. I believe that key exchange security models and proof techniques are
still very useful for demonstrating security even on pairwise sender-key type
of constructions.

Big thanks to my supervisor Kenny Paterson and to the various friends that
talked group messaging with me over the past years!

