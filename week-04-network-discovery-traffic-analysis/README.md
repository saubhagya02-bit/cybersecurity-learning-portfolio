# Windows--Ubuntu VirtualBox Network and Nmap/Wireshark Activity

## Overview

This activity demonstrates communication between a Windows host and an
Ubuntu virtual machine running in Oracle VirtualBox. The Host-Only
network was used to allow direct communication between the Windows host
and Ubuntu VM.

Nmap was used to identify the target host and open TCP ports, while
Wireshark was used to capture and inspect the network traffic generated
during the tests.

## Network Topology

``` text
Windows Host
192.168.56.1
      |
      | VirtualBox Host-Only Network
      | 192.168.56.0/24
      |
Ubuntu VM
enp0s8: 192.168.56.101
      |
      +-- TCP/80 (HTTP) OPEN

Ubuntu also has:
enp0s3: 10.0.2.15
Default gateway: 10.0.2.2
```

## Requirements

-   Windows host
-   Oracle VirtualBox
-   Ubuntu virtual machine
-   Nmap 7.991
-   Wireshark

## Network Configuration

The Ubuntu VM uses two network interfaces:

### Adapter 1 / NAT

``` text
Interface: enp0s3
IPv4:      10.0.2.15/24
Gateway:   10.0.2.2
```

This interface provides the VM's NAT connection.

### Adapter 2 / Host-Only

``` text
Interface: enp0s8
IPv4:      192.168.56.101/24
Network:   192.168.56.0/24
```

The Windows host uses:

``` text
192.168.56.1
```

This Host-Only interface is the network used for the testing activity.

## Step 1 --- Verify Ubuntu IP Address

On Ubuntu:

``` bash
ip addr
```

The relevant address is:

``` text
enp0s8
inet 192.168.56.101/24
```

## Step 2 --- Verify the Routing Table

Run:

``` bash
ip route
```

The important routes include:

``` text
default via 10.0.2.2 dev enp0s3
10.0.2.0/24 dev enp0s3
192.168.56.0/24 dev enp0s8
```

## Step 3 --- Test Connectivity from Windows

From Windows Command Prompt:

``` cmd
ping 192.168.56.101
```

The test completed successfully with:

``` text
Sent = 4, Received = 4, Lost = 0 (0% loss)
```

Therefore, Windows could communicate successfully with the Ubuntu VM.

## Step 4 --- Run Nmap

From Windows:

``` cmd
nmap 192.168.56.101
```

### Result

``` text
Nmap scan report for 192.168.56.101
Host is up (0.010s latency).
Not shown: 999 closed tcp ports (reset)

PORT   STATE SERVICE
80/tcp open  http

MAC Address: 08:00:27:EE:D0:1F (Oracle VirtualBox virtual NIC)
```

The scan found:

-   Host: `192.168.56.101`
-   Host status: **Up**
-   Open port: **80/tcp**
-   Service: **HTTP**
-   Closed ports reported: **999**

## Step 5 --- Capture Traffic with Wireshark

Wireshark was started on the Windows host using the **Ethernet 2**
interface.

The capture demonstrated that traffic was being generated and received
successfully.

Observed traffic included:

-   ICMP ping packets
-   ARP packets
-   TCP SYN packets from the Nmap scan
-   TCP SYN/ACK responses
-   TCP RST/ACK responses

To focus on port 80 traffic, the following display filter can be used:

``` text
ip.addr == 192.168.56.101 && tcp.port == 80
```

## Interpretation

The results show that the Host-Only network is functioning correctly.

The Windows host (`192.168.56.1`) can reach the Ubuntu VM
(`192.168.56.101`). Nmap confirms that the Ubuntu VM is active and that
TCP port 80 is accessible. Wireshark independently shows the packets
associated with the connectivity test and Nmap scan.

## Conclusion

The activity was successfully completed:

1.  Ubuntu received the expected Host-Only IP address.
2.  Windows successfully pinged Ubuntu with **0% packet loss**.
3.  Nmap successfully detected the Ubuntu host.
4.  Nmap identified **TCP port 80 (HTTP)** as open.
5.  Wireshark successfully captured the network traffic on Ethernet 2.
6.  The captured packets provide evidence of ICMP, ARP, and TCP
    communication.

