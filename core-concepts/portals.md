# Portals

Portals are the entry point into the registry for attestations.  All attestations are made through portals.  A portal is normaly associated with a specific issuer, who would create a portal specifically to issue their attestations with, but portals can also be open to allow anyoen to use them.

A portal is basically chain of modules that attestations run through before being issued to the registry.  Portals also have metadata associated with them and can optionally execute lifecycle hooks.

## Portal Metadata

All portals contain certain metadata that is associated with them when they are registered:

<table><thead><tr><th width="170">Field</th><th width="120">Type</th><th>Description</th></tr></thead><tbody><tr><td>name</td><td>string</td><td>(required) A descriptive name for the module</td></tr><tr><td>description</td><td>string (URI)</td><td>(optional) A link to documentation about the module, it’s intended use etc.</td></tr><tr><td>owner</td><td>address</td><td>(required) The address of portal owner</td></tr><tr><td>ownerName</td><td>string</td><td>The human readable name of the owner (usually issuer)</td></tr><tr><td>modules</td><td>bytes32[]</td><td>The ids of the modules that should run for each attestation that is created by the portal</td></tr><tr><td>hooks</td><td>bytes32[]</td><td>The modules that shoud be run on each lifeycle hook</td></tr></tbody></table>

## Lifecycle Hooks

Each portal can specify optional lifecycle hooks that are executed at specific points in an attestations lifecycle.  To execute on a hook, a portal specifies which module should be run when a certain lifecycle hook is fired.  The available lifecycle hooks include:

* onIssue - executed when an attestation is first created
* onRevoke - executed when an attestation is revoked
* onAcknowledge - executed when an attestation is acknowledged by the subject it is issued to
* onReplace - executed when an attestation is replaced - this is the same as revoking an attestation and creating a new one, but it fires a "_Replace_" event instead of "_Revoke_" and "_Issue_" events

## Creating a Portal

In order to create a portal, yousimply call the `registerPortal` function on the portal registry contract, providing the metadata above as arguments to the function.  Note however that any modules you specify on the module chain or for teh lifecycle hooks will be to be deployed and regsitered first.  For more details on the smart contract calls and the required arguments, see the [Create a Portal](../developer-guides/issuers/create-a-portal.md) page.

One important caveat is that initially creating a portal requires that you have a "portal issuer" attestation in the registry.  To acquire one of these attestations, you simply need to reach out to any of the core contributors.  In the early stages of the project, we will be in a closed beta, which means you'll need to get one of these attestations before deploying a portal. Getting a portal issuer attestations is as easy as getting in touch using any of our [communication channels](../get-involved/get-in-touch.md)!
