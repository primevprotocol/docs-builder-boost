---
title: Getting builder-boost Running In Production
description: Get builder-boost up and running in production using the below details

---

# {$frontmatter.title}

{$frontmatter.description}

<script>
    import BBDiagram from '$img/builder-boost-diagram.png';
    import Security from '$img/Security.png';
</script>

# Running builder-boost

builder-boost is a software sidecar module that facilitates a provider’s participation in the Primev network. The diagram below outlines the module’s position within a provider's local environment.

## **builder-boost Diagram**

<img src={BBDiagram} alt="builder boost diagram" width="100%"/>

## **Prerequisites**

- golang version 1.20.4+
- Updated builder with Primev relay enabled
- Git

# **Setting Up builder-boost**

## **Step 1. Ensure your builder is able to send payloads to builder-boost**

Begin by reviewing the Builder Reference Implementation instructions provided **[here](https://hackmd.io/wmmCgKJdTom9WXht2PcdLA)**. After performing the necessary modifications, you can proceed to set up Builder Boost to receive block templates from your builder.

## **Step 2. Download builder-boost via GitHub**

For now, simply do a Git clone:

```
$ git clone git@github.com:primevprotocol/builder-boost.git
```

## **Step 3. Build the builder-boost instance**

```
$ cd builder-boost
$ make all
```

You should now see two binaries built in the root folder:

```
pythonCopy code
.
├── boost
├── ...
├── searcher
...

```

## **Step 4. Run builder-boost**

We highly recommend you use **`systemctl`** to run builder-boost. The following is a recommended **`systemctl`** file that can be used and placed in your **`/home/systemd/system/<filename>`** folder.

```
[Unit]
Description=boost service
After=network.target

[Service]
User=ec2-user
ExecStart=/home/ec2-user/builder-boost/boost
restart=always
Environment="ENV=sepolia"
Environment="ROLLUP_KEY=<builder-key>"
Environment="BUILDER_AUTH_TOKEN=<builder-x-boost-token>"

[Install]
WantedBy=multi-user.target

```

Note that the **`BUILDER_AUTH_TOKEN`** should be set to the same value set for **`--builder-auth-key`** in your modified builder implementation. This token is used to authenticate your builders to Boost, ensuring no threat actor can send false payloads.

## **Running Builder and Searcher in Docker Compose**

1. Copy **`.env.example`** to **`.env`** and set the required variables.
2. Run **`docker-compose up --build`** to run Builder and Searcher connected to Builder Boost.

## **More Links**

**[Searcher Testing Guide](https://chat.openai.com/docs/searcher-testing.md)** - This guide walks you through the setup process for connecting searcher emulators to your Boost for end-to-end testing and monitoring.

**[Builder Modifications](https://hackmd.io/wmmCgKJdTom9WXht2PcdLA)** - A detailed walkthrough with example commits to modify a builder, add the Primev relay, and enable Boost.

# FAQ

## Where do I run this?

We recommend you run this on a standalone server. This means if you’re running it in a Cloud Environment, ensure you do not share CPU and other core resources with others.

## How many instances of builder-boost should I run?

Currently you can only run a single instance of builder-boost. We will release an update that will allow builders to run multiple builder-boost instances in the future. The stateful nature of open connections between searchers and builders creates a restriction in the current version.

## How secure and private is my block template when it’s sent to builder-boost?

### Confidentiality

A key reason of builder-boost's architecture as a sidecar module in the builder’s own environment is to ensure the privacy of block templates and the underlying bundle & transaction data, and having the provider have ultimate control over what's being shared and how.

### Integrity

To ensure the integrity of the payloads that are being constructed we’ve implemented a shared secret authentication between the builder and the builder-boost instance. As long as the the Builder constructs a **secure password and keeps it secret**, the setup gives strong assurances over the integrity of the payloads being published and the metadata being sent to searchers.

<img src={Security} alt="Security Diagram" width="100%"/>

### Availability Provisions

Staleness of the payloads is the key considerations when considering the payload being sent to searchers. We recommend setting up some forms of DDOS protection offered by Cloudflare to ensure you
