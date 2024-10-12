# Intra Domain Routing OSPF Single Area and Multi Area

This repository contains a Python script (`ospf-lab.py`) that simulates an OSPF (Open Shortest Path First) network using Mininet. The script creates a topology with multiple routers and hosts, demonstrating both single-area and multi-area OSPF configurations.

## Requirements

- Python 3
- Mininet
- FRRouting (FRR)

## Usage

To run the OSPF lab simulation:

```bash
sudo python3 ospf-lab.py
```

## Topology

The script creates a network topology with three subnets, each containing a main router, a switch between the main router and two sub-routers, and two client hosts. The main routers are interconnected to form the backbone of the network.

### Multi-Area OSPF

In multi-area OSPF, the network is divided into multiple areas, with Area 0 serving as the backbone. This configuration improves scalability and reduces the processing load on routers.

```mermaid
graph TB
    subgraph "Area 0 (Backbone)"
        R1((R1)) --- R2((R2))
        R2 --- R3((R3))
        R3 --- R1
    end
    
    subgraph "Area 1"
        R1 ---|eth0| S1{S1}
        S1 ---|eth0| R1_1((R1_1))
        S1 ---|eth0| R1_2((R1_2))
        R1_1 ---|eth1| C1_1[C1_1]
        R1_2 ---|eth1| C1_2[C1_2]
    end
    
    subgraph "Area 2"
        R2 ---|eth0| S2{S2}
        S2 ---|eth0| R2_1((R2_1))
        S2 ---|eth0| R2_2((R2_2))
        R2_1 ---|eth1| C2_1[C2_1]
        R2_2 ---|eth1| C2_2[C2_2]
    end
    
    subgraph "Area 3"
        R3 ---|eth0| S3{S3}
        S3 ---|eth0| R3_1((R3_1))
        S3 ---|eth0| R3_2((R3_2))
        R3_1 ---|eth1| C3_1[C3_1]
        R3_2 ---|eth1| C3_2[C3_2]
    end

    classDef switch fill:#f9f,stroke:#333,stroke-width:4px;
    class S1,S2,S3 switch;
```

## Network Components

- R1, R2, R3: Main routers (Area Border Routers in multi-area configuration)
- R1_1, R1_2, R2_1, R2_2, R3_1, R3_2: Sub-routers
- C1_1, C1_2, C2_1, C2_2, C3_1, C3_2: Client hosts
- S1, S2, S3: Switches connecting routers within each subnet

## Configuration

The script generates configuration files for each router using templates. These configurations are stored in the specified directory and are used to set up OSPF routing on the routers.

## Customization

You can modify the `OSPFLab` class in the script to adjust the network topology, IP addressing, or OSPF areas as needed for your specific requirements.

## Note

This lab is designed for educational purposes and may need additional configuration for production use. Ensure you have the necessary permissions and understanding of OSPF before deploying in a live environment.

## ADDITIONAL CONFIGURATION !
```bash
sudo nano /etc/sysctl.conf
```
Uncomment this section :
```bash
net.ipv4.ip_forward=1
net.ipv6.conf.all.forwarding=1
```

And, config this too :
```bash
sudo modprobe bridge
sudo modprobe br_netfilter
```

The command :
```bash
lsmod | grep bridge
```
```bash
bridge                335872  1 br_netfilter
stp                    12288  1 bridge
llc                    16384  2 bridge,stp
```

example results when running the ospf-lab.py :
```bash
========================================
Warning: Linux bridge may not work with net.bridge.bridge-nf-call-arptables = 1
Warning: Linux bridge may not work with net.bridge.bridge-nf-call-iptables = 1
Warning: Linux bridge may not work with net.bridge.bridge-nf-call-ip6tables = 1
Finished initializing network in: 1.1319239139556885 seconds
```
