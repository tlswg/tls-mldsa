---
title: "Use of ML-DSA in TLS 1.3"
abbrev: "Use of ML-DSA in TLS 1.3"
category: info

docname: draft-ietf-tls-mldsa-latest
submissiontype: IETF
number:
date:
consensus: true
v: 3
area: Security
workgroup: Transport Layer Security
keyword:
 - ML-DSA
 - FIPS204
venue:
  group: "Transport Layer Security"
  type: "Working Group"
  mail: "tls@ietf.org"
  arch: "https://mailarchive.ietf.org/arch/browse/tls/"
  github: "tlswg/tls-mldsa"
  latest: "https://tlswg.github.io/tls-mldsa/draft-ietf-tls-mldsa.html"

author:
 -
    fullname: "Tim Hollebeek"
    organization: DigiCert
    email: tim.hollebeek@digicert.com
 -
    fullname: "Sophie Schmieg"
    organization: Google
    email: sschmieg@google.com
 -
    ins: B.E. Westerbaan
    fullname: "Bas Westerbaan"
    organization: Cloudflare
    email: bas@cloudflare.com

normative:
 RFC8446:

informative:
 RFC5246:
 TLSIANA: RFC9847
 MLDSACERTS: RFC9881



--- abstract

This memo specifies how the post-quantum signature scheme ML-DSA (FIPS 204)
is used for authentication in TLS 1.3.


--- middle

# Introduction

ML-DSA is a post-quantum module-lattice-based digital signature algorithm
standardised by NIST in {{!FIPS204=DOI.10.6028/NIST.FIPS.204}}.

This memo specifies how ML-DSA can be negotiated for authentication in TLS 1.3
via the `signature_algorithms` and `signature_algorithms_cert` extensions.

# Conventions and Definitions

{::boilerplate bcp14-tagged}

# ML-DSA SignatureScheme Values

As defined in {{RFC8446}}, the SignatureScheme namespace is used for
the negotiation of signature scheme for authentication via the
`signature_algorithms` and `signature_algorithms_cert` extensions.
This document adds three new SignatureScheme values for the three
ML-DSA parameter sets listed in Section 4, Table 1 of {{FIPS204}} as follows.

| SignatureScheme | FIPS 204  | Certificate SPKI AlgorithmIdentifier   |
|-----------------|-----------|----------------------------------------|
| mldsa44(0x0904) | ML-DSA-44 | id-ML-DSA-44 (2.16.840.1.101.3.4.3.17) |
| mldsa65(0x0905) | ML-DSA-65 | id-ML-DSA-65 (2.16.840.1.101.3.4.3.18) |
| mldsa87(0x0906) | ML-DSA-87 | id-ML-DSA-87 (2.16.840.1.101.3.4.3.19) |
{: #schemes title="SignatureSchemes for ML-DSA" }

Note that these are different from the HashML-DSA pre-hashed
variants defined in Section 5.4 of {{FIPS204}},
which are not used here
because of the reasons laid out in {{Section 8.3 of MLDSACERTS}}.

## Certificate Chain
For the purpose of signalling support for signatures on certificates
as per {{Section 4.2.3 of RFC8446}}, these values indicate support
for signing using the given AlgorithmIdentifier shown in {{schemes}}
as defined in {{MLDSACERTS}}.

## Handshake Signature
When one of those SignatureScheme values is used in a CertificateVerify message,
then the signature MUST be computed and verified as specified in
{{Section 4.4.3 of RFC8446}}, using
Algorithm 2 (ML-DSA.Sign) and Algorithm 3 (ML-DSA.Verify)
of {{FIPS204}} respectively. The context (ctx) parameter
MUST be the empty string. Note that the context parameter of FIPS 204
is different from the context string of {{Section 4.4.3 of RFC8446}}.

The corresponding end-entity
certificate MUST use the corresponding AlgorithmIdentifier
from {{schemes}} in its SubjectPublicKeyInfo.

If the signature or public key is of the wrong length, the client MUST
treat this as a verification failure, and thus terminate the handshake
with `decrypt_error` alert.

## TLS 1.2
The schemes defined in this document MUST NOT be used in TLS 1.2 {{RFC5246}}
or earlier versions.
A peer that receives ServerKeyExchange or CertificateVerify message in a TLS
1.2 connection with schemes defined in this document MUST abort the connection
with an `illegal_parameter` alert.

# Security Considerations

The security considerations of {{RFC8446}} (eg. {{Appendices C.2 and E.1 of RFC8446}}
and {{Section 4.4.3 of RFC8446}}) and {{FIPS204}} (Section 3.4 and 3.6) apply.


# IANA Considerations

This document requests new entries to the TLS SignatureScheme registry,
according to the procedures in {{Section 6 of TLSIANA}}.

| Value   | Description | Recommended | Reference      |
|---------|-------------|-------------|----------------|
| 0x0904  | mldsa44     | N           | This document. |
| 0x0905  | mldsa65     | N           | This document. |
| 0x0906  | mldsa87     | N           | This document. |

--- back

# Acknowledgments
{:numbered="false"}

Thanks to
    Alicja Kario,
    John Mattsson,
    Rebecca Guthrie,
    Alexander Bokovoy,
    Niklas Block,
    Ryan Appel,
    Loganaden Velvindron,
    Rob Sayre,
    Daniel Van Geest,
    Martin Thomson,
    Wang Guilin,
    and Nick Sullivan
    for their review and feedback.
