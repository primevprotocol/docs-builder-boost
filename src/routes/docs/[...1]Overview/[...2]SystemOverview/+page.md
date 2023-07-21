---
title: System Overview
description: Technical overview of the primev protocol.
---

# {$frontmatter.title}

{$frontmatter.description}
<script>
import { Button } from '@svelteness/kit-docs';
import BuilderMods from '$img/BuilderModificationWalkthrough.png';
import ActorOverview from '$img/ActorOverview.png';
import PrimevOverview from '$img/primev-overview.png';
import SearcherAuthNow from '$img/searcher-auth-now.png';
import SearcherAuthFuture from '$img/searcher-auth-future.png';
</script>

## Terminology

**Block Builder** - Refers to builder instance at a protocol level represented by a single address. 

**Builder** - Is the organization that owns Private Keys associated with block builders. A single *builder* can own multiple private keys, and thus control multiple block builders.

******Searcher****** - Is the entity that **currently** consumes data from the Primev network and is identified by their address.

**Builder Boost** - The name of the client software that sits as a sidecar and allows *Builders* to enable Primev.

## Overview

The technical motivation of the Primev network is to disseminate metadata about block templates (also known as execution payloads) in realtime and route them to relevant parties; in the current case these are searchers.

To this end, a key starting point for the system is for builder nodes to ***relay*** templates/execution payloads to to Builder Boost, which Builder Organizations will self-host.

<img src={BuilderMods}  alt="primev" width="100%"/>


<aside>
Builder Boost is a sidecar that sits within the builders custody. Therefore, no raw templates are ever revealed to the Primev network.

</aside>

<aside>
🤝 There are some light trust-assumptions made with regards to the Builders. The address that a builder presents for itself, is assumed to be in it’s control.

</aside>

## Detailed Actor Interactions

<img src={ActorOverview} alt="actor overview" width="100%"/>

### **Interface Modification (Builder)**

Example commit for Relay modifications can be seen [here](https://github.com/primevprotocol/primev-builder/commit/6d8ae9aae17a1b966ac0f682ba9843bd8746dc4f#diff-d371e466d16c4d92dff29b2806e847c619149cd6de147f1cb9ae7e9511d5da7f).

```
type IRelay interface {
	SubmitBlock(msg *boostTypes.BuilderSubmitBlockRequest, vd ValidatorData) error
	SubmitBlockCapella(msg *capellaapi.SubmitBlockRequest, vd ValidatorData) error
	**+ SubmitBlockPrimev(msg *capellaapi.SubmitBlockRequest) error**
	GetValidatorForSlot(nextSlot uint64) (ValidatorData, error)
	Start() error
	Stop()
}
```

We use the relay API to simplify the integration process for Builder Organizations. Similar to the sending of templates to Flashbots, we suggest adding a function named **`SubmitBlockPrimev`** to the relay interface of your Builder Node Implementation, so that can send payloads to Builder Boost.

<aside>
💡 The API Request to Builder Boost should look as follows:
1. The following header is added `X-BUILDER-TOKEN: <primev-token>` 
2. The payload is the same as the one passed to SubmitBlockCapella

</aside>

### Templates Processing

<img src={PrimevOverview} alt="primev overview" width="100%"/>

Notice in the diagram above, the flow of templates follow the following pattern:

1. Templates are generated and sent to Flashbots relay to be committed
2. Those same templates are subsequently sent to Builder Boost to have metadata extracted  
3. Metadata is sent to the Primev network

## Builder Boost Overview

### Directory Structure Overview

```bash
.
├── cmd # Contains Entrypoints
│   ├── boost # Entrypoint for main boost utility
│   └── searcher # Entrypoint for searcher emulator
...
├── integrationtest
│   ├── searcher_test.go # Tests focused on searcher connections
│   └── test_payloads.go
├── pkg
│   ├── api.go # HTTP Server Endpoints
│   ├── boost.go # Business Logic for builder hints extraction
│   ├── contracts # Bindings for Contract RPC Calls
│   ├── rollup # Wrapper & Business logic to consume searcher payments data
│   ├── service.go
│   └── worker.go
```

### **Authentication & Authorization**

The Boost API Authenticates two different types of entities:

1. The Block Builder running Builder Boost
2. Searchers

### Block Builder running Builder Boost

The Block Builder Node uses password based authentication to identify itself to its Builder Boost Instance. The Password should be included in the **`X-Builder-Token`** header when making a post to **`/primev/v1/builder/blocks`** to submit templates.

Only the builder with the password set in Builder Boost Environment variable will be authorized to send blocks to Builder Boost.

<aside>
❗ Keep X-BUILDER-TOKEN private, do not share it outside your builder organization
</aside>

## Searchers Auth

Due to the need for privacy, searcher auth is slightly more involved. A deep dive into the process is laid out here: https://hackmd.io/AbEznH1PT4GYiX7lDjaZew

The **Authorization** and **Authentication** are split into to parts.

The **authorization** state of searchers is stored in the *rollup*, where as **authentication** occurs via *private key signatures.*

After authenticating each other, the searcher receives a unique token that it must store in the payments contract, along with it’s payment to get authorized to receive builder hints from the block builder.

The diagram below, showcases the high-level flow:

<img src={SearcherAuthNow} alt="searcher auth now" width="100%"/>

## Optional - Dual Authentication of Builders (Coming Soon)

We plan to add a step in which a Builder can provide a self-signed URL, to authenticate the URL as owning the Address being connected to (Note: This defers the security of the authentication to the DNS resolution of the URL or Public Key Infrastructure (PKI)). It would be up to the Searcher to verify the signature. However, in the current model, there is an assumed level of trust towards builders.

<img src={SearcherAuthFuture} alt="searcher auth future" width="100%"/>

## General Security Model

There’s an implied trust in Builders due to their ability to store the private bundle payloads of searchers. We make soft assumptions that the builder behaves honestly with regards to the builder hints it sends to a searcher.

## Rollup Details

The Rollup can be thought of as the authorization server for the Primev protocol. It allows Builders to set minimum payment requirements to connect.

## Endpoints

1. **Endpoint:** **`/health`**
    - **Method:** GET
    - **Description:** Checks the health of the service and provides information about the connected searchers and the worker's heartbeat.
    - **Handler Function:** **`handleHealthCheck`**
2. **Endpoint:** **`/builder`**
    - **Method:** GET
    - **Description:** Retrieves the builder ID.
    - **Handler Function:** **`handleBuilderID`**
3. **Endpoint:** **`/commitment`**
    - **Method:** GET
    - **Description:** Gets the commitment to the builder by searcher address.
    - **Handler Function:** **`handleSearcherCommitment`**
4. **Endpoint:** **`/primev/v1/builder/blocks`**
    - **Method:** POST
    - **Description:** Submits a block for processing by the service.
    - **Handler Function:** **`submitBlock`**
5. **Endpoint:** **`/ws`**
    - **Method:** GET
    - **Description:** Establishes a WebSocket connection for a searcher to receive execution hints.
    - **Handler Function:** **`ConnectedSearcher`**
