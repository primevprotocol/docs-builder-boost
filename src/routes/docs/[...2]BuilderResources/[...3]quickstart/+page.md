---
title: builder-boost Quickstart Guide
description: Run builder-boost in <5 minutes

---

# {$frontmatter.title}

{$frontmatter.description}
# Block builders: Run builder-boost

## Pre-Requisites:

- An Ethereum address with Sepolia Eth (DM us on Twitter if you need Sepolia ETH)

## Steps:

1. `$ git clone git@github.com:primevprotocol/builder-boost.git`
2. `$ cd builder-boost`
3. `$ make all`
4. `$ ./boost --rollupkey <your-private-key> --buildertoken <secret-password>`

The `<private-key>` is the key associated with your payments account in the primev protocol. It should contain some Sepolia Eth to allow transactions to be sent.

The `<secret-password>` is the secret value sent in the `X-Builder-Token` header from Builder and is used to authenticate your personal Block Builder Nodes.


- Head to [Update Builder Method](https://sepolia.etherscan.io/address/0x6219a236EFFa91567d5ba4a0A5134297a35b0b2A#writeContract#F4) - On the top left corner of the **Write Contract** section press **Connect to Web3**
- Fill in `updateBuilder` method with the values of the following 2 fields specified: `<_minimalStake>` and `<_minimalSubscriptionPeriod>`. Press the **Write** button and sign the transaction using your Web3 provider. Once the transaction is confirmed, searchers can start authorizing connections to your builder.

  - `_minimalStake`: minimal amount for searcher to deposit to this builder in order to start receiving builder hints
  - `_minimalSubscriptionPeriod`: minimal subscription period given to searcher for depositing minimal stake. In case searcher deposits more than minimal stake, subscription period will be extended linearly.

💡 We recommend setting the `_minimalSubscriptionPeriod`
