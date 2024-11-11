---
layout: post
title: Security Tools
date: 2024-11-10 20:38 +0530
author: "Gaurav Kumar"
tags: "tools theory"
categories: "security"
---

## SECURITY ESSENTIALS

### NMAP

Network Mapper (Nmap) is an open-source network analysis and security auditing tool written in C, C++, Python, and Lua.

#### Scan Techniques

```bash
root@96250515cfd7:/# nmap --help
# <SNIP>
SCAN TECHNIQUES:
  -sS/sT/sA/sW/sM: TCP SYN/Connect()/ACK/Window/Maimon scans
  -sU: UDP Scan
  -sN/sF/sX: TCP Null, FIN, and Xmas scans
  --scanflags <flags>: Customize TCP scan flags
  -sI <zombie host[:probeport]>: Idle scan
  -sY/sZ: SCTP INIT/COOKIE-ECHO scans
  -sO: IP protocol scan
  -b <FTP relay host>: FTP bounce scan
# <SNIP>
```

- If our target sends a `SYN-ACK` flagged packet back to us, Nmap detects that the port is open.
- If the target responds with an `RST` flagged packet, it is an indicator that the port is closed.
- If Nmap does not receive a packet back, it will display it as filtered. Depending on the firewall configuration, certain packets may be dropped or ignored by the firewall.

```bash
root@96250515cfd7:/# nmap -sS localhost
Starting Nmap 7.93 ( https://nmap.org ) at 2024-11-10 15:10 UTC
Nmap scan report for localhost (127.0.0.1)
Host is up (0.0000040s latency).
Other addresses for localhost (not scanned): ::1
Not shown: 999 closed tcp ports (reset)
PORT   STATE SERVICE
80/tcp open  http

Nmap done: 1 IP address (1 host up) scanned in 0.28 seconds
```

#### Host Discovery

To see which systems are online on a network, we can use Nmap's host discovery options. Nmap offers several methods to check if a target is active, with `ICMP` echo requests (ping) being the most effective way to discover live hosts.

| Option         | Description                                                                                           |
| -------------- | ----------------------------------------------------------------------------------------------------- |
| -sn            | Disables port scanning.                                                                               |
| -oA host       | Stores the results in all formats (normal, XML, and grepable) with the base filename 'host'.          |
| -PE            | Performs the ping scan using ICMP Echo requests to check if the target is online.                     |
| --packet-trace | Shows all packets sent and received during the scan, useful for troubleshooting network interactions. |

```bash
root@96250515cfd7:/# nmap bing.com -sn -oA host -PE --packet-trace
Starting Nmap 7.93 ( https://nmap.org ) at 2024-11-10 15:25 UTC
SENT (0.0672s) ICMP [172.17.0.2 > 13.107.21.200 Echo request (type=8/code=0) id=9855 seq=0] IP [ttl=49 id=10747 iplen=28 ]
RCVD (0.0805s) ICMP [13.107.21.200 > 172.17.0.2 Echo reply (type=0/code=0) id=9855 seq=0] IP [ttl=63 id=34399 iplen=28 ]
NSOCK INFO [0.1660s] nsock_iod_new2(): nsock_iod_new (IOD #1)
NSOCK INFO [0.1660s] nsock_connect_udp(): UDP connection requested to 192.168.65.7:53 (IOD #1) EID 8
NSOCK INFO [0.1660s] nsock_read(): Read request from IOD #1 [192.168.65.7:53] (timeout: -1ms) EID 18
NSOCK INFO [0.1660s] nsock_write(): Write request for 44 bytes to IOD #1 EID 27 [192.168.65.7:53]
NSOCK INFO [0.1660s] nsock_trace_handler_callback(): Callback: CONNECT SUCCESS for EID 8 [192.168.65.7:53]
NSOCK INFO [0.1660s] nsock_trace_handler_callback(): Callback: WRITE SUCCESS for EID 27 [192.168.65.7:53]
NSOCK INFO [0.2420s] nsock_trace_handler_callback(): Callback: READ SUCCESS for EID 18 [192.168.65.7:53] (44 bytes): .............200.21.107.13.in-addr.arpa.....
NSOCK INFO [0.2430s] nsock_read(): Read request from IOD #1 [192.168.65.7:53] (timeout: -1ms) EID 34
NSOCK INFO [0.2430s] nsock_iod_delete(): nsock_iod_delete (IOD #1)
NSOCK INFO [0.2430s] nevent_delete(): nevent_delete on event #34 (type READ)
Nmap scan report for bing.com (13.107.21.200)
Host is up (0.014s latency).
Other addresses for bing.com (not scanned): 204.79.197.200 2620:1ec:c11::200
Nmap done: 1 IP address (1 host up) scanned in 0.24 seconds
```

The results are stored in the current working directory which would look like this (name is host due to `-oA` argument used):

- host.nmap: The standard output format of the scan.
- host.xml: The XML format, useful for parsing and automation.
- host.gnmap: The grepable format, which is easier to parse in scripts.

```bash
root@96250515cfd7:/test# cat host.gnmap
# Nmap 7.93 scan initiated Sun Nov 10 15:29:29 2024 as: nmap -sn -oA host -PE --packet-trace bing.com
Host: 13.107.21.200 ()  Status: Up
# Nmap done at Sun Nov 10 15:29:30 2024 -- 1 IP address (1 host up) scanned in 0.32 seconds
root@96250515cfd7:/test#
root@96250515cfd7:/test# cat host.nmap
# Nmap 7.93 scan initiated Sun Nov 10 15:29:29 2024 as: nmap -sn -oA host -PE --packet-trace bing.com
SENT (0.0732s) ICMP [172.17.0.2 > 13.107.21.200 Echo request (type=8/code=0) id=28583 seq=0] IP [ttl=40 id=47322 iplen=28 ]
RCVD (0.0859s) ICMP [13.107.21.200 > 172.17.0.2 Echo reply (type=0/code=0) id=28583 seq=0] IP [ttl=63 id=60460 iplen=28 ]
Nmap scan report for bing.com (13.107.21.200)
Host is up (0.013s latency).
Other addresses for bing.com (not scanned): 204.79.197.200 2620:1ec:c11::200
# Nmap done at Sun Nov 10 15:29:30 2024 -- 1 IP address (1 host up) scanned in 0.32 seconds
root@96250515cfd7:/test#
root@96250515cfd7:/test# cat host.xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE nmaprun>
<?xml-stylesheet href="file:///usr/bin/../share/nmap/nmap.xsl" type="text/xsl"?>
<!-- Nmap 7.93 scan initiated Sun Nov 10 15:29:29 2024 as: nmap -sn -oA host -PE -&#45;packet-trace bing.com -->
<nmaprun scanner="nmap" args="nmap -sn -oA host -PE -&#45;packet-trace bing.com" start="1731252569" startstr="Sun Nov 10 15:29:29 2024" version="7.93" xmloutputversion="1.05">
<verbose level="0"/>
<debugging level="0"/>
<host><status state="up" reason="echo-reply" reason_ttl="63"/>
<address addr="13.107.21.200" addrtype="ipv4"/>
<hostnames>
<hostname name="bing.com" type="user"/>
</hostnames>
<times srtt="12988" rttvar="12988" to="100000"/>
</host>
<runstats><finished time="1731252570" timestr="Sun Nov 10 15:29:30 2024" summary="Nmap done at Sun Nov 10 15:29:30 2024; 1 IP address (1 host up) scanned in 0.32 seconds" elapsed="0.32" exit="success"/><hosts up="1" down="0" total="1"/>
</runstats>
</nmaprun>
```

We can also use `--reason` option to find why Nmap has marked the target as `alive`:

- Option `-PE` Performs the ping scan by using 'ICMP Echo requests' against the target.

```bash
root@96250515cfd7:/test# nmap bing.com -sn -oA host -PE --reason
Starting Nmap 7.93 ( https://nmap.org ) at 2024-11-10 15:32 UTC
Nmap scan report for bing.com (13.107.21.200)
Host is up, received echo-reply ttl 63 (0.013s latency).
Other addresses for bing.com (not scanned): 204.79.197.200 2620:1ec:c11::200
Nmap done: 1 IP address (1 host up) scanned in 0.16 seconds
```
