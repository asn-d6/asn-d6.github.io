---
layout: post
title: "Flute: A simple secure multiparty messaging system"
---
*[Post [originally](https://moderncrypto.org/mail-archive/messaging/2016/002181.html) [appeared](https://moderncrypto.org/mail-archive/messaging/2016/002196.html) on the messaging mailing list]*

Hello messaging wizards,

I'd like to present you a first version of Flute: a secure multiparty messaging
system. It has been a side-project for the past month, and I'm glad to finally
push it out to the world :)

You can find the code repository here: [https://github.com/asn-d6/flute](https://github.com/asn-d6/flute)

### The flute protocol

You can find the protocol spec here:
  [https://github.com/asn-d6/flute/blob/master/flute_spec.txt](https://github.com/asn-d6/flute/blob/master/flute_spec.txt)

The primary aim of the flute protocol has been to offer the basic security
properties while keeping the protocol easy to understand, analyze and
implement. Essentially, everytime I was faced with a design decision I picked
the simplest design possible. At its present form, I consider it an educational
and experimentation tool for multiparty messaging, but it also has potential to
grow into a powerful messaging system.

The current protocol offers end-to-end confidentiality of messages as well as
entity authentication. It does not offer security properties like deniability
or transcript/room consistency yet.

The flute protocol does not use a (multiparty) key exchange protocol to
generate group encryption keys. Instead the flute protocol uses a room captain
and a key transport protocol to ship encryption keys to the room
participants. For more information, please read the protocol spec.

### The flute codebase:

I decided there would not be much benefit in designing yet another multiparty
messaging protocol without first building it to see how well it works.

So I implemented a prototype of Flute for use over IRC with Weechat. I used
Python and the Weechat python bindings which allowed me to prototype pretty
quickly.

The current code is far from user-ready and should not be used for anything
sensitive. The weechat UX still sucks, and there are dozens of XXXs sprinkled
around the code. However the basic functionality seems to work fine and that's
what's important at this stage.

### Future steps:

I don't have much spare time these days, so I don't know how much time I will
have for Flute. Now that I managed to publish the first version of the protocol
I'm going to take a step back and wait for feedback and suggestions before
acting further.

My secret hope is that people will find interest in this project and will
submit patches, bug fixes and UX improvements. If you want to help out but you
are not sure how, please read TODO.txt or check all those XXXs in the codebase;
some are dead simple to solve, some are harder. Also, testing Flute yourself
for a few minutes should give you plenty of ideas on things to improve on the UX.

On the protocol side, if you take a minute and understand the flute spec, you
will realize that it's actually a quite simple protocol with great potential
for improvements in every step. If you have a flute improvement in mind, I
invite you to hack the spec and the code, and actually implement and test the
improvement yourself. If you find a great improvement that works, please let me
know!

I also hope that more people will help with breaking the protocol, and
conducting missing security analysis. For example, please read the "Security
discussion" section in the spec and all the XXXs.

Furthermore, the current flute codebase works over IRC powered by weechat and
Python. However, I actually don't think that Python or Weechat or IRC are
necessarily the best tools for this sort of protocol. I'm definitely open to
alternative implementations, or extending flute to more chat clients and
frameworks.

Feel free to use github for any flute patches that I should look at, or use
this mailing list thread for any design suggestions you might have. Also, if
you are interested in helping, but you are missing required documentation or
help to get started, let me know as well.

###  Testing flute:

If you find this project interesting, please give Flute a try.  Read the README
file to get started with testing flute.

You can test flute on real IRC networks or just setup your own testing IRC
server and test it out offline. I've been using hybrid-ircd for local testing
which only takes 20 seconds of setup time. You can also create weechat aliases
that allow you to join the server, and start flute with just one command.

Finally, if you follow the README instructions you will end up in the
#flute_test IRC channel of baconsvin. I have parked a room captain there that
will accept any room joins, so feel free to join that flute room for testing.

### Acknowledgments

As always, Flute would not be here if it was not for the celo crew: joe, ahf and infinity0.

* * *

Have fun playing with Flute and please provide feedback!!!
