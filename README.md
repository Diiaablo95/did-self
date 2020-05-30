# DID self method specification
## Author
* Nikos Fotiou, [Mobile Multimedia Laboratory, AUEB](https://mm.aueb.gr)

## Abstract
The did self method enables DID document management without registries. Each
DID owner is responsible for maintaining their DID documents and the corresponding
revision history.

## DID Method 
The name of this DID method is: `self`

The method specific identifier is represented as the hex-encoded value of the last 20 bytes
of a Ed25519 public key (akin to how Ethereum addresses are generated). 

## CRUD Operation Definitions
### Create
In order to create a DID, a key pair needs to be generated. The public key is the initial
DID `controller`. The create method generates a minimal DID document which is referred to as
the `genesis` document. The `genesis` document includes a proof generated using the controller's
key.    

A DID document for the `did:self:ad6a3d9f938e13cd947ec05abc7fe734df8dd8a2` would look like this:

```
{
  "@context": "https://w3id.org/did/v1",
  "id": "did:self:ad6a3d9f938e13cd947ec05abc7fe734df8dd8a2",
  "publicKey": [{
    "id": "did:self:ad6a3d9f938e13cd947ec05abc7fe734df8dd8a2#key1",
    "type: "ED25519SignatureVerification",
    "owner: "did:self:ad6a3d9f938e13cd947ec05abc7fe734df8dd8a2",
    "publicKeyBase64": ""
  },
  "authentication": [{
       "type": "ED25519SigningAuthentication",
       "publicKey": "did:self:ad6a3d9f938e13cd947ec05abc7fe734df8dd8a2#key1"
  }],
  "proof": {
    type: 'Ed25519Signature2018',
    created: '2020-05-30T11:32:14Z',
    jws: 'eyJhbGc..',
    proofPurpose: 'assertionMethod',
    verificationMethod: 'did:self:ad6a3d9f938e13cd947ec05abc7fe734df8dd8a2#key1'
  }
}
```

### Update
The update method, replaces an existing DID document with a new one. In order to distinguish the various versions of each DID document, we append to the `id` field a `serial number` (preceded by
the `#` symbol) which is increased by one every time a new version of the document is generated. The DID document with serial number `1` must include a proof that can be verified by the authentication key of the `genesis` document, whereas any DID document with serial number `X|X>1` must include a proof that can be verified by the authentication key of the document that corresponds to serial number `X-1`.

An update to the genesis document of `did:self:ad6a3d9f938e13cd947ec05abc7fe734df8dd8a2`, that modifies
the authentication key would look like this:
```
{
  "@context": "https://w3id.org/did/v1",
  "id": "did:self:ad6a3d9f938e13cd947ec05abc7fe734df8dd8a2#1",
  "publicKey": [{
    "id": "did:self:ad6a3d9f938e13cd947ec05abc7fe734df8dd8a2#key2",
    "type: "ED25519SignatureVerification",
    "owner: "did:self:ad6a3d9f938e13cd947ec05abc7fe734df8dd8a2",
    "publicKeyBase64": ""
  },
  "authentication": [{
       "type": "ED25519SigningAuthentication",
       "publicKey": "did:self:ad6a3d9f938e13cd947ec05abc7fe734df8dd8a2#key2"
  }],
  "proof": {
    type: 'Ed25519Signature2018',
    created: '2020-06-1T11:32:14Z',
    jws: 'ab3hbGc..',
    proofPurpose: 'assertionMethod',
    verificationMethod: 'did:self:ad6a3d9f938e13cd947ec05abc7fe734df8dd8a2#key1'
  }
}
```


