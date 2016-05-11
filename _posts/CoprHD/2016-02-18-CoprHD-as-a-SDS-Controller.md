---
layout: post
title: "CoprHD as a SDS Controller"
description: ""
category: CoprHD
tags:  [CoprHD]
---
{% include JB/setup %}

## CoprHD Overview:
CoprHD  is an open source software defined storage (SDS) controller that discovers, pools and automates the management of a heterogeneous(合成的) storage ecosystem.

- Discover different kinds of  storage systems and provide visibility and control of all discovered resources.
    + Namely, it centralizes and transforms multivendor storage into a simple and extensible platform.
    +  e.g. traditional, scale-out, SAN/IP networking, host config, across one or more DCs for new and existing storage.
- Classify storage using policies20
    + abstracts the resources into virtual storage arrays and pools 
    + capable of block, object and file storage provider
- Self-service provisioning via REST APIs and catalogs
- Integrate with traditional, cloud, cloud native computing stacks
- End to end storage automation
    + Provide intelligent resource selection and placement
    + Local and remote protection
    + SAN Zoning
    + Host attach, migration and tech refresh

### Note:
- CoprHD itself does not provide storage. 
- It holds an inventory of all storage devices in the data center and understands their connectivity. 
- It allows the storage administrator to group these resources into either virtual arrays or virtual pools
    

## Concepts: SDS Controller

SDS Controller enables granular(粒化) monitoring and QoS enforcement of storage data types(Volume, shares, containers)
- Discover storage systems and capabilities. e.g. performance, capacity, tiers(层). etc
- Administrators composed  virtual storage pools
- Application  request storages services using  SLOs
    + Controller allocates storage volume from pool that can best service the request
- Storage gets assigned to applivation in VM
- Controller work with compute, network to set QoS

### Note:

#### Storage Quality of Service (QoS):

Storage QoS is a new cluster-wide feature in clustered Data ONTAP 8.2 that requires no additional license. It allows users to set throughput limits and/or monitor IOPS or MB/s on the storage object. A storage object can be:
- A Vserver with FlexVol volumes
- A FlexVol volumeQoS workload
- A LUN
- A file (typically represents a virtual machine)

#### Service Level Agreement (SLA) :
- A part of a service contract where a service is formally defined.
- Refer to the  contracted delivery time (of the service or perfromance)
- Include :
    + MTBF: mean time between failures
    + MTTR: mean time to repair or mean time ti recovery

#### Service Level Objective (SLO):
- a key element of SLA between a service provider and a customer
- a means of measuring the performance of the service provider
- avoiding disputes(争议) between the two parties based on misunderstanding.

## References:
[1] Delivering a standards based SDS Framework with an Open Stack SDS Controller Implementation
[2] Introduction to CoprHD: An Open Source Software Defined Storage Controller
[3] A short Guild to the CoprHD Architecture
[4] FAQ: Storage Quality of Service (QoS)

