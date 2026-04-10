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
 RFC9846:
 RFC9881:

informative:
 RFC9847:




--- abstract

This memo specifies how the post-quantum signature scheme ML-DSA (FIPS 204)
is used for authentication in TLS 1.3.


--- middle

# Introduction

The Module-Lattice-Based Digital Signature Algorithm (ML-DSA) is a
post-quantum digital signature algorithm
standardised by the US National Institute of Standards and Technology (NIST)
in {{!FIPS204=DOI.10.6028/NIST.FIPS.204}}.

This memo specifies how ML-DSA can be negotiated for authentication in TLS 1.3
via the `signature_algorithms` and `signature_algorithms_cert` extensions.

# Conventions and Definitions

{::boilerplate bcp14-tagged}

# ML-DSA SignatureScheme Values

As defined in {{RFC9846}}, the SignatureScheme namespace is used for
the negotiation of signature scheme for authentication via the
`signature_algorithms` and `signature_algorithms_cert` extensions.
This document maps three new SignatureScheme values for the three
ML-DSA parameter sets listed in Section 4, Table 1 of {{FIPS204}}
to the SignatureAlgorithmIdentifiers from {{RFC9881}} as follows.

| SignatureScheme | FIPS 204  | Signature AlgorithmIdentifier   |
|-----------------|-----------|----------------------------------------|
| mldsa44(0x0904) | ML-DSA-44 | id-ML-DSA-44 (2.16.840.1.101.3.4.3.17) |
| mldsa65(0x0905) | ML-DSA-65 | id-ML-DSA-65 (2.16.840.1.101.3.4.3.18) |
| mldsa87(0x0906) | ML-DSA-87 | id-ML-DSA-87 (2.16.840.1.101.3.4.3.19) |
{: #schemes title="SignatureSchemes for ML-DSA" }

Note that these are different from the HashML-DSA pre-hashed
variants defined in Section 5.4 of {{FIPS204}},
which are not used here
because of the reasons laid out in {{Section 8.3 of RFC9881}}.

## Certificate Chain
For the purpose of signalling support for signatures on certificates
as per {{Section 4.3.3 of RFC9846}}, these values indicate support
for signing using the given AlgorithmIdentifier shown in {{schemes}}
as defined in {{RFC9881}}.

## Handshake Signature
When one of those SignatureScheme values is used in a CertificateVerify message,
then the signature MUST be computed and verified as specified in
{{Section 4.5.2 of RFC9846}}, using
Algorithm 2 (ML-DSA.Sign) and Algorithm 3 (ML-DSA.Verify)
of {{FIPS204}} respectively. The context (ctx) parameter
MUST be the empty string. Note that the context parameter of FIPS 204
is different from the context string of {{Section 4.5.2 of RFC9846}}.

The corresponding end-entity
certificate MUST use the corresponding AlgorithmIdentifier
from {{schemes}} in its SubjectPublicKeyInfo.

# Security Considerations

The security considerations described in {{Appendices C.2 and F.1 of RFC9846}}
and {{Section 4.5.2 of RFC9846}} apply. In particular, signature-based modes of
TLS depend on the signature scheme being secure against chosen message
attacks {{?SIGMA=DOI.10.1007/978-3-540-45146-4_24}}. Per Section 3.1 of
{{FIPS204}}, ML-DSA is designed to meet this property.

Implementation failures, such as side channels, in cryptographic primitives can
also compromise the primitive and thus a TLS connection depending on it.
Sections 3.4 and 3.6 of {{FIPS204}} discuss additional considerations for
implementing ML-DSA, including guidance on the choice of hedged vs deterministic
variants. These considerations apply when ML-DSA is used for TLS.


# IANA Considerations

This document requests new entries to the TLS SignatureScheme registry,
according to the procedures in {{Section 6 of RFC9847}}.

| Value   | Description | Recommended | Reference      |
|---------|-------------|-------------|----------------|
| 0x0904  | mldsa44     | N           | This document. |
| 0x0905  | mldsa65     | N           | This document. |
| 0x0906  | mldsa87     | N           | This document. |

As defined in {{Section 3 of RFC9847}}, the value N indicates

> That the item has not been evaluated by the IETF and
> that the IETF has made no statement about the suitability of
> the associated mechanism. This does not necessarily mean that
> the mechanism is flawed, only that no consensus exists. The
> IETF might have consensus to leave an item marked as "N" on the
> basis of the item having limited applicability or usage constraints

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
    David Benjamin,
    Viktor Dukhovni,
    Daniel Van Geest,
    Martin Thomson,
    Wang Guilin,
    Muhammad Usama Sardar,
    and Nick Sullivan
    for their review and feedback.
