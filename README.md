# VPC Traffic Flow and Security

**Author:** Thato Ntoahae

## Overview

This project builds an Amazon VPC and configures traffic flow and security
controls across it — route tables, security groups, and network ACLs.

Amazon VPC (Virtual Private Cloud) provides an isolated section of AWS where
you control IP ranges, subnets, routing, and access. In this project it was
used to create subnets, configure route tables, and apply network ACLs on
top of security groups for layered network defense.

## Architecture

> Fill in: CIDR block for the VPC, public/private subnet split, AZs used,
> and whether a NAT gateway is present. A diagram here (even a simple one)
> will do more for this README than any paragraph of text.

## Route Tables

Route tables are sets of rules ("routes") that determine where subnet
traffic is directed. A subnet is "public" only if its route table sends
internet-bound traffic to an Internet Gateway.

**Route configured:**
| Destination | Target |
|---|---|
| `0.0.0.0/0` | Internet Gateway |

This routes all traffic not destined for the VPC's local CIDR out to the
internet via the IGW.

## Security Groups

Security groups act as **stateful, instance-level** firewalls — return
traffic is automatically allowed, so you only need to define one direction
explicitly.

> Fill in with actual rules, e.g.:
> - **Inbound:** TCP 22 (SSH) from `<your IP>/32`; TCP 80/443 from `0.0.0.0/0`
> - **Outbound:** default allow-all (`0.0.0.0/0`, all protocols)

Replace this with your real rule set — protocol, port range, and source/destination CIDR. "Allows traffic in the public network" isn't specific enough to audit or defend in review.

## Network ACLs

Network ACLs are **stateless, subnet-level** firewalls — they evaluate
numbered rules in order and require explicit rules for both inbound and
outbound traffic (including return traffic on ephemeral ports), since
nothing is remembered between directions.

| | Security Group | Network ACL |
|---|---|---|
| Scope | Instance-level | Subnet-level |
| State | Stateful (return traffic auto-allowed) | Stateless (must allow both directions) |
| Rule evaluation | All rules evaluated | Rules evaluated in order, first match wins |
| Layer | First layer of defense | Second layer of defense |

**Default vs. custom NACLs:**
- A **default NACL** (the one AWS creates automatically) allows **all**
  inbound and outbound traffic out of the box.
- A **custom NACL** does the opposite — it **denies all traffic by
  default** until you add explicit allow rules.

> Fill in your actual custom ACL rules if you created one, and note why
> each rule exists (e.g., "allow inbound 443 from 0.0.0.0/0 for public web
> traffic; allow outbound ephemeral ports 1024–65535 for return traffic").

## What I Didn't Expect

Creating a custom NACL wasn't something I initially thought the VPC
needed — it made me realize security groups alone aren't a complete
answer, since they don't protect against misconfiguration at the subnet
boundary.

## Time Spent

~3 hours

---
*Project based on a NextWork.ai guided lab.*
