# Portals

Portals are the entry point into the registry for attestations.  All attestations are made through portals.  A portal is normally associated with a specific issuer, who would create a portal specifically to issue their attestations with, but portals can also be open to allow anyone to use.

A portal is a smart contract that executes specific verification logic through a chain of modules  that attestations run through before being issued to the registry.  Portals also have metadata associated with them and can optionally execute lifecycle hooks.

## Portal Metadata

All portals contain certain metadata that is associated with them when they are registered:

<table><thead><tr><th width="170">Field</th><th width="120">Type</th><th>Description</th></tr></thead><tbody><tr><td>id</td><td>address</td><td>The portal id which is the address of portal contract</td></tr><tr><td>name</td><td>string</td><td>(required) A descriptive name for the module</td></tr><tr><td>description</td><td>string (URI)</td><td>(optional) A link to documentation about the module, it’s intended use etc.</td></tr><tr><td>modules</td><td>bytes32[]</td><td>The ids of the modules that should run for each attestation that is created by the portal</td></tr></tbody></table>

## Lifecycle Hooks

Each portal can specify optional lifecycle hooks that are executed at specific points in an attestations lifecycle.  Hooks are specific functions called at specific points, which include:

* **onBeforeCreate** - executed just before an attestation is first created
* **onAfterCreate** - executed just after an attestation is first created
* **onBeforeRevoke** - executed before an attestation is first revoked
* **onAfterRevoke** - executed after an attestation is first revoked
* **onBeforeReplace** - executed before an attestation is first replaced
* **onAfterReplace** - executed after an attestation is first replaced

To find out how to how to create a portal, see the [Create a Portal](../developer-guides/issuers/create-a-portal.md) page for more information.
