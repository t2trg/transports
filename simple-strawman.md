This describes a simple possible way to provide alternative transport negotiation
losely based on HTTP's Alt-Svc header ([RFC8738]);
it is not intended as a roadmap
but as a straw man text to hone our understanding of the problem.

Alternative Services for CoAP
=============================

We define a link relation type `transport-proxy`,
which is used with plain-origin URIs (cf. [16]).

A link from A to B with that rel means that when accessing resources on A,
it can be efficient to use B as a proxy.
That is, a resource on A can be accessed by sending a request to B
with a Proxy-Scheme option of A's scheme and a's host in the Uri-Host.

A client that discovers such a link MUST only use it
if it has reasonable assurances that B is under control of and valid for all of A
(as is required for Alt-Svc in [RFC8738]).

(This will probably need a bit widening up:
An OSCORE connection doesn't demonstrate that B is under the control of A
as it may just forward the messages and analyze the patterns,
but then again neither is an h2 connection,
for the attacker could have set up a TCP tunnel and monitor the same way).

With this mechanism,
alternative transports can be annoucned the same way on regular service discovery
and in a resource directory (even without using the endpoint lookup interface).

For example, a CoAP server may answer like this:

```
GET coaps://node.example.com/.well-known/core

2.05 Content
Content-Format: application/link-format
Payload:
<coaps+tcp://node-alt.example.com>;rel=transport-proxy
```

(The different name was chosen purely to be able to tell identifiers apart,
as it would be unclear in the example where the name comes from)

The client can then, in case it wants to use CoAP over TLS,
start a connection like this (pardon my simplified TLS):

```
Client: Server name indicsated is node.example.com
Server: sends a certificate indicating the name node.example.com

GET /some/path
Proxy-Scheme: coaps
Uri-Host: absent (as within this connection, the default value for Uri-Host is node.example.com)
...
```

[16]: https://github.com/t2trg/transports/issues/16
[RFC8738]: https://tools.ietf.org/html/rfc7838

Transport-Agnostic schemes
==========================

Based on the terminology established above,
we can easily define CoAP schemes that do not indicate a particular transport,
but a particular way of transport discovery.

For example, one could describe:

`coap+sd`
---------

A new URI scheme `coap+sd` is defined.
It uses the same URI structure as the other CoAP schemes,
but gives particular meaning to the authority component.
This scheme does not use port assignments.

The host component of the URI indicates a DNS name under which services are announced using DNS-SD,
following the mapping being established in [RD-DNS-SD] using a particular to-be-defined service name.

Dereferencing a `coap+sd` URI can not be dereferenced directly,
but only usign a proxy --
typically indicated in using a `transport-proxy` rel.

A client that obtains a DNS-SD record with that service name for a host name A
interprets it not only by the target attributes indicated by the RD-DNS mapping,
but additionaly treats it as a link from `coap+sd://A` to the indicated link target.
