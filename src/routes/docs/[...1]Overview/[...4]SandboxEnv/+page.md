---
title: Sandbox Environment
description: You can play around with a live instance of builder-boost.
---

# {$frontmatter.title}

{$frontmatter.description}

Primev Builder Boost Sandbox: https://boost.primev.xyz/builder

The sandbox environment allows you to explore what it's like to own or connect to a builder-boost instance. It currently has a data feed from a modified Geth instance generating templates for flashbots. The `X-Builder-Token` for it is `tmptoken`

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