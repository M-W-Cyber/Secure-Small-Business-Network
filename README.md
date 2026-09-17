# Secure-Small-Business-Network
# Secure Small Business Network

## Overview

This project is a small-business network I designed and configured in Cisco Packet Tracer to practice networking and cybersecurity concepts I have been learning through my coursework.

The network separates an Accounting department and an IT department into two different IPv4 networks connected by a Cisco router. After establishing connectivity between the networks, I implemented access controls to restrict communication from Accounting into the IT network while still allowing access to an authorized HTTP service.

I also configured SSH for secure remote administration of the router and disabled Telnet.

## Network Topology

![Network Topology](screenshots/network-topology.png)

The network consists of:

- **Accounting Network:** 192.168.10.0/24
- **IT Network:** 192.168.20.0/24
- **R1 G0/0:** 192.168.10.1
- **R1 G0/1:** 192.168.20.1
- **Accounting PC:** 192.168.10.10
- **IT PC:** 192.168.20.20
- **File Server:** 192.168.20.100

## Project Objectives

The main objectives of this project were to:

- Build two separate departmental networks
- Configure routing between the networks
- Verify connectivity before implementing security controls
- Secure remote router administration with SSH
- Disable insecure Telnet remote access
- Implement an extended ACL between Accounting and IT
- Apply least-privilege access to a server
- Test and verify permitted and denied traffic

## Access Control

I created an extended ACL named `ACCOUNTING-SECURITY`.

The final access policy was:

| Source | Destination | Service | Action |
|---|---|---|---|
| Accounting | File Server | HTTP (TCP/80) | Permit |
| Accounting | IT Network | All other IP traffic | Deny |
| Other Traffic | Any | IP | Permit |

The ACL configuration was:

```text
ip access-list extended ACCOUNTING-SECURITY
 5 permit tcp 192.168.10.0 0.0.0.255 host 192.168.20.100 eq 80
 10 deny ip 192.168.10.0 0.0.0.255 192.168.20.0 0.0.0.255
 20 permit ip any any
