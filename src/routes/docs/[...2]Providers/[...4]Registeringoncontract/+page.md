---
title: Registering to Primev Contract
description: You must register your builder-boost instance on the Primev contract to enable authorized connections and generate revenue from providing execution data.

---

# {$frontmatter.title}

{$frontmatter.description}

## Open Primev Contract in Etherscan

Head to [Update Builder Method](https://sepolia.etherscan.io/address/0x6219a236EFFa91567d5ba4a0A5134297a35b0b2A#writeContract#F4) in Etherscan. This method allows you to register a new builder or update settings for your existing builder.

## Connect Wallet

On the top left corner of the **Write Contract** section press **Connect to Web3**. Make sure you use the builder address (the one you use for running the builder geth instance). This address should be funded with some Sepolia ETH to cover transaction costs.

## Specify Builder Parameters and Register

In the `updateBuilder` method there are two fields to specify:

- `_minimalStake`: minimal amount of ETH for a user to deposit to in order to authorize a connection. We suggest 0.1 ETH as a minimum.
- `_minimalSubscriptionPeriod`: minimal subscription period in blocks. We suggest 216,000 blocks, which is about 30 days worth. If the user deposits more than the minimum amount, the subscription period will be extended in a linear fashion.

After all the required fields are specified, press the **Write** button and sign the transaction using your Web3 provider. Once the transaction is confirmed, users can start connecting to a provider. Make sure you run the builder-boost instance before registering a new provider.
    
| Time Period | Number of Slots/Blocks | Cost - Rate of 0.1 ETH  / 30 Days | Provider Revenue (80%)  |
| --- | --- | --- | --- |
| 1 day | 7,200 | 0.0034 ETH | 0.00272 ETH |
| 30 days | 216,000 | 0.1 ETH | 0.08 ETH |
| 90 days | 648,000 | 0.3 ETH | 0.24 ETH |
| 180 days | 1,296,000 | 0.6 ETH | 0.48 ETH |
| 1 year | 2,628,000 | 1.2 ETH | 0.96 ETH |

See [the Pricing page](https://docs.primev.xyz/docs/Overview/pricing) for additional details on how revenue share works with the protocol and how providers may expect to own their share of the protocol as it tokenizes

