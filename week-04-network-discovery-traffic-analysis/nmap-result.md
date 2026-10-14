# Nmap Scan Result

## Scan Information

| Item | Result |
|---|---|
| Scanner | Nmap 7.991 |
| Target | `192.168.56.101` |
| Scan Type | Default Nmap Scan |
| Date | 2026-10-08 |
| Target Status | Host is up |
| Latency | 0.010s |
| Ports Not Shown | 999 closed TCP ports (reset) |

## Nmap Output

``` text
C:\Users\ASUS>nmap 192.168.56.101

Starting Nmap 7.991 ( https://nmap.org ) at 2026-10-08 21:00 +0530
Nmap scan report for 192.168.56.101
Host is up (0.010s latency).
Not shown: 999 closed tcp ports (reset)

PORT   STATE SERVICE
80/tcp open  http

MAC Address: 08:00:27:EE:D0:1F (Oracle VirtualBox virtual NIC)

Nmap done: 1 IP address (1 host up) scanned in 1.19 seconds
```

## Result Analysis

-   The Ubuntu virtual machine was reachable from the Windows host at
    `192.168.56.101`.
-   Nmap confirmed that the host was **up**.
-   TCP port **80** was **open** and identified as the **HTTP** service.
-   The other **999 TCP ports** in the default scan were reported as
    closed.
-   The MAC address was identified as an **Oracle VirtualBox virtual
    NIC**, confirming that the target is a VirtualBox virtual machine.

## Network Configuration

The Ubuntu VM used the Host-Only network for communication with Windows:

-   Windows Host-Only IPv4: `192.168.56.1`
-   Ubuntu Host-Only IPv4: `192.168.56.101/24`
-   Ubuntu interface: `enp0s8`
-   Network: `192.168.56.0/24`

The Ubuntu VM also had a separate NAT interface:

-   Ubuntu interface: `enp0s3`
-   IPv4 address: `10.0.2.15/24`
-   Default gateway: `10.0.2.2`

## Connectivity Verification

A Windows ping test to the Ubuntu VM was successful:

``` text
Pinging 192.168.56.101 with 32 bytes of data:
Reply from 192.168.56.101: bytes=32 time<1ms TTL=64
Reply from 192.168.56.101: bytes=32 time=1ms TTL=64
Reply from 192.168.56.101: bytes=32 time=1ms TTL=64
Reply from 192.168.56.101: bytes=32 time=1ms TTL=64

Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
```

This confirms successful network connectivity between the Windows host
and the Ubuntu VM.

## Wireshark Verification

Wireshark was used on the Windows host to capture traffic on **Ethernet
2**, which corresponds to the Host-Only network.

The capture showed:

-   ICMP Echo Requests and Echo Replies during the ping test.
-   ARP requests and replies for `192.168.56.1` and `192.168.56.101`.
-   TCP SYN packets generated during the Nmap scan.
-   TCP SYN/ACK and RST/ACK responses from the target.
-   Traffic involving TCP port `80`, confirming the HTTP port discovered
    by Nmap.

A Wireshark display filter used to examine the HTTP-port traffic was:

``` text
ip.addr == 192.168.56.101 && tcp.port == 80
```

## Conclusion

The network connection between Windows and the Ubuntu VirtualBox VM was
successfully established through the Host-Only adapter. Ping confirmed
connectivity, Nmap identified the Ubuntu VM as an active host with TCP
port 80 open, and Wireshark successfully captured the related network
traffic.
