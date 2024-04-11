# Create an Attestation

After determining the Schema and potential Modules to use, and after the deployment and registration of your Portal, you can begin creating [Attestations](../../core-concepts/attestations.md).

## Attestation registration

Attestation registration at the Portal level takes 2 parameters, defined as follows:

<table><thead><tr><th width="190.54206896551727">Property</th><th>Description</th></tr></thead><tbody><tr><td>attestationPayload</td><td>The payload to attest</td></tr><tr><td>validationPayload</td><td>The payloads to validate via the modules to issue the attestation</td></tr></tbody></table>

`attestationPayload` is defined in Solidity as follows:

```solidity
struct AttestationPayload {
  bytes32 schemaId;
  uint64 expirationDate;
  bytes subject;
  bytes attestationData;
}
```

<table><thead><tr><th width="190.54206896551727">Property</th><th>Description</th></tr></thead><tbody><tr><td>schemaId</td><td>The identifier of the schema this attestation adheres to</td></tr><tr><td>expirationDate</td><td>The expiration date of the attestation</td></tr><tr><td>subject</td><td>The ID of the attestee, EVM address, DID, URL etc</td></tr><tr><td>attestationData</td><td>The attestation data</td></tr></tbody></table>

## Manually registering an Attestation in the `AttestationRegistry` contract

To register an Attestation into the `AttestationRegistry` contract, you need to go through the Portal you have previously deployed an registered, using the attest function:

```solidity
function attest(
  AttestationPayload memory attestationPayload,
  bytes[] memory validationPayload
) public payable;
```

When attesting, the registry performs a set of integrity checks on the new attestation:

* verifies the `schemaId` exists
* verifies the `subject` field is not blank
* verifies the `attestationData` field is not blank

## Using a blockchain explorer

Instead of drafting the csmart contract call by hand, you can benefit from a chai explorer interface. Let's use the Linea testnet explorer, [Lineascan](https://goerli.lineascan.build/).



## Attestation Metadata

When creating an attestation, you simply need to specify four properties, as outlined above, however, the attestation registry itself automatically populates the other fields in the attestation metadata at the point at which it is created. The full list of attestation metadata is included below:

<table><thead><tr><th width="180.54206896551727">Property</th><th width="114.33333333333331">Datatype</th><th>Description</th></tr></thead><tbody><tr><td>attestationId</td><td>bytes32</td><td>The unique identifier of the attestation</td></tr><tr><td>schemaId</td><td>bytes32</td><td>The identifier of the <a href="../../core-concepts/schemas.md">schema</a> this attestation adheres to</td></tr><tr><td>replacedBy</td><td>uint256</td><td>The attestation ID that replaces this attestation</td></tr><tr><td>attester</td><td>address</td><td>The address issuing the attestation to the subject</td></tr><tr><td>portal</td><td>address</td><td>The address of the <a href="../../core-concepts/portals.md">portal</a> that created the attestation</td></tr><tr><td>attestedDate</td><td>uint64</td><td>The date the attestation is issued</td></tr><tr><td>expirationDate</td><td>uint64</td><td>The expiration date of the attestation</td></tr><tr><td>revocationDate</td><td>uint64</td><td>The date when the attestation was revoked</td></tr><tr><td>version</td><td>uint16</td><td>Version of the registry when the attestation was created</td></tr><tr><td>revoked</td><td>bool</td><td>Whether the attestation is <a href="revoke-an-attestation.md">revoked</a> or not</td></tr><tr><td>revocationDate</td><td>uint64</td><td>If revoked, the date it was revoked / replaced</td></tr><tr><td>subject</td><td>bytes</td><td>The ID of the attestee e.g. an EVM address, DID, URL etc.</td></tr><tr><td>attestationData</td><td>bytes</td><td>The raw attestation data</td></tr></tbody></table>

The attestationId is derived from an internal auto-incremented counter, converted from uint32 to byte32. This corresponds to the total attestations issued.

Once the attestation is successfully created in the registry, an `AttestationRegistered` event is emitted with the attestation ID, and subsequently picked up by indexers.
