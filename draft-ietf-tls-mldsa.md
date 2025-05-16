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
  arch: https://example.com/WG
  arch: "https://mailarchive.ietf.org/arch/browse/tls/"
  github: "bwesterb/tls-mldsa"
  latest: "https://bwesterb.github.io/tls-mldsa/draft-ietf-tls-mldsa.html"

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
    fullname: "Bas Westerbaan"
    organization: Cloudflare
    email: bas@cloudflare.com

normative:

informative:
 RFC5246:
 RFC8446:
 TLSIANA: I-D.ietf-tls-rfc8447bis
 I-D.ietf-lamps-dilithium-certificates:



--- abstract

This memo specifies how the post-quantum signature scheme ML-DSA (FIPS 204)
is used for authentication in TLS 1.3.


--- middle

# Introduction

ML-DSA is a post-quantum module-lattice based digital signature algorothm
standardised by NIST in {{!FIPS204=DOI.10.6028/NIST.FIPS.204}}.

This memo specifies how ML-DSA can be negotiated for authentication in TLS 1.3
via the "signature_algorithms" and "signature_algorithms_cert" extensions.

# Conventions and Definitions

{::boilerplate bcp14-tagged}

# ML-DSA SignatureSchemes Types
As defined in {{RFC8446}}, the SignatureScheme namespace is used for
the negotiation of signature scheme for authentication via the
"signature_algorithms" and "signature_algorithms_cert" extensions.
This document adds three new SignatureSchemes
types for the three ML-DSA parameter sets as follows.

~~~
enum {
  mldsa44(0x0904),
  mldsa65(0x0905),
  mldsa87(0x0906)
} SignatureScheme;
~~~

These correspond to ML-DSA-44, ML-DSA-65, and ML-DSA-87 defined
in {{FIPS204}} respectively. Note that these are different
from the HashML-DSA pre-hashed variantsadefined in Section 5.4 of {{FIPS204}}.

The signature MUST be computed and verified as specified in
{{Section 4.4.3 of RFC8446}}.

The context parameter defined in {{FIPS204}} Algorithm 2 and 3
MUST be the empty string.

The corresponding end-entity certificate when negotiated MUST
use id-ML-DSA-44, id-ML-DSA-65, id-ML-DSA-87 respectively as
defined in {{I-D.ietf-lamps-dilithium-certificates}}.

The schemes defined in this document MUST NOT be used in TLS 1.2 {{RFC5246}}.
A peer that receives ServerKeyExchange or CertificateVerify message in a TLS
1.2 connection with schemes defined in this document MUST abort the connection
with an illegal_parameter alert.

# Security Considerations

TODO Security


# IANA Considerations

This document requests new entries to the TLS SignatureScheme registry,
according to the procedures in {{Section 6 of TLSIANA}}.

| Value           | Description | Recommended | Reference      |
|-----------------|-------------|-------------|----------------|
| 0x0904 (please) | mldsa44     | N           | This document. |
| 0x0905 (please) | mldsa65     | N           | This document. |
| 0x0906 (please) | mldsa87     | N           | This document. |

--- back

# Acknowledgments
{:numbered="false"}

Thanks to Alicja Kario, John Mattsson, and Rebecca Guthrie
    for their review and feedback.
