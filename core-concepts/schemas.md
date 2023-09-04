# Schemas

A schema is a blueprint for an attestation.  It describes the various fields an attestation contains and what their datatypes are.  It also describes any canonical links to other attestations that an attestation should have.  Anyone can create a schema in the registry, and once created, schemas can be re-used by anyone else.

Schemas are stored in the registry as a string value that describes the various field.  For example, to create attestations that describe a person, we can create a schema as follows:\
\
`firstName string, lastName string`

This describes a schema with two fields, both of type string.  Any attestation based on this schema can be decoded in Solidity as follows:

`(firstName, lastName) = abi.decode(attestationData, (string, string)`)

As you can see from this example, a schema is more or less a comma-separated list of tuples of datatype and property name.

## Nested Data vs. Linked Data

The convention in Verax is to use linked data rather than nested data, so for for example, to create an attestation of a "_person_" that lives at a "_place_", one would first create a **Place** schema, and then you would create a **Person** schema with a canoncial relationship field, denoted by round braces:

`firstName string, lastName string, ( isResidentAt Place 0xa1b2c3 )`&#x20;

... where `isResidentAt` is the relationship type, `Place` is the schema name, and `0xa1b2c3` is the schema id.  This indicates that any attestation based on the **Person** schema is expected to be linked to some other **Place** attestation via a relationship attestation that links them together.

This approach reduces redundant attestations, and allows for more fine-grained and reusable schemas, contributing to a growing standard library of well known and widely used schemas.

Similarly, in order to create a one-to-many canonical relationship field, you would use the syntax:

`firstName string, lastName string, [( isResidentAt Place 0xa1b2c3 )]`&#x20;

If you do decide that your schema requires nested data instead of linked data, you can use the nested data syntax, which uses curly braces instead of round braces:\
\
`firstName string, lastName string, isResidentAt {Street string, City string}`

or:

`firstName string, lastName string, isResidentAt [{Street string,City string}]`

### Relationship Attestations

The examples above use what is called a "_relationship attestation_", which is any attestation based on the special `Relationship` schema.  The relationship schema conforms to the following structure:

`subject bytes32, predicate string, object bytes32`

This `Relationship` schema exists as a first class citizen of the registry, and attestations that are based on this schema are used for linking other attestations together.  The `subject` field is the attestation that is being linked to another attestation, the `predicate` field is a name that describes the _type_ of relationship, and the `subject` is the attestation being linked to.

Examples of relationship attestations are:

* `0x46582...` "isFollowerOf" `0x10345...`
* `0x31235...` "hasVotedFor" `0x52991...`
* `0x74851...` "isAlumniOf" `0x31122...`

Anyone can create any type of relationship between any attestation and any number of other attestations, allowing for the emergence of an organic [folksonomy](https://en.wikipedia.org/wiki/Folksonomy).  However, it also makes canonical relationships important to define in the schema, as otherwise there will be ambiguity between which relationship attestations were intended by the attestation issuer, and which were relationship attestations that were arbitrarily added later by third parties.

## Triples, Quads and Named Graphs

Many readers will have recognised that the Relationship schema is actually just an [RDF triple](https://en.wikipedia.org/wiki/Semantic\_triple).  The fact that RDF triples are used to link attestations to each other means that the on-chain attestation data can easily be indexed into a graph DB and can be serialized using RDFS, OWL, Turtle etc.

Certain use cases may require that relationships be grouped together into a named graph.  In this case there is a special `namedGraphRelationship` schema that can be utilized, which looks like:

`namedGraph string, subject bytes32, predicate string, object bytes32`

## Counterfactual Schemas

Schema ids are unique to the schema content, and are created from the keccak hash of the schema string.  This means that schemas can be created counterfactually, allowing for things like circular relationships between schemas.  It also means that two people can't create the exact same schema, which in turn, promotes re-usability within the registry.

## Inheritance

Schemas can also inherit fron other schema, which is another way that Verax reduces redundant schema data and promotes reusability.  To inherit from another schema, simply add the parent schema id at the very start of the schema string preceded by the `@extends` keyword, e.g.:

`@extends 0xa1b2c3... firstName string, lastName string`

This will tell indexers to look up the schema referenced by the `extends` keyword, and concatentate it's schema string with the schema string in this schema.  Note that any conflicting field names will be overridden by the last previous definition, so for example, if a field name exist in a parent schema and a child schema, the field definition from the child schema will be used.  Also, schemas can only inherit from one parent at a time.

**NOTE:** caution should be taken with this feature as it it somewhat experimental and may not be supported by all indexers!

## Schema MetaData

As well as the schema string, schemas have some metadata that is stored with them:

<table><thead><tr><th width="162.33333333333331">Property</th><th width="108">Datatype</th><th>Description</th></tr></thead><tbody><tr><td>name</td><td>string</td><td>(r<em>equired)</em> The name of schema, stored on-chain</td></tr><tr><td>description</td><td>string</td><td>A link to off-chain description / documentation</td></tr><tr><td>context</td><td>string</td><td>(r<em>equired)</em> A link to some shared vocabulary / ontology</td></tr><tr><td>schema</td><td>string</td><td><em>(required)</em> The raw schema string as described above</td></tr></tbody></table>

## Schema Contexts / Shared Vocabularies

Schemas have a field called "_context_" which is usually a link to some shared vocabulary.  This is an important field that gives semantic meaning to the schema, allowing consumers of the attestation to understand exactly what the fields in the schema represent.  This removes ambiguity and also allows reputation protocols to build powerful semantic graphs when indexing attestations.

For more information on this property and how important it is, see the [Linked Data](linked-data.md) page.

