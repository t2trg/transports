notes from 2019-05-03 group call

Bill, Klaus, Ines, Christian

Ch: Where are the old ones? this repository.
 
Christian: didn't do homework
Bil: looked into OSCORE for security context, and RFC7800 (PoP for JWTs) so host can assert its transports. Not sure if OSCORE is suitable, b/c end-to-end, whereas this is about secure method of asserting which addresses are "mine". (CA: OSCORE only symmetric)
Ines: checked SMS in OMA. Last file found is from 2015, they gave proposal to use SMS w/ HTTP and defined input and output resources. Use JSON and urlencode formats. Send SMS to check status and push and poll modes. There's a IETF CoAP-over-SMS draft from 2017, with scenarios for mobile-only, IP-only and IP-and-mobile. In OMA, they define basic resources to allow the client to poll for inbound messages, subscribe to inbound messages. Can use delivery status. Also define HTTP actions. OMA defined restful API.
Bill: good starting point for reviving CoAP-over-SMS.
Ines: OMA did not include CoAP, only HTTP -- no work ongoing on that. Newest status is from 2015.
Bill: trying to get feedback from OMA ppl to check whether they actually implement that. When I read OMA 1.1, it talked about CoAP and SMS, but need to check again.
Ch: Are the IETF work and the OMA work related?
Ines: IETF draft mentions that OMA did something there, but does not reference the full work there.
Bill: not sure about origin; affiliation was w/ a university
K: Draft came from Bremen but was used then by OMA. Sounds like they use it without having a copy of how it's working, without text we could take back into the draft.
Ines: Will ask Hannes

Bil: what's there on NB-IoT?
Ch: not sure whether using a pre-set-up channel over NB-IoT with static header compression, or CoAP-over-NB directly, or whether it makes a difference at all.
K: from Prague remember: Finish CoAP-over-SMS in CoRE, and do experimentation in T2TRG
Bil, Ch: agree
K: For SMS, get status from OMA. Then, try to contact editors of SMS to see how to move. IIRC, draft was already looking good except request-response-transport-splitting, if that's removed looks good. Bil: agree, core is good
Bil: URI format; nontrad. resposnes to T2TRG
Ines: SMS draft pointed to alternative-transports for URIs, passed ball for URI registraion on.

Ines: defining next tasks?
→ Ines ask Hannes on OMA SMS use, contact editors of SMS draft (check whether authors have moved)
Bil: Klaus, where to find your slides at the pre-IETF-104 meeting regarding CoRAL/-reef? 
→ K: if not uploaded, should do; will send link.
→ Bil, Ch: Alt-Svc

Bil: draft from Carsten over CoAP over 802.15 networks that expired; referred to in OSCORE (Ch: <https://tools.ietf.org/html/draft-bormann-6lo-coap-802-15-ie-00>)

Bil: still not sure on model, keep illusion of all-separate-servers or assert that there is one server with single crypo identity.
Ch: not sure if need to pick sides, could have non-URI-aliasing scheme sent along and then allow server to prove aliasing. ("Avoiding URI aliasing: can be done by sending Proxy-Scheme and Uri-Host, allowing optimization ("Just call me 2001:db8...") in later doc / later in the negotiation when talking to authority?")

K: github.com/T2TRG/transports -- collect ideas there in the wiki or commit markdown

Next meeting?

* W3C WoT meeting 3-5th of June in Munich
* just doodle for end of May? maybe 31st?
* Montreal? Bil won't be there
* RIOT summit? 5/6 Sept, maybe day before (*Wed*, Thu, Fri)

