# Create a Portal

[Portals](../../core-concepts/portals.md) are smart contracts that are registered in the "Portal Registry" and that you can consider as the entry point to the Verax Attestation Registry. This is where the payloads to be attested start their journey.

There are two ways of creating a Portal: use a default Portal or create your own custom Portal. I you just need an entry point to the Verax Attestation Registry, go for the [default Portal](create-a-portal.md#using-a-default-portal). But if you want to add your own custom logic to this entry point, go for a [custom Portal.](create-a-portal.md#using-a-custom-portal)

## Using a default Portal

It is possible to deploy a Portal without writing a single line of code, by simply calling the `deployDefaultPortal` function on the Portal Registry contract. This function accepts the following parameters:

```solidity
function deployDefaultPortal(
  address[] calldata modules,
  string memory name,
  string memory description,
  bool isRevocable,
  string memory ownerName
)
```

Descriptions for the parameters are as follows:

<table><thead><tr><th width="160.08201438848917">Parameter</th><th width="114">Datatype</th><th>Description</th></tr></thead><tbody><tr><td>modules</td><td>address[]</td><td>Address of the modules to execute for all attestations</td></tr><tr><td>name</td><td>string</td><td>A descriptive name for the portal</td></tr><tr><td>description</td><td>string</td><td>A description of the portal's functionality</td></tr><tr><td>isRevocable</td><td>bool</td><td>Whether attestations issued by the portal can be revoked</td></tr><tr><td>ownerName</td><td>string</td><td>The portal owner's name</td></tr></tbody></table>

Once you have created, deployed and registered your portal, you are ready to issue your first attestation!

And, of course, you can also do this operation via the Verax SDK:

```typescript
await this.veraxSdk.portal.deployDefaultPortal(
        modules: [],
        name: "ExamplePortal",
        description: "This Portal is used as an example",
        isRevocable: true,
        ownerName: "Verax",
);
```

{% hint style="danger" %}
With a default Portal, the `withdraw` function is not implement. You won't be able to withdraw funds collected while issuing attestations!
{% endhint %}

## Using a custom Portal

A Portal must implement the `AbstractPortal` contract to be considered as valid by Verax. Hopefully, we provide all the core contracts of the Verax platform as an npm package to help developers create their own custom implementations.

* Install the dependency: `npm i @verax-attestation-registry/verax-contracts`
*   Import the `AbstractPortal` contract:\


    ```solidity
    import {AbstractPortal} from "@verax-attestation-registry/verax-contracts/contracts/abstracts/AbstractPortal.sol";
    ```
*   Define your custom Portal:\


    ```solidity
    contract ExamplePortal is AbstractPortal { ... }
    ```

And now ... the floor is yours! You can add your custom functions, of course, but also use the hooks exposed by the `AbstractPortal`. They will help you add some custom logic in the main processes.



{% hint style="danger" %}
Don't forget to implement the `withdraw` function if you plan on collecting a fee to issue attestations!
{% endhint %}

## Hooks

### `_onAttest`

This hook is called during the attestation issuance process, _after_ the modules are run, and _before_ the payload is sent to the `AttestationRegistry`.

### `_onReplace`

This hook is called during the attestation replacement process, _after_ the modules are run, and _before_ the payload is sent to the `AttestationRegistry`.

### `_onBulkAttest`

This hook is called during the bulk attestation issuance process, _after_ the modules are run, and _before_ the payloads are sent to the `AttestationRegistry`.

### `_onBulkreplace`

This hook is called during the bulk attestation replacement process, _after_ the modules are run, and _before_ the payloads are sent to the `AttestationRegistry`.

### `_onRevoke`

This hook is called during the attestation revocation process, _before_ the revocation is registered at the `AttestationRegistry` level.

### `_onBulkRevoke`

This hook is called during the bulk attestation revocation process, _before_ the revocation are registered at the `AttestationRegistry` level.

## Example

Here is a simple custom Portal example:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.21;

import {Ownable} from "@openzeppelin/contracts/access/Ownable.sol";
import {AbstractPortal} from "@verax-attestation-registry/verax-contracts/contracts/abstracts/AbstractPortal.sol";
import {AttestationPayload} from "@verax-attestation-registry/verax-contracts/contracts/types/Structs.sol";

contract ExamplePortal is AbstractPortal, Ownable {

    error InsufficientFee();
    error WithdrawFail();

    constructor(address[] memory modules, address router) AbstractPortal(modules, router) {
    }

    function _onAttest(
        AttestationPayload memory /*attestationPayload*/,
        address /*attester*/,
        uint256 value
    ) internal pure override {
        if (value < 1000000000000000) revert InsufficientFee();
    }

    function withdraw(address payable to, uint256 amount) external override onlyOwner {
        (bool s,) = to.call{value: amount}("");
        if (!s) revert WithdrawFail();
    }
}
```

This `ExamplePortal` implements the `AbstractPortal` contract, but is also an `Ownable` contract. This means that you can clearly add a lot of features to your Portal, for example via the OpenZeppelin contracts.

In this example, we are using the `_onAttest` hook to add some logic in the attestation issuance process. In this case, it verifies that the issuing transaction is paying a 0.001 ETH fee. If this condition is not met, an error is thrown.

This example also implements the `withdraw` function, to be able to get the money from the fees out of the contract. Which is obviously a good idea...
