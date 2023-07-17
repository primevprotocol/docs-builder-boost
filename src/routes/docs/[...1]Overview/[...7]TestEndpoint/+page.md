---
title: Using Primev Builder Test Endpoint
description: Searchers can use test Primev Builder endpoint to access payloads. Endpoint is located under https://boost.primev.xyz/builder
---

# {$frontmatter.title}

{$frontmatter.description}


**Running a Test Searcher instance using test binary:**

1. Clone https://github.com/primevprotocol/builder-boost repo and change working directory to it
2. Run `make build` to build searcher binary
3. Run searcher instance replacing `a1b2c3` with your searcher private key:
    
    ```bash
    ./searcher --searcherkey a1b2c3 --boosturl https://boost.primev.xyz
    ```
    
    If running first time it could show error `lost connection, retrying in 5 seconds` which might mean you need to make deposit to Primev Boost account. When starting searcher instance, there is `generated commitment for builder` log with `commitment` field, use it to make deposit.
    

**Running a Test Searcher instance using cli commands:**

1. Get builder address from builder boost endpoint:
    
    `curl https://boost.primev.xyz/builder`
    
2. Generate authentication token for builder using **geth** console
    
    `eth.sign(eth.accounts[0], web3.sha3("0xfE9596C9798571CE953CAd4B8bfDf7586BdD88A9"))`
    
3. Get commitment hash using authentication token
    
    `curl -H "X-Primev-Signature: address:token" https://boost.primev.xyz/commitment`
    
    Replace address with your searcher address and token with auth token received on step 2
    

**Depositing to Primev Boost account:**

1. Open [Primev smart contract](https://sepolia.etherscan.io/address/0x6e100446995f4456773Cd3e96FA201266c44d4B8#writeContract) in **Write Contract** tab
2. Connect your Web3 wallet, e.g. Metamask
3. Open **deposit** method
4. Set **Deposit Amount** to any number higher than 100 wei, **Builder Address** to `0xfE9596C9798571CE953CAd4B8bfDf7586BdD88A9` and **Commitment** to hash received while running searcher instance.
5. Press **Write** button to send transaction which deposits on behalf of searcher account
6. Now you when running `searcher` binary with appropriate private key you should receive builder hints from **Primev Builder Endpoint**
