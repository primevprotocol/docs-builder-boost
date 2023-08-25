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

**Block Builder** - Refers to a builder instance at a protocol level represented by a single address. 

**Provider** - is the organization that owns private keys associated with block builders. A single *provider* can own multiple private keys, and thus control multiple block builders.

**User** - is the entity that **currently** consumes data from the Primev network and is identified by their address.

**builder-boost** - The name of the client software that sits as a sidecar module and allows *providers* to enable Primev.

## Overview

A key starting point for the system is for provider nodes to ***relay*** block templates/execution payloads to builder-boost, which providers will self-host.

<img src={BuilderMods}  alt="primev" width="100%"/>


<aside>
builder-boost is a sidecar module that sits within the providers' custody. Therefore, no raw block templates are ever revealed to the Primev network, avoiding MEV theft attack surfaces.

</aside>

<aside>
🤝 There are some light trust assumptions made with regards to providers. The block builder address that a provider presents is assumed to be in its control.

</aside>

## Detailed Actor Interactions

<img src={ActorOverview} alt="actor overview" width="100%"/>

### **Block Builder Interface Modification**

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

We use the relay API to simplify the integration process for providers. Similar to the sending of block to a relay, we suggest adding a function named **`SubmitBlockPrimev`** to the relay interface of your Block builder Node Implementation, so that it can send payloads to builder-boost.

<aside>
💡 The API Request to builder-boost should look as follows:
1. The following header is added `X-BUILDER-TOKEN: <primev-token>` 
2. The payload is the same as the one passed to SubmitBlockCapella

</aside>

### Processing Block Templates

<img src={PrimevOverview} alt="primev overview" width="100%"/>

Notice in the diagram above, the flow of templates follow the following pattern:

1. Block bids are sent to Flashbots relay
2. Block templates are sent to builder-boost to compute execution data
3. Execution data is sent to the Primev network

## builder-boost Overview

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
│   ├── boost.go # Business Logic to compute execution data
│   ├── contracts # Bindings for Contract RPC Calls
│   ├── rollup # Wrapper & Business logic to consume searcher payments data
│   ├── service.go
│   └── worker.go
```

### **Authentication & Authorization**

The Boost API Authenticates two different types of entities:

1. The Provider running builder-boost
2. Users

### Provider running builder-boost

The Provider uses password based authentication to identify itself to its builder-boost Instance. The Password should be included in the **`X-Builder-Token`** header when making a post to **`/primev/v1/builder/blocks`** to submit templates.

Only the provider with the password set in builder-boost Environment variable will be authorized to send blocks to builder-boost.

<aside>
❗ Keep X-BUILDER-TOKEN private, do not share it outside your builder organization
</aside>

## User authentication

User authentication has additional steps due to privacy requirements. You can learn more in the [user privacy section](https://docs.primev.xyz/docs/Users/userprivacy).

**Authorization** and **Authentication** are split into two parts.

The **authorization** state of users is stored in the *registry contract*, where as **authentication** occurs via *signatures.*

After authenticating, the user receives a unique token that it must store in the registry contract, along with its payment to authorize its connection to receive execution data from the provider.

The diagram below showcases the high-level flow:

<img src={SearcherAuthNow} alt="searcher auth now" width="100%"/>

## Optional - Dual Authentication of Block Builders (Coming Soon)

We plan to add a step in which a provider can provide a self-signed URL, to authenticate the URL as owning the Address being connected to (Note: This defers the security of the authentication to the DNS resolution of the URL or Public Key Infrastructure (PKI)). It would be up to the Searcher to verify the signature. However the current model inherits existing block builder trust assumptions.

<img src={SearcherAuthFuture} alt="searcher auth future" width="100%"/>

## General Security Model

There’s an implied trust in providers due to their ability to store the private bundle payloads of searchers. The current version of builder-boost makes honesty assumptions that the provider provides correct and accurate execution data it sends to a user. The Primev team has planned a protocol integrity layer to remove these assumptions in the future.

## Registry Contract Details

The registry contract can be thought of as the authorization destination for the Primev protocol. It allows providers to set the minimum payment requirement to authorize a connection.

## Endpoints

1. **Endpoint:** **`/health`**
    - **Method:** GET
    - **Description:** Checks the health of the service and provides information about the connected searchers and the worker's heartbeat.
    - **Handler Function:** **`handleHealthCheck`**
2. **Endpoint:** **`/builder`**
    - **Method:** GET
    - **Description:** Retrieves the provider ID.
    - **Handler Function:** **`handleBuilderID`**
3. **Endpoint:** **`/commitment`**
    - **Method:** GET
    - **Description:** Gets the commitment to the provider by user address.
    - **Handler Function:** **`handleSearcherCommitment`**
4. **Endpoint:** **`/primev/v1/builder/blocks`**
    - **Method:** POST
    - **Description:** Submits a block to be processed by the service.
    - **Handler Function:** **`submitBlock`**
5. **Endpoint:** **`/ws`**
    - **Method:** GET
    - **Description:** Establishes a WebSocket connection for a user to receive execution data.
    - **Handler Function:** **`ConnectedSearcher`**
