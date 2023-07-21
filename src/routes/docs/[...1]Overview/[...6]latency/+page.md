---
title: Latency
description: Latency breakdown for builder-boost
---

# {$frontmatter.title}

{$frontmatter.description}

We recorded latency of boost across a variety of situations. There are 3 keys attributes that impact latency, these are:
1. Wire Latency (~10-20ms)
2. Core Processing Latency (~10ms)
3. _Optional- Inclusion List (~5-10ms)_ 

Our intial test with e2e latency for the regular builder-boost instance with inactive inlusion list being consumed by a user in the same aws-west-2 region as the builder-boost is as follows:


| Latency Bucket | Blocks Per Bucket | Percentile |
| --- | --- | --- |
| <20ms | 18300 | 66% |
| <30ms | 25473 | 93% |
| <50ms | 27035 | 99% |
| <100ms | 27325 | 99.8% |
Total Recorded Blocks: 27368

With the inclusion lists added, we see an uptick in the latency as follows. Note Inclusion lists is an optional and experimental feature.


| Latency Bucket | Blocks Per Bucket | Percentile |
| --- | --- | --- |
| <20ms | 231 | 17% |
| <30ms | 758 | 58% |
| <50ms | 1251 | 96% |
| <100ms | 1289 | 99% |
Total Recorded Blocks: 1300