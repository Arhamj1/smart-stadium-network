# Smart Stadium Network

Design and simulation of a segmented network for a smart football stadium.
BCSE308P Computer Networks, Fall Semester 2026–2027.
Pavit Singh (24BDS0246) and Arham Jain (24BDS0366).

## The problem

A stadium runs ticketing, card payments, live TV, security cameras and public fan WiFi on one physical network. On a flat network a fan's phone could reach the payment system or the cameras, a crowd surge could break the live TV feed, and anyone could plug a laptop into a camera's socket.

## The solution

| Problem | Fix |
|---|---|
| Everyone can reach everyone | Five VLANs, one per group, with an ACL rule list at each zone's gateway |
| Fan traffic slows the TV feed | QoS: TV traffic goes first, payments get reserved bandwidth |
| Rogue devices plugged in | Port security: each socket only accepts the device it first learned |

DHCP gives each zone's devices addresses automatically. A Layer 3 main switch routes between zones, and an Internet router connects the stadium to the outside world.

## How to run it

Open it online at https://arhamj1.github.io/smart-stadium-network/ or download `index.html` and open it in any browser. It needs no install, no server and no internet connection (fonts fall back to system fonts when offline).

## What's in the simulator

| Tab | What it does |
|---|---|
| Map (always visible) | All zones, switches and devices. Click a device to see its address, MAC and socket. |
| Test | Send a ping, HTTPS, HTTP or DNS message between any two devices and watch it travel. 15 planned tests compare what the design promises with what happens. |
| Addresses | Zone address ranges, every device's address, and the DHCP conversation step by step. |
| Security | Who-can-reach-whom grid, every ACL rule in plain English with match counters, port-security table and three attack scenarios. |
| Fast lane | Crowd-surge slider, QoS on/off switch, live delay and throughput graphs. |
| Device setup | Cisco IOS commands for every device, generated from the same data the simulator runs. |
| Design | Network layers, zone requirements and changes since Review 1. |
| Guide | Glossary of every term and a 5-minute demo script. |

## Addressing plan

| VLAN | Zone | Range | Gateway |
|---|---|---|---|
| 10 | Ticketing | 192.168.10.0/24 | 192.168.10.1 |
| 20 | Payments (POS) | 192.168.20.0/24 | 192.168.20.1 |
| 30 | Press & TV | 192.168.30.0/24 | 192.168.30.1 |
| 40 | Security cameras (CCTV) | 192.168.40.0/24 | 192.168.40.1 |
| 50 | Fan WiFi | 192.168.50.0/24 | 192.168.50.1 |
| — | Main Switch ↔ Internet Router | 192.168.100.0/30 | — |

Addresses .1 to .20 in each zone are kept for fixed devices. DHCP hands out .21 upward.

## Changes since Review 1

| Part | Review 1 plan | Now |
|---|---|---|
| Routing | Router-on-a-stick (one router, five sub-interfaces) | Layer 3 main switch with one gateway per zone; the router only handles the Internet |
| Access switches | One or more | One per zone |
| DHCP | On the router | On the main switch |
| ACLs | Fan WiFi blocked from Ticketing, POS and CCTV | A full rule list for every zone |
| Port security | POS and CCTV ports | Every port, strict on POS, CCTV and WiFi |
| QoS | Not planned | Priority for the TV feed, reserved bandwidth for payments |
| Tool | Cisco Packet Tracer | Browser simulator, plus Cisco commands ready for Packet Tracer |

## Limits

This is a simulation written for the project, not real Cisco software. It models VLANs, inter-VLAN routing, DHCP, ACL matching (including return traffic), port security and a queue model for QoS. It does not run spanning tree or routing protocols, and the delay and loss figures come from a simple model. The Cisco commands have not been tested on real hardware.
