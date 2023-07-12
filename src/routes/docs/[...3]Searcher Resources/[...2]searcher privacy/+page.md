---
title: Searcher Privacy
description: We take the privacy of Searchers seriously at Primev.
---

# {$frontmatter.title}

{$frontmatter.description}

We’ve designed our protocol to protect the privacy of searchers in two fold:

- Identity & connection privacy

- Bundle & transaction privacy

That’s why our protocol consists of a variety of controls to ensure pseudonymity for searchers.

# Searcher Privacy

Primev contracts store data that can be leveraged to get a sense of which builders and searchers are connected. To control for this we’ve implemented the following simple commitment mechanism to protect the privacy of searchers
```
$$
c = commit{}(PrivKey_{builder} || address_{searcher})
$$
```
c is the commitment that get’s added to the rollup. A more detailed flow of data is highlighted below:

![Screenshot 2023-06-15 at 4.25.48 PM.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d3d082bc-e9ce-4771-9b0f-efb6a6a1a725/Screenshot_2023-06-15_at_4.25.48_PM.png)

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
Through this scheme we can ensure that entity in the system, can learn more about the searcher <> builder relationships than the ones they are party to.

We’ve also architected builder-boost as a side-car module to ensure block data remains in the builder’s environment, and existing trust relationships between searchers and builders aren’t broken.