# Using the Subgraph

There is a public subgraph that is currently maintained by the Linea team, and it can be accessed from the following URL:

{% embed url="https://graph-query.linea.build/subgraphs/name/Consensys/linea-attestation-registry/graphql?query=query+MyQuery+%7B%0A++attestations+%7B%0A++++attestationData%0A++++id%0A++++expirationDate%0A++++portal%0A++++replacedBy%0A++++revocationDate%0A++%7D%0A%7D" %}
Public subgraph on Linea mainnet with default query
{% endembed %}

If you want to access the public subgraph on the Linea testnet, you can access it here:

{% embed url="https://graph-query.goerli.linea.build/subgraphs/name/Consensys/linea-attestation-registry/graphql?query=query+MyQuery+%7B%0A++attestations+%7B%0A++++attestationData%0A++++id%0A++++expirationDate%0A++++portal%0A++++replacedBy%0A++++revocationDate%0A++%7D%0A%7D" %}
Public subgraph on Linea testnet
{% endembed %}

You can use this default web interface to write queries in GraphQL in order to serarch through the attestation registry.  Alternatively you can use a tool such as Postman, or use the subgraph's API to query the regsitry directly from your own dapp.

As well as querying the attestation registry, you can query the schema registry, module registry and portal registry.

To get more information on using the subgraph, please refer to [The Graph's documentation](https://thegraph.com/docs/en/).

The source code for our subgraph is available in our monorepo:

{% embed url="https://github.com/Consensys/linea-attestation-registry/tree/dev/subgraph" %}
