# Create a Module

Modules are smart contracts that are registered in the registry that perform specific validation and / or transformation logic on attestations before they are issued into the registry.

Modules are intended to perform a single function, and should be designed in a minimal way so as to be atomic and reusable.  Modules can be chained together in series so that the output from one becomes the input of the next, resulting the output from the last module in the chain becoming the attestation that's stored in the registry.

To create a module, you must deploy a smart contract that implements the `ModuleInterface` interface, which looks like this:

```solidity
interface ModuleInterface {
    function run(
        bytes memory attestationData,
        bytes memory validationData
    ) external view returns (bytes, bytes);
}
```

The `run` function accepts two arguments, the first is the raw attestation data that will be stored in the registry, and the second is whatever other validation logic the module needs in order to execute it's verification / transformation logic.

Both arguments are passed in the transaction that creates the attestation.  The `validationData` can be any data, and can include fields from the attestation metadata, or other accompanying data, such as ZKP proof for example.

As well as implementing the `ModuleInterface` interface, the module contract must also implement the [ERC-165](https://eips.ethereum.org/EIPS/eip-165) interface, which looks like this:

```solidity
interface ERC165 {
    function supportsInterface(bytes4 interfaceID) external view returns (bool);
}
```

This interface should return true when called with the value `0xTBD...` which is the 4byte signature of the `run` function described above.

## Registering a deployed Module contract

Once you have deployed your module contract, you can then register it in the `ModuleRegistry` contract using the `register` function:

```solidity
function register(
    string memory name,
    string memory description,
    address moduleAddress
);
```

The arguments that this function accepts are:

<table><thead><tr><th width="172">Parameter</th><th>Description</th></tr></thead><tbody><tr><td>name</td><td>A descriptive name for the module that's stored on-chain</td></tr><tr><td>description</td><td>A link to an off-chain document that describes the module</td></tr><tr><td>moduleAddress</td><td>The address of the module smart contract</td></tr></tbody></table>

There a few caveats: a module contract must be first deployed and cannot be registered twice under different names and it must implement the interfaces described above.  Also the name parameter must not be empty.

***

To dive deeper on exactly how the `ModuleRegistry` contract works, you can check out the [source code on GitHub](https://github.com/Consensys/linea-attestation-registry/blob/dev/src/ModuleRegistry.sol).

Once you have deployed one or more schemas and optionally also deployed one or more modules, you ready to register a [portal](create-a-portal.md) and tie them all together so that you can issue your first attestations.
