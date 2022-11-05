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

CACAO objects expressed as UCAN MUST be canonicalized prior 



| UCAN Field | CACAO Field | Comments                                                               | 
|------------|-------------|------------------------------------------------------------------------|
|            | `domain`    | The RFC 3986 authority that is requesting the signature                |
| `iss`      | `iss`       | MUST be `did:pkh` in CACAO                                             |
| `aud`      | `aud`       | MAY be any URI in CACAO, MUST be a DID in UCAN                         | <!-- TODO in UCAN, replace this with did:web -->
|            | `version`   | CACAO version                                                          | 
| `ucv`      |             | UCAN version                                                           | <!-- TODO Where to put in UCAN -> CACAO? Is that even possible? -->
| `nonce`    | `nonce`     |                                                                        |
|            | `iat`       |                                                                        | <!-- TODO MAY be included as a UCAN directly as an extended field. This will cause problems for IPLD unless we put it in the facts -->
| `nbf`      | `nbf`       | Given as RFC 3339 date-time format in CACAO, as Unix timestamp in UCAN |
| `exp`      | `exp`       | Given as RFC 3339 date-time format in CACAO, as Unix timestamp in UCAN |
|            | `statement` | Optional in CACAO                                                      |
|            | `requestId` |                                                                        | <!-- TODO Probably a fact -->
|            | `resources` | Resouces as URIs, ability scoping not defined in CACAO                 |
| `att`      |             | Equivalent to CACAO `resources`, but MUST include ability scoping      |


## 2.1 Ability Harmonization

The CACAO `resource` field does not specify a fixed ability format. Using the query parameter `?cacao-can=` to explicitly scope abilities to the resources is RECOMMENDED. If no ability is specified, the UCAN parser MUST interpret the ability as the superuser `*/*`. If several `cacao-can` fields are given, then the UCAN parser MUST treat this is multiple separate claims.





!! FIXME add resource scoping upstream !!





### 2.1.1 Examples


CACAO

``` json
{
  "h": {
    "t": "eip4361"
  },
  "p": {
    "aud": "http://localhost:3000/login",
    "exp": "2022-03-10T18:09:21.481+03:00",
    "iat": "2022-03-10T17:09:21.481+03:00",
    "iss": "did:pkh:eip155:1:0xBAc675C310721717Cd4A37F6cbeA1F081b1C2a07",
    "nbf": "2022-03-10T17:09:21.481+03:00",
    "nonce": "328917",
    "domain": "localhost:3000",
    "version": "1",
    "requestId": "request-id-random",
    "resources": [
      "ipfs://bafybeiemxf5abjwjbikoz4mc3a3dla6ual3jsgpdr4cjr3oz3evfyavhwq",
      "https://example.com/my-web2-claim.json"
    ],
    "statement": "I accept the ServiceOrg Terms of Service: https://service.org/tos"
  },
  "s": {
    "s": "5ccb134ad3d874cbb40a32b399549cd32c953dc5dc87dc64624a3e3dc0684d7d4833043dd7e9f4a6894853f8dc555f97bc7e3c7dd3fcc66409eb982bff3a44671b",
    "t": "eip191"
  }
}
```

UCAN

``` js
{
  alg: "eip4361",
  typ: "JWT",
  ucv": "0.9.1"
}
{
  iss: "did:pkh:eip155:1:0xBAc675C310721717Cd4A37F6cbeA1F081b1C2a07",
  aud: "did:web:localhost:3000/login",
  iat: 1646924961, // FIXME Switch to facts for IPLD reasons? Change ucan-ipld? Just ignore this field because it doesn't make sense in trustless use cases?
  nbf: 1646924961,
  exp: 1646924961,
  nnc: "328917",
  fct: [
    {"domain": "localhost:3000"}, // Move inside each cap?
    {"cacao.version": "1"},
    {"requestId": "request-id-random"},
    {"statement": "I accept the ServiceOrg Terms of Service: https://service.org/tos"}
  ],
  att: [
    {
      with: "ipfs://bafybeiemxf5abjwjbikoz4mc3a3dla6ual3jsgpdr4cjr3oz3evfyavhwq",
      can: "*/*"
    },
    {
      with: "https://example.com/my-web2-claim.json",
      can: "*/*"
    }
  ]
}
"5ccb134ad3d874cbb40a32b399549cd32c953dc5dc87dc64624a3e3dc0684d7d4833043dd7e9f4a6894853f8dc555f97bc7e3c7dd3fcc66409eb982bff3a44671b"
```


# 3 IPLD


# 4 Acknowledgments




