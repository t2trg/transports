Meetin point: https://meet.jit.si/T2TRG-Transports

Agenda for today:

* 1. mini charter review and comments

    Bill drafted mini-charter - https://github.com/t2trg/transports/blob/master/mini-charter.md 

    Klaus: really good.

    Ines: Can we assign tasks/issues to the items of that text? Bill: yes. Split into smaller parts and track as issues. Github labels. Klaus: Will help track assignments.

* 2. updates (Christian, Bill, Ines and Klaus)

    Klaus summarizing what Jaime said about users of CoAP-over-SMS


    Christian: looked into Alt-Svc [1] and prepared a summary inside [2]


    Some work already in Christian's https://tools.ietf.org/html/draft-amsuess-t2trg-rdlink-00#section-3


    Bill: contacting Michael about OCF URIs


    Bill: keep things simple (discovery mechanism at origin server would be good idea, but not worked out whether it'll be one resource to provide information, or resource collection with a set of links discovered by client browsing together with a signature on that). Proof of ownership of targets is hard. Christian: that sounds hard, and may be needless if Alt-Svc route is taken.

    Bill: looked at resource collections. ad FETCH/PATCH on them: would be useful, anything started yet? Christian: not yet AFAIK. Bill: PATCH has few content formats. Do web links in PATCH need a new content format? Christian: open question in CoRE. See https://github.com/core-wg/wiki/wiki/On-media-types-for-FETCH-and-(i)PATCH


    Christian: General agreement to transport alternative transports in Web Links? Bill: sounded good as starting point, currently done that way. Agree on model, not necessarily serialization (CoRAL? link-format?)


    Ines:


    Bill: Three items. SMS wil be single draft. Others in one? Klaus: SMS very specific, we can revive existing draft in CoRE and align with OMA. For rest, not at point to writer drafts, get out some of the ideas, write markdown and decide how to put into drafts later. Created several labels.


    Christian on charter: Do we want to state that advertising possible-but-not-active transports enabling and disabling transports is out of scope? Bill: Could start with only active transports.

    Klaus on charter: In which WG is this done? Was discussed but not in text. When that's in, run it by CoRE and T2TRG chairs. Bill: iterate a day or two on charter, then contact them. Klaus: Could be done in WISHI in afternoon (1600 CEST). For that, make mini-charter ready. https://github.com/t2trg/wishi/wiki/Agenda-items



* 3. Next meetings: Montreal (no Bill), RIOT summit (Christian, Ines), Singapore (no Christian)
    around 2019-08-07 (before CoRE? but only IETF recovery done, no work on transports done)? August 14th 10:00 CEST proposed, but send doodle.

* 4. Way forward
    
    
Actions to work on:

* 1- Set up the github wiki
* 2- Contact authors of SMS draft https://github.com/t2trg/transports/issues/3
* 3- Set up issue tracker to map out agenda -- https://github.com/t2trg/transports/issues/2
* 4- Find out how ICE fits in with transports-- https://github.com/t2trg/transports/issues/1
* 5- Contacting Michael Koster about OCF URIs

[1]: https://tools.ietf.org/html/rfc7838#section-3
[2]: https://github.com/t2trg/transports/blob/master/meeting-20190716/chrysn-preparation.md
