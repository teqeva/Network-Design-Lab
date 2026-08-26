# Networking Design Mini-Capstone

The company has:

- an Admin team
- an Engineering team
- an internal Server segment

You have been given this parent network block: **10.20.30.0/24**

Design three subnets from this block and document your addressing plan.

---

## Task 1: Subnet Planning

In Binary:

```text
00001010.00010100.00011110.00000000

2^7    2^6    2^5    2^4    2^3    2^2    2^1    2^0
128     64     32     16      8      4      2      1
````

New subnet mask: **255.255.255.0**

The subnet masks/prefixes needed for each segment based on host requirements.

**No. of hosts = 2^n − 2, where n = host bits that can be added to the network portion of IP in order to create subnets**

**No. of subnets = 2^n**

**All borrowing happens in octet 4 only**

### 1. Admin

Required hosts: 25

Chosen prefix: **/27**

Usable hosts provided:

**Working:**

```text
n=5: 2^5−2 = allocate 30 IPs, (30 > 25)

Host bits = 5

borrowed bits = 8 − 5 = 3
prefix = 24 + 3 = /27
```

Answer: 30

### 2. Engineering

Required hosts: 60

Chosen prefix: **/26**

Usable hosts provided:

**Working:**

```text
n=6: 2^6−2 = allocate 62 IPs, (62 > 60)

Host bits = 6

borrowed bits = 8 − 6 = 2
prefix = 24 + 2 = /26
```

Answer: 62

### 3. Server

Required hosts: 12

Chosen prefix: **/28**

Usable hosts provided:

**Working:**

```text
n=4: 2^4−2 = allocate 14 IPs, (14 > 12)

Host bits = 4

borrowed bits = 8 − 4 = 4
prefix = 24 + 4 = /28
```

Answer: 14

### 4. Guest Wifi

Required hosts: 20

Chosen prefix: **/27**

Usable hosts provided:

**Working:**

```text
n=5: 2^5−2 = allocate 30 IPs, (30 > 20)

Host bits = 5

borrowed bits = 8 − 5 = 3
prefix = 24 + 3 = /27
```

Answer: 30

---

## Task 2: Build an Addressing Table

| **Subnet Name** | **CIDR** | **Subnet Mask** | **Network Address** | **First Host** | **Last Host** | **Broadcast** | **Gateway** |
| --------------- | -------- | --------------- | ------------------- | -------------- | ------------- | ------------- | ----------- |
| ADMIN           | /27      | 255.255.255.224 | 10.20.30.64         | 10.20.30.65    | 10.20.30.94   | 10.20.30.95   | 10.20.30.65 |
| ENGINEERING     | /26      | 255.255.255.192 | 10.20.30.0          | 10.20.30.1     | 10.20.30.62   | 10.20.30.63   | 10.20.30.1  |
| SERVER          | /28      | 255.255.255.240 | 10.20.30.96         | 10.20.30.97    | 10.20.30.110  | 10.20.30.111  | 10.20.30.97 |

### UPDATED VERSION WITH GUEST WIFI

| **Subnet Name** | **CIDR** | **Subnet Mask** | **Network Address** | **First Host** | **Last Host** | **Broadcast** | **Gateway**  |
| --------------- | -------- | --------------- | ------------------- | -------------- | ------------- | ------------- | ------------ |
| Engineering     | /26      | 255.255.255.192 | 10.20.30.0          | 10.20.30.1     | 10.20.30.62   | 10.20.30.63   | 10.20.30.1   |
| Admin           | /27      | 255.255.255.224 | 10.20.30.64         | 10.20.30.65    | 10.20.30.94   | 10.20.30.95   | 10.20.30.65  |
| Server          | /28      | 255.255.255.240 | 10.20.30.96         | 10.20.30.97    | 10.20.30.110  | 10.20.30.111  | 10.20.30.97  |
| Guest Wi-Fi     | /27      | 255.255.255.224 | 10.20.30.128        | 10.20.30.129   | 10.20.30.158  | 10.20.30.159  | 10.20.30.129 |

### WORKING

### 1. ADMIN:

**SUBNET MASK:**

```text
11111111.11111111.11111111.11100000
255.255.255.192 (2^7 + 2^6 =192)
```

CIDR: **/27**

Subnet Mask: **255.255.255.224**

Block Size:

```text
256 - 224 = 32
```

Subnets:

```text
0, 32, 64, 96, 128, 160, 192, 224
```

**Network Address:** 10.20.30.64

**First host:** 10.20.30.65

**Last host:** 10.20.30.94

**Broadcast (last address in a subnet):**

```text
.64 + (32 - 1) = .64 + 31 = 10.20.30.95
```

**Gateway** = .0 + 1 = 10.20.30.1

Last octet: 64

```text
64 ÷ 32 = 2 remainder 0
```

### 2. ENGINEERING:

**SUBNET MASK:**

```text
11111111.11111111.11111111.11000000
255.255.255.224 (2^7 + 2^6 + 2^5=224)
```

CIDR: **/26**

Subnet Mask: **255.255.255.192**

Block Size:

```text
256 - 192 = 64
```

Subnets:

```text
0, 64, 128, 192
```

Starting IP: 10.20.30.0

**Network Address:** 10.20.30.0

**First host:** 10.20.30.1

**Last host:** 10.20.30.62

**Broadcast (last address in a subnet):**

```text
.0 + (64 - 1) = .0 + 63 = 10.20.30.63
```

**Gateway =** .0 + 1 = 10.20.30.1

Last octet: 0

```text
0 ÷ 64 = 0 remainder 0
```

### 3. SERVER:

**SUBNET MASK:**

```text
11111111.11111111.11111111.11110000
255.255.255.240 (2^7 + 2^6 + 2^5 + 2^4 =240)
```

CIDR: **/28**

Subnet Mask: **255.255.255.240**

Block Size:

```text
256 - 240 = 16
```

Subnets:

```text
0, 16, 32, 48, 64, 80, 96, 112, 128, 144, 160, 176, 192, 208, 224, 240
```

Starting after Admin: 10.20.30.96

**Network Addresss:** 10.20.30.96

**First host:** 10.20.30.97

**Last host:** 10.20.30.110

**Broadcast (last address in a subnet):**

```text
96 + (16 - 1) = .96 + 15 = 10.20.30.111
```

**Gateway** = .96 + 1 = 10.20.30.97

Last octet: 96

```text
96 ÷ 16 = 6 remainder 0
```

### 4. GUEST WIFI:

**SUBNET MASK:**

```text
11111111.11111111.11111111.11100000
```

```text
255.255.255.224 (128 + 64 + 32 = 224)
```

CIDR: **/27**

Subnet Mask: **255.255.255.224**

Block Size:

```text
256 - 224 = 32
```

Subnets:

```text
0, 32, 64, 96, 128, 160, 192, 224
```

Starting after Server subnet:

```text
10.20.30.96 - 10.20.30.111
```

Guest Wi-Fi starts at:

```text
10.20.30.128
```

**Network Address:** 10.20.30.128

**First host:** 10.20.30.129

**Last host:** 10.20.30.158

**Broadcast (last address in subnet):**

```text
.128 + (32 - 1) = .128 + 31 = 10.20.30.159
```

**Gateway** = .128 + 1 = 10.20.30.129

Last octet: 128

```text
128 ÷ 32 = 4 remainder 0
```

---

## VISUAL DIAGRAM OF NETWORK

```text
                                  ┌─────────────────┐
                                  │                 │
                                  │    INTERNET     │
                                  │                 │
                                  └────────┬────────┘
                                           │
                                  ┌────────▼────────┐
                                  │                 │
                                  │   ISP Modem     │
                                  │   Public IP     │
                                  │   203.0.113.10  │
                                  └────────┬────────┘
                                           │
                                  ┌────────▼──────────────────┐
                                  │                           │
                                  │   Edge Router/NAT         │
                                  │   Inside: 10.20.30.1      │
                                  │   Outside: 203.0.113.10   │
                                  │   PAT Enabled              │
                                  └────────┬──────────────────┘
                                           │
                         ┌─────────────────┼─────────────────┐
                         │                 │                 │
                  ┌──────▼──────┐   ┌──────▼──────┐   ┌─────▼────────┐
                  │   Layer 3   │   │   Layer 3   │   │   Layer 3    │
                  │   Switch    │   │   Switch    │   │   Switch     │
                  │   (Core)    │   │   (Core)    │   │   (Core)     │
                  └──────┬──────┘   └──────┬──────┘   └─────┬────────┘
                         │                 │                 │
          ┌──────────────┼─────────────────┼─────────────────┼───────────────┐
          │              │                 │                 │               │
   ┌──────▼──────┐ ┌─────▼──────┐ ┌──────▼──────┐ ┌───────▼──────┐ ┌──────▼──────┐
   │ Engineering │ │    Admin   │ │    Guest    │ │    Server    │ │  Reserved   │
   │   Subnet    │ │   Subnet   │ │   Wi-Fi     │ │    Subnet    │ │   Subnets   │
   │    /26      │ │    /27     │ │    /27      │ │     /28      │ │    /28 x3   │
   │  62 hosts   │ │  30 hosts  │ │  30 hosts   │ │   14 hosts   │ │  14 hosts   │
   │ 10.20.30.0  │ │ 10.20.30.64│ │10.20.30.128 │ │ 10.20.30.96  │ │ .144-.191   │
   │ Gateway: .1 │ │ Gateway:.65│ │ Gateway:.129 │ │ Gateway:.97   │ │  (Future)   │
   │ Broadcast:.63│ │Broadcast:.95│ │Bcast:.127    │ │ Bcast:.143    │ │             │
   │             │ │             │ │              │ │              │ │             │
   │ ┌───────────┐│ │ ┌─────────┐ │ │ ┌────────┐  │ │ ┌───────┐   │ │ ┌───────┐   │
   │ │ PC1 .2    ││ │ │ PC1 .66 │ │ │ │ Guest1 │  │ │ │Server1│   │ │ │Future │   │
   │ │ PC2 .3    ││ │ │ PC2 .67 │ │ │ │ Guest2 │  │ │ │Server2│   │ │ │Growth │   │
   │ │ PC3 .4    ││ │ │ PC3 .68 │ │ │ │ Guest3 │  │ │ │Server3│   │ │ │       │   │
   │ │ ...       ││ │ │ ...     │ │ │ │ ...    │  │ │ │ ...   │   │ │ │       │   │
   │ │ PC60 .61  ││ │ │ PC25 .89│ │ │ │Guest20 │  │ │ │Server12│  │ │ │       │   │
   │ └───────────┘│ │ └─────────┘ │ │ └────────┘  │ │ └───────┘   │ │ └───────┘   │
   └──────────────┘ └─────────────┘ └──────────────┘ └──────────────┘ └─────────────┘
```

---

## Task 3: NAT and Internet Boundary

Write a short explanation (5 to 10 lines) covering:

* which addresses are private
* where NAT is applied
* what source IP transformation looks like
* why NAT is needed in this design

### ANSWER:

All addresses in the 10.20.30.0/24 block (Admin, Engineering, and Server subnets) are private RFC 1918 addresses and are not routable on the public internet.

Host 10.20.30.20 (Engineering) **sends traffic to a public website.**

Traffic reaches the default gateway, then the firewall performs NAT (PAT - Port Address Translation).

Source changes from **10.20.30.20:54211** to **203.0.113.10:61022.**

A NAT table entry is created mapping the private and public IP:port pairs.

Return traffic arrives addressed to 203.0.113.10:61022. NAT table lookup maps it back to 10.20.30.20:54211, and it is forwarded internally.

NAT is needed because private addresses cannot be routed on the internet, and it lets all internal hosts share one public IP while hiding internal addressing.

---

## Task 4: Traffic Flow Explanation

When an Engineering user (10.20.30.20) visits a public website, the host first checks whether the destination is on its local subnet, if not present the packet is sent to its default gateway, 10.20.30.1. The gateway router checks its routing table, finds no local match, and forwards the packet toward the WAN interface.

At this point the edge router **applies NAT**, **translating the source** from 10.20.30.20 to the **router's public IP** with a unique port number, and recording this mapping in its NAT table. The packet then leaves the network and travels across the ISP's toward the internet backbone, hopping through multiple routers until it reaches the destination web server.

The server responds to the public IP it received the request from. That response arrives at the edge router, which checks the NAT table, translates the destination back to the internal host's private IP, and forwards it onto the Engineering subnet via the local switch. The host receives the page, and once the session ends, then the NAT table entry eventually expires.

---

# VLSM Design for BlueOrbit Labs

**Designed by:** Eva Muthoni  
**Role:** Network Engineer, BlueOrbit Labs  
**Date:** 2026-08-26

---

## What is VLSM?

VLSM (Variable Length Subnet Masking) allows each subnet to have a different subnet mask, meaning each subnet is sized **exactly** to its host requirements. This minimizes wasted IP addresses and maximizes efficiency.

---

## Why VLSM is Better

| Approach | Subnets | Addresses Used | Wasted | Efficiency |
|----------|---------|----------------|--------|------------|
| Fixed-Length (/26) | 4 subnets × 62 = 248 | 97 | 151 | 39% |
| **VLSM** | Custom sizes | 136 | 39 | **71%** |

**VLSM saves 112 IP addresses** for future growth!

---

## VLSM Subnet Planning

### Parent Network: 10.20.30.0/24
- **Total Addresses:** 256
- **Total Usable:** 254

### Allocation Order (Largest to Smallest)

| Order | Subnet | Required | Host Bits | Borrowed | CIDR | Usable | Block Size | Start Address |
|-------|--------|----------|-----------|----------|------|--------|------------|---------------|
| 1 | Engineering | 60 | 6 | 2 | /26 | 62 | 64 | 10.20.30.0 |
| 2 | Admin | 25 | 5 | 3 | /27 | 30 | 32 | 10.20.30.64 |
| 3 | Guest Wi-Fi | 20 | 5 | 3 | /27 | 30 | 32 | 10.20.30.96 |
| 4 | Server | 12 | 4 | 4 | /28 | 14 | 16 | 10.20.30.128 |
| 5 | Reserved 1 | - | 4 | 4 | /28 | 14 | 16 | 10.20.30.144 |
| 6 | Reserved 2 | - | 4 | 4 | /28 | 14 | 16 | 10.20.30.160 |
| 7 | Reserved 3 | - | 4 | 4 | /28 | 14 | 16 | 10.20.30.176 |
| 8 | Reserved 4 | - | 4 | 4 | /28 | 14 | 16 | 10.20.30.192 |
| 9 | Reserved 5 | - | 4 | 4 | /28 | 14 | 16 | 10.20.30.208 |
| 10 | Remaining | - | 6 | 2 | /26 | 62 | 64 | 10.20.30.224 |

---

## Detailed VLSM Calculations

### 1. Admin Subnet (/27) - 30 Hosts
```
Required: 25 hosts
n = 5: 2^5 - 2 = 32 - 2 = 30 usable ✓
Host bits = 5 | Borrowed bits = 3 | Prefix = /27
Subnet Mask: 255.255.255.224 | Block Size: 32

Network: 10.20.30.64
Gateway: 10.20.30.65
Broadcast: 10.20.30.95
Host Range: 10.20.30.65 - 10.20.30.94

Verification: 64 ÷ 32 = 2 remainder 0 ✓
```
### 2. Engineering Subnet (/26) - 62 Hosts
```
Required: 60 hosts
n = 6: 2^6 - 2 = 64 - 2 = 62 usable ✓
Host bits = 6 | Borrowed bits = 2 | Prefix = /26
Subnet Mask: 255.255.255.192 | Block Size: 64

Network: 10.20.30.0
Gateway: 10.20.30.1
Broadcast: 10.20.30.63
Host Range: 10.20.30.1 - 10.20.30.62

Verification: 0 ÷ 64 = 0 remainder 0 ✓
```

### 3. Guest Wi-Fi Subnet (/27) - 30 Hosts
```
Required: 20 hosts
n = 5: 2^5 - 2 = 32 - 2 = 30 usable ✓
Host bits = 5 | Borrowed bits = 3 | Prefix = /27
Subnet Mask: 255.255.255.224 | Block Size: 32

Network: 10.20.30.96
Gateway: 10.20.30.97
Broadcast: 10.20.30.127
Host Range: 10.20.30.97 - 10.20.30.126

Verification: 96 ÷ 32 = 3 remainder 0 ✓
```

### 4. Server Subnet (/28) - 14 Hosts
```
Required: 12 hosts
n = 4: 2^4 - 2 = 16 - 2 = 14 usable ✓
Host bits = 4 | Borrowed bits = 4 | Prefix = /28
Subnet Mask: 255.255.255.240 | Block Size: 16

Network: 10.20.30.128
Gateway: 10.20.30.129
Broadcast: 10.20.30.143
Host Range: 10.20.30.129 - 10.20.30.142

Verification: 128 ÷ 16 = 8 remainder 0 ✓
```

### 5. Reserved Subnet 1 (/28) - 14 Hosts
```
Network: 10.20.30.144
Broadcast: 10.20.30.159
Host Range: 10.20.30.145 - 10.20.30.158
Verification: 144 ÷ 16 = 9 remainder 0 ✓
```

### 6. Reserved Subnet 2 (/28) - 14 Hosts
```
Network: 10.20.30.160
Broadcast: 10.20.30.175
Host Range: 10.20.30.161 - 10.20.30.174
Verification: 160 ÷ 16 = 10 remainder 0 ✓
```

### 7. Reserved Subnet 3 (/28) - 14 Hosts
```
Network: 10.20.30.176
Broadcast: 10.20.30.191
Host Range: 10.20.30.177 - 10.20.30.190
Verification: 176 ÷ 16 = 11 remainder 0 
```

### 8. Reserved Subnet 4 (/28) - 14 Hosts
```
Network: 10.20.30.192
Broadcast: 10.20.30.207
Host Range: 10.20.30.193 - 10.20.30.206
Verification: 192 ÷ 16 = 12 remainder 0 
```

### 9. Reserved Subnet 5 (/28) - 14 Hosts
```
Network: 10.20.30.208
Broadcast: 10.20.30.223
Host Range: 10.20.30.209 - 10.20.30.222
Verification: 208 ÷ 16 = 13 remainder 0 
```

### 10. Remaining Space (/26) - 62 Hosts
```
Network: 10.20.30.224
Broadcast: 10.20.30.255
Host Range: 10.20.30.225 - 10.20.30.254
Verification: 224 ÷ 64 = 3 remainder 32 
```

---

## Complete VLSM Addressing Table

| Subnet Name | CIDR | Subnet Mask | Network Address | Gateway | Broadcast | Usable Hosts |
|-------------|------|-------------|-----------------|---------|-----------|--------------|
| Engineering | /26 | 255.255.255.192 | 10.20.30.0 | 10.20.30.1 | 10.20.30.63 | 62 |
| Admin | /27 | 255.255.255.224 | 10.20.30.64 | 10.20.30.65 | 10.20.30.95 | 30 |
| Guest Wi-Fi | /27 | 255.255.255.224 | 10.20.30.96 | 10.20.30.97 | 10.20.30.127 | 30 |
| Server | /28 | 255.255.255.240 | 10.20.30.128 | 10.20.30.129 | 10.20.30.143 | 14 |
| Reserved 1 | /28 | 255.255.255.240 | 10.20.30.144 | - | 10.20.30.159 | 14 |
| Reserved 2 | /28 | 255.255.255.240 | 10.20.30.160 | - | 10.20.30.175 | 14 |
| Reserved 3 | /28 | 255.255.255.240 | 10.20.30.176 | - | 10.20.30.191 | 14 |
| Reserved 4 | /28 | 255.255.255.240 | 10.20.30.192 | - | 10.20.30.207 | 14 |
| Reserved 5 | /28 | 255.255.255.240 | 10.20.30.208 | - | 10.20.30.223 | 14 |
| Remaining | /26 | 255.255.255.192 | 10.20.30.224 | - | 10.20.30.255 | 62 |

---

## VLSM Visual Layout

```
10.20.30.0/24 (Parent Block)
┌────────────────────────────────────────────────────────────────────────────┐
│ 10.20.30.0                                                    10.20.30.255 │
└────────────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────┬──────────────┬──────────────┬───────────────┐
│     ENGINEERING (/26)       │  ADMIN (/27) │ GUEST (/27)  │ SERVER (/28)  │
│     62 usable hosts         │  30 hosts    │  30 hosts    │  14 hosts     │
│     10.20.30.0 - .63        │  .64 - .95   │ .96 - .127   │ .128 - .143   │
├─────────────────────────────┼──────────────┼──────────────┼───────────────┤
│ Network: 10.20.30.0         │ Network:.64  │ Network:.96  │ Network:.128  │
│ Gateway: 10.20.30.1         │ Gateway:.65  │ Gateway:.97  │ Gateway:.129  │
│ Broadcast: .63              │ Broadcast:.95│ Broadcast:.127│ Broadcast:.143│
└─────────────────────────────┴──────────────┴──────────────┴───────────────┘

┌──────────────┬──────────────┬──────────────┬──────────────┬──────────────┐
│  RESERVED 1  │  RESERVED 2  │  RESERVED 3  │  RESERVED 4  │  RESERVED 5  │
│  /28         │  /28         │  /28         │  /28         │  /28         │
│  14 hosts    │  14 hosts    │  14 hosts    │  14 hosts    │  14 hosts    │
│ .144 - .159  │ .160 - .175  │ .176 - .191  │ .192 - .207  │ .208 - .223  │
└──────────────┴──────────────┴──────────────┴──────────────┴──────────────┘

┌──────────────────────────────────────────────────────────────────────────────┐
│   REMAINING SPACE                                                            │
│   /26 (62 hosts)                                                             │
│   10.20.30.224 - .255                                                        │
└──────────────────────────────────────────────────────────────────────────────┘
```
---

## Reflection

### 1. What part of subnetting was hardest?

The most challenging aspect was visualizing the subnet boundaries, keeping track of where each subnet ends and the next begins, especially when subnets have different sizes.

### 2. How did you verify your calculations?

I used three verification methods:

i) block size method (256 - mask value) to determine subnet ranges

ii) division verification to confirm network addresses were at valid boundaries:

```text
128 ÷ 32 = 4 remainder 0
```

iii) cross-reference checking to ensure each subnet's broadcast address was exactly one less than the next subnet's network address, confirming no overlaps or gaps existed.

### 3. Why is NAT useful in private enterprise networks?

NAT is essential because it enables **IPv4 conservation by allowing hundreds of devices to share one public IP address**, provides security by hiding internal IP structures from external threats, and offers cost savings since organizations don't need to purchase expensive public IPs for every device.

NAT also provides flexibility to reorganize internal addressing schemes without affecting internet connectivity, making it a standard practice in modern enterprise networks.

---

## AUTHOR

*Eva Muthoni*- A Network Engineer for BlueOrbit Labs.


---


