*This project has been created as part of the 42 curriculum by lupalomi*

# Description
This project consist on series of TCP/IP addressing problems that need solution. Our objective is change the configurations when possible to allow communication between two points.

## TCP/IP addressing

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

## Routers and Switches

## OSI Layers

# Instructions

# Resources
