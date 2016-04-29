Using DNS SRV records with HTTP
===============================

These are some notes I keep for a vague et very remote project of
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
  parallel (happy eyeballs)
* broken middleboxes
* apparent complete lack of interest from browser vendors
* increase of DNS requests (probably negligible when you think of all
  the DNS requests needed for the simplest Web page)
* subtle DNS issues with caching?
  
The Mark Andrews draft
----------------------

[draft-andrews-http-srv](https://datatracker.ietf.org/doc/draft-andrews-http-srv/)

TODO: read it

The HTTP/2 project
------------------

[Reasons for rejection of
SRV](http://lists.w3.org/Archives/Public/ietf-http-wg/2015AprJun/0674.html)
(desire to stay compatible with HTTP/1, latency issues and
interoperability)

CNAME and other data
--------------------

Explain in detail why CNAME is not a solution?
([old issue](https://chriswa.wordpress.com/2008/02/01/bind9-vs-cname-rrs/))


