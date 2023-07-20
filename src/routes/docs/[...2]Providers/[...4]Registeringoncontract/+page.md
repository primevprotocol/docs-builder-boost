---
title: Registering to Primev Contract
description: You must register your builder on the Primev contract to enable authorized connections and generate revenue from providing execution data.

---

# {$frontmatter.title}

{$frontmatter.description}

## Open Primev Contract in Etherscan

Head to [Update Builder Method](https://sepolia.etherscan.io/address/0x6e100446995f4456773Cd3e96FA201266c44d4B8#writeContract#F4) in Etherscan. This method allows you to register a new builder or update settings for your existing builder.

## Connect Wallet

On the top left corner of the **Write Contract** section press **Connect to Web3**. Make sure you use the builder address (the one you use for running the builder geth instance). This address should be funded with some Sepolia ETH to cover transaction costs.

## Specify Builder Parameters and Register

In the `updateBuilder` method there are 2 fields to specify:

- `_minimalStake`: The minimal amount for a searcher to deposit to this builder in order to start receiving builder hints.
- `_minimalSubscriptionPeriod`: The minimal subscription period given to a searcher for depositing the minimal stake. If the searcher deposits more than the minimal stake, the subscription period will be extended linearly.

After all the required fields are specified, press the **Write** button and confirm the transaction using your Web3 provider. Once the transaction is confirmed, searchers can start connecting to your builder. Make sure you run the builder-boost instance before registering a new builder.

### Revenue Details

The `_minimalSubscriptionPeriod` parameter represents the number of **Blocks/Slots** that a perspective searcher is allowed to connect for. We recommend setting 90 Days as the `_minimalStake` and 0.1 ETH as the `_minimalStake`
    
    
| Time Period | Number of Slots/Blocks | Cost - Rate of 0.1 ETH  / 90 Days | Builder Revenue (80%)  |
| --- | --- | --- | --- |
| 1 day | 7,200 | 0.00111 ETH | 0.001 ETH |
| 45 days | 324,000 | 0.05 ETH | 0.04 ETH |
| 90 days | 648,000 | 0.1 ETH | 0.08 ETH |
| 180 days | 1,296,000 | 0.2 ETH | 0.16 ETH |
| 1 year | 2,628,000 | 0.41 ETH | 0.328 ETH |

*Note that the amount deposited is subject to revenue share with the Primev Protocol Treasury. Early participants on the network will receive a proportional amount of Primev tokens for their governance claim over the protocol treasury funds.*
