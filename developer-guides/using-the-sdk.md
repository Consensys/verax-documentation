# 🛠 Using the SDK

{% hint style="danger" %}
Please be advised that the SDK is currently under development and is not currently available.  This documentation is a work in progress and is being continually revised.
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
import VeraxSdk from "@verax-attestation-registry/verax-sdk";
```

### 2. Instantiate VeraxSdk <a href="#user-content-2-instantiate-veraxsdk" id="user-content-2-instantiate-veraxsdk"></a>

```javascript
// Default configuration for Linea Testnet
const veraxSdk = new VeraxSdk(VeraxSdk.LineaConfTestnet);
```

or:

```javascript
// Default configuration for Linea Mainnet
const veraxSdk = new VeraxSdk(VeraxSdk.LineaConfMainnet);
```

or:

```javascript
// Custom configuration
const myVeraxConfiguration = {
  rpcUrl: "https://my.rpc.url",
  chain: 12345,
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
// Each Verax class has its DataMapper
// Get them from the SDK instance
const portalDataMapper = veraxSdk.portalDataMapper; // RW Portals
const schemaDataMapper = veraxSdk.schemaDataMapper; // RW Schemas
const moduleDataMapper = veraxSdk.moduleDataMapper; // RW Modules
const attestationDataMapper = veraxSdk.attestationDataMapper; // RW Attestations
```

### 2. Read content (one object) <a href="#user-content-2-read-content-one-object" id="user-content-2-read-content-one-object"></a>

Each DataMapper comes with the method `findOneById` to get one object by Id.

```javascript
const myAttestation = await attestationDataMapper.findOneById("12345");
console.log(myAttestation);

//
// id: "12345"
// schemaId: "99AE34"
// portalId: "37773"
// data: decoded payload {...}
// ...
```

### 3. Read content (list / many objects) <a href="#user-content-3-read-content-list--many-objects" id="user-content-3-read-content-list--many-objects"></a>

Each DataMapper comes with the method `findBy` to get objects by criterias.

```javascript
//
// args:
// 	- criterias: object {property1: value1, property2: value2, ...}
// 	- page: integer (optional, default 0)
// 	- offset: integer (optional, default 50, max= 500)
// 	- orderBy: string (optional, default createdAt)
// 	- order(property?): enum string "ASC", "DESC" (optional, default "DESC")
//
const myAttestations = await attestationDataMapper.findBy(
  { portalId: "37773", subject: "John" },
  4,
  30,
  "schemaId",
  "ASC",
);

console.log(myAttestations);
//
// totalNumber: 147,
// page: 0,
// objects: [
// 	{id: "12345", schemaId: "99AE34", portalId: "37773", subject: "Florian", ...},
// 	{id: "2221E", schemaId: "AAF77E", portalId: "37773", subject: "Florian", ...},
// 	...
// ]
//
```

### 4. Write content <a href="#user-content-4-write-content" id="user-content-4-write-content"></a>

\[WORK IN PROGRESS] Each dataMapper comes with write methods, that may vary depending on the class. See the detail of write method per class dataMapper.

```javascript
const result = await attestationDataMapper.attest({
  schemaId: "muhpoih"
  portalId: "OJOJ43432"
  expirationDate: null
  subject: "florian.demiramon@..."
  data: {
    foo: "bar",
    zee: "plop"
  }
}, signer);

console.log(result);
//
// txReceipt: ....
// object: {
//		{
//		id: "0XAZFZF"
//		schemaId: "muhpoih"
//		portalId: "OJOJ43432"
//		expirationDate: null
//		subject: "florian.demiramon@..."
//		attestationData: {
//				foo: "bar",
//				zee: "plop"
//			}
//		attestationRawData: "1234234124124FAAZC"
//		replacedBy: null
//		attester: "0x1111"
//		attestedDate: "today"
//		revocationDate: null
//		version: 1
//		revoked: false
//		}
//
```

***

## Other operations <a href="#user-content-other-operations" id="user-content-other-operations"></a>

\[Work in progress] The class `veraxSdk.utils` extends the capabilities:

* precompute the id of an attestation
* encode decode payload
