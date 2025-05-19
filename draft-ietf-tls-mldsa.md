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
workgroup: Transport Layer Security§
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

informative:
 RFC5246:
 RFC8446:
 TLSIANA: I-D.ietf-tls-rfc8447bis
 MLDSACERTS: I-D.ietf-lamps-dilithium-certificates



--- abstract

This memo specifies how the post-quantum signature scheme ML-DSA (FIPS 204)
is used for authentication in TLS 1.3.


--- middle

# Introduction

ML-DSA is a post-quantum module-lattice based digital signature algorothm
standardised by NIST in {{!FIPS204=DOI.10.6028/NIST.FIPS.204}}.

This memo specifies how ML-DSA can be negotiated for authentication in TLS 1.3
via the `signature_algorithms` and `signature_algorithms_cert` extensions.

# Conventions and Definitions

{::boilerplate bcp14-tagged}

# ML-DSA SignatureScheme values

As defined in {{RFC8446}}, the SignatureScheme namespace is used for
the negotiation of signature scheme for authentication via the
`signature_algorithms` and `signature_algorithms_cert` extensions.
This document adds three new SignatureScheme values for the three
ML-DSA parameter sets from {{FIPS204}} as follows.

| SignatureScheme | FIPS 204  | Certificate AlgorithmIdentifier |
|-----------------|-----------|---------------------------------|
| mldsa44(0x0904) | ML-DSA-44 | id-ML-DSA-44                    |
| mldsa65(0x0905) | ML-DSA-65 | id-ML-DSA-64                    |
| mldsa87(0x0906) | ML-DSA-87 | id-ML-DSA-87                    |
{: #schemes title="SignatureSchemes for ML-DSA" }

Note that these are different from the HashML-DSA pre-hashed
variants defined in Section 5.4 of {{FIPS204}}.

## Certificate chain
For the purpose of signalling support for signatures on certificates
as per {{Section 4.2.4 of RFC8446}}, these values indicate support
for signing using the given AlgorithmIdentifier shown in {{schemes}}
as defined in {{MLDSACERTS}}.

## Handshake signature
When one of those SignatureScheme values is used in a CertificateVerify message,
then the signature MUST be computed and verified as specified in
{{Section 4.4.3 of RFC8446}}, and the corresponding end-entity
certificate MUST use the corresponding AlgorithmIdentifier from {{schemes}}.

The context parameter defined in {{FIPS204}} Algorithm 2 and 3
MUST be the empty string.

## TLS 1.2
The schemes defined in this document MUST NOT be used in TLS 1.2 {{RFC5246}}.
A peer that receives ServerKeyExchange or CertificateVerify message in a TLS
1.2 connection with schemes defined in this document MUST abort the connection
with an illegal_parameter alert.

# Security Considerations

TODO Security


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

Thanks to Alicja Kario, John Mattsson, Rebecca Guthrie, Alexander Bokovoy,
    and Niklas Block
    for their review and feedback.
