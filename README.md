# UCAN/CACAO Interoperation Spec v0.1.0

## Editors

* [Brooklyn Zelenka](https://github.com/expede), [Fission](https://fission.codes)

## Authors
    
* [Irakli Gozalishvili](https://github.com/Gozala), [DAG House](https://dag.house)
* [Joel Thorstensson](https://github.com/oed), [3Box Labs](https://3boxlabs.com/)
* [Sergey Ukustov](https://github.com/ukstv), [Ceramic Network](https://ceramic.network/)
* [Brooklyn Zelenka](https://github.com/expede), [Fission Codes](https://fission.codes)

## Language

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC2119](https://datatracker.ietf.org/doc/html/rfc2119).

## Dependencies

* [UCAN](https://github.com/ucan-wg/spec)
* [CACAO](https://github.com/ChainAgnostic/CAIPs/blob/master/CAIPs/caip-74.md)
* [UCAN Canonicalization](https://github.com/ucan-wg/canonicalization/)

# 0 Abstract

[CAIP-74 ("CACAO")](https://github.com/ChainAgnostic/CAIPs/blob/master/CAIPs/caip-74.md) is a token format designed as a machine-readable representation of a "Sign-in with X" (e.g. [EIP-4361 "Sign in with Ethereum"](https://eips.ethereum.org/EIPS/eip-4361)) payloads.

# 1 Motivation

Sign in with X is a method for providing human-readable authorization signing in wallets and other key management systems.


# 2 Convertion


| UCAN Field | CACAO Field | Comments                                                               | 
|------------|-------------|------------------------------------------------------------------------|
|            | `domain`    | The RFC 3986 authority that is requesting the signature                |
| `iss`      | `iss`       | MUST be `did:pkh` in CACAO                                             |
| `aud`      | `aud`       | MAY be any URI in CACAO, MUST be a DID in UCAN                         |
|            | `version`   | CACAO version                                                          | 
| `ucv`      |             | UCAN version                                                           |
| `nonce`    | `nonce`     |                                                                        |
|            | `iat`       |                                                                        |
| `nbf`      | `nbf`       | Given as RFC 3339 date-time format in CACAO, as Unix timestamp in UCAN |
| `exp`      | `exp`       | Given as RFC 3339 date-time format in CACAO, as Unix timestamp in UCAN |
|            | `statement` | Optional in CACAO                                                      |
|            | `requestId` |                                                                        |
|            | `resources` | Resouces as URIs, ability scoping not defined in CACAO                 |
| `att`      |             | Equivalent to CACAO `resources`, but MUST include ability scoping      |


## 2.1 Ability Harmonization

The CACAO `resource` field does not specify a fixed ability format.

If no 

FIXME add resource scoping upstream


# 3 IPLD


# 4 Acknowledgments




----------

type Payload struct {
  domain String // =domain
  iss String // = DID pkh
  aud String // =uri
  version String
  nonce String
  iat String // RFC3339 date-time =issued-at
  nbf optional String // RFC3339 date-time =not-before
  exp optional String // RFC3339 date-time = expiration-time
  statement optional String // =statement
  requestId optional String // =request-id
  resources optional [ String ] // =resources as URIs
}
