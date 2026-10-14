# Network Scanning and Packet Analysis Report

**Cybersecurity Foundations – Week 4: Network Discovery and Traffic Analysis**

## 1. Objective and Scope

The objective of this practical was to combine networking, Linux, Nmap, and Wireshark knowledge in a controlled local laboratory environment.

The main objectives were to:

* Identify the IP configuration of the Windows host and Ubuntu virtual machine.
* Verify connectivity between the Windows host and Ubuntu VM.
* Examine the Linux routing table and listening network services.
* Perform a basic Nmap scan against the Ubuntu VM.
* Capture and analyse network traffic using Wireshark.
* Compare Nmap results with Linux listening socket information.
* Interpret network observations from a cybersecurity perspective.

All scanning activities were performed against my own Ubuntu virtual machine in an authorised local laboratory environment.

## 2. Lab Environment

The practical used the following environment:

| Component                   | Configuration     |
| --------------------------- | ----------------- |
| Host operating system       | Microsoft Windows |
| Virtual machine             | Ubuntu Linux      |
| Virtualisation software     | Oracle VirtualBox |
| Network configuration       | NAT and Host-Only |
| Windows host-only IP        | `192.168.56.1`    |
| Ubuntu VM host-only IP      | `192.168.56.101`  |
| Ubuntu host-only interface  | `enp0s8`          |
| Host-only network           | `192.168.56.0/24` |
| Nmap version                | 7.991             |
| Packet analysis tool        | Wireshark         |
| Wireshark capture interface | Ethernet 2        |

The Ubuntu VM used `enp0s3` for its NAT connection and `enp0s8` for communication through the host-only network.

### Network Diagram

```text
          VirtualBox Host-Only Network
                192.168.56.0/24
                       |
             +---------+---------+
             |                   |
       Windows Host          Ubuntu VM
       192.168.56.1         192.168.56.101
             |                   |
             +------ Nmap -------+
                       |
                 TCP Port 80
                      HTTP

          Wireshark captured traffic
           on the Windows host
```

## 3. Networking Observations

### 3.1 Windows IP Configuration

The Windows `ipconfig` command was used to identify the host-only adapter's network configuration.

The relevant results were:

* IPv4 address: `192.168.56.1`
* Subnet mask: `255.255.255.0`
* Default gateway: None on the host-only adapter

The address can be represented as `192.168.56.1/24`.

### 3.2 Ubuntu IP Configuration

The Ubuntu command `ip addr` was used to identify the virtual machine's network interfaces.

The relevant host-only interface was `enp0s8`, with the address `192.168.56.101/24`.

Both systems were therefore connected to the `192.168.56.0/24` host-only network.

### 3.3 Routing Table

The command below was used to inspect the Ubuntu VM's routing configuration:

```bash
ip route
```

The relevant entries were:

```text
default via 10.0.2.2 dev enp0s3
10.0.2.0/24 dev enp0s3
192.168.56.0/24 dev enp0s8
```

The route for `192.168.56.0/24` uses `enp0s8`. This indicates that traffic destined for the host-only network is sent through that interface.

The default route through `10.0.2.2` uses `enp0s3`, which is associated with the NAT connection.

### 3.4 Connectivity Test

Connectivity from Windows to the Ubuntu VM was tested using:

```cmd
ping 192.168.56.101
```

The command returned four replies. The reported statistics were:

* Packets sent: 4
* Packets received: 4
* Packets lost: 0
* Packet loss: 0%

This confirmed successful IP connectivity between the Windows host and Ubuntu VM during the test.

### 3.5 Week 2 Traffic Observations

Previous networking exercises used Wireshark to examine ICMP, DNS, TCP, and TLS traffic.

| Protocol | Purpose                       | Observation                                                  |
| -------- | ----------------------------- | ------------------------------------------------------------ |
| ICMP     | Connectivity testing          | Echo requests and replies were observed during ping testing. |
| DNS      | Domain name resolution        | Queries and responses were examined during the earlier lab.  |
| TCP      | Reliable communication        | TCP connection-establishment packets were studied.           |
| TLS      | Encrypted communication       | TLS traffic was examined during HTTPS browsing.              |
| ARP      | Local IPv4 address resolution | ARP requests involving the host and VM were observed.        |

These observations provided a useful baseline for interpreting the traffic captured during Week 4.

## 4. Linux Observations

### 4.1 Listening Sockets

The following command was executed inside Ubuntu:

```bash
ss -tuln
```

The output included listening sockets on TCP port 80:

```text
0.0.0.0:80
[::]:80
```

Port 80 is conventionally associated with HTTP. These entries indicate that a service was listening on port 80 for IPv4 and IPv6, respectively.

The exact addresses on which a service listens are important. A listener bound to all interfaces may be reachable through more than one network interface, subject to routing and firewall rules.

### 4.2 Comparison with Nmap

The `ss -tuln` command reports listening sockets on the Ubuntu system. Nmap, on the other hand, tests the reachability of ports from the scanning host.

The local socket information and the Nmap result were consistent for TCP port 80. However, a listening socket is not necessarily reachable from every network, and an Nmap scan cannot describe every aspect of the service's configuration or security.

## 5. Nmap Results

A basic scan was run from Windows against the Ubuntu VM:

```cmd
nmap 192.168.56.101
```

The important output was:

```text
Nmap scan report for 192.168.56.101

Host is up (0.010s latency).

Not shown: 999 closed tcp ports (reset)

PORT   STATE SERVICE
80/tcp open  http
```

### 5.1 Results Summary

| Item                      | Result           |
| ------------------------- | ---------------- |
| Target IP                 | `192.168.56.101` |
| Host status               | Up               |
| Approximate latency       | 0.010 seconds    |
| Default TCP ports scanned | 1,000            |
| Closed TCP ports reported | 999              |
| Open TCP ports reported   | 1                |
| Open port                 | `80/tcp`         |
| Associated service        | HTTP             |

### 5.2 Interpretation

The scan detected the Ubuntu VM and reported TCP port 80 as open. Nmap associated the port with HTTP, which is the service normally associated with TCP port 80.

The other 999 ports in the default TCP scan were reported closed. This result applies to the default ports scanned and does not establish the state of every possible TCP or UDP port on the VM.

The scan was consistent with the listening socket observed using `ss -tuln`.

## 6. Wireshark Packet Evidence

Wireshark was used to capture traffic on the Windows host's `Ethernet 2` interface. The capture included ARP, ICMP, ICMPv6, and TCP traffic.

The following display filters were useful during analysis.

Traffic involving the Ubuntu VM:

```text
ip.addr == 192.168.56.101
```

TCP traffic involving port 80:

```text
ip.addr == 192.168.56.101 && tcp.port == 80
```

### 6.1 ARP Packets

ARP requests similar to the following were observed:

```text
Who has 192.168.56.101?
Tell 192.168.56.1
```

ARP helps resolve a local IPv4 address to a MAC address. These requests are consistent with address resolution on the host-only network.

### 6.2 ICMP Packets

ICMP Echo Request and Echo Reply packets were observed between the Windows host and Ubuntu VM during the ping test.

The request was sent from the Windows host to the VM, and the reply travelled in the opposite direction. This matched the successful ping results.

### 6.3 TCP Port 80

A TCP SYN packet was observed from a client source port to destination port 80. A SYN-ACK response from the VM's port 80 was also observed.

These packets represent the initial stages of the TCP three-way handshake:

```text
Windows Host                     Ubuntu VM
     |                                |
     | -------- SYN ----------------> |
     |                                |
     | <------ SYN, ACK ------------- |
     |                                |
     | -------- ACK ----------------> |
     |                                |
```

The SYN requests a TCP connection. The SYN-ACK acknowledges the request and indicates that the server is prepared to continue establishing the connection.

A complete handshake requires the final ACK as well. The presence of only SYN and SYN-ACK packets should not, by itself, be described as proof that the connection fully completed.

### 6.4 TCP Retransmissions

The capture also displayed TCP retransmission annotations associated with port 80, including a retransmitted SYN-ACK.

A retransmission occurs when a TCP segment is sent again because the expected acknowledgement was not received within the relevant timeout period.

Possible explanations include packet loss, filtering, timing issues, or an incomplete connection. The available evidence does not establish which explanation applies in this case.

A retransmission is not, by itself, proof of a security incident.

## 7. Packet Observation Table

The table below summarises representative packet types observed during the exercise.

| Packet / observation | Protocol | Source              | Destination         | Interpretation                                 |
| -------------------- | -------- | ------------------- | ------------------- | ---------------------------------------------- |
| Echo Request         | ICMP     | `192.168.56.1`      | `192.168.56.101`    | Connectivity test request                      |
| Echo Reply           | ICMP     | `192.168.56.101`    | `192.168.56.1`      | Response to the connectivity test              |
| ARP request          | ARP      | `192.168.56.1`      | Local network       | Resolves a local IPv4 address to a MAC address |
| TCP SYN              | TCP      | Windows host        | `192.168.56.101:80` | Requests a TCP connection                      |
| TCP SYN-ACK          | TCP      | `192.168.56.101:80` | Windows host        | Responds to the connection request             |
| TCP retransmission   | TCP      | `192.168.56.101:80` | Windows host        | A TCP segment was transmitted again            |


## 8. Security Interpretation

The practical demonstrated how a listening service can be identified using both host-level commands and network-level scanning.

TCP port 80 was listening on the Ubuntu VM and was reported open by Nmap from the Windows host. This indicates that the HTTP service was reachable from the host-only network during the scan.

Exposed services can increase a system's attack surface. Their security significance depends on whether they are required, correctly configured, maintained, and appropriately restricted.

An open port does not automatically indicate a vulnerability or compromise. Likewise, a network scan can be a legitimate activity when performed with authorisation and within an approved scope.

The observations in this exercise were consistent with an authorised local laboratory test. The evidence does not establish that malicious activity occurred.

## 9. Limitations and Further Investigation

The practical had the following limitations:

1. The investigation focused on one Ubuntu VM and one Windows host.
2. The scan used Nmap's default TCP port selection.
3. The scan did not assess every TCP or UDP port.
4. The scan did not establish whether the HTTP service contained vulnerabilities.
5. Wireshark only captured traffic visible on the selected interface during the capture period.
6. The reason for the TCP retransmissions was not conclusively determined.
7. The exercise did not constitute a comprehensive security audit or penetration test.

Further authorised investigation could include identifying the software providing HTTP, reviewing its configuration and update status, checking relevant firewall rules, and comparing service behaviour with system logs.

## 10. Key Lessons Learned

1. IP addresses and subnet masks help determine which systems belong to a network.
2. Routing tables show which interface is used to reach a destination.
3. Ping can help verify basic network connectivity.
4. The Linux `ss -tuln` command identifies listening TCP and UDP sockets.
5. Nmap provides a view of ports reachable from the scanning host.
6. Wireshark supports detailed packet-level analysis.
7. ARP is used to resolve local IPv4 addresses to MAC addresses.
8. TCP SYN and SYN-ACK packets represent the initial stages of connection establishment.
9. TCP retransmissions need context and are not automatically signs of malicious activity.
10. Reliable security conclusions use multiple sources of evidence rather than relying on a single scan or packet.

## 11. Commands and Filters Used

### Windows

Identify network configuration:

```cmd
ipconfig
```

Test connectivity:

```cmd
ping 192.168.56.101
```

Scan the Ubuntu VM:

```cmd
nmap 192.168.56.101
```

### Ubuntu

Display network interfaces:

```bash
ip addr
```

Display routes:

```bash
ip route
```

Display listening sockets:

```bash
ss -tuln
```

### Wireshark

Display traffic involving the Ubuntu VM:

```text
ip.addr == 192.168.56.101
```

Display TCP traffic involving port 80:

```text
ip.addr == 192.168.56.101 && tcp.port == 80
```

## 12. Conclusion

The Week 4 network discovery and traffic analysis practical was completed in a controlled VirtualBox laboratory environment.

The Windows host and Ubuntu VM communicated through the host-only network. The successful ping test showed four replies with no packet loss. The Ubuntu routing table associated the host-only network with `enp0s8`, and the `ss -tuln` output showed a listening service on TCP port 80.

The basic Nmap scan identified the VM as active and reported `80/tcp` as open with the service label `http`. This result was consistent with the local listening socket information.

Wireshark captured ARP, ICMP, and TCP traffic involving the host-only network. TCP SYN and SYN-ACK packets associated with port 80 were observed, along with retransmission annotations that could not be attributed to a single confirmed cause from the available evidence.

Overall, the practical demonstrated how Linux commands, Nmap, and Wireshark can be combined to investigate network configuration, identify reachable services, and interpret packet behaviour. It also reinforced the importance of authorised testing, accurate evidence, and cautious security conclusions.

The objectives of this practical were achieved within the scope of the available lab evidence.
