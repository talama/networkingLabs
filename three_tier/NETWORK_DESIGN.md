# Physical and Logical Design

## Physical Topology and Device Specifications

This section describes the network's physical layout and the GNS3 devices that will be used to model it. The design follows a 3-layer hierarchical model (Core, Distribution, Access).

### General implementation notes

#### Naming Convention

- **Function-Location-Number:** (e.g., `CORE-HQ-SW1`, `ACCESS-DEV-SW1`)

#### Routing

For the point-to-point links in our network backbone (core-to-core and core-to-distribution), using routed ports is the modern best practice. It's simpler, more efficient, and avoids any potential Layer 2 issues like Spanning Tree Protocol loops on these critical backbone links.

We are going to use OSPF as routing protocol for the CORE and DISTRIBUTION layer. It's an open standard supported by all major vendors. OSPF's concep of 'Areas' allows the network to be segmented into a hierarchical routing topology (i.e Area 0 for the backbone, separate areas for HQ and Data Center).

---

### A. HQ - Core Layer

The core is responsible for high-speed packet switching between distribution modules.

- **Devices:**

  - `CORE-HQ-SW1` (L3 Switch)
  - `CORE-HQ-SW2` (L3 Switch)

- **Connectivity:**

  - Redundant links between `CORE-HQ-SW1` and `CORE-HQ-SW2`.
  - Each Core switch connects to both Distribution switches.

### B. HQ - Distribution Layer

This layer aggregates all access layer blocks and the data center. It handles inter-VLAN routing, ACLs, and QoS.

- **Devices:**

  - `DIST-HQ-SW1` (L3 Switch)
  - `DIST-HQ-SW2` (L3 Switch)

- **Connectivity:**

  - Each Distribution switch connects to both Core switches.
  - Each Distribution switch connects to all Access layer switches in a redundant fashion.
  - Each Distribution switch connects to the Data Center distribution switches.

### C. HQ - Access Layer

This layer provides connectivity to end-user devices. Each department is on a separate switch for organizational clarity and physical segmentation.

- **Executive Floor (15 Users):**
  - `ACCESS-EXEC-SW1` (L2 Switch)
  - **Uplinks:** Connects to `DIST-HQ-SW1` and `DIST-HQ-SW2`.
- **Development Teams (60 Users):**
  - `ACCESS-DEV-SW1` (L2 Switch)
  - `ACCESS-DEV-SW2` (L2 Switch)
  - **Uplinks:** Each switch connects to `DIST-HQ-SW1` and `DIST-HQ-SW2`.
- **IT Operations (20 Users):**
  - `ACCESS-IT-SW1` (L2 Switch)
  - **Uplinks:** Connects to `DIST-HQ-SW1` and `DIST-HQ-SW2`.
- **Sales & Marketing (25 Users):**
  - `ACCESS-SALES-SW1` (L2 Switch)
  - **Uplinks:** Connects to `DIST-HQ-SW1` and `DIST-HQ-SW2`.
- **HR & Finance (15 Users):**
  - `ACCESS-HRFIN-SW1` (L2 Switch)
  - **Uplinks:** Connects to `DIST-HQ-SW1` and `DIST-HQ-SW2`.

### D. Data Center (DC) Block

A dedicated block for servers and infrastructure services, designed for high availability and performance.

- **Distribution Layer (DC):**
  - `DIST-DC-SW1` (L3 Switch)
  - `DIST-DC-SW2` (L3 Switch)
  - **Connectivity:** Connects to `DIST-HQ-SW1` and `DIST-HQ-SW2`.
- **Access Layer (DC):**
  - `ACCESS-DC-PROD-SW1` (L2 Switch for Production Servers)
  - `ACCESS-DC-INFRA-SW1` (L2 Switch for Infrastructure Services)
  - **Uplinks:** Each switch connects to `DIST-DC-SW1` and `DIST-DC-SW2`.

### E. Edge & Perimeter Block

This block secures the network perimeter and connects to external networks.

- **Devices:**
  - `EDGE-FW-1` (Firewall Appliance)
  - `EDGE-RTR-1` (Router for ISP/VPN)
- **Connectivity:**
  - The `EDGE-FW-1` connects to the Core layer (or a dedicated distribution block, for simplicity we'll connect to Core).
  - `EDGE-RTR-1` connects to the "outside" of the firewall, representing the ISP connection and the Site-to-Site VPN for the branch office.
  - A DMZ network will be created on a dedicated firewall interface.
