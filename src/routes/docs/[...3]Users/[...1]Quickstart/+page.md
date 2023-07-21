---
title: User Quickstart
description: Start consuming execution data
---

# {$frontmatter.title}

{$frontmatter.description}

## Step 1: Obtain a Commitment Hash from your Providers

1. Run your Searcher instance with the correct `SEARCHER_KEY` and `BOOST_ADDR` variables. Make sure you're taking note of the `token` parameter, which is logged when you run this instance.

2. Send an HTTP request to the builder endpoint to get a commitment hash. This hash is unique for each searcher-builder pair. The `token` parameter mentioned in the first step should be specified in this request.

   - For example, if `http://localhost:18550` is your builder boost endpoint and `ABCD` is your searcher token for this builder, your HTTP request would be:

```shell
$ curl http://localhost:18550/commitment?token=ABCD
```

3. You will receive an output that looks like this:

```javascript
{"commitment":"0x688d0031ba0ce02c2049786ca6fd70d04869688dd84d6310b7fdb052d199612f"}
```

This is your commitment hash you can use to make a deposit using the contract and authorize your connection.

## Step 2: Open Primev Contract in Etherscan

1. Go to the [Deposit Method](https://sepolia.etherscan.io/address/0x6e100446995f4456773Cd3e96FA201266c44d4B8#writeContract#F1) on Etherscan. This method allows you to deposit a stake for a specific builder on behalf of the searcher.

## Step 3: Connect Wallet

1. At the top left corner of the "Write Contract" section, click on "Connect to Web3".
2. Make sure you're using the builder address that you used for running the builder geth instance. This address should have some Sepolia ETH to cover transaction costs.

## Step 4: Specify Deposit Parameters and Send Transaction

1. In the `deposit` method, you need to fill in 3 fields:
   - `payableAmount`: Enter the amount of stake you wish to deposit for the specific provider.
   - `_builder`: Enter the provider address.
   - `_commitment`: Enter the commitment hash obtained in Step 1.
2. Click on the "Write" button.
3. Sign the transaction using your Web3 provider. After the transaction is confirmed, you will be able to receive execution data.
