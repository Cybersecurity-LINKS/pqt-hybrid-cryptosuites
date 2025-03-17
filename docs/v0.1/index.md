---
layout: respec
title: Post-Quantum/Traditional hybrid cryptosuite v0.1
respec: >
  {
    "name": "pqt-hybrid-cryptosuite",
    "status": "CG-DRAFT",
    "latest": "https://cybersecurity-links.github.io/pqt-cryptosuites/spec/latest",
    "repository": "https://github.com/Cybersecurity-LINKS/pqt-cryptosuites",
    "issues": "https://github.com/Cybersecurity-LINKS/pqt-cryptosuites/issues",
    "group": {
      "name": "Credentials Community Group",
      "url": "https://www.w3.org/community/credentials/",
      "list": "public-credentials",
      "patentUri": "https://www.w3.org/community/about/agreements/cla/"
    },
    "editors": [
      {
        "name": "Andrea Vesco",
        "url": "https://www.linkedin.com/in/avesco",
        "company": "LINKS Foundation",
        "companyURL": ""
      },
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
   This specification describes several Data Integrity Cryptosuites for use when generating a digital signature using Post-Quantum/Traditional (PQ/T) hybrid digital signature algorithms.
  </p>
</section>

<section id="sotd">
  <p>
    This specification was published by the Cybersecurity Research Group @ LINKS Foundation. 
  </p><p>  
    This specification is experimental, do not use it in any production setting. 
  </p><p>  
    <a href="https://github.com/Cybersecurity-LINKS/pq-cryptosuites/issues"> GitHub Issues </a> are preferred for discussion of this specification.
  </p>
</section>

<style>
.red43 {
  color: red;
}
</style>

## Introduction

This specification defines several cryptographic suites for the purpose of creating, and verifying proofs for Post-Quantum/Traditional (PQ/T) hybrid signatures in conformance with the Data Integrity [VC-DATA-INTEGRITY](https://w3c.github.io/vc-data-integrity/) specification.

This specification uses either the RDF Dataset Canonicalization Algorithm [RDF-CANON](https://www.w3.org/TR/rdf-canon/) or the JSON Canonicalization Scheme [RFC8785](https://www.rfc-editor.org/rfc/rfc8785) to transform the input document into its canonical form. It uses SHA-256 and SHA-512 [RFC6234](https://datatracker.ietf.org/doc/html/rfc6234) as message digest algorithms and ML-DSA [FIPS204](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.204.pdf), ECDSA and EdDSA [FIPS-186-5](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.186-5.pdf) as component signature algorithms to create PQ/T hybrid signatures.

### Terminology

Terminology used throughout this document is defined in the <a href="https://www.w3.org/TR/vc-data-integrity/#terminology">Terminology</a> section of the <a href="https://www.w3.org/TR/vc-data-integrity"> Verifiable Credential Data Integrity 1.0</a> specification.

## Data Model

The following sections outline the data model that is used by this specification to express verification methods, such as cryptographic composite public keys, and PQ/T hybrid data integrity proofs, such as PQ/T hybrid digital signatures.

### Verification Methods

This cryptographic suite is used to verify Data Integrity Proofs [VC-DATA-INTEGRITY](https://w3c.github.io/vc-data-integrity/) produced using PQ/T hybrid key material. 

[//]: # (Lossless cryptographic key transformation processes that result in equivalent cryptographic key material MAY be used for the processing of digital signatures.)

This is a set of examples of public keys encoded as a `JsonWebKey`, the examples are instrumental to introduce composite public keys encoded as `CompositeJwk` 

[//]: # (JWK RFC 7517 - crv P-256, P-384, P-521)
[//]: # (JWK with Ed RFC 8037 - OKP Octet Key Pair - crv Ed25519, Ed448)
[//]: # (JWK structure for ML-DSA - https://datatracker.ietf.org/doc/html/draft-ietf-cose-dilithium-02)

<pre class="example nohighlight" title="A P-256 public key encoded as a JsonWebKey">
{
  "id": "https://example.com/issuer/123#key-0",
  "type": "JsonWebKey",
  "controller": "https://example.com/issuer/123",
  "publicKeyJwk": {
      "kid": "key-0",
      "kty": "EC",
      "crv": "P-256",
      "x": "f83OJ3D2xF1Bg8vub9tLe1gHMzV76e8Tus9uPHvRVEU",
      "y": "x_FEzRu9m36HLN_tue659LNpXW6pCyStikYjKIWI5a0"
  }
}
</pre>

<pre class="example nohighlight" title="A P-384 public key encoded as a JsonWebKey">
{
  "id": "https://example.com/issuer/123#key-0",
  "type": "JsonWebKey",
  "controller": "https://example.com/issuer/123",
  "publicKeyJwk": {
      "kid": "key-0",
      "kty": "EC",
      "crv": "P-384",
      "x": "gakWbm3Elw5D2attP36eFTHWG1K4VcuLfKQY1m9bSAaJXk7gbFxX_UgRxBfgR4f",
      "y": "aTB1BP54ydv0G7o5lHeFeX5zL1Jf9gvtPOhiEnwI0Cw2aeiNjD44zqeqOTFWVVYD"
  }
}
</pre>

<pre class="example nohighlight" title="An Ed25519 public key encoded as a JsonWebKey">
{
  "id": "https://example.com/issuer/123#key-0",
  "type": "JsonWebKey",
  "controller": "https://example.com/issuer/123",
  "publicKeyJwk": {
      "kid": "key-0",
      "kty": "OKP", 
      "crv": "Ed25519",
      "x": "0X-QEhA0qbmbRTtoupKNnmE0u_H_1XK6V3KORTwP990"
  }
}
</pre>

<pre class="example nohighlight" title="An Ed448 public key encoded as a JsonWebKey">
{
  "id": "https://example.com/issuer/123#key-0",
  "type": "JsonWebKey",
  "controller": "https://example.com/issuer/123",
  "publicKeyJwk": {
      "kid": "key-0",
      "kty": "OKP", 
      "crv": "Ed448",
      "x": "GhMR4aZWgygPxQgKfqvriwaznQEqUJI0gOPwQhHCxGFTBMHKlc6HlbWyiQL7pjFG7nzlxTXbpkSA"
  }
}
</pre>

<pre class="example nohighlight" title="A ML-DSA-44 public key encoded as a JsonWebKey">
{
  "id": "https://example.com/issuer/123#key-0",
  "type": "JsonWebKey",
  "controller": "https://example.com/issuer/123",
  "publicKeyJwk": {
    "kid": "key-0",
    "kty": "ML-DSA",
    "alg": "ML-DSA-44",
    "pub": "0IbUzQGj-Rzaf7xQW_PL0t2nWmoY-y6xcDYxKeJnR2cYiLPwoJ9INp7AJq7xD3dKLCVuQ74TvF9pOG8aRSNGOJElvOrTzKd7CvU-5VaE-DpCxUNk2-oYuxOTO2An51SwtkkX9_YbdOarwScGsWuRNF8ehD5rnButrMt96RBXR9lcMXRlNc1fxhC9sdTDE26MNI5deGaQ3jQCcYMCO1FY1L0ncMy0Vy5tS8Tw3VQ1wsLfCakMPCCUYRCX0nN0kK2K2KjmtGopY8s_PHosY_0cYm4nCq1W3K-ApCGlehY81DU4D_2hR2Hty4qbVFzy3kX4FF6RlD31ysZHGiPosZymj7vgOz_JWrInbFNO-s9xTSXDuj0iEReASI0rF1_tt_358JwYGgTakPrIExwvXgJX8qnRhhH58-mCZoLgbZjmjOxBqINRppCbXPkREgS8nXDQIWVs2fyyWLi3OD5m-tGghp6vgBpv_ZDA0fAHr2Chxowx4gzolTtEim7G39ISXThMTtWG1IAWHOL67Ol19pNLa3mow-IWNyqNfsu6KhFUmjDcbGgiTXcoGlDCaZaGRRuM9CGXdJMmAvbqwzqyPcf-l8OVb45mrMbwDGN9DDZ5cy2oWGm7otkhgix1bh-CDXnr2X8XC6fbD0HUqM8R9mT3O3l1EzUpL8aAhNCQ_r0BuvgcZzuGX-BdEmuy4wpZfXjudX0iZjSIFdUyxw5hVSBaZb-2iQBvUs9EPhPPEBNadZqLFTG6Qv9ejMIg7xKT9wLbqcrpeqqRPhU-vzNC7_ZnTZkhkzWuEFDawCKSXxwMA959eCD2zZ2Z6dihOzojqxlBXt_gPsLtmdIyBK_kNmxUgN34oDViXccre3UABSahFPzI9rtPc4S1JF8IlEG9Anmk0_ufXOEfqK9lA578RFFzusSpkWfsM-8WJ6dTPRbnkVcr8pe5tSGIXXU4e6mpIajHGXbhLhWetBu0ea5vCoPQ23qsPAfnZuRT5AkTTxZ65kxdvRGwWq1XOmaws7IB9RZiv_wqRtAjO0EJEc-hSkLJe4rSmrbBlrWAwlX2OQXZ3B13FMrQW7vC0CdXsrfoBsIc_CtIg_x9kgVaBZeKYkGwJm8vnm3_XA2AIlTsbU3TQbBzwCqH_AVBGPcefjvs5Ip_50Ld1mOl4obR-Mxt_4mwg2OuIN3aOEV-gase19SlUujQUhg-IkgvCzSRhpQ8CIy-t1hlnbUs7M7xNAiGRQQZUinbadUeky-sskQcM61DlZKIoHPal1l6F7IuJCHIGQPdOTP2U5f4yU6qrRnpsfhmIO9WdwN38ej-AeCM4Q2Z09BHumiKHWYz72H9QBLlBu4KLyuZlXu-e0fM7LcspTGnFciUfhhgqdDcas0F3DeIFajH9UTervyfIF4m7nu4ohhNvR-gW4r8zDA1zrcdf_OqLzRqL2yF6gx65aSpBacodugdHHOhNjokaOO9xLzX2uCQBC9AG9zl2-Kxx3z58aVCyotqBZlTp9qe8aHpz0YPdXA0Csjd-Dhus7OmdJxKD_CbF0HNHifkTKDp81Vlas-aaVqVRNLlHNQzdOiHSeig6tGVLYfv47067Vo40z9_XDdsxPZjbCQjuyVr5_5H_Py7QPgQTLCzMN45a1MuKSQkDEKjlirqL5hiShNCalBmnEBYgDxTMHi6l7B_jalz1VJPYHtebsTC6bmfmAaOtIZOYr4kiYqwpb00dRntrco4feev7ZxDnzZ6mpud-VNxjP4c-g"
  }
}
</pre>

<pre class="example nohighlight" title="A ML-DSA-65 public key encoded as a JsonWebKey">
{
  "id": "https://example.com/issuer/123#key-0",
  "type": "JsonWebKey",
  "controller": "https://example.com/issuer/123",
  "publicKeyJwk": {
    "kid": "key-0",
    "kty": "ML-DSA",
    "alg": "ML-DSA-65",
    "pub": "-1nlFKfO2TGDDr-29gZ1CmxJFfgq25PqCkzZPcaeQkJBNTU_RDZzbKmKAkJzD1KYgge4zAkT7l0xwSCVVIBFS8-Eg6edDcJXCJSLzRyDE8GV8bQ-a0KJCBawzNUht-z-Av6qo8eUiJSOJ2o_qVuKWP8wExXZtxARWnakDAJRwfGwOOov2ztsLt6RNbERHkVNxqd4ZULmE39emC4rHT2vXBtG7F47VQjoG4I5u_xB3EqslcW0BUkG-ABBtc8Bx4SDE5yYLF_3cgO5IUDUetmQPk1fcpbBW6nR8FM2ajbGdSYekMzIpy9f5oDBPU_pu5xi16wO-45PA4bYGPPBmUDLznaVR4oneZiTh1K8Mq_5UBC3bpVMqnijlyNnB8PAM-6CSxDPO7u2FKLrlkGGI9STD6oMO_nWY2OHcdj2q8UZmIteaJlu-lZjc28er43eSSe-jLpPnkx9gGBDgyFltJDhRGTBj1ymwzK6mN0jNrHRANcBWmwhCuozC38py51ccHxzq_rgpwTr3MPSlL6sFp_FlFKUgRwRrbO9n_T_iriXpdLb3aUQkeqJyFgYK953_6qL45W0sKghpi4T5PVu8gqRH4E1mz-HXTdB4mvY1RFQP-qaD4AAxTvaKkn6FGJEN6sk8UQlbKpeG9ZtI0CO4-H7vdLn24X5SiB7RtMnX-zh2tVB0FW3lLUwKSOCYu0FZRdrgDQGOZ9M9N7sSwAztNdwLIhG-sSP-nl37Tn4RxjXmunIELjUGxbd9eDOX4LaCHiGAA8uV-winl4qZwfowq7kl_PQGj06SRXtTZoJkQbC21G3JoWdvvKp1i1tHnXtYoDhZWRzdWk_AE1Y7n7QT2qJu3GDRu2MAA-ch6YOzuYsoE-cwd1AsS6bnFNbZEq8V9zneXTArGR69iax8xolD1RnBs9fzCeMZxb-ww3orh_LZo1w0FIvFgglL8uY_n4pFTH6bRXS_JXherOqUwqr55Auy0BP5sumDiSNa8gb_p1nTNUWzruGNH-L3ALLsuno1DOrnKo2CVKTraET6wWport5sitcz931f7XEIVrzmUleTzbk48reykAJZUPXCG9XCHSP04fnszE4EY6csx86LvMfxEOHrnhzfZnoVj3IPUObiL5Z3xxOygv3INDNICZaaOgkVkjSR9KYnhIg6c7WAaUzU5fdmBfL5bf1K6m5MkEtLvYvmVp0hbtYy7-eo02y9RHv6PYYp4JkXKW6zva4_fG6l-g8_VwBWUenF6qPGY4SLo79Q40qU2p7jmhrjHu8vdAOi2bfpOItP2Te-5LoqkOqchpu8ndwKAzNWMdahVNLfuZAx9qZBNca2QaowCkkssu5Tm-R_LtRzWW0yQW78kdDjTcOio-pqtaoaFd6q2xZiHLPAEF6t8lePYF6e0KGyOgujQEPZ3cGq5-fSTbDZ7DNGu632wzcCsbOYUV-5z-hHEbVU8iaYlOiQnfRYUk6ehzu1dl8wItIishxd-Oqs_yFUpkjXrWxtHPCpkSATu29ckZjgh6VSRZY1UPIockqMFZFQBy4E-NkPROt-rVYW29Znc0fINXvkSVYWR-wN9MdJzN09UsLUo_6AOoWiKqOYko0LaUVwQ3KXdvFt6ApnTQ1vogsC--aJD93B371q2ZKOfXdihpMizjolDw-RcnjVJj3-Kb1fqXXF3o9uhAtwPOuGlIynQiqfihOLFSA7xVHGm3NffZKYzkaTOj1LL_8DwjFydfsIDI_QUnybrqMmV0cH4uvY8AAPVimZPRNAOeNu0oOy2E6MlX2CC5ATntBcXMmLpgZ7u-k6Q_gJMBQfr8qhul96GOTbSMMIzFXp0A1lOY8Cj61J2s408OFPAdPW6KuFmXgyPjxYf-eX-Y1YyuzMGsUAEszOfiUoGH0z7a5n1VwtDwKCHERo3O3VnnimT9wbyRPHWaymTVn9CYxdlF0x6pdsMmrPIB6snhZX8JJXx-eczFyVtrQZbjQeg-G317RivOAtnjGeZ_j39z_-9caGCcnrbnodAqVEcD8c3qLSniBcnVgHNYXPDUb14541kAcdmack1BOMfoislXjIuMXYB_c6Vqvcf55aBAJMejfghjR8b4Axf0-PjJVG8ptwYZqmf0m_M2WlM-779RodwkDwOTYU2QkwOuq4-uBpo83E-0KTPqc88jt89WZMqSdKa873EwX-EZk7Gangy1P8y1bDwKHjnNTMG-morrpN50RaTCvt4IdMygUTHIrmR5j8jCQQupAm2Ae7vMqF_78jIBiIkpvg89nf04MWRwXqCmL7_ytyNucfpEdzKWu4mo9OEkHXMKj9c187qwNE5jbY9jkNzHsVWk3I5cynfdzDdkUOtcic2YeaNFFhbBwSsZckLjOcQFmmo0niVaPcFBG5F4D1AsSJQ5i9_xrXJKmVaSfPIXme3WAD8DufVyNIIUz_VmFWYyh7YcgirSqfabpIyCeG_l4GH44DHJA1_u9OdxG3cioTN2o1yvmYvj02ZxudKAlVQlK1AastEqkb9Eys_vy_F8EvppNEh4WMS5nkXYY3iMx4OtbIYEVUcfj5kKjk6lr2M_20guptFsdp9GG_FwIV4YNtaBkUrj6QRfwWDdu_YU"
  }
}
</pre>

<pre class="example nohighlight" title="A ML-DSA-87 public key encoded as a JsonWebKey">
{
  "id": "https://example.com/issuer/123#key-0",
  "type": "JsonWebKey",
  "controller": "https://example.com/issuer/123",
  "publicKeyJwk": {
    "kid": "key-0",
    "kty": "ML-DSA",
    "alg": "ML-DSA-87",
    "pub": "a8vTva_0Bp00H8r3mYY22oqsQxJ7khwV0IDDTjqr6hpZDEVONmo-leplOsY6tPVtwW6igrtF5gshKgr7acaj_6jOWdoo5MXzgRhKg-AT3o6SRno41Je7DC2ahrUbUKQqc9F3NW6L8H3C0b3g5DGmRu3AWIeg10Arg6Gbv6s8aenc0DMk-bP1Q682AiqwY29vI6zitOsPAZ3bp7qibsr4LezXf1DhTlUUQF38tmcDTovg1-BqVA6XTVoCPMdal6X5RwjBU_z9J1VqNmlc4wCbUFmikox57d1qqWY-0Xhig52ihP1CRdYnrcJEKi8W1n3Idv1c5ZF8j0zl9n-bD5IuCeSZi1qqocKFsgzRcWbgWmQiT33W7qoi8htRjRV-STyszFiOTILj6-qmk7rmghkBw9GfS6zrJHics9zHWuelI1aqHkSoa2Nlat8vLDL6FoAxUZiK7DMG0Gl0eNjPEEqu46yRJm7kZ-zIkw8umROUEk_eLqXXxvMoEJSJ2yg1TVDg3P0KIg14-8i59dH9qz1pDcM_iwRSr_renfKS0-y5PEI3B-Cj9LfY_zvab4l9P5kontns0y_lr33WmxtrfaykBNFy94_46STfnhvi0goG6A-CzLJUSFlv8_e1SVfa3fB2vgV5xahF0FReGVgxEhbkb9zTRB6UB27N6HxsS1T5u7uOFgCGCRfv0TREeb2XHVYOttPMUV9w7Kc4-iKWpkCBdowkb9-EWYX6hvnOnT57Q3Oa2KZC9fvELSkMy_f-HfEF1NMUEhJdTkUpy4m6HdfHPVrzNV71PxAtI0S2RdolF572np6GOts0FXnZAjuplKqjrzDHQHNrC5PVzu9YsKWiwjQBRHnu4EdKZzC24AU46MIAWuA-hSZARyi5dSxqkZLJYjs_-TcKSuOlzDlWTMHahpnaoDkQ9xjj4oqEjMefea4ULqzt8YaxJtd54-knF5dQUOGBmieyJUKRcVa6i4PgR4_kd72b7S11tTx1TB5bHvmsA99YuwTjNpw5u2c4-Qh-fjWhwcbcOrUomnxcIlspBtJxtFhSNqCrg6ZJg1FE_cDRAMVNuSPNlIwPjQ3serGo_O0BBneeZ53j5EecC8onRGhwGtNRchYYEBJLaKKkw8ok9-uzjGXblAY409BOhlWCAoP4sBr6F35OmiTm1_tNdP28GPO3G4jXCQ-XSiOfN7dMutdX3zAl4S3Ny4P2Q8rzLewEtK83cibsjvfVRZ-UQr-Q2dREi2VFLfCbldJ6A6-tEHZ2ObFT2PNlwX0BdMqPlb00AU2Ae4_UGqWRFAoHzjwrAhvUEWGHxfzeKciudSQK2lSO_i94J-yK01RbQKJ-ZEfC86XaI91cxAsxADU_4IhijLw9-Nv8qKndWC292Ftw8HH6LKhBmv18sxle8DO1y5vkfT6d8a-9GkecHL3Xyj_ckth2zUPCCIn-OJ-yapPmcgi7rnI-2Gq1c0MuL39dHIik9aB9u3Qtm27BaPujT6myOzPktqTco6nPSkFHSshwnKuXHv_wXJjMMWk3qZaOhYCyOCvPnrekZNyBB-uooGcdn4yfBGT7ANGKg1lPP6DaCFHI0BpDhdV9DArCrUp5PLWi2sgmY_YWztcDrB9Q-GbEUd-e_fzpW5BacPYNw-DcgDUJxV068WNy0uwSV4Tfo6Bw4h2CFuYtfTqRxYYWavepHB4NjMUARy29s_4KBB0Mt0f2Fp16UBHfGOZiYP_16kp1NtYfPrENxsdySX5ZtVrVm8D7Fyq34_rRwiI-JBOJLbLMJdceckrvgZuEwzDeeEsnaKSJ4eiHu3QRWMO3A_WrosKf_CXHoWhz4Xcc2qdQ1g_m4en1c_0IizN9CC92boUsnNLecS6NyDbep7lZHjyQEP3pJzVfCUJXrxMeDtvCuztHaWOglAYmQFXSodroio01fUVxq9Pw5qTqayRCU3rpBt4Ui6x8uF8YMid7_lxOdjQBMoWxX-ej-yTHJ3SxZfOns6aJuH0-W5l1bMbIcknUyGFLonOjeLXVALlfHOMCIBTEnGCqJ68J-YeD8nRSAymzsSabXI3edpc42WkYU9NOZZ4N_8Y7fpu8HX8e60eyIfz5eOTI_LIBK5M2o_cLZIUCQtl6Lt3vrkVMKV7oSOhDUWZzqhj7lTPT_1jn5uANp59bLbGe6VZkXEqVIs0HS6puiR3XMwPHdVaQI0PCjtIRkVN3-vOcnq1W0RJRYdjyd3oefyzXYFcxBhWNIqZDYZ9VB0jA3x7AW1wsWeYZXzeieGViJ8Or_09e41R9PLAFvBdlDl0t1QZa-FiUFl7BWOmvCGazQ-yPsOwz66-zZJZf2b3iNABo46o-dRFsuyTc5x3SIQw9O7QbIrgy1rKXT72MhQhEMxW9qtH0LORb_lJWWM7IQbBfAfc3o1LBlsH2o-nYODtpZOLkUNQFP38BOJ0O41WgcVTBoCxDtB8YmzBXqXwogp5ZBreNScFdrlxwy8xGofMxAB18uPVFSOBSvvIbfRUwIC4SmB5W4zEGX3kPUc_gnNQVx7g29Hd_zqeXGLJjQHw8xkP7abVF6AyNHKaK7Y95IC6e_tKEbxUjRP1MD3fYJHeSUJB7ZokmFPy6kZnErpT77eRYDh7_tRaq-7y58csT76kr-Wz436C58yMRfv-MtHv-FxqRr3CG-50pd_QKUMuQfuWlZyXUsXER2Sp9nQYOEOUvJ9wmiVKp4cXxHHRHJdHFwsrOguUDE3N0ry5jBi0nAn01t786KtgrWlLnJaRgXxO7ocfOWGqgAx_pZdP5RNpa3BtelJDUkmO1vbRNCwRw884aDldRZRvWn-bdNCq8K0jLElgtTSpNrsTRyhMHPUhV1Eaes1KuDyqtIWrjA1_unFRolyadv7NV-7FghYIC1xRzPQRUDV_bSSucYizuRSjrhbvvIZkeEbRO7-O6Hihg047UNmQ23taz9p6QtoPVK52BQH8qxBxpGT0rWttgMYNw0I3V6Q8Y1rgUFd7cKM_meVYVOWJkgLsCuVoJA4euRDcZsY-QY--vUs9-X4BpweJ1vTahXHu0zoPS670BcQRk4hf3XlU5YLXgC_UAm_D7oIXDGe3xFdU76MJKHiEDMjusCG9GpvnwyPZyW9MPdMIMdRdzwENcG8rSXPJ5Y_js4kCFeCpJ9gChcAZ-ELUZvIIPoNuZTlDSeyqoIaroStGLNr15E6-V-8eYjUdfbTCLc1YDuKpKwGsa9-XSvYJjXOVYvh5tArRgBpuE4cxuLL1j7wSNdFzvAbTTOSWbC0NTkwMU5ojsTBx26R1BaFxeb0R-oVlvQYqdlFXYqLnN2gktLlJSgOCqoAExyNiW1RJPVjgMbLZSCygrlS6rox0lhlgSdhjdZmgaMpIW0s1gBGdouhrInwIb-Nft6Tev3ugtzy4D-bg-Lda61q7OLCFUaESppn0CRCnXSleREIYPOTc_xUu78r3sntZe"
  }
}
</pre>

[//]: # (https://datatracker.ietf.org/doc/draft-ietf-lamps-pq-composite-sigs/04/)
[//]: # (pure composite and hashed composite)

#### CompositeJWK
A composite public key encoded as a `CompositeJWK` contains a PQ public key and a traditional public key both encoded as `JsonWebKey`. In addition, the `CompositeJWK` contains a string `algId` which represents the name of the algorithms used to generate the PQ/T hybrid data integrity proof (**TBD:** *and of the hash algorithm used to pre-hash the document*).

<pre class="example nohighlight" title="A ML-DSA-44/P-256 composite public key encoded as a CompositeJWK">
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
      "pub": "0IbUzQGj-Rzaf7xQW_PL0t2nWmoY-y6xcDYxKeJnR2cYiLPwoJ9INp7AJq7xD3dKLCVuQ74TvF9pOG8aRSNGOJElvOrTzKd7CvU-5VaE-DpCxUNk2-oYuxOTO2An51SwtkkX9_YbdOarwScGsWuRNF8ehD5rnButrMt96RBXR9lcMXRlNc1fxhC9sdTDE26MNI5deGaQ3jQCcYMCO1FY1L0ncMy0Vy5tS8Tw3VQ1wsLfCakMPCCUYRCX0nN0kK2K2KjmtGopY8s_PHosY_0cYm4nCq1W3K-ApCGlehY81DU4D_2hR2Hty4qbVFzy3kX4FF6RlD31ysZHGiPosZymj7vgOz_JWrInbFNO-s9xTSXDuj0iEReASI0rF1_tt_358JwYGgTakPrIExwvXgJX8qnRhhH58-mCZoLgbZjmjOxBqINRppCbXPkREgS8nXDQIWVs2fyyWLi3OD5m-tGghp6vgBpv_ZDA0fAHr2Chxowx4gzolTtEim7G39ISXThMTtWG1IAWHOL67Ol19pNLa3mow-IWNyqNfsu6KhFUmjDcbGgiTXcoGlDCaZaGRRuM9CGXdJMmAvbqwzqyPcf-l8OVb45mrMbwDGN9DDZ5cy2oWGm7otkhgix1bh-CDXnr2X8XC6fbD0HUqM8R9mT3O3l1EzUpL8aAhNCQ_r0BuvgcZzuGX-BdEmuy4wpZfXjudX0iZjSIFdUyxw5hVSBaZb-2iQBvUs9EPhPPEBNadZqLFTG6Qv9ejMIg7xKT9wLbqcrpeqqRPhU-vzNC7_ZnTZkhkzWuEFDawCKSXxwMA959eCD2zZ2Z6dihOzojqxlBXt_gPsLtmdIyBK_kNmxUgN34oDViXccre3UABSahFPzI9rtPc4S1JF8IlEG9Anmk0_ufXOEfqK9lA578RFFzusSpkWfsM-8WJ6dTPRbnkVcr8pe5tSGIXXU4e6mpIajHGXbhLhWetBu0ea5vCoPQ23qsPAfnZuRT5AkTTxZ65kxdvRGwWq1XOmaws7IB9RZiv_wqRtAjO0EJEc-hSkLJe4rSmrbBlrWAwlX2OQXZ3B13FMrQW7vC0CdXsrfoBsIc_CtIg_x9kgVaBZeKYkGwJm8vnm3_XA2AIlTsbU3TQbBzwCqH_AVBGPcefjvs5Ip_50Ld1mOl4obR-Mxt_4mwg2OuIN3aOEV-gase19SlUujQUhg-IkgvCzSRhpQ8CIy-t1hlnbUs7M7xNAiGRQQZUinbadUeky-sskQcM61DlZKIoHPal1l6F7IuJCHIGQPdOTP2U5f4yU6qrRnpsfhmIO9WdwN38ej-AeCM4Q2Z09BHumiKHWYz72H9QBLlBu4KLyuZlXu-e0fM7LcspTGnFciUfhhgqdDcas0F3DeIFajH9UTervyfIF4m7nu4ohhNvR-gW4r8zDA1zrcdf_OqLzRqL2yF6gx65aSpBacodugdHHOhNjokaOO9xLzX2uCQBC9AG9zl2-Kxx3z58aVCyotqBZlTp9qe8aHpz0YPdXA0Csjd-Dhus7OmdJxKD_CbF0HNHifkTKDp81Vlas-aaVqVRNLlHNQzdOiHSeig6tGVLYfv47067Vo40z9_XDdsxPZjbCQjuyVr5_5H_Py7QPgQTLCzMN45a1MuKSQkDEKjlirqL5hiShNCalBmnEBYgDxTMHi6l7B_jalz1VJPYHtebsTC6bmfmAaOtIZOYr4kiYqwpb00dRntrco4feev7ZxDnzZ6mpud-VNxjP4c-g"
    },
    "traditionalPublicKey": {
        "kid": "key-0",
        "kty": "EC",
        "crv": "P-256",
        "x": "f83OJ3D2xF1Bg8vub9tLe1gHMzV76e8Tus9uPHvRVEU",
        "y": "x_FEzRu9m36HLN_tue659LNpXW6pCyStikYjKIWI5a0"
    }
  }
}
</pre>

<pre class="example nohighlight" title="A ML-DSA-65/P-256 public key encoded as a CompositeJWK">
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
      "pub": "-1nlFKfO2TGDDr-29gZ1CmxJFfgq25PqCkzZPcaeQkJBNTU_RDZzbKmKAkJzD1KYgge4zAkT7l0xwSCVVIBFS8-Eg6edDcJXCJSLzRyDE8GV8bQ-a0KJCBawzNUht-z-Av6qo8eUiJSOJ2o_qVuKWP8wExXZtxARWnakDAJRwfGwOOov2ztsLt6RNbERHkVNxqd4ZULmE39emC4rHT2vXBtG7F47VQjoG4I5u_xB3EqslcW0BUkG-ABBtc8Bx4SDE5yYLF_3cgO5IUDUetmQPk1fcpbBW6nR8FM2ajbGdSYekMzIpy9f5oDBPU_pu5xi16wO-45PA4bYGPPBmUDLznaVR4oneZiTh1K8Mq_5UBC3bpVMqnijlyNnB8PAM-6CSxDPO7u2FKLrlkGGI9STD6oMO_nWY2OHcdj2q8UZmIteaJlu-lZjc28er43eSSe-jLpPnkx9gGBDgyFltJDhRGTBj1ymwzK6mN0jNrHRANcBWmwhCuozC38py51ccHxzq_rgpwTr3MPSlL6sFp_FlFKUgRwRrbO9n_T_iriXpdLb3aUQkeqJyFgYK953_6qL45W0sKghpi4T5PVu8gqRH4E1mz-HXTdB4mvY1RFQP-qaD4AAxTvaKkn6FGJEN6sk8UQlbKpeG9ZtI0CO4-H7vdLn24X5SiB7RtMnX-zh2tVB0FW3lLUwKSOCYu0FZRdrgDQGOZ9M9N7sSwAztNdwLIhG-sSP-nl37Tn4RxjXmunIELjUGxbd9eDOX4LaCHiGAA8uV-winl4qZwfowq7kl_PQGj06SRXtTZoJkQbC21G3JoWdvvKp1i1tHnXtYoDhZWRzdWk_AE1Y7n7QT2qJu3GDRu2MAA-ch6YOzuYsoE-cwd1AsS6bnFNbZEq8V9zneXTArGR69iax8xolD1RnBs9fzCeMZxb-ww3orh_LZo1w0FIvFgglL8uY_n4pFTH6bRXS_JXherOqUwqr55Auy0BP5sumDiSNa8gb_p1nTNUWzruGNH-L3ALLsuno1DOrnKo2CVKTraET6wWport5sitcz931f7XEIVrzmUleTzbk48reykAJZUPXCG9XCHSP04fnszE4EY6csx86LvMfxEOHrnhzfZnoVj3IPUObiL5Z3xxOygv3INDNICZaaOgkVkjSR9KYnhIg6c7WAaUzU5fdmBfL5bf1K6m5MkEtLvYvmVp0hbtYy7-eo02y9RHv6PYYp4JkXKW6zva4_fG6l-g8_VwBWUenF6qPGY4SLo79Q40qU2p7jmhrjHu8vdAOi2bfpOItP2Te-5LoqkOqchpu8ndwKAzNWMdahVNLfuZAx9qZBNca2QaowCkkssu5Tm-R_LtRzWW0yQW78kdDjTcOio-pqtaoaFd6q2xZiHLPAEF6t8lePYF6e0KGyOgujQEPZ3cGq5-fSTbDZ7DNGu632wzcCsbOYUV-5z-hHEbVU8iaYlOiQnfRYUk6ehzu1dl8wItIishxd-Oqs_yFUpkjXrWxtHPCpkSATu29ckZjgh6VSRZY1UPIockqMFZFQBy4E-NkPROt-rVYW29Znc0fINXvkSVYWR-wN9MdJzN09UsLUo_6AOoWiKqOYko0LaUVwQ3KXdvFt6ApnTQ1vogsC--aJD93B371q2ZKOfXdihpMizjolDw-RcnjVJj3-Kb1fqXXF3o9uhAtwPOuGlIynQiqfihOLFSA7xVHGm3NffZKYzkaTOj1LL_8DwjFydfsIDI_QUnybrqMmV0cH4uvY8AAPVimZPRNAOeNu0oOy2E6MlX2CC5ATntBcXMmLpgZ7u-k6Q_gJMBQfr8qhul96GOTbSMMIzFXp0A1lOY8Cj61J2s408OFPAdPW6KuFmXgyPjxYf-eX-Y1YyuzMGsUAEszOfiUoGH0z7a5n1VwtDwKCHERo3O3VnnimT9wbyRPHWaymTVn9CYxdlF0x6pdsMmrPIB6snhZX8JJXx-eczFyVtrQZbjQeg-G317RivOAtnjGeZ_j39z_-9caGCcnrbnodAqVEcD8c3qLSniBcnVgHNYXPDUb14541kAcdmack1BOMfoislXjIuMXYB_c6Vqvcf55aBAJMejfghjR8b4Axf0-PjJVG8ptwYZqmf0m_M2WlM-779RodwkDwOTYU2QkwOuq4-uBpo83E-0KTPqc88jt89WZMqSdKa873EwX-EZk7Gangy1P8y1bDwKHjnNTMG-morrpN50RaTCvt4IdMygUTHIrmR5j8jCQQupAm2Ae7vMqF_78jIBiIkpvg89nf04MWRwXqCmL7_ytyNucfpEdzKWu4mo9OEkHXMKj9c187qwNE5jbY9jkNzHsVWk3I5cynfdzDdkUOtcic2YeaNFFhbBwSsZckLjOcQFmmo0niVaPcFBG5F4D1AsSJQ5i9_xrXJKmVaSfPIXme3WAD8DufVyNIIUz_VmFWYyh7YcgirSqfabpIyCeG_l4GH44DHJA1_u9OdxG3cioTN2o1yvmYvj02ZxudKAlVQlK1AastEqkb9Eys_vy_F8EvppNEh4WMS5nkXYY3iMx4OtbIYEVUcfj5kKjk6lr2M_20guptFsdp9GG_FwIV4YNtaBkUrj6QRfwWDdu_YU"
    },
    "traditionalPublicKey": {
      "kid": "key-0",
      "kty": "EC",
      "crv": "P-256",
      "x": "f83OJ3D2xF1Bg8vub9tLe1gHMzV76e8Tus9uPHvRVEU",
      "y": "x_FEzRu9m36HLN_tue659LNpXW6pCyStikYjKIWI5a0"
    }
  }
}
</pre>

<pre class="example nohighlight" title="A ML-DSA-87/P-384 composite public key encoded as a CompositeJWK">
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
    "pub": "a8vTva_0Bp00H8r3mYY22oqsQxJ7khwV0IDDTjqr6hpZDEVONmo-leplOsY6tPVtwW6igrtF5gshKgr7acaj_6jOWdoo5MXzgRhKg-AT3o6SRno41Je7DC2ahrUbUKQqc9F3NW6L8H3C0b3g5DGmRu3AWIeg10Arg6Gbv6s8aenc0DMk-bP1Q682AiqwY29vI6zitOsPAZ3bp7qibsr4LezXf1DhTlUUQF38tmcDTovg1-BqVA6XTVoCPMdal6X5RwjBU_z9J1VqNmlc4wCbUFmikox57d1qqWY-0Xhig52ihP1CRdYnrcJEKi8W1n3Idv1c5ZF8j0zl9n-bD5IuCeSZi1qqocKFsgzRcWbgWmQiT33W7qoi8htRjRV-STyszFiOTILj6-qmk7rmghkBw9GfS6zrJHics9zHWuelI1aqHkSoa2Nlat8vLDL6FoAxUZiK7DMG0Gl0eNjPEEqu46yRJm7kZ-zIkw8umROUEk_eLqXXxvMoEJSJ2yg1TVDg3P0KIg14-8i59dH9qz1pDcM_iwRSr_renfKS0-y5PEI3B-Cj9LfY_zvab4l9P5kontns0y_lr33WmxtrfaykBNFy94_46STfnhvi0goG6A-CzLJUSFlv8_e1SVfa3fB2vgV5xahF0FReGVgxEhbkb9zTRB6UB27N6HxsS1T5u7uOFgCGCRfv0TREeb2XHVYOttPMUV9w7Kc4-iKWpkCBdowkb9-EWYX6hvnOnT57Q3Oa2KZC9fvELSkMy_f-HfEF1NMUEhJdTkUpy4m6HdfHPVrzNV71PxAtI0S2RdolF572np6GOts0FXnZAjuplKqjrzDHQHNrC5PVzu9YsKWiwjQBRHnu4EdKZzC24AU46MIAWuA-hSZARyi5dSxqkZLJYjs_-TcKSuOlzDlWTMHahpnaoDkQ9xjj4oqEjMefea4ULqzt8YaxJtd54-knF5dQUOGBmieyJUKRcVa6i4PgR4_kd72b7S11tTx1TB5bHvmsA99YuwTjNpw5u2c4-Qh-fjWhwcbcOrUomnxcIlspBtJxtFhSNqCrg6ZJg1FE_cDRAMVNuSPNlIwPjQ3serGo_O0BBneeZ53j5EecC8onRGhwGtNRchYYEBJLaKKkw8ok9-uzjGXblAY409BOhlWCAoP4sBr6F35OmiTm1_tNdP28GPO3G4jXCQ-XSiOfN7dMutdX3zAl4S3Ny4P2Q8rzLewEtK83cibsjvfVRZ-UQr-Q2dREi2VFLfCbldJ6A6-tEHZ2ObFT2PNlwX0BdMqPlb00AU2Ae4_UGqWRFAoHzjwrAhvUEWGHxfzeKciudSQK2lSO_i94J-yK01RbQKJ-ZEfC86XaI91cxAsxADU_4IhijLw9-Nv8qKndWC292Ftw8HH6LKhBmv18sxle8DO1y5vkfT6d8a-9GkecHL3Xyj_ckth2zUPCCIn-OJ-yapPmcgi7rnI-2Gq1c0MuL39dHIik9aB9u3Qtm27BaPujT6myOzPktqTco6nPSkFHSshwnKuXHv_wXJjMMWk3qZaOhYCyOCvPnrekZNyBB-uooGcdn4yfBGT7ANGKg1lPP6DaCFHI0BpDhdV9DArCrUp5PLWi2sgmY_YWztcDrB9Q-GbEUd-e_fzpW5BacPYNw-DcgDUJxV068WNy0uwSV4Tfo6Bw4h2CFuYtfTqRxYYWavepHB4NjMUARy29s_4KBB0Mt0f2Fp16UBHfGOZiYP_16kp1NtYfPrENxsdySX5ZtVrVm8D7Fyq34_rRwiI-JBOJLbLMJdceckrvgZuEwzDeeEsnaKSJ4eiHu3QRWMO3A_WrosKf_CXHoWhz4Xcc2qdQ1g_m4en1c_0IizN9CC92boUsnNLecS6NyDbep7lZHjyQEP3pJzVfCUJXrxMeDtvCuztHaWOglAYmQFXSodroio01fUVxq9Pw5qTqayRCU3rpBt4Ui6x8uF8YMid7_lxOdjQBMoWxX-ej-yTHJ3SxZfOns6aJuH0-W5l1bMbIcknUyGFLonOjeLXVALlfHOMCIBTEnGCqJ68J-YeD8nRSAymzsSabXI3edpc42WkYU9NOZZ4N_8Y7fpu8HX8e60eyIfz5eOTI_LIBK5M2o_cLZIUCQtl6Lt3vrkVMKV7oSOhDUWZzqhj7lTPT_1jn5uANp59bLbGe6VZkXEqVIs0HS6puiR3XMwPHdVaQI0PCjtIRkVN3-vOcnq1W0RJRYdjyd3oefyzXYFcxBhWNIqZDYZ9VB0jA3x7AW1wsWeYZXzeieGViJ8Or_09e41R9PLAFvBdlDl0t1QZa-FiUFl7BWOmvCGazQ-yPsOwz66-zZJZf2b3iNABo46o-dRFsuyTc5x3SIQw9O7QbIrgy1rKXT72MhQhEMxW9qtH0LORb_lJWWM7IQbBfAfc3o1LBlsH2o-nYODtpZOLkUNQFP38BOJ0O41WgcVTBoCxDtB8YmzBXqXwogp5ZBreNScFdrlxwy8xGofMxAB18uPVFSOBSvvIbfRUwIC4SmB5W4zEGX3kPUc_gnNQVx7g29Hd_zqeXGLJjQHw8xkP7abVF6AyNHKaK7Y95IC6e_tKEbxUjRP1MD3fYJHeSUJB7ZokmFPy6kZnErpT77eRYDh7_tRaq-7y58csT76kr-Wz436C58yMRfv-MtHv-FxqRr3CG-50pd_QKUMuQfuWlZyXUsXER2Sp9nQYOEOUvJ9wmiVKp4cXxHHRHJdHFwsrOguUDE3N0ry5jBi0nAn01t786KtgrWlLnJaRgXxO7ocfOWGqgAx_pZdP5RNpa3BtelJDUkmO1vbRNCwRw884aDldRZRvWn-bdNCq8K0jLElgtTSpNrsTRyhMHPUhV1Eaes1KuDyqtIWrjA1_unFRolyadv7NV-7FghYIC1xRzPQRUDV_bSSucYizuRSjrhbvvIZkeEbRO7-O6Hihg047UNmQ23taz9p6QtoPVK52BQH8qxBxpGT0rWttgMYNw0I3V6Q8Y1rgUFd7cKM_meVYVOWJkgLsCuVoJA4euRDcZsY-QY--vUs9-X4BpweJ1vTahXHu0zoPS670BcQRk4hf3XlU5YLXgC_UAm_D7oIXDGe3xFdU76MJKHiEDMjusCG9GpvnwyPZyW9MPdMIMdRdzwENcG8rSXPJ5Y_js4kCFeCpJ9gChcAZ-ELUZvIIPoNuZTlDSeyqoIaroStGLNr15E6-V-8eYjUdfbTCLc1YDuKpKwGsa9-XSvYJjXOVYvh5tArRgBpuE4cxuLL1j7wSNdFzvAbTTOSWbC0NTkwMU5ojsTBx26R1BaFxeb0R-oVlvQYqdlFXYqLnN2gktLlJSgOCqoAExyNiW1RJPVjgMbLZSCygrlS6rox0lhlgSdhjdZmgaMpIW0s1gBGdouhrInwIb-Nft6Tev3ugtzy4D-bg-Lda61q7OLCFUaESppn0CRCnXSleREIYPOTc_xUu78r3sntZe"
  },
  "traditionalPublicKey": {
      "kid": "key-0",
      "kty": "EC",
      "crv": "P-384",
      "x": "gakWbm3Elw5D2attP36eFTHWG1K4VcuLfKQY1m9bSAaJXk7gbFxX_UgRxBfgR4f",
      "y": "aTB1BP54ydv0G7o5lHeFeX5zL1Jf9gvtPOhiEnwI0Cw2aeiNjD44zqeqOTFWVVYD"
  }
}
}
</pre>

<pre class="example nohighlight" title="A ML-DSA-44/Ed25519 composite public key encoded as a CompositeJWK">
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
      "pub": "0IbUzQGj-Rzaf7xQW_PL0t2nWmoY-y6xcDYxKeJnR2cYiLPwoJ9INp7AJq7xD3dKLCVuQ74TvF9pOG8aRSNGOJElvOrTzKd7CvU-5VaE-DpCxUNk2-oYuxOTO2An51SwtkkX9_YbdOarwScGsWuRNF8ehD5rnButrMt96RBXR9lcMXRlNc1fxhC9sdTDE26MNI5deGaQ3jQCcYMCO1FY1L0ncMy0Vy5tS8Tw3VQ1wsLfCakMPCCUYRCX0nN0kK2K2KjmtGopY8s_PHosY_0cYm4nCq1W3K-ApCGlehY81DU4D_2hR2Hty4qbVFzy3kX4FF6RlD31ysZHGiPosZymj7vgOz_JWrInbFNO-s9xTSXDuj0iEReASI0rF1_tt_358JwYGgTakPrIExwvXgJX8qnRhhH58-mCZoLgbZjmjOxBqINRppCbXPkREgS8nXDQIWVs2fyyWLi3OD5m-tGghp6vgBpv_ZDA0fAHr2Chxowx4gzolTtEim7G39ISXThMTtWG1IAWHOL67Ol19pNLa3mow-IWNyqNfsu6KhFUmjDcbGgiTXcoGlDCaZaGRRuM9CGXdJMmAvbqwzqyPcf-l8OVb45mrMbwDGN9DDZ5cy2oWGm7otkhgix1bh-CDXnr2X8XC6fbD0HUqM8R9mT3O3l1EzUpL8aAhNCQ_r0BuvgcZzuGX-BdEmuy4wpZfXjudX0iZjSIFdUyxw5hVSBaZb-2iQBvUs9EPhPPEBNadZqLFTG6Qv9ejMIg7xKT9wLbqcrpeqqRPhU-vzNC7_ZnTZkhkzWuEFDawCKSXxwMA959eCD2zZ2Z6dihOzojqxlBXt_gPsLtmdIyBK_kNmxUgN34oDViXccre3UABSahFPzI9rtPc4S1JF8IlEG9Anmk0_ufXOEfqK9lA578RFFzusSpkWfsM-8WJ6dTPRbnkVcr8pe5tSGIXXU4e6mpIajHGXbhLhWetBu0ea5vCoPQ23qsPAfnZuRT5AkTTxZ65kxdvRGwWq1XOmaws7IB9RZiv_wqRtAjO0EJEc-hSkLJe4rSmrbBlrWAwlX2OQXZ3B13FMrQW7vC0CdXsrfoBsIc_CtIg_x9kgVaBZeKYkGwJm8vnm3_XA2AIlTsbU3TQbBzwCqH_AVBGPcefjvs5Ip_50Ld1mOl4obR-Mxt_4mwg2OuIN3aOEV-gase19SlUujQUhg-IkgvCzSRhpQ8CIy-t1hlnbUs7M7xNAiGRQQZUinbadUeky-sskQcM61DlZKIoHPal1l6F7IuJCHIGQPdOTP2U5f4yU6qrRnpsfhmIO9WdwN38ej-AeCM4Q2Z09BHumiKHWYz72H9QBLlBu4KLyuZlXu-e0fM7LcspTGnFciUfhhgqdDcas0F3DeIFajH9UTervyfIF4m7nu4ohhNvR-gW4r8zDA1zrcdf_OqLzRqL2yF6gx65aSpBacodugdHHOhNjokaOO9xLzX2uCQBC9AG9zl2-Kxx3z58aVCyotqBZlTp9qe8aHpz0YPdXA0Csjd-Dhus7OmdJxKD_CbF0HNHifkTKDp81Vlas-aaVqVRNLlHNQzdOiHSeig6tGVLYfv47067Vo40z9_XDdsxPZjbCQjuyVr5_5H_Py7QPgQTLCzMN45a1MuKSQkDEKjlirqL5hiShNCalBmnEBYgDxTMHi6l7B_jalz1VJPYHtebsTC6bmfmAaOtIZOYr4kiYqwpb00dRntrco4feev7ZxDnzZ6mpud-VNxjP4c-g"
    },
    "traditionalPublicKey": {
      "kid": "key-0",
      "kty": "OKP", 
      "crv": "Ed25519",
      "x": "0X-QEhA0qbmbRTtoupKNnmE0u_H_1XK6V3KORTwP990"
    }
  }
}
</pre>

<pre class="example nohighlight" title="A ML-DSA-65/Ed25519 public key encoded as a CompositeJWK">
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
      "pub": "-1nlFKfO2TGDDr-29gZ1CmxJFfgq25PqCkzZPcaeQkJBNTU_RDZzbKmKAkJzD1KYgge4zAkT7l0xwSCVVIBFS8-Eg6edDcJXCJSLzRyDE8GV8bQ-a0KJCBawzNUht-z-Av6qo8eUiJSOJ2o_qVuKWP8wExXZtxARWnakDAJRwfGwOOov2ztsLt6RNbERHkVNxqd4ZULmE39emC4rHT2vXBtG7F47VQjoG4I5u_xB3EqslcW0BUkG-ABBtc8Bx4SDE5yYLF_3cgO5IUDUetmQPk1fcpbBW6nR8FM2ajbGdSYekMzIpy9f5oDBPU_pu5xi16wO-45PA4bYGPPBmUDLznaVR4oneZiTh1K8Mq_5UBC3bpVMqnijlyNnB8PAM-6CSxDPO7u2FKLrlkGGI9STD6oMO_nWY2OHcdj2q8UZmIteaJlu-lZjc28er43eSSe-jLpPnkx9gGBDgyFltJDhRGTBj1ymwzK6mN0jNrHRANcBWmwhCuozC38py51ccHxzq_rgpwTr3MPSlL6sFp_FlFKUgRwRrbO9n_T_iriXpdLb3aUQkeqJyFgYK953_6qL45W0sKghpi4T5PVu8gqRH4E1mz-HXTdB4mvY1RFQP-qaD4AAxTvaKkn6FGJEN6sk8UQlbKpeG9ZtI0CO4-H7vdLn24X5SiB7RtMnX-zh2tVB0FW3lLUwKSOCYu0FZRdrgDQGOZ9M9N7sSwAztNdwLIhG-sSP-nl37Tn4RxjXmunIELjUGxbd9eDOX4LaCHiGAA8uV-winl4qZwfowq7kl_PQGj06SRXtTZoJkQbC21G3JoWdvvKp1i1tHnXtYoDhZWRzdWk_AE1Y7n7QT2qJu3GDRu2MAA-ch6YOzuYsoE-cwd1AsS6bnFNbZEq8V9zneXTArGR69iax8xolD1RnBs9fzCeMZxb-ww3orh_LZo1w0FIvFgglL8uY_n4pFTH6bRXS_JXherOqUwqr55Auy0BP5sumDiSNa8gb_p1nTNUWzruGNH-L3ALLsuno1DOrnKo2CVKTraET6wWport5sitcz931f7XEIVrzmUleTzbk48reykAJZUPXCG9XCHSP04fnszE4EY6csx86LvMfxEOHrnhzfZnoVj3IPUObiL5Z3xxOygv3INDNICZaaOgkVkjSR9KYnhIg6c7WAaUzU5fdmBfL5bf1K6m5MkEtLvYvmVp0hbtYy7-eo02y9RHv6PYYp4JkXKW6zva4_fG6l-g8_VwBWUenF6qPGY4SLo79Q40qU2p7jmhrjHu8vdAOi2bfpOItP2Te-5LoqkOqchpu8ndwKAzNWMdahVNLfuZAx9qZBNca2QaowCkkssu5Tm-R_LtRzWW0yQW78kdDjTcOio-pqtaoaFd6q2xZiHLPAEF6t8lePYF6e0KGyOgujQEPZ3cGq5-fSTbDZ7DNGu632wzcCsbOYUV-5z-hHEbVU8iaYlOiQnfRYUk6ehzu1dl8wItIishxd-Oqs_yFUpkjXrWxtHPCpkSATu29ckZjgh6VSRZY1UPIockqMFZFQBy4E-NkPROt-rVYW29Znc0fINXvkSVYWR-wN9MdJzN09UsLUo_6AOoWiKqOYko0LaUVwQ3KXdvFt6ApnTQ1vogsC--aJD93B371q2ZKOfXdihpMizjolDw-RcnjVJj3-Kb1fqXXF3o9uhAtwPOuGlIynQiqfihOLFSA7xVHGm3NffZKYzkaTOj1LL_8DwjFydfsIDI_QUnybrqMmV0cH4uvY8AAPVimZPRNAOeNu0oOy2E6MlX2CC5ATntBcXMmLpgZ7u-k6Q_gJMBQfr8qhul96GOTbSMMIzFXp0A1lOY8Cj61J2s408OFPAdPW6KuFmXgyPjxYf-eX-Y1YyuzMGsUAEszOfiUoGH0z7a5n1VwtDwKCHERo3O3VnnimT9wbyRPHWaymTVn9CYxdlF0x6pdsMmrPIB6snhZX8JJXx-eczFyVtrQZbjQeg-G317RivOAtnjGeZ_j39z_-9caGCcnrbnodAqVEcD8c3qLSniBcnVgHNYXPDUb14541kAcdmack1BOMfoislXjIuMXYB_c6Vqvcf55aBAJMejfghjR8b4Axf0-PjJVG8ptwYZqmf0m_M2WlM-779RodwkDwOTYU2QkwOuq4-uBpo83E-0KTPqc88jt89WZMqSdKa873EwX-EZk7Gangy1P8y1bDwKHjnNTMG-morrpN50RaTCvt4IdMygUTHIrmR5j8jCQQupAm2Ae7vMqF_78jIBiIkpvg89nf04MWRwXqCmL7_ytyNucfpEdzKWu4mo9OEkHXMKj9c187qwNE5jbY9jkNzHsVWk3I5cynfdzDdkUOtcic2YeaNFFhbBwSsZckLjOcQFmmo0niVaPcFBG5F4D1AsSJQ5i9_xrXJKmVaSfPIXme3WAD8DufVyNIIUz_VmFWYyh7YcgirSqfabpIyCeG_l4GH44DHJA1_u9OdxG3cioTN2o1yvmYvj02ZxudKAlVQlK1AastEqkb9Eys_vy_F8EvppNEh4WMS5nkXYY3iMx4OtbIYEVUcfj5kKjk6lr2M_20guptFsdp9GG_FwIV4YNtaBkUrj6QRfwWDdu_YU"
    },
    "traditionalPublicKey": {
      "kid": "key-0",
      "kty": "OKP", 
      "crv": "Ed25519",
      "x": "0X-QEhA0qbmbRTtoupKNnmE0u_H_1XK6V3KORTwP990"
    }
  }
}
</pre>

<pre class="example nohighlight" title="A ML-DSA-87/Ed448 composite public key encoded as a CompositeJWK">
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
    "pub": "a8vTva_0Bp00H8r3mYY22oqsQxJ7khwV0IDDTjqr6hpZDEVONmo-leplOsY6tPVtwW6igrtF5gshKgr7acaj_6jOWdoo5MXzgRhKg-AT3o6SRno41Je7DC2ahrUbUKQqc9F3NW6L8H3C0b3g5DGmRu3AWIeg10Arg6Gbv6s8aenc0DMk-bP1Q682AiqwY29vI6zitOsPAZ3bp7qibsr4LezXf1DhTlUUQF38tmcDTovg1-BqVA6XTVoCPMdal6X5RwjBU_z9J1VqNmlc4wCbUFmikox57d1qqWY-0Xhig52ihP1CRdYnrcJEKi8W1n3Idv1c5ZF8j0zl9n-bD5IuCeSZi1qqocKFsgzRcWbgWmQiT33W7qoi8htRjRV-STyszFiOTILj6-qmk7rmghkBw9GfS6zrJHics9zHWuelI1aqHkSoa2Nlat8vLDL6FoAxUZiK7DMG0Gl0eNjPEEqu46yRJm7kZ-zIkw8umROUEk_eLqXXxvMoEJSJ2yg1TVDg3P0KIg14-8i59dH9qz1pDcM_iwRSr_renfKS0-y5PEI3B-Cj9LfY_zvab4l9P5kontns0y_lr33WmxtrfaykBNFy94_46STfnhvi0goG6A-CzLJUSFlv8_e1SVfa3fB2vgV5xahF0FReGVgxEhbkb9zTRB6UB27N6HxsS1T5u7uOFgCGCRfv0TREeb2XHVYOttPMUV9w7Kc4-iKWpkCBdowkb9-EWYX6hvnOnT57Q3Oa2KZC9fvELSkMy_f-HfEF1NMUEhJdTkUpy4m6HdfHPVrzNV71PxAtI0S2RdolF572np6GOts0FXnZAjuplKqjrzDHQHNrC5PVzu9YsKWiwjQBRHnu4EdKZzC24AU46MIAWuA-hSZARyi5dSxqkZLJYjs_-TcKSuOlzDlWTMHahpnaoDkQ9xjj4oqEjMefea4ULqzt8YaxJtd54-knF5dQUOGBmieyJUKRcVa6i4PgR4_kd72b7S11tTx1TB5bHvmsA99YuwTjNpw5u2c4-Qh-fjWhwcbcOrUomnxcIlspBtJxtFhSNqCrg6ZJg1FE_cDRAMVNuSPNlIwPjQ3serGo_O0BBneeZ53j5EecC8onRGhwGtNRchYYEBJLaKKkw8ok9-uzjGXblAY409BOhlWCAoP4sBr6F35OmiTm1_tNdP28GPO3G4jXCQ-XSiOfN7dMutdX3zAl4S3Ny4P2Q8rzLewEtK83cibsjvfVRZ-UQr-Q2dREi2VFLfCbldJ6A6-tEHZ2ObFT2PNlwX0BdMqPlb00AU2Ae4_UGqWRFAoHzjwrAhvUEWGHxfzeKciudSQK2lSO_i94J-yK01RbQKJ-ZEfC86XaI91cxAsxADU_4IhijLw9-Nv8qKndWC292Ftw8HH6LKhBmv18sxle8DO1y5vkfT6d8a-9GkecHL3Xyj_ckth2zUPCCIn-OJ-yapPmcgi7rnI-2Gq1c0MuL39dHIik9aB9u3Qtm27BaPujT6myOzPktqTco6nPSkFHSshwnKuXHv_wXJjMMWk3qZaOhYCyOCvPnrekZNyBB-uooGcdn4yfBGT7ANGKg1lPP6DaCFHI0BpDhdV9DArCrUp5PLWi2sgmY_YWztcDrB9Q-GbEUd-e_fzpW5BacPYNw-DcgDUJxV068WNy0uwSV4Tfo6Bw4h2CFuYtfTqRxYYWavepHB4NjMUARy29s_4KBB0Mt0f2Fp16UBHfGOZiYP_16kp1NtYfPrENxsdySX5ZtVrVm8D7Fyq34_rRwiI-JBOJLbLMJdceckrvgZuEwzDeeEsnaKSJ4eiHu3QRWMO3A_WrosKf_CXHoWhz4Xcc2qdQ1g_m4en1c_0IizN9CC92boUsnNLecS6NyDbep7lZHjyQEP3pJzVfCUJXrxMeDtvCuztHaWOglAYmQFXSodroio01fUVxq9Pw5qTqayRCU3rpBt4Ui6x8uF8YMid7_lxOdjQBMoWxX-ej-yTHJ3SxZfOns6aJuH0-W5l1bMbIcknUyGFLonOjeLXVALlfHOMCIBTEnGCqJ68J-YeD8nRSAymzsSabXI3edpc42WkYU9NOZZ4N_8Y7fpu8HX8e60eyIfz5eOTI_LIBK5M2o_cLZIUCQtl6Lt3vrkVMKV7oSOhDUWZzqhj7lTPT_1jn5uANp59bLbGe6VZkXEqVIs0HS6puiR3XMwPHdVaQI0PCjtIRkVN3-vOcnq1W0RJRYdjyd3oefyzXYFcxBhWNIqZDYZ9VB0jA3x7AW1wsWeYZXzeieGViJ8Or_09e41R9PLAFvBdlDl0t1QZa-FiUFl7BWOmvCGazQ-yPsOwz66-zZJZf2b3iNABo46o-dRFsuyTc5x3SIQw9O7QbIrgy1rKXT72MhQhEMxW9qtH0LORb_lJWWM7IQbBfAfc3o1LBlsH2o-nYODtpZOLkUNQFP38BOJ0O41WgcVTBoCxDtB8YmzBXqXwogp5ZBreNScFdrlxwy8xGofMxAB18uPVFSOBSvvIbfRUwIC4SmB5W4zEGX3kPUc_gnNQVx7g29Hd_zqeXGLJjQHw8xkP7abVF6AyNHKaK7Y95IC6e_tKEbxUjRP1MD3fYJHeSUJB7ZokmFPy6kZnErpT77eRYDh7_tRaq-7y58csT76kr-Wz436C58yMRfv-MtHv-FxqRr3CG-50pd_QKUMuQfuWlZyXUsXER2Sp9nQYOEOUvJ9wmiVKp4cXxHHRHJdHFwsrOguUDE3N0ry5jBi0nAn01t786KtgrWlLnJaRgXxO7ocfOWGqgAx_pZdP5RNpa3BtelJDUkmO1vbRNCwRw884aDldRZRvWn-bdNCq8K0jLElgtTSpNrsTRyhMHPUhV1Eaes1KuDyqtIWrjA1_unFRolyadv7NV-7FghYIC1xRzPQRUDV_bSSucYizuRSjrhbvvIZkeEbRO7-O6Hihg047UNmQ23taz9p6QtoPVK52BQH8qxBxpGT0rWttgMYNw0I3V6Q8Y1rgUFd7cKM_meVYVOWJkgLsCuVoJA4euRDcZsY-QY--vUs9-X4BpweJ1vTahXHu0zoPS670BcQRk4hf3XlU5YLXgC_UAm_D7oIXDGe3xFdU76MJKHiEDMjusCG9GpvnwyPZyW9MPdMIMdRdzwENcG8rSXPJ5Y_js4kCFeCpJ9gChcAZ-ELUZvIIPoNuZTlDSeyqoIaroStGLNr15E6-V-8eYjUdfbTCLc1YDuKpKwGsa9-XSvYJjXOVYvh5tArRgBpuE4cxuLL1j7wSNdFzvAbTTOSWbC0NTkwMU5ojsTBx26R1BaFxeb0R-oVlvQYqdlFXYqLnN2gktLlJSgOCqoAExyNiW1RJPVjgMbLZSCygrlS6rox0lhlgSdhjdZmgaMpIW0s1gBGdouhrInwIb-Nft6Tev3ugtzy4D-bg-Lda61q7OLCFUaESppn0CRCnXSleREIYPOTc_xUu78r3sntZe"
  },
  "traditionalPublicKey": {
      "kid": "key-0",
      "kty": "OKP", 
      "crv": "Ed448",
      "x": "GhMR4aZWgygPxQgKfqvriwaznQEqUJI0gOPwQhHCxGFTBMHKlc6HlbWyiQL7pjFG7nzlxTXbpkSA"
  }
}
}
</pre>

<pre class="example nohighlight" title="A ML-DSA-65/Ed25519 public key encoded as a CompositeJWK for assertion method, and a ML-DSA-44/Ed25519 public key encoded as a CompositeJWK for authentication method in a controller document">
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
        "kid": "key-0",
        "kty": "ML-DSA",
        "alg": "ML-DSA-65",
        "pub": "-1nlFKfO2TGDDr-29gZ1CmxJFfgq25PqCkzZPcaeQkJBNTU_RDZzbKmKAkJzD1KYgge4zAkT7l0xwSCVVIBFS8-Eg6edDcJXCJSLzRyDE8GV8bQ-a0KJCBawzNUht-z-Av6qo8eUiJSOJ2o_qVuKWP8wExXZtxARWnakDAJRwfGwOOov2ztsLt6RNbERHkVNxqd4ZULmE39emC4rHT2vXBtG7F47VQjoG4I5u_xB3EqslcW0BUkG-ABBtc8Bx4SDE5yYLF_3cgO5IUDUetmQPk1fcpbBW6nR8FM2ajbGdSYekMzIpy9f5oDBPU_pu5xi16wO-45PA4bYGPPBmUDLznaVR4oneZiTh1K8Mq_5UBC3bpVMqnijlyNnB8PAM-6CSxDPO7u2FKLrlkGGI9STD6oMO_nWY2OHcdj2q8UZmIteaJlu-lZjc28er43eSSe-jLpPnkx9gGBDgyFltJDhRGTBj1ymwzK6mN0jNrHRANcBWmwhCuozC38py51ccHxzq_rgpwTr3MPSlL6sFp_FlFKUgRwRrbO9n_T_iriXpdLb3aUQkeqJyFgYK953_6qL45W0sKghpi4T5PVu8gqRH4E1mz-HXTdB4mvY1RFQP-qaD4AAxTvaKkn6FGJEN6sk8UQlbKpeG9ZtI0CO4-H7vdLn24X5SiB7RtMnX-zh2tVB0FW3lLUwKSOCYu0FZRdrgDQGOZ9M9N7sSwAztNdwLIhG-sSP-nl37Tn4RxjXmunIELjUGxbd9eDOX4LaCHiGAA8uV-winl4qZwfowq7kl_PQGj06SRXtTZoJkQbC21G3JoWdvvKp1i1tHnXtYoDhZWRzdWk_AE1Y7n7QT2qJu3GDRu2MAA-ch6YOzuYsoE-cwd1AsS6bnFNbZEq8V9zneXTArGR69iax8xolD1RnBs9fzCeMZxb-ww3orh_LZo1w0FIvFgglL8uY_n4pFTH6bRXS_JXherOqUwqr55Auy0BP5sumDiSNa8gb_p1nTNUWzruGNH-L3ALLsuno1DOrnKo2CVKTraET6wWport5sitcz931f7XEIVrzmUleTzbk48reykAJZUPXCG9XCHSP04fnszE4EY6csx86LvMfxEOHrnhzfZnoVj3IPUObiL5Z3xxOygv3INDNICZaaOgkVkjSR9KYnhIg6c7WAaUzU5fdmBfL5bf1K6m5MkEtLvYvmVp0hbtYy7-eo02y9RHv6PYYp4JkXKW6zva4_fG6l-g8_VwBWUenF6qPGY4SLo79Q40qU2p7jmhrjHu8vdAOi2bfpOItP2Te-5LoqkOqchpu8ndwKAzNWMdahVNLfuZAx9qZBNca2QaowCkkssu5Tm-R_LtRzWW0yQW78kdDjTcOio-pqtaoaFd6q2xZiHLPAEF6t8lePYF6e0KGyOgujQEPZ3cGq5-fSTbDZ7DNGu632wzcCsbOYUV-5z-hHEbVU8iaYlOiQnfRYUk6ehzu1dl8wItIishxd-Oqs_yFUpkjXrWxtHPCpkSATu29ckZjgh6VSRZY1UPIockqMFZFQBy4E-NkPROt-rVYW29Znc0fINXvkSVYWR-wN9MdJzN09UsLUo_6AOoWiKqOYko0LaUVwQ3KXdvFt6ApnTQ1vogsC--aJD93B371q2ZKOfXdihpMizjolDw-RcnjVJj3-Kb1fqXXF3o9uhAtwPOuGlIynQiqfihOLFSA7xVHGm3NffZKYzkaTOj1LL_8DwjFydfsIDI_QUnybrqMmV0cH4uvY8AAPVimZPRNAOeNu0oOy2E6MlX2CC5ATntBcXMmLpgZ7u-k6Q_gJMBQfr8qhul96GOTbSMMIzFXp0A1lOY8Cj61J2s408OFPAdPW6KuFmXgyPjxYf-eX-Y1YyuzMGsUAEszOfiUoGH0z7a5n1VwtDwKCHERo3O3VnnimT9wbyRPHWaymTVn9CYxdlF0x6pdsMmrPIB6snhZX8JJXx-eczFyVtrQZbjQeg-G317RivOAtnjGeZ_j39z_-9caGCcnrbnodAqVEcD8c3qLSniBcnVgHNYXPDUb14541kAcdmack1BOMfoislXjIuMXYB_c6Vqvcf55aBAJMejfghjR8b4Axf0-PjJVG8ptwYZqmf0m_M2WlM-779RodwkDwOTYU2QkwOuq4-uBpo83E-0KTPqc88jt89WZMqSdKa873EwX-EZk7Gangy1P8y1bDwKHjnNTMG-morrpN50RaTCvt4IdMygUTHIrmR5j8jCQQupAm2Ae7vMqF_78jIBiIkpvg89nf04MWRwXqCmL7_ytyNucfpEdzKWu4mo9OEkHXMKj9c187qwNE5jbY9jkNzHsVWk3I5cynfdzDdkUOtcic2YeaNFFhbBwSsZckLjOcQFmmo0niVaPcFBG5F4D1AsSJQ5i9_xrXJKmVaSfPIXme3WAD8DufVyNIIUz_VmFWYyh7YcgirSqfabpIyCeG_l4GH44DHJA1_u9OdxG3cioTN2o1yvmYvj02ZxudKAlVQlK1AastEqkb9Eys_vy_F8EvppNEh4WMS5nkXYY3iMx4OtbIYEVUcfj5kKjk6lr2M_20guptFsdp9GG_FwIV4YNtaBkUrj6QRfwWDdu_YU"
      },
      "traditionalPublicKey": {
        "kid": "key-0",
        "kty": "OKP", 
        "crv": "Ed25519",
        "x": "0X-QEhA0qbmbRTtoupKNnmE0u_H_1XK6V3KORTwP990"
      }
  }{
    "id": "did:example:123#key-1",
    "type": "CompositeJWK",
    "controller": "did:example:123",
    "compositeJwk": {
      "algId": "id-MLDSA44-Ed25519-xxx",
      "pqPublicKey": {
        "kid": "key-1",
        "kty": "ML-DSA",
        "alg": "ML-DSA-44",
        "pub": "0IbUzQGj-Rzaf7xQW_PL0t2nWmoY-y6xcDYxKeJnR2cYiLPwoJ9INp7AJq7xD3dKLCVuQ74TvF9pOG8aRSNGOJElvOrTzKd7CvU-5VaE-DpCxUNk2-oYuxOTO2An51SwtkkX9_YbdOarwScGsWuRNF8ehD5rnButrMt96RBXR9lcMXRlNc1fxhC9sdTDE26MNI5deGaQ3jQCcYMCO1FY1L0ncMy0Vy5tS8Tw3VQ1wsLfCakMPCCUYRCX0nN0kK2K2KjmtGopY8s_PHosY_0cYm4nCq1W3K-ApCGlehY81DU4D_2hR2Hty4qbVFzy3kX4FF6RlD31ysZHGiPosZymj7vgOz_JWrInbFNO-s9xTSXDuj0iEReASI0rF1_tt_358JwYGgTakPrIExwvXgJX8qnRhhH58-mCZoLgbZjmjOxBqINRppCbXPkREgS8nXDQIWVs2fyyWLi3OD5m-tGghp6vgBpv_ZDA0fAHr2Chxowx4gzolTtEim7G39ISXThMTtWG1IAWHOL67Ol19pNLa3mow-IWNyqNfsu6KhFUmjDcbGgiTXcoGlDCaZaGRRuM9CGXdJMmAvbqwzqyPcf-l8OVb45mrMbwDGN9DDZ5cy2oWGm7otkhgix1bh-CDXnr2X8XC6fbD0HUqM8R9mT3O3l1EzUpL8aAhNCQ_r0BuvgcZzuGX-BdEmuy4wpZfXjudX0iZjSIFdUyxw5hVSBaZb-2iQBvUs9EPhPPEBNadZqLFTG6Qv9ejMIg7xKT9wLbqcrpeqqRPhU-vzNC7_ZnTZkhkzWuEFDawCKSXxwMA959eCD2zZ2Z6dihOzojqxlBXt_gPsLtmdIyBK_kNmxUgN34oDViXccre3UABSahFPzI9rtPc4S1JF8IlEG9Anmk0_ufXOEfqK9lA578RFFzusSpkWfsM-8WJ6dTPRbnkVcr8pe5tSGIXXU4e6mpIajHGXbhLhWetBu0ea5vCoPQ23qsPAfnZuRT5AkTTxZ65kxdvRGwWq1XOmaws7IB9RZiv_wqRtAjO0EJEc-hSkLJe4rSmrbBlrWAwlX2OQXZ3B13FMrQW7vC0CdXsrfoBsIc_CtIg_x9kgVaBZeKYkGwJm8vnm3_XA2AIlTsbU3TQbBzwCqH_AVBGPcefjvs5Ip_50Ld1mOl4obR-Mxt_4mwg2OuIN3aOEV-gase19SlUujQUhg-IkgvCzSRhpQ8CIy-t1hlnbUs7M7xNAiGRQQZUinbadUeky-sskQcM61DlZKIoHPal1l6F7IuJCHIGQPdOTP2U5f4yU6qrRnpsfhmIO9WdwN38ej-AeCM4Q2Z09BHumiKHWYz72H9QBLlBu4KLyuZlXu-e0fM7LcspTGnFciUfhhgqdDcas0F3DeIFajH9UTervyfIF4m7nu4ohhNvR-gW4r8zDA1zrcdf_OqLzRqL2yF6gx65aSpBacodugdHHOhNjokaOO9xLzX2uCQBC9AG9zl2-Kxx3z58aVCyotqBZlTp9qe8aHpz0YPdXA0Csjd-Dhus7OmdJxKD_CbF0HNHifkTKDp81Vlas-aaVqVRNLlHNQzdOiHSeig6tGVLYfv47067Vo40z9_XDdsxPZjbCQjuyVr5_5H_Py7QPgQTLCzMN45a1MuKSQkDEKjlirqL5hiShNCalBmnEBYgDxTMHi6l7B_jalz1VJPYHtebsTC6bmfmAaOtIZOYr4kiYqwpb00dRntrco4feev7ZxDnzZ6mpud-VNxjP4c-g"
      },
      "traditionalPublicKey": {
        "kid": "key-1",
        "kty": "OKP", 
        "crv": "Ed25519",
        "x": "0X-QEhA0qbmbRTtoupKNnmE0u_H_1XK6V3KORTwP990"
      }
  }],
  "assertionMethod": [
    "did:example:123#key-0"
  ],
  "authentication": [
    "did:example:123#key-1"
  ]
}
</pre>

### Proof Representations

This section details the proof representation formats that are defined by this specification.

#### DataIntegrityProof
A proof contains the attributes specified in the [Proofs section](https://www.w3.org/TR/vc-data-integrity/#proofs) of [VC-DATA-INTEGRITY](https://www.w3.org/TR/vc-data-integrity/) with the following restrictions.

The `type` property of the proof MUST be `DataIntegrityProof`.

The `cryptosuite` property of the proof MUST be `experimental-ml-dsa-ecdsa-2025` or `experimental-ml-dsa-eddsa-2025` based on the composite used.

The value of the `proofValue` property of the proof MUST be produced as the concatenation of ML-DSA-44 with ECDSA or EdDSA signatures produced according to using the algorithms specified in [section 3](#algorithms), encoded according to [FIPS204](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.204.pdf) and ECDSA or EdDSA [FIPS-186-5](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.186-5.pdf), then encoded using the base-64-url-nopad header and alphabet as described in the [Multibase section](https://www.w3.org/TR/vc-data-integrity/#multibase-0) of [VC-DATA-INTEGRITY](https://www.w3.org/TR/vc-data-integrity/).

EXAMPLE 13: A composite ML-DSA-44/Ed25519 hybrid digital signature expressed as a DataIntegrityProof
<pre class="example nohighlight" title="A composite ML-DSA-44/Ed25519 hybrid digital signature expressed as a DataIntegrityProof">
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
</pre>

## Algorithms

The following section describes multiple Data Integrity cryptographic suites that utilize the ML-DSA, ECDSA and EdDSA in composite signature algorithms.

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
