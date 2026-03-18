
CRLSets ([background](https://www.imperialviolet.org/2012/02/05/crlsets.html)) are primarily a means by which Chrome can quickly block certificates in emergency situations. As a secondary function they can also contain some number of non-emergency revocations. These latter revocations are obtained by crawling CRLs published by CAs.

Online (i.e. [[Online Certificate Status Protocol|OCSP]] and [[Certificate revocation list|CRL]] ) checks are not, generally, performed by Chrome. They can be enabled in the options and, in some cases, the underlying system certificate library always performs these checks no matter what Chromium does. Otherwise they are only performed when verifying an EV certificate that is not covered by a fresh CRLSet.

The Chromium source code that implements CRLSets is, of course, [public](https://chromium.googlesource.com/chromium/src/+/master/net/cert/crl_set_storage.cc). But the process by which they are generated is not.

We maintain an internal list of crawled CRLs. The CRLs from that set go to make up the published CRLSet. For size reasons, the list doesn't include all CRLs - EV CRLs and CRLs with good reason codes are taken in preference. CRLs which cover intermediates are typically small and valuable so we try to take as many as possible.

Several CAs have requested that their CRLs be handled in confidence so the full list isn't public. An individual CA can email agl at chromium dot org is they wish to know which of their CRLs are included, or to add CRLs.

CRLs on the list are fetched infrequently (at most once every few hours) and verified against the correct signing certificate for that CRL. (The signing certs are included in the static config.)

Entries in the CRL are filtered by reason code. Only entries that have no reason code, or one of the following reason codes are included in the CRLSet: Unspecified, KeyCompromise, CACompromise or AACompromise.

The limit of the CRLSet size is 250KB. In the event that something causes a CRL to bloat significantly and pushes the CRLSet over the size limit, we are alerted and will typically drop the CRL from the list and notify the CA.

The current CRLSet can be fetched and dumped out using the code at

[https://github.com/agl/crlset-tools](https://github.com/agl/crlset-tools).

CRLSets can (and do) include non-public CRLs - i.e. CRLs that are not pointed to by any certificate but still exist if one knows the URL.

Any CA is welcome to contact agl at chromium dot org if they have questions.