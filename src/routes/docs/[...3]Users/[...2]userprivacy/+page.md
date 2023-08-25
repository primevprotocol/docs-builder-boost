---
title: User Privacy
description: Primev provides a simple cryptographic mechanism to abstract user address and connection privacy from external parties.
---

# {$frontmatter.title}

{$frontmatter.description}


<script>
import SecurityDiagram from '$img/searcher-privacy-diagram.png';
</script>

Primev protocol protects user privacy under two different contexts:

- Identity & connection privacy

- Bundle & transaction privacy

## User Privacy

Primev contracts store data that could otherwise be leveraged to get a sense of which builders and searchers are connected. To control for this we’ve implemented the following simple commitment mechanism to protect user privacy:
```
$$
c = commit{}(PrivKey_{builder} || address_{searcher})
$$
```
c is the commitment that get’s added to the rollup. A more detailed flow of data is highlighted below:


<img src={SecurityDiagram} alt="security diagram" width="100%"/>

To open the commitment, someone requires:
```
$$
c, PrivKey_{builder},address_{searcher}
$$
```
The builder can subsequently verify the commitment as follows:
```
$$
true/false := verify(c, Privkey_{builder}, address_{searcher})
$$
```
Through this scheme we can ensure that entity in the system can learn more about the searcher <> builder relationships than the ones they are party to.

We’ve also architected builder-boost as a side-car module to ensure block data remains in the builder’s environment, and existing trust relationships between searchers and builders aren’t broken.
