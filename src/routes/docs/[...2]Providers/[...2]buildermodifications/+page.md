---
title: Builder Modifications (~5 minutes)
description: Below is a general overview of the minor changes needed to an Ethereum Execution Client like geth to enable Primev. We will walk through the changes needed using a standard fork of the Flashbots builder. We also detail changes needed with commit references in case your geth instance has custom modifications.

---

# {$frontmatter.title}

{$frontmatter.description}

<script>
import TemplateGeneration from '$img/block-gen.png';
</script>


> Please reach out to us if you need any assistance.
> 

### **Overview**

Builder-boost sits as a sidecar module to your execution client. The builder will consistently spin up lightweight go-routines to POST new block templates.

<img src={TemplateGeneration} alt="block template generation" width="100%"/>

## **Quickstart (5 mins)**

You can follow the steps below to use our reference implementation:

1. Clone the repository from **[https://github.com/primevprotocol/builder-reference-primev](https://github.com/primevprotocol/builder-reference-primev)**.
2. Build the instance and add the flag **`-builder.remote_primev_endpoint <your-builder-boost-url> --builder.primev_token <your-selected-private-token>`**.

## **[optional] Advanced Instructions**

### **General Overview of Changes Needed**

The idea behind the builder changes is to enable a new local “relay” to publish blocks to, in a fashion similar to how blocks are sent to relays like flashbots, blocknative, or agnostic today.

Below is a more detailed perspective of the standard flow in the Flashbots builder. We add a command of **`go SubmitBlockPrimev(&blockData)`** in the SubmitBlock section.

Geth instances have the concept of an IRelay. You simply need to add a **`SubmitBlockPrimev`** to such a relay that is in use and configure any necessary configurations via modifications similar to this commit: **[https://github.com/primevprotocol/builder-reference-primev/commit/9f160e8934fef1aadd21e095c432db41538b93af](https://github.com/primevprotocol/builder-reference-primev/commit/9f160e8934fef1aadd21e095c432db41538b93af)**.

## **Step by Step Walkthrough**

### **Step 1. Registering the Builder Boost environment variables**

#### Step 1a. Primev Endpoint

You can view the entire commit for this section below.

**[Commit: 9f160e8934fef1aadd21e095c432db41538b93af](https://github.com/primevprotocol/builder-reference-primev/commit/9f160e8934fef1aadd21e095c432db41538b93af)**

You can build Geth and you will see the following output by the end of this:

```
**WARN [timestamp] Remote Primev Endpoint is not Set, payloads will not be sent to Primev Network**

```

If you set the correct environment variable **`BUILDER_REMOTE_PRIMEV_ENDPOINT`** or pass in the flag **`builder.remote_primev_endpoint`** while running Geth, you'll see the following successful message:

```
**INFO [timestamp] Remote Primev Endpoint is Set, payloads will be sent to Primev Network endpoint=<Your builder boost URL>**

```

#### Step 1b. Primev Auth Token

The **primev auth token** is used to authorize and authenticate the builder instance. This is to ensure only your block builders can post payloads to builder-boost.

The following link showcases the necessary modifications to set up the Primev Auth Token:

**[Commit: d9b8addd1fdeea8c841a43ff7653e7c9345e7e0d](https://github.com/primevprotocol/primev-builder/commit/d9b8addd1fdeea8c841a43ff7653e7c9345e7e0d)**

We will describe below in Step 3 how to submit the request. Ensure you make the modification to add the **`X-Builder-Token: <primev-token>`** header to the POST request that is made to Boost.

### **Step 2. Updating the Relay Interface to Support Block Submission to Primev/Builder Boost**

In this commit, we simply add all the required function details to support updating the Relay interface to publish to builder-boost.

**[Commit: 6d8ae9aae17a1b966ac0f682ba9843bd8746dc4f](https://github.com/primevprotocol/builder-reference-primev/commit/6d8ae9aae17a1b966ac0f682ba9843bd8746dc4f)**

#### **Step 2a. Update interface needs for Unit Tests**

Follow the commit to add a stub for the **`testRelay`** to ensure it adheres to the new interface.

**[Commit: 122e82ee6072ff2cda94ed8e75fdd861befccd19](https://github.com/primevprotocol/builder-reference-primev/commit/122e82ee6072ff2cda94ed8e75fdd861befccd19)**

### **Step 3. Start Submitting Blocks to builder-boost**

Finally, we submit the block details via a go-routine to ensure we don't block for the response:

**[Commit: 04d81efdf499ada3cee303316665edf0d5945a27](https://github.com/primevprotocol/builder-reference-primev/commit/04d81efdf499ada3cee303316665edf0d5945a27)**

Voila, your Block builder is now a bleeding edge builder that can share execution data! Not some off the shelf Block builder 😉
