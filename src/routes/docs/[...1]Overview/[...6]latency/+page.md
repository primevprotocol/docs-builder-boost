---
title: Latency
description: Latency breakdown for builder-boost
---

# {$frontmatter.title}

{$frontmatter.description}

We recorded the latency builder-boost introduces across a variety of situations. There are 3 keys attributes that impact latency:
1. Wire Latency (~10-20ms)
2. Core Processing Latency (~10ms)
3. _Optional- Inclusion List (~5-10ms)_ 

Our initial test results for e2e latency for a standard builder-boost instance with transaction inclusion list inactive are as follows:

| Latency Bucket | Blocks Per Bucket | Percentile |
| --- | --- | --- |
| <20ms | 18300 | 66% |
| <30ms | 25473 | 93% |
| <50ms | 27035 | 99% |
| <100ms | 27325 | 99.8% |
Total Recorded Blocks: 27368

With the transaction inclusion list added, we see a slight increase in latency as follows. Note transaction inclusion list is an optional and experimental feature.

| Latency Bucket | Blocks Per Bucket | Percentile |
| --- | --- | --- |
| <20ms | 231 | 17% |
| <30ms | 758 | 58% |
| <50ms | 1251 | 96% |
| <100ms | 1289 | 99% |
Total Recorded Blocks: 1300

*Note: all tests were conducted with users in the same region as providers. As geographically distributed providers join the network, we suggest users deploy a geodistributed set of consumers to minimize wire latency impact.*
