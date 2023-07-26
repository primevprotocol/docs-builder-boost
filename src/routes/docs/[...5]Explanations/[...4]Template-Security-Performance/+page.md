---
title: Template Security & Performance Considerations
description: In this section, we address the security of block templates and performance considerations as part of your builder-boost integration.

---

# {$frontmatter.title}

{$frontmatter.description}

<script>
import ClarificationRelay from '$img/clarification-relay.png';

import BBDiagram from '$img/builder-boost-diagram.png';
import Security from '$img/Security.png';

</script>

In the builder requirements, an additional relay is introduced to your Builder Implementation. We emphasize that payloads aren't sent out of your builder environment.

The inclusion of an extra relay is solely aimed at streamlining the integration process with your builder-boost sidecar module. Here is a simplified example of a Primev-enabled builder setup:

<img src={ClarificationRelay} width="100%" alt="Clarification of design of relay"/>

**Are there any performance costs of sending Block Templates to builder-boost?**

The impact of sending templates to an additional relay specification on system performance is minimal. Incorporating additional computation into a system always carries some costs. However, to prevent sequential  blocking, the builder-boost call is executed in a dedicated, short-lived go-routine. This impact is mitigated by the fact that we make only a single HTTP call, utilizing data that is already allocated on the heap for the relays' operation.

**Where does Geth construct the payload being sent to builder-boost?**

Geth has already allocated the payload in the heap to send it to relays. Primev builder modifications simply obtain a read-only reference to the data to ensure that it can be sent to your builder-boost instance.

## How secure and private is my block template when it’s sent to builder-boost?


To safeguard your builder-boost sidecar, we employ a share-secret authentication mechanism that can be configured in both the builder-boost and the modified geth instances. This setup ensures that malicious payloads cannot be posted to your builder-boost instance externally.

### Confidentiality

A key reason of builder-boost's architecture as a sidecar module in the builder’s own environment is to ensure the privacy of block templates and the underlying bundle & transaction data, and having the provider have ultimate control over what's being shared and how.


For maintaining the *Confidentiality* of your block data, we strongly recommend utilizing an HTTPS connection when sending data to builder-boost.


### Integrity

To ensure the integrity of the payloads that are being constructed we’ve implemented a shared secret authentication between the builder and the builder-boost instance. As long as the the Builder constructs a **secure password and keeps it secret**, the setup gives strong assurances over the integrity of the payloads being published and the metadata being sent to searchers.

<img src={Security} alt="Security Diagram" width="100%"/>

### Availability Provisions

Staleness of the payloads is the key considerations when considering the payload being sent to searchers. We recommend setting up some forms of DDOS protection offered by Cloudflare to ensure you
