---
title: Primev Documentation
description: Welcome to Primev technical documentation; a comprehensive set of information and resources to help you understand, use, and contribute to Primev! 
---

# {$frontmatter.title}

{$frontmatter.description}

All repositories and documentation are open source and available on [Primev's github](https://github.com/primevprotocol) and we encourage community engagement and contributions to Primev.

## Introduction

Primev is a real time p2p network for MEV actors. The network has 2 primary purposes:
1. To share and consume execution data
2. To engage in commitment schemes for execution "execution games" in the form of signatures

For #1, execution data is shared over websocket connections to users. This is available today.

For #2, a peer-to-peer network is being built. You can track progress on [the p2p branch](https://github.com/primevprotocol/builder-boost/tree/iowar/p2p) of builder-boost. This will be merged with the main branch as the first bid/commit use case goes live.

In the future, there will be a settlement layer to allow value related to the commitment schemes to settle. This is planned to be a rollup with federated sequencers.

## Network Actors

### Providers
Providers of execution services and data. These actors will integrate builder-boost, provide execution data, and provide execution services in the form of commitment signatures and enactment of execution commitments. They will be able to optimize execution over p2p across other providers.

**Block builders**: specialists who accept transactions from a multitude of sources, and try to build the most profitable block possible from those transactions[^1]

**Sequencers**: like a block builder, a sequencer processes transactions, produces rollup blocks, and submits rollup transactions to the L1 chain[^2]

### Users
Users of execution services and data. These actors will open connections to providers, consume execution data, submit bid signatures for execution games.

**Searchers**: users that monitor the state of the chain and send transaction bundles[^3]

**AA bundlers**: users that monitor the userOp mempool and bundle userOps together and send them for execution[^4]. While AA bundlers can be categorized under searchers, we separate this class of actor for Primev use cases due to their differing set of needs from searchers.

**Other blockspace consumers**: Users that can range from any blockspace consumer to L2s looking to secure L1 block or blobspace to Centralized Exchanges optimizing their on-chain costs or providing better on/offramp UX.

#### Future actors
Current actors are based on how the Ethereum ecosystem is positioned in 2023 with out-of-protocol PBS and rollup sequencers. As the ecosystem evolves with planned additions like ePBS, SUAVE, mev-boost++, and decentralized sequencers, we anticipate new actors ranging from proposers to executors and more to participate in the network to provide services and gain efficiency benefits.

## Get Started
We hope you find this documentation helpful and invite you to contribute to the protocol and docs. As a next step, you can:
- Dive deeper into the [System Overview section](https://docs.primev.xyz/docs/Overview/SystemOverview)
- Jump to the [Quickstart section for providers](https://docs.primev.xyz/docs/Providers/quickstart)
- Jump to the [Quickstart section for users](https://docs.primev.xyz/docs/Users/Quickstart)

We look forward to having you join the Primev network!

## Contributing

For all bug reporting, feature requests, code contributions, or technical suggestions feel free to create a PR or [DM us on Twitter](https://twitter.com/primev_xyz).

Primev will evolve as the network grows across different types of actors who leverage emerging use cases. We're constantly evolving our technical approaches in collaboration with researchers and advisors across the ecosystem. If you have a suggestion or a constructive approach, do reach out!

## License

Builder-boost and Primev documentation is currently available under an MIT license.

## Support

If you need further assistance or have any questions, feel free to [DM us on Twitter](https://twitter.com/primev_xyz). If you want a direct communication channel you can tell us your issue and our team will can create a Telegram group with the relevant people. We're open to suggestions if you're looking for an alternative way of receiving support.

### References

[^1]: [Source](https://docs.flashbots.net/flashbots-auction/overview#block-builders): Flashbots, "Block Builders"
[^2]: [Source](https://ethereum.org/en/developers/docs/scaling/optimistic-rollups/#transaction-execution-and-aggregation): Ethereum, "Optimistic Rollups"
[^3]: [Source](https://docs.flashbots.net/flashbots-auction/overview#searchers): Flashbots, "Searchers"
[^4]: [Source](https://www.blocknative.com/blog/account-abstraction-erc-4337-guide): Blocknative, "Bundler"
