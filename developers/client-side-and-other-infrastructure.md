---
description: COMING SOON!
---

# Client Side and Other Infrastructure

Two problems that Ethereum has is storing large amounts of data, and querying data from smart contracts. The solution for storage is using a distributed storage layer such as [IPFS](https://ipfs.tech/). This can store the build for the client side assets, as well as documents that need to be stored such as the evidence or meta evidence document. There is various solutions for querying data such as [Moralis](https://moralis.io/), which for our use is too centralized, and there are also _more_ decentralized solutions such as [The Graph](https://thegraph.com/).&#x20;

The Graph makes it possible to do more complex querying such as \[aggregation, search, relationships, and non-trivial filtering (do i have to quote this?)], by having a network of indexers incentivized by The Graph _GRT_ token, that index data from smart contract events, and store it in predefined data types. Complex queries can be performed on this data by using [GraphQL](https://graphql.org/) queries to indexers. This allows for a rich UI experience. Although this creates an additional dependency on  a DAO, The Graph is only used for querying data and has no ability to do any write operations. In the unlikely event that something happens to The Graph network, or a big portion of people decide to rug the project, some other solution could be deployed to correct for the error without any data losses.

## IPFS

## The Graph
