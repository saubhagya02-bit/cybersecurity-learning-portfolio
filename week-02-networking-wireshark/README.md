# Week 2 - Networking Fundamentals and Wireshark

## Overview

Week 2 focused on understanding how normal network communication works and learning how to observe network traffic directly using Wireshark.

The practical exercise involved generating my own network traffic and analysing ICMP, DNS, TCP, and TLS/HTTPS packets.

## Objectives

* Understand IP addresses and MAC addresses.
* Understand TCP and UDP.
* Understand common ports and services.
* Understand DNS and domain-name resolution.
* Understand HTTP and HTTPS.
* Understand ICMP and ping.
* Understand TCP connection establishment.
* Identify source and destination IP addresses.
* Identify source and destination ports.
* Capture and analyse network traffic using Wireshark.
* Understand Wireshark capture and display filters.

## Concepts Learned

### IP Address

An IP address identifies a device or network interface at the network layer and allows network packets to be routed between networks.

### MAC Address

A MAC address identifies a network interface at the data-link layer and is primarily used for communication within a local network.

### TCP vs UDP

TCP is connection-oriented and provides reliable, ordered communication. UDP is connectionless and has less protocol overhead.

### Common Ports

| Port | Common Service |
| ---: | -------------- |
|   22 | SSH            |
|   53 | DNS            |
|   80 | HTTP           |
|  443 | HTTPS          |

### DNS

The Domain Name System translates domain names into IP addresses so that users and applications can use human-readable names for network resources.

### HTTP vs HTTPS

HTTP commonly uses port 80 and does not provide TLS encryption by itself. HTTPS commonly uses port 443 and uses TLS to protect application data during transmission.

### ICMP

ICMP is used for network control and diagnostic messages. The `ping` command commonly uses ICMP Echo Request and Echo Reply messages.

### TCP Three-Way Handshake

TCP establishes a connection using:

```text
SYN
  ↓
SYN/ACK
  ↓
ACK
```

This allows the client and server to establish the TCP connection before normal communication.

## Practical Lab

The following traffic was generated and analysed:

### ICMP

Command:

```text
ping 8.8.8.8
```

Wireshark display filter:

```text
icmp
```

Observed:

* Echo Request
* Echo Reply
* Source IP
* Destination IP

### DNS

Command:

```text
nslookup example.com
```

Wireshark display filter:

```text
dns
```

Observed:

* DNS query
* DNS response
* Queried domain
* DNS server
* Returned address

### TCP

Wireshark display filter:

```text
tcp
```

Observed:

* SYN
* SYN/ACK
* ACK
* Source port
* Destination port

### TLS/HTTPS

Wireshark display filter:

```text
tls
```

Observed:

* TLS communication
* Port 443
* TLS-related information
* Network metadata visible despite encrypted application data

## Evidence

Screenshots collected during the practical are stored in the `screenshots` directory.

* `active-capture.png` - Active Wireshark capture/interface
* `icmp-request-reply.png` - ICMP Echo Request and Echo Reply
* `dns-query-response.png` - DNS query and response
* `tcp-handshake.png` - TCP SYN, SYN/ACK and ACK
* `tls-https.png` - TLS/HTTPS traffic

## Lab Report

The complete practical analysis is available in:

[wireshark-lab.md](wireshark-lab.md)


## Capture Filters vs Display Filters

A capture filter controls which packets are captured during a capture session.

A display filter controls which packets are shown after packets have already been captured.

Examples of display filters used in this lab:

```text
icmp
dns
tcp
tls
```