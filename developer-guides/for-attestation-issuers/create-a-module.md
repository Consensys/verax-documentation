# Create a Module

[Modules](../../core-concepts/modules.md) are smart contracts that are registered in the "Module Registry" and that perform specific validation logic on attestations before they are issued into the registry.

{% hint style="info" %}
The Module mechanism may change in the future, following [Verax Improvement Proposal #5](https://community.ver.ax/t/allow-variable-modules-in-portals/51/2) and what we call "[Modules V2](https://github.com/Consensys/linea-attestation-registry/pull/562)".
{% endhint %}

## Using a custom Module

A Module must implement the `AbstractModule` contract to be considered as valid by Verax. Hopefully, we provide all the core contracts of the Verax platform as an npm package to help developers create their own custom implementations.

* Install the dependency: `npm i @verax-attestation-registry/verax-contracts`
*   Import the `AbstractModule` contract:\


    ```solidity
    import {AbstractModule} from "@verax-attestation-registry/verax-contracts/contracts/abstracts/AbstractModule.sol";
    ```
*   Define your custom Module:\


    ```solidity
    contract ExampleModule is AbstractModule { ... }
    ```

And now ... the floor is yours! You can add your custom functions, of course, but mostly you need to implement the `run` function that is called in the validation process, before registering the attestation payload.

## Example

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.21;

import {AbstractModule} from "@verax-attestation-registry/verax-contracts/contracts/abstracts/AbstractModule.sol";
import {AttestationPayload} from "@verax-attestation-registry/verax-contracts/contracts/types/Structs.sol";

contract ExampleModule is AbstractModule {

    error InsufficientFee();

    function run(
        AttestationPayload memory /*attestationPayload*/,
        bytes memory /*validationPayload*/,
        address /*txSender*/,
        uint256 value
    ) public pure override {
        if (value < 1000000000000000) revert InsufficientFee();
    }
}
```

This `ExampleModule` implements the `AbstractModule` contract.

In this example, we are checking that the paid value for this attestation is at least 0.001 ETH. If it is the case, the process goes onto the next module check, and if not, the process stops there and the transaction reverts.
