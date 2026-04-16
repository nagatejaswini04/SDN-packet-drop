# SDN Mininet Simulation — Packet Drop using POX

> **Course:** Computer Networks  
> **Student Name:** NAGA TEJASWINI  
> **SRN:** PES2UG24AM095  
> **Controller:** POX (OpenFlow v1.0)  
> **Topology:** Single Switch, 2 Hosts  

---

## Problem Statement

This project implements an **SDN-based network simulation** using **Mininet** and the **POX OpenFlow controller**. It demonstrates:

* Controller–switch interaction using the OpenFlow protocol  
* Explicit flow rule design using **match–action logic**  
* Traffic filtering using a **packet drop rule**  
* Network behavior observation using ping  

---

## SDN Behaviors Demonstrated

| Behavior        | Description                                      |
|----------------|--------------------------------------------------|
| Packet Drop    | Traffic from h1 → h2 is blocked                  |
| Flow Matching  | Filtering based on source and destination IP     |
| Packet Loss    | 100% packet loss observed for blocked flow       |
| Controller Logic | POX handles PacketIn events and applies rules |

---

## Network Topology
h1 (10.0.0.1)
     |
h2 (10.0.0.2) --- [s1 Switch] --- [POX Controller]

### 🔹 Topology Explanation

* The network consists of **2 hosts (h1, h2)** and **1 switch (s1)**  
* Both hosts are connected to a **single OpenFlow switch**  
* The switch is controlled by the **POX SDN controller**  
* Traffic from **h1 to h2 is blocked using a drop rule**  

---

## Controller Logic

* The controller listens for **packet_in events** from the switch  
* When a packet arrives:

  1. The controller checks the **source and destination IP**
  2. It decides whether to **allow or block the traffic**

### Logic implemented:

* If packet is from **h1 → h2**
  → Apply **DROP rule**

* Otherwise
  → Forward packet using **flooding**

This enables **centralized control of network behavior**

---

## Match–Action Mechanism

In SDN, flow rules are based on the **match–action principle**:

* **Match:** Identify packets based on:
  * Source IP address
  * Destination IP address

* **Action:** Define what to do:
  * **Drop** → discard packet
  * **Forward** → send packet to switch ports

### In this project:

* Match = packet from **h1 → h2**  
  Action = **DROP**

* Other packets  
  Action = **FORWARD**

---

## Flow Rules

Flow rules are applied dynamically by the controller.

### Implemented Flow Rules:

1. **Drop Rule**
   * Match: h1 → h2 traffic
   * Action: DROP

2. **Default Rule**
   * Action: Send packets to controller

---

## Setup & Installation

### Step 1 — Install Mininet

```bash
sudo apt update
sudo apt install mininet -y
```
### Step 2 -Clone pox
```bash
git clone https://github.com/noxrepo/pox.git
cd pox
```
## How to Run

### Terminal 1 — Start Controller

```bash
cd ~/pox
./pox.py misc.controller
```
### terminal 2-Start mininet
```bash
sudo mn --topo single,2 --controller=remote,ip=127.0.0.1,port=6633
```
## Test Scenarios

### Scenario — Blocked Traffic
```bash
h1 ping -c 5 h2
```

### Expected:
```bash

100% packet loss
```
## Connectivity Test
```bash
pingall
```

### Expected:

Packet loss observed due to drop rule
##Behavior Evaluation
```
Traffic from h1 to h2 is successfully blocked.
Packet loss confirms drop rule.

Reverse traffic may also be affected due to simple forwarding logic.
```

## Regression Test
```
After restarting Mininet and controller, drop rule still works.
```

## Performance Analysis
```
Packet Loss (h1 → h2): 100%
Latency: ~0.1 ms
```

## Proof of Execution
```
Screenshots folder contains:
- Controller output
- Ping result
- Mininet topology
```
- 
## Repository Structure
```
project/
├── controller.py
├── README.md
└── screenshots/
```
### Validation
```
- Drop rule works correctly
- Packet loss confirms enforcement
- Controller interacts with switch
```
## Conclusion
```
This project demonstrates SDN using POX.
Traffic is controlled centrally using flow rules.
```

