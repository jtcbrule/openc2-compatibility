
# Introduction

OpenC2 is an evolving standard for computer and network security with continued debate over language semantics. Development and adoption is somewhat hampered by the following "chicken and egg problem": ideally, the OpenC2 forum will converge on language and protocols that work well in practice, but implementers need to agree on language and protocols to build interoperable systems --- to see what works well in practice.

This repository is an attempt to bootstrap the process by capturing core design principles and suggesting implementation guidelines. In the short term, it is meant to act as an evolving reference guide to OpenC2 prototype implementers. The long term goal is to adapt the information in this repository into a white paper that will serve as an official OpenC2 standard document.

Currently, this repository is divided into the following sections:

- [semantics.md](semantics.md) - the conceptual model of OpenC2 ("What does the language mean?")
- [syntax.md](syntax.md) - concrete representations of OpenC2 messages ("What does the language look like 'on the wire'?")
- [messaging.md](communication.md) - OpenC2 communication protocols ("How do OpenC2 system components talk to each other?")

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED",  "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC 2119](https://tools.ietf.org/html/rfc2119).

This work is licensed under a [Creative Commons Attribution 4.0 International License](http://creativecommons.org/licenses/by/4.0/).

