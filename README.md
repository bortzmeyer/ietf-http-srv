Using DNS SRV records with HTTP
===============================

These are some notes I keep for a vague and very remote project of
submitting an IETF Internet-Draft on "Using SRV records for HTTP". *Do
not hold your breath* I have many things to do and this specific
project seems very unlikeley to succeed one day. I just store notes in
the mean time.

The reasons
-----------

All Internet protocols but HTTP allow you to separate the domain
(`example.com`) from the server hosting the domain
(`gandalf.dc1.example.com`). They do it with MX (email) but most often
with SRV. The idea is to bring the same power to HTTP.

Avoiding stupid things like
[CNAME DNS records at the apex of a zone](http://serverfault.com/questions/613829/why-cant-a-cname-record-be-used-at-the-apex-aka-root-of-a-domain)
or the
[Cloudflare hack](https://support.cloudflare.com/hc/en-us/articles/200169056-CNAME-Flattening-RFC-compliant-support-for-CNAME-at-the-root).

The issues
----------

* URL with an explicit port: who wins?
* latency issues: SRV and A/AAAA requests will have to be done in
  parallel (happy eyeballs). This does not solve the problem of the second A/AAAA request,
  the one on the target, for which one must wait the response to the
  SRV request. (Except if in the same zone.)
* broken middleboxes
* apparent complete lack of interest from browser vendors
* increase of DNS requests (probably negligible when you think of all
  the DNS requests needed for the simplest Web page). If both names
  are in the same zone, returning more additional
  data may help. (Except for non-existing names, where the client
  cannot know if the server did not send the data or if the data did
  not exist.)
* subtle DNS issues with caching?
* when there are several HTTP connections to the same target, should every SRV lookup be treated separately (default randomization) or should all connections go to the same host?
* transition issues: what should the browsers do during the deployment?
* most current name resolution APIs take a domain name and return an address. No way to make that work with SRV, we need new APIs and patch applications to use them.
* wildcards may, as usual, create problems, since `_something._tcp.*.example.com` is not "wildcarded"

The Mark Andrews draft
----------------------

[draft-andrews-http-srv](https://datatracker.ietf.org/doc/draft-andrews-http-srv/)

* requests the HTTP client to do all connections of a "session" to the same host (not the default working of SRV)
* disable SRV use if there is an explicit port in the URI
* very optimistic about deployment ("within one to two years")

The HTTP/2 project
------------------

[Reasons for rejection of
SRV](http://lists.w3.org/Archives/Public/ietf-http-wg/2015AprJun/0674.html)
(desire to stay compatible with HTTP/1, latency issues and
interoperability)

CNAME and other data
--------------------

Explain in detail why CNAME is not a solution?
([old issue](https://chriswa.wordpress.com/2008/02/01/bind9-vs-cname-rrs/)) Among other things, they are  not protocol-specific so a CNAME "swallows" everything.


