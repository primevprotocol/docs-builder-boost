---
title: Block Safety & Performance Considerations
description: You may have questions about the safety of your block templates when integrating boost and any performance impacts you may be suceptible too as part of the integration.

---

# {$frontmatter.title}

{$frontmatter.description}

In the builder requirements, an additional relay is introduced to your Builder Implementation. However, it's crucial to note that this does not imply that payloads are sent out of your builder environment.

The inclusion of an extra relay is solely aimed at streamlining the integration process with your Primev Builder Boost sidecar. Here is a simplified example of a Primev-enabled builder setup

![clarification-relay.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4f3e9742-041a-4a7d-b752-c4771ddc777b/clarification-relay.png)

**Would I incur any performance cost from sending Block Templates to Boost?**

The impact on system performance is minimal. Incorporating additional computation into a system always carries some costs. However, to prevent sequential  blocking, the boost call is executed in a dedicated, short-lived go-routine. Moreover, this impact is mitigated by the fact that we make only a single HTTP call, utilizing data that is already allocated on the heap for the MEV Relays (Flashbots) operation.

******************Where does Geth construct the payload being sent to builder-boost?******************

Geth has already allocated the payload in the heap to send it to MEV Relays(Flashbots). We just obtain a read-only reference to the data to ensure that we can also send it to your primev Builder Boost sidecar

************************************************Security Considerations?************************************************

To safeguard your builder-boost sidecar, we employ a share-secret authentication mechanism that can be configured in both the Builder Boost and Modified Geth instances. This setup ensures that malicious payloads cannot be posted to your Builder Boost instance.

For maintaining the *Confidentiality* of your block data, we recommend utilizing an HTTPS connection when sending data to builder-boost