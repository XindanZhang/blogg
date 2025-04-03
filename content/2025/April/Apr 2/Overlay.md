---
title: "Overlay Networks"
date: 2025-04-02
tags:
  - networking
  - virtualization
  - SDN
---

# Overlay Networks

An overlay network is a virtual network that is built on top of an underlying network infrastructure (often called the underlay network). Overlay networks create an abstraction that allows network administrators to deploy virtual networks on top of traditional physical networks without requiring changes to the physical infrastructure.

## Key Characteristics

- **Virtual Network Layer**: Creates a logical topology that may differ completely from the physical network
- **Encapsulation**: Uses protocols to encapsulate original data packets within other packets
- **Decoupling**: Separates the logical network services from physical network constraints
- **Scalability**: Can scale beyond the limitations of physical networks
- **Management**: Typically managed through software controllers rather than physical device configuration

## Common Overlay Technologies

- **VXLAN (Virtual Extensible LAN)**: Extends Layer 2 segments over Layer 3 networks
- **NVGRE (Network Virtualization using Generic Routing Encapsulation)**
- **STT (Stateless Transport Tunneling)**
- **Geneve (Generic Network Virtualization Encapsulation)**

## Use Cases

1. **Data Center Networking**: Allows for multi-tenant environments where each tenant has isolated network resources
2. **Cloud Computing**: Enables flexible deployment of virtual machines across physical locations
3. **Software-Defined Networking (SDN)**: Forms the basis for many SDN implementations
4. **Network Function Virtualization (NFV)**: Supports deployment of virtualized network functions

## Benefits

- **Flexibility**: Easily create and modify virtual networks without physical changes
- **Isolation**: Provides strong separation between different tenants or applications
- **Mobility**: Supports workload mobility across physical boundaries
- **Programmability**: Enables network automation and orchestration

## Challenges

- **Performance Overhead**: Encapsulation adds processing overhead
- **Troubleshooting Complexity**: Diagnosing issues can be more difficult across virtual/physical boundaries
- **Visibility**: Monitoring traffic can be challenging due to encapsulation

Overlay networks have become fundamental components in modern cloud data centers and are key enablers for network virtualization technologies.

## References

- [TechTarget: Overlay Network Definition](https://www.techtarget.com/searchnetworking/definition/overlay-network)




## Overlay networks and SDN

SDN is a quickly growing network strategy where the network operating system separates the data plane (packet handling) from the control plane (the network topology and routing rules).
