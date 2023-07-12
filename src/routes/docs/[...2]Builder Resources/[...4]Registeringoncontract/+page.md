---
title: Registering to Primev Contract
description: You must register yourself as a builder on the primev contract to enable activity.

---

# {$frontmatter.title}

{$frontmatter.description}
    
**Builders set two parameters when registering with Primev:**

These parameters will allow you to set the price searchers must pay for access to your Builder-Boost payloads. 

- `_minimalStake`: minimal amount for searcher to deposit to this builder in order to start receiving builder hints
- `_minimalSubscriptionPeriod`:  parameter represents the number of **Blocks/Slots** that a searcher would connect per `_minimalStake` deposited. The rate progresses linearly as follows:
```    
$$T_{Subscription} := 0\  \text{if}\ stake_{searcher} < stake_{minimum}
$$

$$
T_{Subscription} := stake_{searcher} * (stake_{minimum}/T_{sub\ min})\  \text{if}\ stake_{searcher} \geq stake_{minimum}
$$
```

The `_minimalSubscriptionPeriod` parameter represents the number of **Blocks/Slots** that a perspective searcher is allowed to connect for. We recommend setting 90 Days as the `_minimalStake` and 0.1 ETH as the `_minimalStake`
    
    
| Time Period | Number of Slots/Blocks | Cost - Rate of 0.1 ETH  / 90 Days | Builder Revenue (80%)  |
| --- | --- | --- | --- |
| 1 day | 7,200 | 0.00111 ETH | 0.001 ETH |
| 45 days | 324,000 | 0.05 ETH | 0.04 ETH |
| 90 days | 648,000 | 0.1 ETH | 0.08 ETH |
| 180 days | 1,296,000 | 0.2 ETH | 0.16 ETH |
| 1 year | 2,628,000 | 0.41 ETH | 0.328 ETH |

**Note that 80% of funds accrue to the Builder who has registered and 20% to a Primev EOA**