# Protocol negotiation hallway meeting at IETF104

Scheduled for 2019-03-27 (Wednesday) 12:30 in the code lounge


# protocol-negotiation

Proposal from <https://tools.ietf.org/html/draft-amsuess-t2trg-rdlink-00>:

       This defines the "alternate-transport" Link Relation Type.


       A link from a context resource to a target resource typed with the

       "alternative-transport" type declares that for any relative refernce

       of the path-noscheme or path-empty form (see [RFC3986] Section 4.2.),

       the reference's resolution with the context as a Base URI can be

       substituted with the reference's resolution with the target as a Base

       URI.


       The expression "can be substituted with" means that for every REST

       operation conducted on the original resource, the same operation on

       the new resource will give equivalent responses and have equivalent

       side effects.

       

       Applications interpreting "alternative-transport" links need to

       carefully consider their trust model: They MUST either have obtained

       the statement from a source that is trusted to speak for the context

       authority, or make additional demands on the target when connecting

       to it (e.g. ask the target to identify as the context authority).


       [ If applications are defined for both CoAP and HTTP, and advertised

       the same way, hosts can onlly advertise alternatives if cross-

       proxying is possble; needs good generic phrasing. ]


       This link relation is roughly equivalent to the "at" RD parameter

       introduced in [I-D.silverajan-core-coap-protocol-negotiation], but

       suitable for multicast discovery.


* Alternative-Transport option: go away

* 2 Phases: Announced transports, and enableable transports

* Phase 1: just consider phase 2 to not break it.

Easier now that we have good security scheme

Resource collection core.pn as entry point to bunch of addresses that describe aliases
document format could even be used with PATCH and FETCH


Goal of BS for this meeting:
    * go through document
    * what stays
    * what must change
    * what is way forward

Issues:
    * URI aliasing and security (referenced document at W3C)
    * Expressing the URI aliases
    * Distributing the expressions
    
How is OSCORE helpful here?
    
 homework:
     * Look at Alt-Svc for aliasing → CA, IR
     * Look at what OMA is doing with SMS → IR
     * Summarize discussion → BS
     * all: Have agenda and slides for both the participants and WGs
     
group call for the people involved: ~Easter? May 3rd 11:00 CEST

(→CA: consider "faster" path to solve restricted small problems)

# coap+at

"required reading"
* https://tools.ietf.org/html/rfc3986 Generic URI Syntax, esp Section 3.2
* https://tools.ietf.org/html/rfc4395 New URI Schemes, esp. Section 2
  "discourage use of the same URI scheme for different purposes"; can purpose be "execute REST operations on" or "execute CoAP operations on"?
* tangentially? https://tools.ietf.org/html/rfc7320 URI Design and Ownership

Protocols to keep in mind
* coap[s]{,+tcp,+ws}
* coap+sms
* slipmux https://tools.ietf.org/html/draft-bormann-t2trg-slipmux-02
* coap over the (a?) single point-to-point link provided by NB-IoT
* out of scope?: names for role-reversal addresses in connection-oriented protocols

Limitations to consider
* Some implementations might barf at authority components that are not DNS valid (not confirmed)
  * coap+sms://+4312345678/ might have run into that (the + could be percent encoded and still conform to authority/host/reg-name, might even be legal in DNS (it's pretty lax AFAIR) but DNS library impls disliked it, or so I heard)
* Some implementations might take names that look like *any* IP address expression as an IP address (confirmed on Firefox 66 and Chromium 73)
  * try http://3244527696/

relevant drafts, anything to start from?

Components
* scheme: "coap+at" seems pretty set
* authority component: port? IP literals?
* path and later: Use from CoAP. (Also use .well-known/* from CoAP)

Questions:
    * For the well-definedness of RFC4395 Section 2.3, are these locators? Then they'd need to have a clearly defined mechanism of location.

ideas general setup:
    * Authority component "can be anything" as long as the system can resolve it by any means
    * Do we need a "way out" of DNS?
      * SMS doesn't need one, there's e164.arpa
      * authority component for slipmux? 

strawman dns-based for the simple cases:
    
    Given coap+at://some.name/path, one way of resolving is this:
        * 
