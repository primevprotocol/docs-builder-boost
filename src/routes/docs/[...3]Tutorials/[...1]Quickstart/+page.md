---
title: Start consuming Execution Data from Sandbox Environment
description: We will walk you through a quick tutorial on how to connect to our sandbox environment and start consuming execution data!
---

# {$frontmatter.title}

{$frontmatter.description}


<script>
    import Reg from '$img/contractreg.png';
</script>

:::steps

!!!step title="Construct the Authentication Token"|description="The Authentication Token is similar to Flashbots Authentication Token but with a fixed payload of the builder address."

The Authentication is passed in as a header: X-Primev-Signature, however instead of signing an arbitrary payload, the signature is made for **your target builder address**, your user address is passed as the prefix: `<User Address>:<Signature(Builder Address)>`

Here is a go snippet to construct the token for our service:

```
msg := <Insert-Your-Builder-Address-Here> // Users can retrieve this from the builder info 
hash := crypto.Keccak256Hash([]byte(msg)).Hex()
signature, err := crypto.Sign(accounts.TextHash(hash), userPrivateKey) // We use an EIP 
token := crypto.PubkeyToAddress(userPrivateKey.PublicKey).Hex() + ":" + hexutil.Encode(sig)
```

!!!

!!!step title="Receive a Special Commitment"|description="Use this token to Receive a special commitment from our Builder."

Make a GET request with `X-Primev-Signature: <token>` to the `/authorization` endpoint. You should receive back a payload as follows:

```javascript
{
"commitment": "0xabc"
}
```

!!!

!!!step title="Open Primev Contract in Etherscan"|description=""

Go to the Deposit Method on Etherscan. This method allows you to deposit a stake for a specific builder on behalf of the user wallet.

!!!

!!!step title="Connect Web3 Wallet"|description="To protect your privacy, we recommend you connect with a different wallet from the user wallet you will use to consume data"

At the top left corner of the 'Write Contract' section, click on 'Connect to Web3'. Make sure you're using the builder address that you used for running the builder geth instance. This address should have some Sepolia ETH to cover transaction costs.

!!!

!!!step title="Specify Deposit Parameters and Send Transaction"|description="After the transaction is confirmed, the user will be able to receive builder hints."

In the `deposit` method, you need to fill in 3 fields: `payableAmount`, `_builder`, and `_commitment`. After you have filled in all the necessary fields, click on the 'Write' button. Confirm the transaction using your Web3 provider. 

<img src={Reg} alt="Registration flow on contract" width="100%"/>


!!!

!!!step title="Connect to the Builder via WebSockets"|description="Connect to the Builder via WebSockets."

Make a GET Request `X-Primev-Signature: <token>` to the `/ws` endpoint. Congrats! You should be connected! This is your commitment hash.

!!!
:::
