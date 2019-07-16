* How visible can an alternative name be?
  * "redirect": no aliasing (just application-level statement)
  * ... something in-between?
  * next-hop proxying: no aliasing (but works badly with (D)TLS)

  * not sending Uri-Host header is an old and unsolved version of this problem

* Alt-Svc
  * Purely per-origin
  * Insists not to be visible above the access mechanism
    * Do not change the origin, no aliasing, just routes, "as if a CNAME were being used"
  * With TLS, original name needs to be present in the certificate
  * Mandatory explicit freshness / lifetime
    * We could use Max-Age there, as we probably do this over a dedicate (link-format) document like we do other HTTP headers
  * Indication of whether an alternative is viable generally (persist=1) or only in this network configuration
  * Alt-Used header for loop prevention
  * Attack scenario: user controlling /~user/ can redirect to :8080
  * Requires certificate pinning, if done, extends to alternative services
  * Recommend explicitly carrying the protocol to avoid servers getting confused on what the expected security properties are
    * That'd be similar to us sending a Proxy-Scheme option along

  * What's different in CoRE?
    * Not everybody is using domain-based trust models (LwM2M just uses task-based trust model), but that just makes those people largely unaffected by all this.
    * We already expose what they call the ALPN name (first member of the tuple, the more specific protocol with potentially options) as independent URIs
    * We don't do everything over TLS ("h2c" aka "HTTP/2 over TCP only" registered, but can't really be used)
    * Alternative services are optional -- in CoRE we can probably not provide that, given there are already pairs of endpoints that can't talk to each other
    * We'll advertise a lot from third parties (RDs, possibly even arbitrary links)
    * We'll want to pack it into links
