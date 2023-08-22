# Create an Attestation

Once you have decide which schemas and/or modules you want to use (or have created your own) and have deployed and registered a portal, then you can start creating attestations.

To create an attestation, you can call the `attest` function on your portal contract directly:

```solidity
function attest(
    AttestationPayload attestationPayload,
    bytes memory validationPayload
  ) external payable virtual returns (bool);
```

The two argument are separated into 1) the actual attestation you wish to create, and 2) any extra data that is required in order to create an attestation.  For example, the `validationPayload` could be a zero-knowledge-proof that verifies the attestationPayload.\
\
The `AttestationPayload` struct is specified as follows:

```solidity
struct AttestationPayload {
  bytes32 schemaId; // The identifier of the schema this attestation adheres to
  bytes subject; // The ID of the attestee, EVM address, DID, URL etc.
  uint256 expirationDate; // The expiration date of the attestation
  bytes[] attestationData; // The attestation data
}
```

The attestation subject can be an EVM address, a DID, a URL or anything that you are issuing your attestation to.   The `attestationData` is the raw bytes that will be `abi.decoded` according the the schema id.

## Constraints on Creating Attestations

When calling the `attest` function, the registry performs certain integrity checks on the new attestation:

* &#x20;the `attestationId` does not already exist
* the `schemaId` exists
* the `subject` field is not blank
* the `attestationData` field is not blank

