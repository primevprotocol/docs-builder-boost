---
title: Pilot Phase Details
description: Primev is under a pilot phase as of September '23 where we and early network participants are monitoring the effects of sharing execution data. While mainnet data is being shared during the pilot phase, the Registry Contract is on the Sepolia testnet so early users can test consuming execution data using Sepolia ETH.
---

# {$frontmatter.title}

{$frontmatter.description}

| Provider name | Provider Address | endpoint |
| --- | --- | --- |
| Titan | 0xd5A5DeD62438b09f6400491CBEE77425Be4604c3 | http://3.126.190.199:18550 |
| Primev test | 0xfE9596C9798571CE953CAd4B8bfDf7586BdD88A9 | https://boost.primev.xyz/builder |

In order to connect to a provider during the Pilot Phase, you will have to make a deposit in Sepolia ETH. Making a deposit requires specifying a commitment hash which is unique to every searcher-builder pair. Refer to the [User Quickstart section](https://docs.primev.xyz/docs/Users/Quickstart) to construct your token, obtain this hash to connect.

We recommend connecting with the key you use to sign transactions so you can view inclusion receipts custom to your address in the payload you receive. Primev abstracts your address so your connection remains private and your address is not exposed.

When the pilot program concludes, the registry contract will be deployed to Ethereum mainnet, requiring existing connections to be re-established and mainnet ETH to be deposited to open connections to providers.


