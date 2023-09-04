# Revoke an Attestation

Revoking an attestation is very straightforward.  The revoke an attestation was previously created in the registry, you simply need to call the `revoke` function on the portal that first issued the attestation.  The `revoke` method is defined as:

```solidity
/**
 * @notice Revokes the attestation for the given identifier and can replace it by a new one
 * @param attestationId the attestation ID to revoke
 * @param replacedBy the replacing attestation ID (leave to just revoke)
 */
function revoke(bytes32 attestationId, bytes32 replacedBy) external virtual {
  _onRevoke(attestationId, replacedBy);
  attestationRegistry.revoke(attestationId, replacedBy);
}
```

The attestation registry will check that the portal actual allows revocations, as defined when the portal was first registered.  If a portal was registered with the `revocable` property set to `false` then it will reject any attempt to revoke an existing attestations.

The other property to be aware of is the `replacedBy` property, which should be set to the id of some attestation that is meant to replace the one being revoked.  If you wish to revoke an attestation without actually replacing it with another one, you can simply set this to `bytes32(0)`.

You can also place custom logic in the `_onRevoke` lifecycle hook to run any additional verification before the revocation takes place, or perform any other action that you wish.

Note that you can't revoke an attestation made by another portal, even if that portal allows revocation.
