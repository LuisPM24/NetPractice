*This project has been created as part of the 42 curriculum by lupalomi*

# Description
This project consist on series of TCP/IP addressing problems that need solution. Our objective is change the configurations when possible to allow communication between two points.

# Instructions
To run the training interface, you must run the **run.sh** file located inside the compressed directory. Once you do this, a web application will open. Here, you’ll need to enter your login credentials. Next, you can choose between “Training” or “Evaluation” modes.

Once you’re in a mode, you’ll need to modify the provided network configuration to match the given objective. Use the **Check Again** button to verify whether you’ve achieved the objective, **Get My Configuration** to download a JSON file containing your configuration for this exercise, and **Continue** to move on to the next exercise.

To submit this exercise, you must provide a JSON configuration file for each training level along with the README file.

# Resources

## TCP/IP Addressing
TCP/IP (Transmission Control Protocol / Internet Protocol) is a set of communication protocols that allows devices to exchange data across networks, including the Internet. An IP address is used to identify a network interface and allows devices to send and receive data.

### IPv4 Addresses
¡
An IPv4 address consists of **32 bits divided into four groups of 8 bits**, called *octets*, separated by dots. Each octet can have a decimal value between 0 and 255. For example:

|IPv4 Adress|Binary|
|:---------:|:----:|
|`192.168.1.25`|`11000000.10101000.00000001.00011001`|

An IPv4 address contains two logical portions:

- **Network portion:** identifies the network to which the device belongs.
- **Host portion:** identifies a specific interface within that network.

The separation between these two portions is determined by the subnet mask.

### Types of IPv4 Addresses

IPv4 addresses can be classified into different categories:

- **Public IP:** an address that can be routed over the public Internet.
- **Private IP:** an address reserved for use within private networks.
- **Loopback IP:** an address used by a device to communicate with itself, usually `127.0.0.1`.

Private IPv4 addresses belong to the following ranges:

| Range | CIDR |
|:------|:----:|
| 10.0.0.0 – 10.255.255.255 | /8 |
| 172.16.0.0 – 172.31.255.255 | /12 |
| 192.168.0.0 – 192.168.255.255 | /16 |

Private IP addresses cannot be routed directly over the public Internet. Devices using private addresses commonly access the Internet through Network Address Translation (NAT).

## Subnet Masks
The main function of a subnet mask is to determine which part of an IP address identifies the network and which part identifies the host.

Devices belonging to the same subnet can communicate directly without requiring a router.

For example:

| Mask (Decimal) | Mask (Binary) | CIDR |
|:--------------:|:-------------:|:----:|
| 255.255.255.0 | 11111111 11111111 11111111 00000000 | /24 |
| 255.255.0.0 | 11111111 11111111 00000000 00000000 | /16 |
| 255.0.0.0 | 11111111 00000000 00000000 00000000 | /8 |

**Bits with a value of 1 identify the network portion, while bits with a value of 0 identify the host portion.**

### Finding the Network Address

To determine which subnet a device belongs to, we perform a bitwise AND operation between its IP address and subnet mask.

For example:

- IP address: `192.168.1.25`
- Subnet mask: `255.255.255.0`
- Network address: `192.168.1.0/24`

### Calculating Available IP Addresses

First, calculate the number of host bits:

```math
N_{\text{host}} = 32 - N_{\text{mask}}
```

Then, calculate the number of usable IP addresses:
```math
N_{\text{IP}} = 2^{N_{\text{host}}} - 2
```

Where:
- `N_host`: Number of host bits.
- `N_mask`: Number of network bits (CIDR prefix).
- `N_IP`: Number of usable IP addresses.

## Default Gateways
## Default Gateways

A **default gateway** is a router's IP address that a device uses to communicate with destinations outside its local network.

When a device wants to send a packet, it checks whether the destination IP belongs to its own subnet:

- **Same subnet:** the packet is sent directly to the destination without using a router.
- **Different subnet:** the packet is sent to a router according to the device's routing table.

For example:

| Device | IP Address | Subnet Mask |
|:-------|:-----------|:------------|
| Computer A | 192.168.1.10 | /24 |
| Router R1 | 192.168.1.1 | /24 |
| Computer B | 192.168.2.20 | /24 |
| Router R2 | 192.168.2.1 | /24 |

If Computer A wants to communicate with Computer B, it cannot send the packet directly because they belong to different subnets.

Instead, Computer A sends the packet to its default gateway, `192.168.1.1`, which belongs to the router connected to its network.

The router then forwards the packet toward the destination network.

### Routing Tables

A routing table contains information about where packets should be sent depending on their destination IP addresses.

Each route contains two main elements:

- **Destination:** the network or IP address that the route applies to.
- **Gateway:** the next router to which the packet should be forwarded.

For example:

| Destination | Gateway |
|:------------|:--------|
| 192.168.2.0/24 | 192.168.1.1 |
| 10.0.0.0/8 | 192.168.1.1 |

The first route indicates that packets addressed to `192.168.2.0/24` should be forwarded to `192.168.1.1`.

### Default Routes
A **default route** is used when no more specific route matches the destination IP address.

It is represented as `0.0.0.0/0` or `default`.

This means that packets without a more specific matching route will be forwarded to `192.168.1.1`. When several routes match a destination, the router selects the most specific one, using **longest prefix matching**.

### Forward and Reverse Paths
For two devices to exchange packets successfully, both the forward and return paths must be available.

- **Forward path:** Computer A → Router → Computer B.
- **Reverse path:** Computer B → Router → Computer A.

If the packet reaches its destination but there is no valid return route, communication may fail.

**Important:** A gateway must be reachable through one of the device's directly connected networks. It is not necessarily the destination's IP address or the next numerical IP address.

## Routers and Switches
Routers and switches are network devices used to connect other devices and allow communication between them. Although both forward data, they serve different purposes.

### Switches
A **switch** connects multiple devices within the same local area network (LAN), allowing them to communicate directly. Switches operate primarily at **Layer 2 (Data Link)** of the OSI model and use **MAC addresses** to determine where Ethernet frames should be forwarded.

A MAC (Media Access Control) address is a hardware-level identifier associated with a network interface.

```text
Computer A ──┐
             ├── Switch
Computer B ──┘
```

If Computer A and Computer B belong to the same subnet, they can communicate through the switch without requiring a router. A switch learns which MAC addresses are reachable through each of its ports and uses this information to forward frames.

### Routers
A **router** connects different IP networks and forwards packets between them. Routers operate primarily at **Layer 3 (Network)** of the OSI model and use IP addresses and routing tables to determine where packets should be forwarded.

```text
192.168.1.0/24                    192.168.2.0/24

Computer A ── Router R ── Computer B
```

The router has two interfaces:

| Interface | IP Address | Subnet |
|:----------|:-----------|:-------|
| R1 | 192.168.1.1 | 192.168.1.0/24 |
| R2 | 192.168.2.1 | 192.168.2.0/24 |

Each interface belongs to a different subnet. When Computer A sends a packet to Computer B, the router receives it through R1 and forwards it through R2.

### Network Interfaces
A **network interface** is the connection point through which a device communicates with a network. Routers usually have multiple interfaces, each connected to a different subnet.

```text
Computer A ── R1 [Router] R2 ── Computer B
```

Each interface has its own IP address and subnet mask. For two directly connected interfaces to communicate using IP, each must consider the other's address reachable on the local link. In a typical network configuration, both interfaces use the same subnet mask.
