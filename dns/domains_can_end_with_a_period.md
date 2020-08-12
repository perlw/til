# Domains can end with a period

Domains can end with a simple period. This simply makes it a FQDN, relative to
the root zone (an absolute name). This is all according to spec, but, browsers
(and other software as well) have for a long time simply assumed the domain to
be equivalent (foo.com==foo.com.).

This is not usually a direct issue when we are browsing the web, however is
something that must be kept in mind if writing systems that check validity of or
use the Host-header to do internal product lookups.

---

References:
* [Archive: No, that dot in the domain name of the URL is not a mistake.](https://web.archive.org/web/20120103091907/http://homepages.tesco.net/J.deBoynePollard/FGA/web-fully-qualified-domain-name.html)
