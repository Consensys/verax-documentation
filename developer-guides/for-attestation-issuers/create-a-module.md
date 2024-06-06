# Create a Module \[WIP]

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

