---
title: Builder Boost Quickstart Guide
description: Run Builder Boost in 5 minutes

---

# {$frontmatter.title}

{$frontmatter.description}
# Blockbuilders: Run builder-boost

## Pre-Requisites:

- A Wallet with Sepolia Eth that contains `<your-private-key>`

## Steps:

1. `$ git clone git@github.com:primevprotocol/builder-boost.git`
2. `$ cd builder-boost`
3. `$ make all`
4. `$ ./boost --rollupkey <your-private-key> --buildertoken <secret-password>`

The `<private-key>` is the key associated with your payments account in the primev protocol. It should contain some Sepolia Eth to allow transactions to be sent.

The `<secret-password>` is the secret value sent in the `X-Builder-Token` header from Builder and is used to authenticate your personal Block Builder Nodes.

- Head to [Update Builder Method](https://sepolia.etherscan.io/address/0x6e100446995f4456773Cd3e96FA201266c44d4B8#writeContract#F4) - On top left corner of **Write Contract** section press **Connect to Web3**
- Fill in `updateBuilder` method with the values of the following 2 fields specified: `<_minimalStake>` and `<_minimalSubscriptionPeriod>`. Press **Write** button and confirm transaction using your Web3 provider. Once transaction is confirmed, searchers could start depositing to your builder.

  - `_minimalStake`: minimal amount for searcher to deposit to this builder in order to start receiving builder hints
  - `_minimalSubscriptionPeriod`: minimal subscription period given to searcher for depositing minimal stake. In case searcher deposits more than minimal stake, subscription period will be extended linearly.

💡 We recommend setting the `_minimalSubscriptionPeriod`
