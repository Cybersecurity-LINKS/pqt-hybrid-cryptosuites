---
layout: respec
title: Post-Quantum / Traditional hybrid cryptosuite v0.1
respec: >
  {
    "name": "your-spec-short-name",
    "status": "CG-DRAFT",
    "latest": "https://transmute-industries.github.io/respec-github-pages/spec/latest",
    "repository": "https://github.com/transmute-industries/respec-github-pages",
    "issues": "https://github.com/transmute-industries/respec-github-pages/issues",
    "group": {
      "name": "Credentials Community Group",
      "url": "https://www.w3.org/community/credentials/",
      "list": "public-credentials",
      "patentUri": "https://www.w3.org/community/about/agreements/cla/"
    },
    "editors": [
      {
        "name": "Andrea Vesco",
        "url": "www.linkedin.com/in/avesco",
        "company": "LINKS Foundation",
        "companyURL": ""
      }
      {
        "name": "Alessio Claudio",
        "url": "https://www.linkedin.com/in/alessio-claudio-ab260923b",
        "company": "LINKS Foundation",
        "companyURL": ""
      }
    ],
    "bibliography": {
      "RDF-DATASET-NORMALIZATION": {
        "title": "RDF Dataset Normalization 1.0",
        "href": "http://json-ld.github.io/normalization/spec/",
        "authors": ["David Longley", "Manu Sporny"],
        "status": "CGDRAFT",
        "publisher": "JSON-LD Community Group"
      }
    }
  }
---

<section id="abstract">
  <p>
    Your specification abstract goes here.
  </p>
</section>

<section id="sotd">
  <p>
    This is an experimental specification and is undergoing regular
    revisions. It is not fit for production deployment.
  </p>
</section>

<section id="terminology">
  <h2>Terminology</h2>
  <p>
    Your specification terminology goes here.
  </p>
</section>

<style>
.red43 {
  color: red;
}
</style>

## Status of This Document

This specification was published by the Cybersecurity Research Group @ LINKS Foundation. 

This specification is experimental, do not use it in any production setting.

[GitHub Issues](https://github.com/Cybersecurity-LINKS/pq-cryptosuites/issues) are preferred for discussion of this specification.


## Introduction

This specification defines several cryptographic suites for the purpose of creating, and verifying proofs for Post-Quantum / Traditional Hybrid signatures in conformance with the Data Integrity [VC-DATA-INTEGRITY](https://w3c.github.io/vc-data-integrity/) specification.

This specification uses either the RDF Dataset Canonicalization Algorithm [RDF-CANON](https://www.w3.org/TR/rdf-canon/) or the JSON Canonicalization Scheme [RFC8785](https://www.rfc-editor.org/rfc/rfc8785) to transform the input document into its canonical form. 

<p class="red43"> **TBU** It uses SHA-256 [RFC6234] as the message digest algorithm and ML-DSA-44 as the signature algorithm.</p>

### Terminology

Terminology used throughout this document is defined in the [Terminology](https://www.w3.org/TR/vc-data-integrity/#terminology) section of the [Verifiable Credential Data Integrity 1.0](https://www.w3.org/TR/vc-data-integrity/) specification.


## Data Model

The following sections outline the data model that is used by this specification to express verification methods, such as cryptographic PQ/T hybrid public keys, and data integrity hybrid PQ/T hybrid proofs, such as PQ/T hybrid digital signatures.

### Verification Methods

This cryptographic suite is used to verify Data Integrity Proofs [VC-DATA-INTEGRITY] produced using <span style="color:red">TBU Edwards Curve cryptographic key</span> material. 

<span style="color:red">
JSON Web Key [RFC7517]
The encoding formats for those key types are provided in this section.
This suite MAY be used to verify Data Integrity Proofs [VC-DATA-INTEGRITY] produced by BLS12-381 public key material encoded as a JsonWebKey.
</span>

 Lossless cryptographic key transformation processes that result in equivalent cryptographic key material MAY be used for the processing of digital signatures.


This is a set of examples of public keys encoded as a JsonWebKey, the examples are instrumental to introduce public keys encoded as CompositeJwk 

[//]: # (JWK RFC 7517 - crv P-256,P-384,P-521)
[//]: # (JWK with Ed RFC 8037 - OKP Octet Key Pair - crv Ed25519, Ed448)
[//]: # (Dove abbiamo preso JWK strcture for ML-DSA?)

EXAMPLE 1a: A P-256 public key encoded as a JsonWebKey 
```json
{
  "id": "https://example.com/issuer/123#key-0",
  "type": "JsonWebKey",
  "controller": "https://example.com/issuer/123",
  "publicKeyJwk": {
      "kid": "key-0",
      "kty": "EC",
      "crv": "P-256",
      "x": "",
      "y": ""
  }
}
```

EXAMPLE 1b: A P-384 public key encoded as a JsonWebKey 
```json
{
  "id": "https://example.com/issuer/123#key-0",
  "type": "JsonWebKey",
  "controller": "https://example.com/issuer/123",
  "publicKeyJwk": {
      "kid": "key-0",
      "kty": "EC",
      "crv": "P-384",
      "x": "",
      "y": ""
  }
}
```

EXAMPLE 2a: An Ed25519 public key encoded as a JsonWebKey 
```json
{
  "id": "https://example.com/issuer/123#key-0",
  "type": "JsonWebKey",
  "controller": "https://example.com/issuer/123",
  "publicKeyJwk": {
      "kid": "key-0",
      "kty": "OKP", 
      "crv": "Ed25519",
      "x": ""
  }
}
```

EXAMPLE 2b: An Ed448 public key encoded as a JsonWebKey 
```json
{
  "id": "https://example.com/issuer/123#key-0",
  "type": "JsonWebKey",
  "controller": "https://example.com/issuer/123",
  "publicKeyJwk": {
      "kid": "key-0",
      "kty": "OKP", 
      "crv": "Ed448",
      "x": ""
  }
}
```

EXAMPLE 3: A ML-DSA-44 public key encoded as a JsonWebKey
```json
{
  "id": "https://example.com/issuer/123#key-0",
  "type": "JsonWebKey",
  "controller": "https://example.com/issuer/123",
  "publicKeyJwk": {
    "kid": "key-0",
    "kty": "ML-DSA",
    "alg": "ML-DSA-44",
    "pub": ""
  }
}
```

EXAMPLE 4: A ML-DSA-65 public key encoded as a JsonWebKey
```json
{
  "id": "https://example.com/issuer/123#key-0",
  "type": "JsonWebKey",
  "controller": "https://example.com/issuer/123",
  "publicKeyJwk": {
    "kid": "key-0",
    "kty": "ML-DSA",
    "alg": "ML-DSA-65",
    "pub": ""
  }
}
```

EXAMPLE 5: A ML-DSA-87 public key encoded as a JsonWebKey
```json
{
  "id": "https://example.com/issuer/123#key-0",
  "type": "JsonWebKey",
  "controller": "https://example.com/issuer/123",
  "publicKeyJwk": {
    "kid": "key-0",
    "kty": "ML-DSA",
    "alg": "ML-DSA-87",
    "pub": ""
  }
}
```


https://datatracker.ietf.org/doc/draft-ietf-lamps-pq-composite-sigs/04/

Pure ML-DSA Composite

id-MLDSA44-ECDSA-P256
id-MLDSA44-Ed25519
id-MLDSA65-ECDSA-P256
id-MLDSA65-ECDSA-P384
id-MLDSA65-Ed25519 
id-MLDSA87-ECDSA-P384
id-MLDSA87-Ed448  

Hashed ML-DSA Composite

id-HashMLDSA44-ECDSA-P256-SHA256
id-HashMLDSA44-Ed25519-SHA512 
id-HashMLDSA65-ECDSA-P256-SHA512
id-HashMLDSA65-ECDSA-P384-SHA512 
id-HashMLDSA65-Ed25519-SHA512 
id-HashMLDSA87-ECDSA-P384-SHA512 
id-HashMLDSA87-Ed448-SHA512

#### CompositeJWK
A public key encoded as a CompositeJWK contains a PQ public key and a traditional public key both encoded as JsonWebKey. In addition, the CompositeJWK contains a string **algId** which represents the name of the algorithms used to generate the hybrid proof and the hash algorithm used to pre-hash the document.

EXAMPLE 6: A ML-DSA-44/P-256 composite public key encoded as a CompositeJWK
```json
{
 "id": "https://example.com/issuer/123#key-0",
 "type": "CompositeJWK",
 "controller": "https://example.com/issuer/123",
 "compositeJwk": {
    "algId": "id-MLDSA44-P256",
    "pqPublicKey": {
      "kid": "key-0",
      "kty": "ML-DSA",
      "alg": "ML-DSA-44",
      "pub": ""
    },
    "traditionalPublicKey": {
        "kid": "key-0",
        "kty": "EC",
        "crv": "P-256",
        "x": "",
        "y": ""
    }
  }
}
```

EXAMPLE 7: A ML-DSA-65/P-256 public key encoded as a CompositeJWK
```json
{
 "id": "https://example.com/issuer/123#key-0",
 "type": "CompositeJWK",
 "controller": "https://example.com/issuer/123",
 "compositeJwk": {
    "algId": "id-MLDSA65-P256",
    "pqPublicKey": {
      "kid": "key-0",
      "kty": "ML-DSA",
      "alg": "ML-DSA-65",
      "pub": ""
    },
    "traditionalPublicKey": {
      "kid": "key-0",
      "kty": "EC",
      "crv": "P-256",
      "x": "",
      "y": ""
    }
  }
}
```

EXAMPLE 8: A ML-DSA-87/P384 composite public key encoded as a CompositeJWK
```json
{
 "id": "https://example.com/issuer/123#key-0",
 "type": "CompositeJWK",
 "controller": "https://example.com/issuer/123",
 "compositeJwk": {
  "algId": "id-MLDSA87-P384",
  "pqPublicKey": {
    "kid": "key-0",
    "kty": "ML-DSA",
    "alg": "ML-DSA-87",
    "pub": ""
  },
  "traditionalPublicKey": {
      "kid": "key-0",
      "kty": "EC",
      "crv": "P-384",
      "x": "",
      "y": ""
  }
}
}
```

EXAMPLE 9: A ML-DSA-44/Ed25519 composite public key encoded as a CompositeJWK
```json
{
 "id": "https://example.com/issuer/123#key-0",
 "type": "CompositeJWK",
 "controller": "https://example.com/issuer/123",
 "compositeJwk": {
    "algId": "id-MLDSA44-ED25519",
    "pqPublicKey": {
      "kid": "key-0",
      "kty": "ML-DSA",
      "alg": "ML-DSA-44",
      "pub": ""
    },
    "traditionalPublicKey": {
      "kid": "key-0",
      "kty": "OKP", 
      "crv": "Ed25519",
      "x": ""
    }
  }
}
```

EXAMPLE 10: A ML-DSA-65/Ed25519 public key encoded as a CompositeJWK
```json
{
 "id": "https://example.com/issuer/123#key-0",
 "type": "CompositeJWK",
 "controller": "https://example.com/issuer/123",
 "compositeJwk": {
    "algId": "id-MLDSA65-ED25519",
    "pqPublicKey": {
      "kid": "key-0",
      "kty": "ML-DSA",
      "alg": "ML-DSA-65",
      "pub": ""
    },
    "traditionalPublicKey": {
      "kid": "key-0",
      "kty": "OKP", 
      "crv": "Ed25519",
      "x": ""
    }
  }
}
```

EXAMPLE 11: A ML-DSA-87/Ed448 composite public key encoded as a CompositeJWK
```json
{
 "id": "https://example.com/issuer/123#key-0",
 "type": "CompositeJWK",
 "controller": "https://example.com/issuer/123",
 "compositeJwk": {
  "algId": "id-MLDSA87-ED448",
  "pqPublicKey": {
    "kid": "key-0",
    "kty": "ML-DSA",
    "alg": "ML-DSA-87",
    "pub": ""
  },
  "traditionalPublicKey": {
      "kid": "key-0",
      "kty": "OKP", 
      "crv": "Ed448",
      "x": ""
  }
}
}
```

EXAMPLE 12: A ML-DSA-65/Ed25519 public key encoded as a CompositeJWK for assertion method, and a ML-DSA-44/Ed25519 public key encoded as a CompositeJWK for authentication method in a controller document

```json
{
  "@context": [
    "https://www.w3.org/ns/did/v1",
    "https://w3id.org/security/jwk/v1"
  ],
  "id": "did:example:123",
  "verificationMethod": [{
    "id": "did:example:123#key-0",
    "type": "CompositeJWK",
    "controller": "did:example:123",
    "compositeJwk": {
      "algId": "id-MLDSA65-Ed25519-xxx",
      "pqPublicKey": {
        "kid": "key-0", (SERVE?)
        "kty": "ML-DSA",
        "alg": "ML-DSA-65",
        "pub": ""
      },
      "traditionalPublicKey": {
        "kid": "key-0", (SERVE?)
        "kty": "OKP", 
        "crv": "Ed25519",
        "x": ""
      }
  }{
    "id": "did:example:123#key-1",
    "type": "CompositeJWK",
    "controller": "did:example:123",
    "compositeJwk": {
      "algId": "id-MLDSA44-Ed25519-xxx",
      "pqPublicKey": {
        "kid": "key-1", (SERVE?)
        "kty": "ML-DSA",
        "alg": "ML-DSA-44",
        "pub": ""
      },
      "traditionalPublicKey": {
        "kid": "key-1", (SERVE?)
        "kty": "OKP", 
        "crv": "Ed25519",
        "x": ""
      }
  }],
  "assertionMethod": [
    "did:example:123#key-0"
  ],
  "authentication": [
    "did:example:123#key-1"
  ]
}
```

### Proof Representations

This section details the proof representation formats that are defined by this specification.

#### DataIntegrityProof
A proof contains the attributes specified in the [Proofs section](https://www.w3.org/TR/vc-data-integrity/#proofs) of [VC-DATA-INTEGRITY](https://www.w3.org/TR/vc-data-integrity/) with the following restrictions.

The type property of the proof MUST be DataIntegrityProof.

The cryptosuite property of the proof MUST be experimental-ml-dsa-ecdsa-2025 or experimental-ml-dsa-eddsa-2025 based on the composite used.

experimental-composite-mldsa-2025


The value of the proofValue property of the proof MUST be produced as the concatenation of a ML-DSA-44/ECDSA signatures, of a ML-DSA-44/EdDSA signatures, of a  ML-DSA-65/ECDSA, and of a ML-DSA-65/EdDSA signatures produced according to using the algorithms specified in [section 3]. 

EXAMPLE 13: A composite ML-DSA-44/Ed25519 hybrid digital signature expressed as a DataIntegrityProof
```json
{
  ...
  "proof": {
    "type": "DataIntegrityProof",
    "cryptosuite": "experimental-composite-mldsa-2025",
    "created": "2025-03-18T08:15:30Z",
    "verificationMethod": "https://vc.example/issuers/4567#key-0",
    "proofPurpose": "assertionMethod",
    "proofValue": "..."
  }
}
```

## Algorithms

The following section describes multiple Data Integrity cryptographic suites that utilize the ML-DSA, ECDSA and EdDSA in composite  signature algorithms.

## Security Considerations

This section is non-normative.

Before reading this section, readers are urged to familiarize themselves with general security advice provided in the [Security Considerations section of the Data Integrity specification](https://www.w3.org/TR/vc-data-integrity/#security-considerations).


## Privacy Considerations

This section is non-normative.

Before reading this section, readers are urged to familiarize themselves with general privacy advice provided in the [Privacy Considerations section of the Data Integrity specification](https://www.w3.org/TR/vc-data-integrity/#privacy-considerations).

The following section describes privacy considerations that developers implementing this specification should be aware of in order to avoid violating privacy assumptions.


<section id='conformance'>
  <!-- This section is filled automatically by ReSpec. -->
</section>
