# Switch, Access Point & Router

## 1. What is a Switch and Why Do We Need It?

- If we want **computers in the same local network to communicate with each other**, we can use a [[switch]].
- We connect computers to a switch using Ethernet cables.
  - Common Ethernet cable categories include **Cat 5e** and **Cat 6**.
  - `Cat` stands for Category.
- Fiber-optic cables generally offer higher bandwidth and support longer distances than traditional copper Ethernet cables.
- A switch forwards Ethernet frames between connected devices using their **MAC addresses**.

> **Important:** A traditional Ethernet switch connects devices through wired connections. It does not provide Wi-Fi by itself.

### What if we want wireless connectivity?

If we want devices in the same network to communicate wirelessly, we use a [[access_point]].

- **Switch** → Connects devices through wired Ethernet.
- **Access Point (AP)** → Allows wireless devices to connect to a network using Wi-Fi.
- **LAN (Local Area Network)** → A network covering a limited geographical area, such as a home, office, or building.

An access point can connect to a switch using an Ethernet cable, allowing wireless devices to communicate with devices on the wired LAN.

## 2. What is a Router and Why Do We Need It?

- The main task of a [[router]] is to **connect different networks and forward packets between them**.
- For example, a router allows devices in our home LAN to communicate with external networks, including the Internet.
- Our Internet connection is provided by an [[isp]] (Internet Service Provider).
- The ISP provides a connection to our home through a suitable medium, such as fiber-optic cable or coaxial cable.

### Do we need a router for devices in the same LAN to communicate?

**No.**

- Devices in the same LAN can communicate through a switch or wireless access point.
- A router is needed when packets must travel between different IP networks.
- For example, when a computer accesses a website, its packets are forwarded through the router towards the Internet.

## 3. Home Router: A Combination Device

A typical home router is actually a combination of multiple networking components:

1. **Router** → Connects the home LAN to external networks.
2. **Switch** → Provides multiple wired Ethernet ports for connecting devices.
3. **Wireless Access Point** → Allows devices to connect through Wi-Fi.

This is why a single home networking device can provide both wired and wireless connectivity.

## 4. Internet

The [[internet]] is a global network of interconnected networks.
==The internet is the network of networks.==

It allows devices and networks across the world to communicate using standard networking protocols.

- Each router must have a special table called "Routing Table".
- This table tells us which route the packet should choose.
- The router learns the packet's destination then it looks at the routing table and learns over which port the packet will be sent. (This operation is called forwarding)
- Routers have special processors inside. These processors create Routing Table by using special algorithms.
- A router always wants to deliver the packet to its destination in the fastest way possible.
## Related Topics

- [[switch]]
- [[router]]
- [[access_point]]
- [[lan]]
- [[wan]]
- [[isp]]
- [[internet]]
- [[ethernet]]
- [[mac_address]]
- [[ip_address]]
- [[computer_networking]]
- [[freecodecamp]]