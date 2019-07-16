(Mini charter proposal for T2TRG/Transports to be discussed)

CoAP today works over UDP, DTLS, TCP, TLS and Websockets. CoAP messages can be conveyed over other alternative transports, both IP-based (such as QUIC) and non-IP (such as SMS). The group will attempt to reach the following goals:

1. The group will define transport mappings for the usage of CoAP over SMS. Compatibility with CoAP over SMS as defined in OMA LWM2M will be considered. As a starting point, draft-becker-core-coap-sms-gprs-06 can be used.

2. The group would also look into defining an appropriate URI format consisting of a single URI scheme “coap+at”, that can be used for all future transports (including SMS). The structure and format of the new URI format must conform to the generic syntax, URI resolution rules and URI design described in RFC 3986 and RFC 7320. Care must also be taken to avoid URI aliasing (see https://www.w3.org/TR/webarch/#uri-aliases). A more ambitious URI design can aim towards the usage of a URI scheme "rest" that could represent both CoAP and HTTP resources, similar to the "ocf://" URIs.

3. One (or more) suitable mechanisms to express the availability of multiple transports at a CoAP-based origin server. The solution must allow CoAP nodes to discover the available transports offered by a CoAP endpoint directly, as well as with the aid of an intermediary such as a Resource Directory. When multiple transports are offered for communication by an endpoint, the URI schemes and authority components for each transport may differ. Therefore the endpoint should be able to attest and prove ownership over the different URIs.
