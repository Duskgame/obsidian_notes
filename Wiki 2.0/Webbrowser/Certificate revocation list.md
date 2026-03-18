---
aliases:
  - " "
  - CRL
---
In cryptography, a certificate revocation list (CRL) is "a list of digital certificates that have been revoked by the issuing certificate authority (CA) before their scheduled expiration date and should no longer be trusted".

Publicly trusted CAs in the Web PKI are required (including by the CA/Browser forum) to issue CRLs for their certificates, and they widely do.

Browsers and other relying parties might use CRLs, or might use alternate certificate revocation technologies (such as OCSP) or CRLSets (a dataset derived from CRLs) to check certificate revocation status. Note that OCSP is falling out of favor due to privacy and performance concerns, resulting in a return to CRLs.

Subscribers and other parties can also use ARI.