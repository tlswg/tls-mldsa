---
title: "Use of ML-DSA in TLS"
abbrev: "Use of ML-DSA in TLS"
category: info

docname: draft-tls-westerbaan-mldsa-latest
submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
number:
date:
consensus: true
v: 3
# area: AREA
# workgroup: WG Working Group
keyword:
 - ML-DSA
 - FIPS204
venue:
#  group: WG
#  type: Working Group
#  mail: WG@example.com
#  arch: https://example.com/WG
  github: "bwesterb/tls-mldsa"
  latest: "https://bwesterb.github.io/tls-mldsa/draft-tls-westerbaan-mldsa.html"

author:
 -
    fullname: "Tim Hollebeek"
    organization: DigiCert
    email: tim.hollebeek@digicert.com
 -
    fullname: "Bas Westerbaan"
    organization: Cloudflare
    email: bas@cloudflare.com

normative:

informative:
 TLSIANA: I-D.ietf-tls-rfc8447bis
 I-D.ietf-lamps-dilithium-certificates:



--- abstract

This memo specifies how the post-quantum signature scheme ML-DSA (FIPS 204)
is used for authentication in TLS.


--- middle

# Introduction

ML-DSA {{!FIPS204=DOI.10.6028/NIST.FIPS.204}} is one of the first
two post-quantum signature schemes standardised by NIST. It is a
module-lattice based scheme.

This memo specifies how ML-DSA can be negotiated for authentication in TLS
via the "signature_algorithms" extension.

# Conventions and Definitions

{::boilerplate bcp14-tagged}

# ML-DSA SignatureSchemes Types
As defined in {{RFC8446}}, the SignatureScheme namespace is used for
the negotiation of signature scheme for authentication via the
"signature_algorithms" extension. This document adds three new SignatureSchemes
types for the three ML-DSA parameter sets as follows.

~~~
enum {
  mldsa44(0x0900),
  mldsa65(0x0901),
  mldsa87(0x0902)
} SignatureScheme;
~~~

These correspond to ML-DSA-44, ML-DSA-64, and ML-DSA-87 defined
in {{FIPS204}} respectively. Note that these should not be confused
with prehashed variants such as HashML-DSA-44 alsodefined in {{FIPS204}}:
we use the pure version.

The corresponding end-entity certificate when negotiated must
use id-ML-DSA-44, id-ML-DSA-64, id-ML-DSA-87 respectively as
defined in {{I-D.ietf-lamps-dilithium-certificates}}.

# Security Considerations

TODO Security


# IANA Considerations

This document requests a new entry to the TLS SignatureSchemes registry,
according to the procedures in {{Section 6 of TLSIANA}}.

| Value           | Description | Recommended | Reference      |
|-----------------|-------------|-------------|----------------|
| 0x0900 (please) | mldsa44     | Y           | This document. |
| 0x0901 (please) | mldsa65     | Y           | This document. |
| 0x0902 (please) | mldsa87     | Y           | This document. |

--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
