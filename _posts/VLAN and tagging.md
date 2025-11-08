## VLAN (Virtual Local Area Network)

* **Definition:** A technology that logically segments a single physical switch into multiple independent **LANs (Local Area Networks)**.
* **Purpose:**
    * Network Performance: Reduces unnecessary traffic by separating **Broadcast Domains**.
    * Security: Enhances security by preventing direct communication between different VLANs.
* **Communication:**
    * Within the same VLAN: L2 communication possible.
    * Between different VLANs: Requires **Inter-VLAN Routing** via a **Router** or **L3 Switch**.
  
## 2. Trunk Link
* **Definition:** A link configured to carry **traffic from multiple VLANs** simultaneously over **one physical connection** between two switches (or a switch and a router).
* **Necessity:** Solves the inefficiency of needing a separate physical link for every VLAN.
* **Protocol:** Commonly uses the **IEEE 802.1Q** standard.
* **Operation:** Frames passing over a trunk link have a **VLAN ID Tag** added to identify their origin.


## VLAN tagging

### What is VLAN Tagging (802.1Q)?

* A method to identify which VLAN a packet (frame) belongs to.
* Uses the **IEEE 802.1Q** standard.
* Adds a 4-byte **tag** (header) into the Ethernet frame.
* This tag contains the **VLAN ID (VID)**.
* Allows a single physical link (Trunk) to carry traffic for many different VLANs.

### Port Types

* **Access Port**
    * Connects to end devices (e.g., PC, printer, server).
    * Belongs to **only one** VLAN.
    * Receives and sends **untagged** traffic.
* **Trunk Port**
    * Connects to another switch.
    * Carries traffic for **multiple** VLANs.
    * Receives and sends **tagged** traffic (using 802.1Q).

### How Tagging Works

1.  **Ingress (Untagged):**
    * A PC sends a normal, untagged packet to an **Access port** (e.g., Port 1) on Switch A.

2.  **VLAN Assignment:**
    * Switch A checks the configuration for Port 1.
    * The port is configured as `VLAN 10`.
    * The switch now internally associates this packet with `VLAN 10`.

3.  **Forwarding Decision (Egress):**
    * The switch checks its MAC address table to find the destination MAC.
    * The destination is on another switch (Switch B), reachable via a **Trunk port**.

4.  **Tagging:**
    * Before sending the packet out the **Trunk port**, Switch A inserts the **802.1Q tag** with `VLAN ID = 10`.

5.  **Transmission:**
    * The **tagged** packet travels across the Trunk link to Switch B.

6.  **Receiving (Tagged):**
    * Switch B receives the tagged packet on its Trunk port.
    * It reads the tag and sees `VLAN ID = 10`.

7.  **Forwarding Decision (VLAN-Specific):**
    * Switch B knows this packet belongs *only* to `VLAN 10`.
    * It looks up the destination MAC in its **`VLAN 10` MAC address table**.
    * The table says the destination PC is on **Access port** 5.

8.  **Untagging (Egress):**
    * Before sending the packet out Access port 5, Switch B **removes** the 802.1Q tag.
    * The destination PC receives a normal, untagged packet.

### Key Concepts
* **Purpose:** Tagging allows a *single VLAN* (e.g., VLAN 10) to be **extended** across multiple physical switches. It is for communication *within* the same VLAN.
* **Isolation:** The tag ensures traffic stays isolated. `VLAN 10` traffic can *never* go to a `VLAN 20` port (at Layer 2).
* **MAC Address Tables:** A switch maintains a **separate MAC table for each VLAN**.
* **Inter-VLAN Routing:** Communication *between* different VLANs (e.g., VLAN 10 to VLAN 20) is not possible with only a switch. It requires a **Router** or **Layer 3 Switch**.
