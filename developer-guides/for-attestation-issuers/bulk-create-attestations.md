# Bulk Create Attestations

For certain use cases, issuers may want to create attestations in bulk to save on transaction fees. Verax has support for both bulk attestation creation and bulk revocation as well.

The `AbstractPortal` contract that every portal inherits from defines the following function:

```solidity
function bulkAttest(
    AttestationPayload[] memory attestationsPayloads,
    bytes[][] memory validationPayloads
) public payable;
```

This function is like the normal `attest` function but with a couple of differences. First, it takes an array of `AttestationPayload` structs as the first parameter, each one corresponding to a single attestation. Second, it takes a two-dimensional array of `validationPayloads` in the second parameter. The first dimension of the array is a validation payload for each respective attestation payload, the second dimension is the validation payload for each module in the module chain, to verify the respective attestation.

{% hint style="danger" %}
This method may have unexpected behavior if one of the checks is done on the attestation ID, as this ID won't be incremented before the end of the transaction. If you need to check the attestation ID, please use the \`attest\` method.
{% endhint %}

## Bulk Revocation of Attestations

Revoking attestations in bulk is basically the same as revoking a single attestation. This is the function that does it:

```solidity
function bulkRevoke(bytes32[] memory attestationIds, bytes32[] memory replacedBy);
```

The two arrays need to be the same size, with each element in the `replacedBy` parameter corresponding to the element in the same position in the `attestationIds` parameter. After that, the same verification checks are performed by the attestation registry.
