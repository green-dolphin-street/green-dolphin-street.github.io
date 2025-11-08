### NAT (Network Address Translator)

- NAT
  - A device (or a part of device) that modifies the **source/destination IP addresses and port numbers** in an IP packet header
  - Representative usage: translate **private IP addresses** into **public IP addresses** to enable Internet communication

- Purpose and characteristics of NAT
    * Enhances **Security** by hiding the internal private network structure from the outside
    * Prevents and resolves IP address conflicts

* **NAT Implementation Device**
    * NAT is a **software function (technology)**, not a separate hardware device
    * Primarily integrated into devices located at the network boundary between internal and external networks, such as **Routers, Firewalls, and Internet Gateways (Home Routers)**

---

### NAT Classification by Type: Operation and Features

#### 1. Source NAT (SNAT)

* **Direction of Operation:** Internal Network $\rightarrow$ External Network (When packets exit)
* **Translation Target:** The packet's **Source IP Address and Port**
* **Operation Features:**
    * Changes the internal Private IP to a **Public IP** address for external communication
    * The most common form is **PAT (Port Address Translation)**, which maps many internal devices to one Public IP using different Port numbers (N:1)
    * **Purpose:** Public IP conservation and allowing internal devices access to the Internet

#### 2. Destination NAT (DNAT)

* **Direction of Operation:** External Network $\rightarrow$ Internal Network (When packets enter)
* **Translation Target:** The packet's **Destination IP Address and Port**
* **Operation Features:**
    * Translates a Public IP request into the **specific Private IP** of an internal server for delivery
    * Often referred to as **Port Forwarding**
    * **Purpose:** Providing a path for external Internet users to access internal services (Web, FTP, etc.) while protecting the server

#### 3. One-to-One NAT (Static NAT)

* **Direction of Operation:** Bi-directional Fixed Mapping (External $\rightleftarrows$ Internal)
* **Translation Target:** IP Address, translated **1:1 and fixed**
* **Operation Features:**
    * **One Private IP** is permanently mapped to **one Public IP**
    * External access to the Public IP is directly forwarded to the internal server (often without Port translation)
    * **Purpose:** Assigning a unique Public IP address to an internal server for easy external accessibility (no IP conservation benefit)

---
