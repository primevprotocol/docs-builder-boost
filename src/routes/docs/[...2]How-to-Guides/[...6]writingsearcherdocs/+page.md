---
title: How to write User Documentation as a provider running builder-boost
description: Once you’re up and ready with an instance of Boost, we recommend you create a set of documentation for searchers to easily connect to you. The following is a standard template you can fill in with your network configurations.

---

# {$frontmatter.title}

{$frontmatter.description}

# Searcher Documentation

## Builder Hints

Builder hints are metadata details about a block builders currently built template, below is an example metadata payload from a Mainnet node running Boost.

```javascript
{
    "builder":"0xaa1488eae4b06a1fff840a2b6db167afc520758dc2c8af0dfb57037954df3431b747e2f900fe8805f05d635e9a29717b",
    "number":17671850,
    "blockHash":"0x9061da7322df7293f00ad84810f767504de083d7a7b483e84f0f4de1d5bc0130",
    "timestamp":"Tue, 11 Jul 2023 17:27:11 UTC",
    "baseFee":2316406169,
    "standard_transactions":{
        "count":154,
        "MinPriorityFee":0,
        "MaxPriorityFee":200000000000
    },
    "personal_transactions":[],
    "sent_timestamp":"2023-07-11T17:27:10.691343244Z",
    "rec_timestamp":"2023-07-11T17:27:10.656115905Z"
}
```

## Builder Details

Builder Address is `<Insert-Your-Builder-Address-Here>`

Builder Endpoint is `<Insert-Your-Builder-Endpoint-Here>`

## Registration Process

**Step 1. First construct the Authentication Token.**

The Authentication Token is similar to Flashbots Authentication Token [here](https://docs.flashbots.net/flashbots-auction/searchers/advanced/rpc-endpoint#authentication), but with a fixed payload of the builder address.

The Authentication is passed in as a header: X-Primev-Signature, however instead of signing an arbitrary payload, the signature is made for **your target builder address**, your consumers wallet address is passed as the prefix: `<User Address>:<Signature(Builder Address)>`

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
