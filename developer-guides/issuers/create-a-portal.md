# Create a Portal

## Creating a Portal

In order to create a portal, you must first deploy a contract which inherits the [`AbstractPortal`](https://github.com/Consensys/linea-attestation-registry/blob/cd8f14463d5e96718b021bb3f66a9467e7c0ea3a/src/interface/AbstractPortal.sol) abstract contract.  This portal contract is where you create attestations in the registry.  You have full control over the logic in this contract, so long as it inherits the base portal contract.  There are two main functions that you need to create:

```solidity
function attest(
    AttestationPayload attestationPayload,
    bytes memory validationPayload
  ) external payable virtual returns (bool);
```

This function allows you to actually create attestations, you can call the various modules and/or apply any other logic you need to.  The convention is to keep as much logic as possible in modules, but it's up to you how you implement your own domain logic.\
\
The `AttestationPayload` struct is specified as follows:

```solidity
struct AttestationPayload {
  bytes32 schemaId; // The identifier of the schema this attestation adheres to
  address attester; // The address issuing the attestation to the subject
  bytes subject; // The ID of the attestee, EVM address, DID, URL etc.
  uint256 expirationDate; // The expiration date of the attestation
  bytes[] attestationData; // The attestation data
}
```

The other function that you need to implement is the `getModules` function, which simply returns the array of modules that your portal uses (if any):

```solidity
function getModules() external virtual returns (address[] memory);
```

Once you have deployed your portal contract, you simply call the `registerPortal` function on the registry contract, providing the portal contract address, as well as the portal name and description.  Note however that any modules you specify on the module chain or for the lifecycle hooks will be to be deployed and registered first.  For more details on the smart contract calls and the required arguments, see the [Create a Portal](create-a-portal.md) page.

{% hint style="warning" %}
While you can put whatever logic you want to in your portal contracts, it is strongly advised that you keep you portal as modular as possible, which means keeping your logic in modules.  In the future, we may pivot to no-code portals, which have no contract, and which simply execute a specific chain of modules!
{% endhint %}

One important caveat is that initially creating a portal requires that you have a "_issuer_" attestation in the registry.  To acquire one of these attestations, you simply need to reach out to any of the core contributors.  In the early stages of the project, we will be in a closed beta, which means you'll need to get one of these attestations before deploying a portal. Getting an issuer attestation is as easy as getting in touch using any of our [communication channels](../../get-involved/get-in-touch.md).

Once you have deployed you portal, you are ready to issue your first attestation!
