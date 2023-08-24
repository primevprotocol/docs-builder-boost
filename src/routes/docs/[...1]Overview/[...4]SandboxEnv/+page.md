---
title: Sandbox Environment
description: You can play around with a live instance of builder-boost.
---

# {$frontmatter.title}

{$frontmatter.description}

Primev Builder Boost Sandbox: https://boost.primev.xyz/builder

The sandbox environment allows you to explore what it's like to own or connect to a builder-boost instance. The data feed is from a vanilla block builder with no private bundles. The `X-Builder-Token` for it is `tmptoken`

- **List of Curl Commands**
    
    ## Get Builder ID
    ```bash
    curl --location 'https://boost.primev.xyz/builder'
    ```

    ## Health
    ```bash
    curl --location 'https://boost.primev.xyz/health' --header 'X-Builder-Token: tmptoken'
    ```

    ## Commitment
    ```bash
    curl --location 'https://boost.primev.xyz/commitment' --header 'X-Primev-Signature: <primev-token>
    ```
