# 🌐 Using the Subgraph

A public subgraph that is maintained by the Linea team, and can be accessed from the following URL:

{% embed url="https://graph-query.linea.build/subgraphs/name/Consensys/linea-attestation-registry/graphql?query=query+MyQuery+{++attestations+{++++attestationData++++decodedData++++schemaString++}}" %}
Public subgraph on Linea mainnet
{% endembed %}

If you want to access the public subgraph on the Linea Sepolia, you can access it here:

{% embed url="https://api.studio.thegraph.com/proxy/67521/verax-v1-linea-sepolia/v0.0.12/graphql" %}
Public subgraph on Linea Sepolia
{% endembed %}

You can use this default web interface to write queries in GraphQL to search through the attestation registry. Alternatively, you can use a tool such as Postman, or use the subgraph's API to query the registry directly from your own dApp.

Examples of queries that you can make using the subgraph:

1.  Get all attestations, along with the respective schema string, and the decoded attestation data:

    ```graphql
    query MyQuery {
      attestations {
        attestationData
        decodedData
        schemaString
      }
    }
    ```
2.  Give me all attestations related issued to a specific address:

    ```graphql
    query MyQuery {
      attestations(where: {subject: "0xd14BF29e486DFC3836757b9B8CCFc95a5160A56D"}) {
        attestationData
        decodedData
        schemaString
      }
    }
    ```
3.  Give me all attestations issued to a specific address, related to a specific schema ID, that have not been revoked.

    ```graphql
    query MyQuery {
      attestations(
        where: {
          subject: "0xd14BF29e486DFC3836757b9B8CCFc95a5160A56D",
          schemaId: "0x7b2d17830782df831c39edcbd728a47f0a470d57fdf452b5f4226f467f48295e",
          revoked: false
        }
      ) {
        attestationData
        decodedData
        schemaString
      }
    }
    ```

As well as querying the attestation registry, you can query the schema registry, module registry, and portal registry. For example, if you want to browse the library of existing schemas:

```graphql
query SchemaQuery {
  schemas {
    name
    description
    id
    context
  }
}
```

***

To get more information on using the subgraph, please refer to [The Graph's documentation](https://thegraph.com/docs/en/).

The source code for our subgraph is available in our monorepo, where you find info on deploying your own subgraph if you want to.

{% embed url="https://github.com/Consensys/linea-attestation-registry/tree/dev/subgraph" %}
