# Create a Module

Modules are smart contracts that are registered in the registry that perform specific validation logic on attestations before they are issued into the registry.

Modules are intended to perform a single specific function. They should be designed to be minimalistic, atomic, and reusable. Modules can be chained together in series so that they are executed in order, with any module in the chain being able to prevent an attestation being issued to the registry by simply reverting if its verification checks fail.

To create a module, you must deploy a smart contract that inherits the `AbstractModule` abstract contract, which looks like this:

```solidity
abstract contract AbstractModule {
  function run(
    AttestationPayload memory attestationPayload,
    bytes[] memory validationPayload,
    address txSender,
    uint256 value
  ) public virtual returns (bytes[] memory moduleValidationPayload);
}
```

The `run` function accepts two arguments, the first is the raw attestation data that will be stored in the registry, and the second is whatever other validation logic the module needs to execute its verification / transformation logic.

The validationData can be any data, which may encompass fields from the attestation metadata or additional data such as ZKP proof, a signature, merkle proof, and so on.

As well as implementing the `AbstractModule` interface, the module contract must also implement the [IERC165Upgradeable](https://github.com/OpenZeppelin/openzeppelin-contracts-upgradeable/blob/master/contracts/utils/introspection/IERC165Upgradeable.sol) interface, which involves including this function:

```solidity
function supportsInterface(bytes4 interfaceID) public pure override returns (bool) {
  return interfaceID == type(AbstractModule).interfaceId || interfaceID == type(IERC165Upgradeable).interfaceId;
}
```

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

<table><thead><tr><th width="173.84946236559142">Parameter</th><th>Description</th></tr></thead><tbody><tr><td>name</td><td>A descriptive name for the module that's stored on-chain</td></tr><tr><td>description</td><td>A link to an off-chain document that describes the module</td></tr><tr><td>moduleAddress</td><td>The address of the module smart contract</td></tr></tbody></table>

**A few caveats:** a module contract must be first deployed and cannot be registered twice under different names, and it must implement the interfaces described above. Also, the `name` parameter must not be empty.

***

To dive deeper on exactly how the `ModuleRegistry` contract works, you can check out the [source code on GitHub](https://github.com/Consensys/linea-attestation-registry/blob/dev/contracts/src/ModuleRegistry.sol).

Once you have deployed one or more schemas and optionally also deployed one or more modules, you are ready to register a [portal](create-a-portal.md) and tie them all together so that you can issue your first attestations.
