---
title: User Quickstart
description: Start consuming execution data
---

# {$frontmatter.title}

{$frontmatter.description}


<script>
    import Reg from '$img/contractreg.png';
</script>

:::steps

!!!step title="Construct the Authentication Token"|description="The Authentication Token is similar to Flashbots Authentication Token but with a fixed payload of the builder address."

The Authentication is passed in as a header: X-Primev-Signature. Instead of signing an arbitrary payload, the signature is made for **your target builder address**, your user address is passed as the prefix: `<User Address>:<Signature(Builder Address)>`

Here is a go snippet to construct the token for Primev:

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

Go to the [Deposit Method](https://sepolia.etherscan.io/address/0x6e100446995f4456773Cd3e96FA201266c44d4B8#writeContract#F1) on Etherscan. This method allows you to deposit some ETH for a specific builder on behalf of the user wallet.

!!!

!!!step title="Connect Web3 Wallet"|description="To protect your privacy, we recommend you connect with a different wallet from the user wallet you will use to consume data"

At the top left corner of the 'Write Contract' section, click on 'Connect to Web3' to connect your wallet. The connected address can be any address, and should not be an address you send transactions to a provider with to preserve privacy. Make sure you have some [Sepolia ETH](https://docs.primev.xyz/docs/Overview/Executiondata#primev-details) to cover transaction costs.

!!!

!!!step title="Specify Deposit Parameters and Send Transaction"|description="After the transaction is confirmed, the user will be able to receive builder hints."

In the `deposit` method, you need to fill in 3 fields: `payableAmount`, `_builder`, and `_commitment`. Fill the fields and click on the 'Write' button. Confirm the transaction using your Web3 provider. 

`payableAmount`: the amount you want to deposit for the builder you're registering with (min **0.1 ETH**)
`_builder`: the builder address you want to authorize a connection for
`_commitment`: the commitment hash obtained on step 2

<img src={Reg} alt="Registration flow on contract" width="100%"/>


!!!

!!!step title="Connect to the Builder via WebSockets"|description="Connect to the Builder via WebSockets."

Make a GET Request `X-Primev-Signature: <token>` to the `/ws` endpoint. Congrats, you should be connected and receiving inclusion receipts!

!!!
:::
