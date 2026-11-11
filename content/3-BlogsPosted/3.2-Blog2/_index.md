---
title: "Blog 2"
date: 2026-09-07
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# What is AWS VPC?

A VPC (Virtual Private Cloud) is a logically isolated virtual network in AWS where you launch and manage resources. Here is what you need to know.

### Subnets

* Subnets split the VPC's IP range into smaller networks.
* **Public subnet:** has a route to an Internet Gateway.
* **Private subnet:** has no direct route to an Internet Gateway.
* Each subnet belongs to exactly one Availability Zone.

### 1. Internet Gateway (IGW)

Enables communication between VPC resources and the internet when routing, public IP addresses and security rules allow it.

### 2. NAT Gateway

Lets resources in private subnets initiate outbound internet connections without allowing unsolicited inbound connections.

### 3. Route Tables

Decide where network traffic goes based on its destination IP address.

### 4. VPC Peering

Connects two VPCs so their resources can communicate using private IP addresses.

**Example:**

* VPC A hosts your application.
* VPC B hosts a shared service.
* VPC Peering lets them talk privately once the required routes and access rules are configured.

### Quick facts

* A VPC belongs to one Region and can span multiple Availability Zones.
* A VPC can contain many subnets, classed as public or private by their routing.
* Each subnet belongs to exactly one Availability Zone.
* An EC2 network interface belongs to one subnet; an instance can have several interfaces.
* Subnets in the same VPC communicate via local routes, subject to Security Groups and Network ACLs.
* VPC Peering connects two VPCs for private communication between their resources.

Source : https://www.facebook.com/groups/awsstudygroupfcj/permalink/2318553045576321/?rdid=dZQc4ChgXZA2BKPx#