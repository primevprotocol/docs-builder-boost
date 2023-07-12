---
title: Latency
description: End to End Latency (~50ms) - Total Recorded Blocks (1300)
---

# {$frontmatter.title}

{$frontmatter.description}


| Latency Bucket | Blocks Per Bucket | Percentile |
| --- | --- | --- |
| <20ms | 231 | 17% |
| <30ms | 758 | 58% |
| <50ms | 1251 | 96% |
| <100ms | 1289 | 99% |

For searchers operating within the same region as a builder boost instance, the average end-to-end latency is typically under 30ms for the majority of payloads. Additionally, it remains below 50ms for approximately 96% of payloads.

It's worth noting that network latency accounts for around 10-20ms of this overall measurement.