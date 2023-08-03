# Modules

Modules are smart contracts that implement the `Module` interface and are registered in the registry.  They allow for attestation creators to run custom logic in order to do things like:

* verify that attestations conform to some business logic
* optionally transform the attestation data according to some pre-determined logic
* perform other actions like transferring some tokens or minting an NFT
* recursively create another attestation

Modules are specified in a [portal](portals.md) and all attestations created by that portal are routed through the specified modules.  Modules can also be chained together so that the output of one module becomes the input of the next module.

Each module exposes a public function called `run`:

`function run(bytes attestationPayload, bytes verificationPayload, uint256 value, address msgSender) public`

The function executes whatever logic it needs to, and returns the same four parameters as it received so that the next module can run it's own processing on them.

As well as implementing the `Module` interface, a module must also implement [ERC-165](https://eips.ethereum.org/EIPS/eip-165) to ensure that it can be verified properly when being registered.

## Module Metadata

Once the module smart contract is deployed, it can be registered with the following metadata:

<table><thead><tr><th width="179">Field</th><th width="120">Type</th><th>Description</th></tr></thead><tbody><tr><td>name</td><td>string</td><td>(required) A descriptive name for the module</td></tr><tr><td>description</td><td>string (URI)</td><td>(optional) A link to documentation about the module, it’s intended use etc.</td></tr><tr><td>moduleAddress</td><td>address</td><td>(required) The address of the module smart contract</td></tr></tbody></table>

The metadata above is intended to help to discover modules that can be reused once created.  Modues are chained together and executed in [portals](portals.md).  The next seciton dives into portals, what they are, and how to create them.
