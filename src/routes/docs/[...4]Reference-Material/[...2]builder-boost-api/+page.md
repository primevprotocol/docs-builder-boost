---
title: builder-boost API Details
description: Details around all API endpoints exposed by builder-boost
---

# {$frontmatter.title}

{$frontmatter.description}

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
