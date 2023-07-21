---
title: User Quickstart
description: Start consuming execution data
---

# {$frontmatter.title}

{$frontmatter.description}


<script>
    import Reg from '$img/contractreg.png';
</script>


**Step 1. First construct the Authentication Token.**

The Authentication Token is similar to Flashbots Authentication Token [here](https://docs.flashbots.net/flashbots-auction/searchers/advanced/rpc-endpoint#authentication), but with a fixed payload of the builder address.

The Authentication is passed in as a header: X-Primev-Signature, however instead of signing an arbitrary payload, the signature is made for **our builder address**, your searcher address is passed as the prefix: `<Searcher Address>:<Signature(Builder Address)>`

Here is a go snippet to construct the token for our service:

```
msg := <Insert-Your-Builder-Address-Here> // Searchers can retrieve this from the builder info 
hash := crypto.Keccak256Hash([]byte(msg)).Hex()
signature, err := crypto.Sign(accounts.TextHash(hash), searcherPrivateKey) // We use an EIP 
token := crypto.PubkeyToAddress(searcherPrivateKey.PublicKey).Hex() + ":" + hexutil.Encode(sig)
```

**Step 2. Use this token to Receive a special commitment from our Builder**

Make a GET request with `X-Primev-Signature: <token>` to the `/authorization` endpoint. You should receive back a payload as follows:

```javascript
{
"commitment": "0xabc"
}
```


**Step 3. Connect to the Builder via WebSockets**

Make a GET Request `X-Primev-Signature: <token>` to the `/ws` endpoint

**Congrats! You should be connected!**
This is your commitment hash.

## Step 2: Open Primev Contract in Etherscan

1. Go to the [Deposit Method](https://sepolia.etherscan.io/address/0x6219a236EFFa91567d5ba4a0A5134297a35b0b2A#writeContract#F1) on Etherscan. This method allows you to deposit a stake for a specific builder on behalf of the searcher.

## Step 3: Connect Web3 Wallet

1. At the top left corner of the "Write Contract" section, click on "Connect to Web3".
2. Make sure you're using the builder address that you used for running the builder geth instance. This address should have some Sepolia ETH to cover transaction costs.

## Step 4: Specify Deposit Parameters and Send Transaction
<img src={Reg} alt="Registration flow on contract" width="100%"/>

1. In the `deposit` method, you need to fill in 3 fields:
   - `payableAmount`: Enter the amount of stake you wish to deposit for the specific builder.
   - `_builder`: Enter the builder address.
   - `_commitment`: Enter the commitment hash obtained in Step 1.
2. After you have filled in all the necessary fields, click on the "Write" button.
3. Confirm the transaction using your Web3 provider. After the transaction is confirmed, the searcher will be able to receive builder hints.
