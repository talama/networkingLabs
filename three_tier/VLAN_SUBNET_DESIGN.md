# VLAN Design and IP Addressing Scheme

This section details the network's logical segmentation using VLANs and the corresponding IP address allocation.

- **Primary IP Range:** `10.0.0.0/8`
- **HQ Allocation:** `10.10.0.0/16`
- **Data Center Allocation:** `10.20.0.0/16`

## IP Plan for Core-to-Core links:

- Link 1 (eth0/0): `10.255.1.0/30`
- Link 2 (eth0/1): `10.255.2.0/30`

## IP Plan for Distribution-to-Core links:

- `DIST-HQ-SW1` <--> `CORE-HQ-SW1`: `10.255.10.0/30`
- `DIST-HQ-SW1` <--> `CORE-HQ-SW2`: `10.255.11.0/30`
- `DIST-HQ-SW2` <--> `CORE-HQ-SW1`: `10.255.12.0/30`
- `DIST-HQ-SW2` <--> `CORE-HQ-SW2`: `10.255.13.0/30`

## IP and VLAN Plan for HQ Access Layer

The default gateway for each VLAN will be the first usable IP address (`.1`).

| VLAN ID | Name         | Department/Purpose    | Subnet            | Gateway              | Notes                                    |
| :------ | :----------- | :-------------------- | :---------------- | :------------------- | :--------------------------------------- |
| 10      | `MGMT`       | Network Device Mgmt   | `10.10.10.0/24`   | `10.10.10.1`         | For SSH/SNMP access to switches/routers. |
| 100     | `EXEC`       | Executive Floor       | `10.10.100.0/27`  | `10.10.100.1`        | 30 IPs for 15 users.                     |
| 110     | `DEV`        | Development Teams     | `10.10.110.0/25`  | `10.10.110.1`        | 126 IPs for 60 users.                    |
| 120     | `IT-OPS`     | IT Operations         | `10.10.120.0/26`  | `10.10.120.1`        | 62 IPs for 20 users.                     |
| 130     | `SALES-MKTG` | Sales & Marketing     | `10.10.130.0/26`  | `10.10.130.1`        | 62 IPs for 25 users.                     |
| 140     | `HR-FIN`     | HR & Finance          | `10.10.140.0/27`  | `10.10.140.1`        | 30 IPs for 15 users.                     |
| 200     | `DC-PROD`    | DC Production Servers | `10.20.200.0/24`  | `10.20.200.1`        | Scalable subnet for production apps.     |
| 210     | `DC-INFRA`   | DC Infra. Services    | `10.20.210.0/24`  | `10.20.210.1`        | For DNS, DHCP, NTP, etc.                 |
| 999     | `DMZ`        | Public-Facing Servers | `192.168.99.0/24` | (Firewall Interface) | Isolated zone for external services.     |
