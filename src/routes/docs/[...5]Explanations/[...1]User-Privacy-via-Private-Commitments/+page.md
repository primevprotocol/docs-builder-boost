---
title: User Privacy through Private Commitments
description: Private Commitments allow users to maintain privacy around which builders they are coordinating with in Primev.
---

# {$frontmatter.title}

{$frontmatter.description}
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.15.2/dist/katex.min.css" integrity="sha384-MlJdn/WNKDGXveldHDdyRP1R4CTHr3FeuDNfhsLPYrq2t0UBkUdK2jyTnXPEK1NQ" crossorigin="anonymous">


<script>
    
import SecurityDiagram from '$img/searcher-privacy-diagram.png';
  import Katex from 'svelte-katex'

</script>

Primev contracts store data that could otherwise be leveraged to get a sense of which builders and searchers are connected. To control for this we’ve implemented the following simple commitment mechanism to protect user privacy:

<KatexCommitment/>

c is the commitment that get’s added to the rollup. A more detailed flow of data is highlighted below:


<img src={SecurityDiagram} alt="security diagram" width="100%"/>

<KatexCommitmentExplain/>

Through this scheme we can ensure that entity in the system, can learn more about the searcher <> builder relationships than the ones they are party to.

We’ve also architected builder-boost as a side-car module to ensure block data remains in the builder’s environment, and existing trust relationships between searchers and builders aren’t broken.
