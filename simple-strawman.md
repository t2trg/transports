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

The host component of the URI indicates a DNS name under which services are announced using DNS-SD like (for `coap+sd://host.example.com`)
```
_coap+sd._udp.host.example.com IN PTR wifitcp._coap+sd._udp.host.example.com
wifitcp._coap+sd._udp.host.example.com IN SRV 0 0 5683 wifi.host.example.com
wifitcp._coap+sd._udp.host.example.com IN TXT txtver=1;path=;scheme=coap+tcp
```
which, following the mapping of [RD-DNS-SD],
can be produced by registering a link like
`<coaps+tcp://wifi.host.example.com>;rel=proxy-transport; ins=wifitcp;st=coap+sd`
on a resource directory
(where first half of the statement is what the device would announce per the first section anyway).

Dereferencing a `coap+sd` URI can not be dereferenced directly,
but only usign a proxy.
In particular, using the one at the proxy-transport address, which is typically the same device.

A client that obtains a coap+sd PTR/SRV/TXT for a host name `A`
can infer from those not only the target attributes indicated by the RD-DNS mapping,
but additionaly treats it as a link from `coap+sd://A` to the indicated link target.
The client should use the priority and weight information encoded in the SRV records if there is more than one,
even though that information is not available in the link interpretation of the records.

[ This is currently described in terms of DNS-SD.
Were there a way to indicate a scheme in a SRV record,
a `host.example.com IN SRV` could be used just as well.
A [URI record] may be used as well --
that is best figured out with people more familiar with DNS. ]

[RD-DNS-SD]: https://tools.ietf.org/html/draft-ietf-core-rd-dns-sd-05
[URI record]: https://tools.ietf.org/html/rfc7553

`coap+hash`
-----------

The [rdlink] draft describes a `coap+hash` scheme,
URIs of which can not be dereferened in general either.
In the structure of its host component,
that scheme indicates how a host can provde possession of that name.

With that scheme, links of rel `transport-proxy` are discovered
either by using a global resource directory
or by multicast querying for `/.well-known/core?anchor=coap+scheme://...`.

[rdlink]: https://tools.ietf.org/html/draft-amsuess-t2trg-rdlink-00
