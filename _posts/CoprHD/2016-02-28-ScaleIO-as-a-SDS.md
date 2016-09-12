---
layout: post
title: "ScaleIO as a SDS"
description: ""
category: ScaleIO
tags:  [SDS]
---
{% include JB/setup %}

## ScaleIO Overview:

EMC ScaleIO is a software-defined solution that uses your existing hardware or EMC servers to turn existing DAS storage into shared block storage.
Here are few facts:

- A Software defined storage (SDS) product from EMC
- It creates a server-based storage area network(SAN) from local application server storage
- It converts direct-attached storage into shared block storage
- It uses exsiting host-based internal storage to create a scalable, high-performance, low-cost server SAN
- ScaleIO can scale from three compute/storage nodes to over 1,000 nodes
- It can drive up to 240 million IOPS of performance.
- ScaleIO can be deployeds
    + as storage only
    + as a converged infrastructure combining storage, computational and network resources into a single block

## Concepts for Software Defined storage (SDS)

### Tranditional storage management is too complex and inefficient:

#### Traditional Storage Pain Points:
* Application mapped to  specific applicance
* Storage resources optimized to run specific workload
* Isolated storage resources

#### Challenges of Traditional Challenges:
* Cose more to manage diverse storage solutions
    + Data growth
    + Maintenance, Operations & Support
    + Infrastructure
* Vendor lock-in (套牢)
    + Limited scalability
    + Not flexiable for innovation
* Need for massively shared data

### What is SDI (Software Defined Infrastructure)

#### Service Assurance (保证)
* Automatically deploy and maintains:
    + Policies
    + intelligent monitoring to trigger dynamic provisioning
    + service assurance as application

#### Provisioning Management
* Based on requirements of an applicaiton to perform
    + orchestration provisions
    + manages
    + allocate resources optimally

#### Pooled resource
* resources are abstracted into resouce pools. E.g.
    + Netwrok
    + Storage
    + Compute elements

### SDS - A Key Component of SDI
SDS is a framework that  delivers a scalable, cost-effective solution to serve the needs of tomorow's data center

- Abstracting software from hardware, providing flexibility& scalability
- Aggregating(汇总) diverse provider solutions, increasing flexibility and drive down costs
- Provisioning resources dynamically (Pay-as-you-grow) increasing efficiency
- Orchestrating application access to diverse atorage systems through Service Level Agreements(SLAs), increasing flexibility and handle data complexity

### Note:

#### Service assurance (SA)
- a procedure or set of procedures intended to optimize performance
- provide management guidance in communications networks, media services and end-user applications.
- Service assurance is an all-encompassing paradigm that revolves around the idea that maximizing customer satisfaction inevitably maximizes the long-term profitability of an enterprise.

### References:
[1] Delivering a standards based SDS Framework with an Open Stack SDS Controller Implementation
[2] What is Service Assurance
[3]EMC Day 3: ScaleIO – Unleashed for the world!
[4]EMC ScaleIO Wiki
