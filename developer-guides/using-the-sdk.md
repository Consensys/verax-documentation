# 🛠 Using the SDK

{% hint style="danger" %}
Please advised that the SDK is currently under development is not currently available.  This documentation is a work in progress and is being continually revised.
{% endhint %}

## Installation

VeraxSDK is a [npm package](https://www.npmjs.com/package/verax-sdk/).

```
# npm
npm i --save @verax-attestation-registry/verax-sdk
```

```
# yarn
yarn add @verax-attestation-registry/verax-sdk
```

***

## Getting Started <a href="#user-content-getting-started" id="user-content-getting-started"></a>

### 1. Import VeraxSdk <a href="#user-content-1-import-veraxsdk" id="user-content-1-import-veraxsdk"></a>

```
// CommonJS
var VeraxSdk = require("@verax-attestation-registry/verax-sdk");
```

```
// ES6
import { VeraxSdk } from "@verax-attestation-registry/verax-sdk";
```

### 2. Instantiate VeraxSdk <a href="#user-content-2-instantiate-veraxsdk" id="user-content-2-instantiate-veraxsdk"></a>

<pre class="language-javascript"><code class="lang-javascript">// Default configuration for Linea Testnet

// Frontend
const veraxSdk = new VeraxSdk(VeraxSdk.DEFAULT_LINEA_TESTNET_FRONTEND);
// Backend
<strong>const veraxSdk = new VeraxSdk(VeraxSdk.DEFAULT_LINEA_TESTNET);
</strong></code></pre>

or:

```javascript
// Default configuration for Linea Mainnet
// Frontend
const veraxSdk = new VeraxSdk(VeraxSdk.DEFAULT_LINEA_MAINNET_FRONTEND);
// Backend
const veraxSdk = new VeraxSdk(VeraxSdk.DEFAULT_LINEA_MAINNET);
```

or:

```javascript
// Custom configuration

import { optimism } from "viem/chains";

const myVeraxConfiguration = {
  chain: optimism,
  mode: SDKMode.BACKEND,
  subgraphUrl: "https://my.subgraph.url",
  portalRegistryAddress: "0xMyPortalRegistryAddress",
  moduleRegistryAddress: "0xMyModuleRegistryAddress",
  schemaRegistryAddress: "0xMySchemaRegistryAddress",
  attestationRegistryAddress: "0xMyAttestationRegistryAddress",
};

const veraxSdk = new VeraxSdk(myVeraxConfiguration);
```

***

## Read and write objects <a href="#user-content-read-and-write-objects" id="user-content-read-and-write-objects"></a>

### 1. Get DataMappers <a href="#user-content-1-get-datamappers" id="user-content-1-get-datamappers"></a>

```javascript
// Each Verax class has its corresponding DataMapper
// Get them from the SDK instance
const portalDataMapper = veraxSdk.portal; // RW Portals
const schemaDataMapper = veraxSdk.schema; // RW Schemas
const moduleDataMapper = veraxSdk.module; // RW Modules
const attestationDataMapper = veraxSdk.attestation; // RW Attestations
const utilsDataMapper = veraxSdk.utils; // Utils
```

### 2. Read content (one object) <a href="#user-content-2-read-content-one-object" id="user-content-2-read-content-one-object"></a>

Each DataMapper comes with the method `findOneById` to get one object by Id.

{% code fullWidth="true" %}
```javascript
const myPortal = await portalDataMapper.findOneById("0x34798a866f52949208e67fb57ad36244024c50c0");

const mySchema = await schemaDataMapper.findOneById("0xce2647ed39aa89e6d1528a56deb6c30667ed2aae1ec2378ec3140c0c5d98a61e");

const myModule = await moduleDataMapper.findOneById("0x4bb8769e18f1518c35be8405d43d7cc07ecf501c");

const mySchema = await schemaDataMapper.findOneById("0xce2647ed39aa89e6d1528a56deb6c30667ed2aae1ec2378ec3140c0c5d98a61e");

const myAttestation = await attestationDataMapper.findOneById("0x000000000000000000000000000000000000000000000000000000000000109b");
```
{% endcode %}

### 3. Read content (list / many objects) <a href="#user-content-3-read-content-list--many-objects" id="user-content-3-read-content-list--many-objects"></a>

Each DataMapper comes with the method `findBy` to get objects by criteria.

<pre class="language-javascript" data-full-width="true"><code class="lang-javascript">//
// args:
<strong>// 	- first: number (e.g first 50 records)
</strong>// 	- skip: number (e.g skip 1st record)
// 	- where: object {property1: value1, property2: value2, ...} (filter object e.g {name: "Msg Sender Module"})
// 	- orderBy: string (e.g "id")
// 	- orderDirection string (e.g "asc" or "desc")

const filter = { ownerName: "John" };
const myPortals = await this.veraxSdk.portal.findBy(5, 1, filter, "name", "desc");

const filter = { description: "Gitcoin Passport Score" };
const mySchemas = await this.veraxSdk.schema.findBy(2, 0, filter, "name", "asc");

const filter = { name_contains: "Msg" };
const myModules = await this.veraxSdk.module.findBy(1, 0, filter, "name", "desc");

const filter = { attester_not: "0x809e815596AbEB3764aBf81BE2DC39fBBAcc9949" };
const myAttestations = await this.veraxSdk.attestation.findBy(10, 0, filter, "attestedDate", "desc");

</code></pre>

### 4. Write content <a href="#user-content-4-write-content" id="user-content-4-write-content"></a>

Each dataMapper comes with write methods, that may vary depending on the class. See the detail of write method per class dataMapper.

{% code fullWidth="true" %}
```javascript
const portalAddress = "0xeea25bc2ec56cae601df33b8fc676673285e12cc";
const attestationPayload = {
        schemaId: "0x9ba590dd7fbd5bd1a7d06cdcb4744e20a49b3520560575cd63de17734a408738",
        expirationDate: 1693583329,
        subject: "0x828c9f04D1a07E3b0aBE12A9F8238a3Ff7E57b47",
        attestationData: [{ isBuidler: true }],
      };
const validationPayloads = [];
const newAttestation = await this.veraxSdk.portal.attest(portalAddress, attestationPayload, validationPayloads));
```
{% endcode %}

***

## Other operations <a href="#user-content-other-operations" id="user-content-other-operations"></a>

\[Work in progress] The class `veraxSdk.utils` extends the capabilities:

* precompute the id of an attestation
* encode decode payload
