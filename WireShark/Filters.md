# Wireshark Display Filters Cheat Sheet

> **Purpose:** This document serves as a quick reference for the most commonly used Wireshark display filters during packet analysis, threat hunting, and SOC investigations.

---

# Table of Contents

## Table of Contents

* [Basic Filters](#basic-filters)
* [IP Address Filters](#ip-address-filters)
* [MAC Address Filters](#mac-address-filters)
* [TCP Filters](#tcp-filters)
* [UDP Filters](#udp-filters)
* [Common Protocol Filters](#common-protocol-filters)
* [HTTP Filters](#http-filters)
* [TLS / HTTPS Filters](#tls--https-filters)
* [DNS Filters](#dns-filters)
* [ICMP Filters](#icmp-filters)
* [DHCP Filters](#dhcp-filters)
* [FTP Filters](#ftp-filters)
* [SMB Filters](#smb-filters)
* [SSH Filters](#ssh-filters)
* [Email Protocol Filters](#email-protocol-filters)
* [Packet Size Filters](#packet-size-filters)
* [Time Filters](#time-filters)
* [Stream Filters](#stream-filters)
* [Operators](#operators)
* [Search Inside Packet](#search-inside-packet)
* [SOC Investigation Filters](#soc-investigation-filters)
* [Tips](#tips)

---

# Basic Filters

Display all packets of a specific protocol.

| Filter | Description           |
| ------ | --------------------- |
| `ip`   | IPv4 packets          |
| `ipv6` | IPv6 packets          |
| `tcp`  | TCP packets           |
| `udp`  | UDP packets           |
| `icmp` | ICMP packets          |
| `arp`  | ARP packets           |
| `dns`  | DNS packets           |
| `http` | HTTP packets          |
| `tls`  | TLS/SSL packets       |
| `ftp`  | FTP packets           |
| `ssh`  | SSH packets           |
| `smtp` | SMTP packets          |
| `pop`  | POP3 packets          |
| `imap` | IMAP packets          |
| `dhcp` | DHCP packets          |
| `ntp`  | Network Time Protocol |
| `snmp` | SNMP packets          |

---

# IP Address Filters

## Source IP

```text
ip.src == 192.168.1.100
```

Displays packets originating from the specified IP.

---

## Destination IP

```text
ip.dst == 8.8.8.8
```

Displays packets sent to the destination IP.

---

## Any IP

```text
ip.addr == 192.168.1.100
```

Matches both source and destination IP.

---

## Exclude an IP

```text
!(ip.addr == 192.168.1.100)
```

Hide all traffic involving the specified IP.

---

# MAC Address Filters

## Source MAC

```text
eth.src == 00:11:22:33:44:55
```

---

## Destination MAC

```text
eth.dst == 00:11:22:33:44:55
```

---

## Any MAC

```text
eth.addr == 00:11:22:33:44:55
```

---

## Broadcast Packets

```text
eth.dst == ff:ff:ff:ff:ff:ff
```

---

# TCP Filters

## TCP Traffic

```text
tcp
```

---

## TCP Port

```text
tcp.port == 80
```

---

## Source Port

```text
tcp.srcport == 443
```

---

## Destination Port

```text
tcp.dstport == 22
```

---

## SYN Packets

```text
tcp.flags.syn == 1
```

---

## SYN Only

```text
tcp.flags.syn == 1 && tcp.flags.ack == 0
```

Useful for detecting connection attempts and port scans.

---

## ACK Packets

```text
tcp.flags.ack == 1
```

---

## FIN Packets

```text
tcp.flags.fin == 1
```

---

## RST Packets

```text
tcp.flags.reset == 1
```

Often seen during failed connections.

---

## PSH Packets

```text
tcp.flags.push == 1
```

---

## URG Packets

```text
tcp.flags.urg == 1
```

---

# UDP Filters

```text
udp
```

```text
udp.port == 53
```

```text
udp.port == 67
```

```text
udp.port == 68
```

---

# Common Protocol Filters

| Protocol  | Filter  |
| --------- | ------- |
| HTTP      | `http`  |
| HTTPS/TLS | `tls`   |
| DNS       | `dns`   |
| FTP       | `ftp`   |
| SSH       | `ssh`   |
| SMB       | `smb`   |
| ICMP      | `icmp`  |
| DHCP      | `bootp` |

---

# HTTP Filters

## HTTP Requests

```text
http.request
```

---

## HTTP Responses

```text
http.response
```

---

## GET Requests

```text
http.request.method == "GET"
```

---

## POST Requests

```text
http.request.method == "POST"
```

---

## Response Code

```text
http.response.code == 404
```

---

## Host Header

```text
http.host
```

---

## URI

```text
http.request.uri
```

---

## User Agent

```text
http.user_agent
```

---

## Cookies

```text
http.cookie
```

---

# TLS / HTTPS Filters

```text
tls
```

TLS Handshake

```text
tls.handshake
```

Client Hello

```text
tls.handshake.type == 1
```

Server Hello

```text
tls.handshake.type == 2
```

TLS Alerts

```text
tls.alert_message
```

---

# DNS Filters

DNS Traffic

```text
dns
```

DNS Query

```text
dns.flags.response == 0
```

DNS Response

```text
dns.flags.response == 1
```

Specific Domain

```text
dns.qry.name == "example.com"
```

Contains

```text
dns.qry.name contains "google"
```

---

# ICMP Filters

```text
icmp
```

Echo Request

```text
icmp.type == 8
```

Echo Reply

```text
icmp.type == 0
```

Destination Unreachable

```text
icmp.type == 3
```

---

# DHCP Filters


> **Protocol:** Dynamic Host Configuration Protocol (DHCP)
> **Purpose:** DHCP is responsible for automatically assigning IP addresses and network configuration (subnet mask, gateway, DNS server, lease time, etc.) to devices on a network. During incident response and network investigations, DHCP packets help identify **hosts, users, hostnames, MAC addresses, and assigned IP addresses**.

---

# Table of Contents

* [Display All DHCP Traffic](#display-all-dhcp-traffic)
* [DHCP Message Types](#dhcp-message-types)
* [DHCP Request Filters](#dhcp-request-filters)
* [DHCP ACK Filters](#dhcp-ack-filters)
* [DHCP NAK Filters](#dhcp-nak-filters)
* [DHCP Option Filters](#dhcp-option-filters)
* [Host Identification](#host-identification)
* [SOC Investigation Tips](#soc-investigation-tips)

---

# Display All DHCP Traffic

## DHCP Traffic

```text
dhcp
```

Displays all DHCP packets.

> **Note:** Newer versions of Wireshark use `dhcp`, while older versions may use `bootp`.

---

## BOOTP (Older Versions)

```text
bootp
```

Displays DHCP traffic in older Wireshark releases.

---

# DHCP Message Types

DHCP communication consists of several message types. Identifying these messages helps analysts determine whether a device successfully obtained an IP address or encountered issues.

| DHCP Message | Filter                  | Description                                           |
| ------------ | ----------------------- | ----------------------------------------------------- |
| Discover     | `dhcp.option.dhcp == 1` | Client searches for available DHCP servers.           |
| Offer        | `dhcp.option.dhcp == 2` | DHCP server offers an IP address.                     |
| Request      | `dhcp.option.dhcp == 3` | Client requests the offered IP address.               |
| Decline      | `dhcp.option.dhcp == 4` | Client rejects the offered IP.                        |
| ACK          | `dhcp.option.dhcp == 5` | Server confirms and assigns the IP address.           |
| NAK          | `dhcp.option.dhcp == 6` | Server rejects the request.                           |
| Release      | `dhcp.option.dhcp == 7` | Client releases its leased IP.                        |
| Inform       | `dhcp.option.dhcp == 8` | Client requests additional configuration information. |

---

# DHCP Request Filters

DHCP Request packets are valuable because they contain information supplied by the client.

## DHCP Request

```text
dhcp.option.dhcp == 3
```

Displays all DHCP Request packets.

These packets commonly contain:

* Hostname
* Requested IP Address
* Lease Time
* Client MAC Address

---

# DHCP ACK Filters

ACK packets indicate that the DHCP server successfully assigned an IP address.

## DHCP ACK

```text
dhcp.option.dhcp == 5
```

Displays successful DHCP assignments.

Useful information includes:

* Assigned lease time
* Domain name
* Successful IP assignment

---

# DHCP NAK Filters

NAK packets indicate that the DHCP server rejected the client's request.

## DHCP NAK

```text
dhcp.option.dhcp == 6
```

Displays rejected DHCP requests.

Possible reasons include:

* Invalid lease request
* IP conflict
* Unauthorized client
* Expired lease

---

# DHCP Option Filters

DHCP packets contain various options that provide useful information during investigations.

---

## Option 12 — Hostname

```text
dhcp.option.hostname
```

Displays the hostname requested by the client.

Example:

```
DESKTOP-01
```

Search for a specific hostname:

```text
dhcp.option.hostname contains "DESKTOP"
```

or

```text
dhcp.option.hostname contains "WIN"
```

### SOC Use

* Identify the computer involved.
* Correlate hostnames with Active Directory assets.
* Detect unknown devices joining the network.

---

## Option 50 — Requested IP Address

```text
dhcp.option.requested_ip_address
```

Shows the IP address requested by the client.

Example:

```
192.168.1.25
```

### SOC Use

* Determine which IP a host attempted to obtain.
* Investigate IP conflicts.
* Track repeated DHCP requests.

---

## Option 51 — Lease Time

```text
dhcp.option.ip_address_lease_time
```

Displays the requested or assigned lease duration.

Example:

```
86400 seconds
```

### SOC Use

* Verify lease duration.
* Detect abnormal lease requests.
* Identify DHCP misconfigurations.

---

## Option 61 — Client Identifier (MAC Address)

```text
dhcp.option.client_id
```

Displays the client's hardware identifier (usually the MAC address).

Example:

```
08:00:27:AA:BB:CC
```

### SOC Use

* Identify the physical device.
* Match DHCP activity with switch logs.
* Correlate with ARP entries.

---

## Option 15 — Domain Name

```text
dhcp.option.domain_name
```

Displays the domain name assigned by the DHCP server.

Search for a specific domain:

```text
dhcp.option.domain_name contains "corp"
```

Example:

```
corp.local
```

### SOC Use

* Identify the internal domain.
* Detect rogue DHCP servers assigning incorrect domains.

---

## Option 56 — DHCP Error Message

```text
dhcp.option.message
```

Displays the rejection or error message contained in DHCP NAK packets.

### SOC Use

Read the message instead of filtering on it, since every DHCP server may use different wording.

Example:

```
Address already in use
```

---

# Host Identification

DHCP is one of the easiest ways to identify devices on a network because it links multiple identifiers together.

| Information  | DHCP Option |
| ------------ | ----------- |
| Hostname     | Option 12   |
| Requested IP | Option 50   |
| Lease Time   | Option 51   |
| MAC Address  | Option 61   |
| Domain Name  | Option 15   |

By combining these fields, an analyst can map:

```
Hostname
      ↓
MAC Address
      ↓
Assigned IP Address
      ↓
Domain
```

This mapping is extremely useful during malware investigations and lateral movement analysis.

---

# SOC Investigation Tips

## Find all DHCP requests

```text
dhcp.option.dhcp == 3
```

Used to identify devices requesting an IP address.

---

## Find successful IP assignments

```text
dhcp.option.dhcp == 5
```

Shows hosts that successfully joined the network.

---

## Find rejected devices

```text
dhcp.option.dhcp == 6
```

Useful when troubleshooting unauthorized or rogue devices.

---

## Search for a specific hostname

```text
dhcp.option.hostname contains "PC-01"
```

Quickly locate DHCP traffic generated by a specific machine.

---

## Search for a domain

```text
dhcp.option.domain_name contains "corp"
```

Useful for identifying internal domain assignments.

---

## Correlate Hostname → MAC → IP

During investigations, always correlate:

* **Hostname** (Option 12)
* **MAC Address** (Option 61)
* **Requested IP** (Option 50)
* **Assigned Lease** (ACK Packet)
* **Domain Name** (Option 15)

This provides a complete picture of the host and greatly simplifies identifying compromised or suspicious systems.


---

# FTP Filters

```text
ftp
```

FTP Username

```text
ftp.request.command == "USER"
```

FTP Password

```text
ftp.request.command == "PASS"
```

---

# SMB Filters

```text
smb
```

```text
smb2
```

---

# SSH Filters

```text
ssh
```

---

# Email Protocol Filters

SMTP

```text
smtp
```

POP3

```text
pop
```

IMAP

```text
imap
```

---

# Packet Size Filters

Packets larger than 100 bytes

```text
frame.len > 100
```

Packets smaller than 100 bytes

```text
frame.len < 100
```

Packets larger than 1000 bytes

```text
frame.len > 1000
```

---

# Time Filters

Frame Time

```text
frame.time
```

Relative Time

```text
frame.time_relative
```

Delta Time

```text
frame.time_delta
```

---

# Stream Filters

TCP Stream

```text
tcp.stream == 0
```

UDP Stream

```text
udp.stream == 0
```

---

# Operators

| Operator   | Meaning               |   |            |
| ---------- | --------------------- | - | ---------- |
| `==`       | Equal                 |   |            |
| `!=`       | Not Equal             |   |            |
| `>`        | Greater Than          |   |            |
| `<`        | Less Than             |   |            |
| `>=`       | Greater Than or Equal |   |            |
| `<=`       | Less Than or Equal    |   |            |
| `contains` | Contains a string     |   |            |
| `matches`  | Regular Expression    |   |            |
| `&&`       | Logical AND           |   |            |
| `          |                       | ` | Logical OR |
| `!`        | Logical NOT           |   |            |

---

# Search Inside Packet

Sometimes the protocol itself isn't enough—you may need to search the **payload** or packet contents for specific strings. This is useful when investigating plaintext protocols such as HTTP, FTP, SMTP, Telnet, or unencrypted application traffic.

## Search for a String

Find packets containing the word **password**.

```text
frame contains "password"
```

---

Find packets containing **login**.

```text
frame contains "login"
```

---

Find packets containing **admin**.

```text
frame contains "admin"
```

---

Find packets containing **token**.

```text
frame contains "token"
```

---

Find packets containing **username**.

```text
frame contains "username"
```

---

Find packets containing **Authorization**.

```text
frame contains "Authorization"
```

---

Find packets containing an email address.

```text
frame contains "@gmail.com"
```

---

Find packets containing a file extension.

```text
frame contains ".exe"
```

---

Find packets containing PowerShell commands.

```text
frame contains "powershell"
```

---

Find packets containing CMD commands.

```text
frame contains "cmd.exe"
```

---

Find packets containing suspicious URLs.

```text
frame contains "http://"
```

---

Find packets containing Base64 strings.

```text
frame contains "=="
```

---

## Search Within Specific Protocols

Search for HTTP packets containing **password**.

```text
http contains "password"
```

---

Search for DNS packets containing a specific domain.

```text
dns contains "example"
```

---

Search for FTP packets containing **USER**.

```text
ftp contains "USER"
```

---

Search for SMTP packets containing **MAIL FROM**.

```text
smtp contains "MAIL FROM"
```

---

## SOC Use Cases

| Investigation           | Filter                           |
| ----------------------- | -------------------------------- |
| Plaintext credentials   | `frame contains "password"`      |
| Login attempts          | `frame contains "login"`         |
| API Tokens              | `frame contains "Authorization"` |
| Malware download        | `frame contains ".exe"`          |
| PowerShell execution    | `frame contains "powershell"`    |
| Command Prompt activity | `frame contains "cmd.exe"`       |
| Suspicious URLs         | `frame contains "http://"`       |
| Email investigation     | `frame contains "@gmail.com"`    |

> **Note:** `frame contains` works only when the searched data is present in plaintext. It cannot search inside encrypted TLS/HTTPS traffic.

---

# SOC Investigation Filters

## HTTP POST Requests

```text
http.request.method == "POST"
```

---

## DNS Traffic

```text
dns
```

---

## SYN Packets

```text
tcp.flags.syn == 1 && tcp.flags.ack == 0
```

---

## TCP Reset

```text
tcp.flags.reset == 1
```

---

## Large Packets

```text
frame.len > 1000
```

---

## Traffic to a Suspicious Host

```text
ip.addr == 10.10.10.10
```

---

## Failed HTTP Requests

```text
http.response.code == 404
```

---

## DNS Query for a Domain

```text
dns.qry.name contains "example"
```

---

# Tips

* Prefer **display filters** over scrolling through packets manually.
* Combine filters with `&&` and `||` to narrow results.
* Use **Follow TCP Stream** after identifying an interesting packet.
* Search packet contents with `frame contains` only for plaintext protocols.
* For encrypted HTTPS traffic, inspect metadata (SNI, certificates, DNS, IPs, timing) instead of expecting to see the encrypted payload.
* Frequently used filters can be saved as **Display Filter Macros** or bookmarked for faster investigations.
