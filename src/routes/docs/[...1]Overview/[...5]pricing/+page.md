---
title: Primev Pricing
---

# {$frontmatter.title}

{$frontmatter.description}

Primev protocol is free to use and open source. Providers on the network may charge fees to users, and users may include bids in their messages to incentivize providers.

Users on the network may deposit any amount of ETH to a provider to authorize their connection to consume execution data. For most providers this value will be a minimum of **0.1 ETH**.

Providers on the network have the flexibility to define their own pricing for users to consume data when registering their address on the Primev contract. Deposits have an 80:20 split between the provider and the Primev protocol. Primev protocol treasury will be tokenized in the future, and participating providers will gain access to protocol tokens.

We suggest **0.1 ETH** as the minimum deposit requirement for a duration of **216000 blocks**, which is about **30 days worth**. Please [see provider registry](https://docs.primev.xyz/docs/Providers/Registeringoncontract#specify-builder-parameters-and-register) for details on setting registration parameters according to your preferences. The payment amount and connection duration can be adjusted by providers at any given time. If a user deposits more than the minimum amount, the subscription period will be extended in a linear fashion.
