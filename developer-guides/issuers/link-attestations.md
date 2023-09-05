# Link Attestations

Linking two attestations together is relatively straightforward.  It involves creating another attestation based on a specific schema called the  `Relationship` schema.

The `Relationship` schema looks like the following:

`bytes subject, string predicate, bytes32 object`

The schema id for the relationship schema is:

`0xce2647ed39aa89e6d1528a56deb6c30667ed2aae1ec2378ec3140c0c5d98a61e`

This `Relationship` schema exists as a first class citizen of the registry, and attestations that are based on this schema are used for linking other attestations together.  The `subject` field is the attestation that is being linked to another attestation, the `predicate` field is a name that describes the _type_ of relationship, and the `subject` is the attestation being linked to.

Examples of relationship attestations are:

* `0x46582...` "isFollowerOf" `0x10345...`
* `0x31235...` "hasVotedFor" `0x52991...`
* `0x74851...` "isAlumniOf" `0x31122...`

Anyone can create any type of relationship between any attestation and any number of other attestations, allowing for the emergence of an organic [folksonomy](https://en.wikipedia.org/wiki/Folksonomy).  However, it also makes canonical relationships important to define in the schema, as otherwise there will be ambiguity between which relationship attestations were intended by the attestation issuer, and which were relationship attestations that were arbitrarily added later by third parties.

## One-to-One, One-to-Many, Many-to-Many

You can create as many `Relationship` attestations as you want, so that one attestation can be related to many other attestations, and vice-versa.

## Named Graphs

Certain use cases may require that relationships to be grouped together into a "named graph".  This allows for ring-fencing a certain group of links, in order to single them out, or label / identify them among the many other relationships that may exist between two or more attestations.  In this case there is a special `namedGraphRelationship` schema that can be utilized, which looks like:

`string namedGraph, bytes32 subject, string predicate, bytes32 object`

The schema id for the named graph relationship schema is:

`0xcef5bf8ad0c26e06ece99156b529d7b473e14e9fbd6e69c342ec9acadca19628`
