# 1. Fiber Optics Overview

Fiber optics is used for:
- Long-distance communication (network backbones)
- High-speed local area networks (LANs)
- High-speed internet delivery (such as Fiber to the Home — FTTH)

**Basic Working Principle:**
- The system converts an electrical signal into a light pulse.
- This light pulse travels through an optical fiber.
- At the receiving end, the light pulse is converted back into an electrical signal.
- The presence of a light pulse represents a **bit 1**, and its absence represents a **bit 0**.

**Core components of an optical transmission system:**
1. **Light Source** (Laser or Light-Emitting Diode - LED)
2. **Transmission Medium** (Extremely thin glass fiber)
3. **Light Detector** (Photodetector that converts light back into an electrical signal)

> **Important Note:**  
> Fiber optic communication is naturally **simplex** — it supports **one-way** communication by default.  
> To achieve bidirectional communication, two fibers or advanced multiplexing techniques are used.

---

# 2. Transmission of Light Through Fiber

## Refraction and Total Internal Reflection

- When light passes from one medium (e.g., glass) to another (e.g., air), it **bends** — this phenomenon is called **refraction**.
- If the **angle of incidence** exceeds a specific value known as the **critical angle**, the light does **not** pass into the second medium.
- Instead, it **reflects back entirely** into the first medium — this phenomenon is called **Total Internal Reflection**.
- Optical fibers are designed to ensure total internal reflection, allowing light to be trapped and guided along the fiber with minimal loss.

**Illustrations (referred to in textbook Figures 2-4(a) and (b)):**
- Refraction: Partial bending of light at the boundary.
- Total Internal Reflection: Light stays confined within the fiber core.

---

## Multimode vs Single-Mode Fiber

| Aspect | Multimode Fiber | Single-Mode Fiber |
|:------|:----------------|:------------------|
| **Core Diameter** | > 50 microns | ~8–10 microns |
| **Light Paths** | Multiple rays at different angles (multiple modes) | A single ray, almost straight |
| **Distance Support** | Short to medium (~up to 15 km) | Long (~up to 100 km without amplification) |
| **Cost** | Less expensive | More expensive |
| **Application** | Local area networks, short links | Long-haul transmission, telecommunication backbones |

---

# 3. Fiber Cables

## Structure of a Fiber Cable

The structure of an optical fiber consists of:

1. **Core**:  
   - Extremely thin glass center where the light travels.
   
2. **Cladding**:  
   - Surrounds the core.
   - Made of glass with a lower refractive index than the core.
   - Causes light to undergo total internal reflection.

3. **Plastic Jacket**:  
   - Provides mechanical protection to the cladding.

4. **Outer Sheath**:  
   - Protects against environmental hazards like moisture, chemicals, and physical stress.

**Typical core sizes:**
- Multimode fiber: ~50 microns (similar to the thickness of a human hair).
- Single-mode fiber: ~8–10 microns (much thinner).

---

## Deployment of Fiber Cables

- **Terrestrial fibers** (land deployment):
  - Buried about 1 meter underground.
- **Undersea fibers** (ocean deployment):
  - Sea plows create a trench in shallow waters and lay cables inside it.
  - In deep oceans, cables are simply laid on the seabed without burial.

---

## Fiber Connection Techniques

1. **Connectors**:
   - Fiber ends are terminated with connectors, allowing easy plugging and unplugging.
   - Connectors typically cause about 10–20% light loss at the connection points.
   - Advantage: Fast and flexible for reconfiguration.

2. **Mechanical Splice**:
   - Two fiber ends are carefully aligned and clamped together inside a sleeve.
   - Precision is checked by passing a light beam and observing the transmission.

3. **Fusion Splice**:
   - (To be discussed later) involves melting the two fiber ends together to create a permanent, low-loss connection.

---

# (Additional Concepts You Should Know)

## What is Attenuation?

- **Attenuation** refers to the loss of signal strength as it travels through the fiber.
- It is measured in **decibels per kilometer (dB/km)**.
- **Example**:
  - If the signal power is reduced by half, it corresponds to approximately 3 dB of loss.
  - Formula:  
    \[
    \text{Loss (dB)} = 10 \times \log_{10} \left( \frac{P_{\text{initial}}}{P_{\text{final}}} \right)
    \]
  
## Important Operational Wavelengths

- Fiber optics typically operates at three wavelengths:
  1. **0.85 microns** — early systems, higher loss
  2. **1.30 microns** — improved systems, moderate loss
  3. **1.55 microns** — modern systems, lowest loss with excellent amplifier compatibility

## Chromatic Dispersion

- **Chromatic dispersion** occurs because different wavelengths of light travel at slightly different speeds within the fiber.
- As a result, light pulses spread out over long distances, which can cause inter-symbol interference.
- **Solution**: Special pulse-shaping techniques, such as using hyperbolic cosine pulse shapes, are employed to minimize dispersion effects.

---

# Final Quick Summary Table

| Heading | Key Point |
|:--------|:----------|
| Fiber Optics Overview | Light-based, high-speed communication for LANs and internet. |
| Transmission of Light Through Fiber | Light remains trapped inside fiber via total internal reflection. |
| Fiber Cables | Fiber has core-cladding-jacket structure; connection via connectors or splicing. |

---

### **Comparison of Fiber Optics and Copper Wire**

---

#### **Overview**

Fiber optics and copper wire are both widely used for data transmission, but they differ significantly in terms of performance, cost, and use cases. Fiber offers higher bandwidth and better long-distance capabilities, while copper remains prevalent due to its lower cost and easier installation.

---

#### **Advantages of Fiber Optics Over Copper**

- **Higher Bandwidth:**  
  Fiber can handle *much higher data rates* (100 Gbps and beyond) compared to copper (typically up to hundreds of Mbps or a few Gbps). This makes fiber essential for high-end network backbones.

- **Lower Attenuation:**  
  Fiber experiences far less signal loss over distance.  
  - Repeaters needed roughly every **50 km** for fiber.  
  - Repeaters needed about every **5 km** for copper.  
  This results in **significant cost savings** for long-distance communication.

- **Immunity to External Factors:**  
  Fiber is not affected by:  
  - Power surges  
  - Electromagnetic interference  
  - Corrosive chemicals in the air  
  This makes it ideal for harsh industrial environments.

- **Thin and Lightweight:**  
  Telephone companies favor fiber because of its size and weight advantages:  
  - **1,000 twisted copper pairs (1 km)** weigh around **8000 kg**.  
  - **Two fibers** (with far higher capacity) weigh about **100 kg**.  
  This weight reduction frees up space in cable ducts and reduces support costs.

- **Security:**  
  Fiber does not leak light and is difficult to tap, providing strong protection against wiretapping.

---

#### **Advantages of Copper Wire Over Fiber**

- **Lower Cost (Materials and Interfaces):**  
  Copper installation and equipment (e.g., Ethernet interfaces) are cheaper than fiber-optic systems.

- **Easier Installation and Maintenance:**  
  Copper technology is more familiar, and maintenance/repairs require less specialized training.

- **Durability:**  
  Copper cables are more flexible and less prone to damage from bending compared to fragile glass fibers.

- **Power Transmission:**  
  Copper cables can carry both data and power (e.g., Power over Ethernet), while fiber cannot.

---

#### **Disadvantages of Fiber Optics**

- Requires **skilled labor** for installation (e.g., fusion splicing) and maintenance.
- Fibers can be damaged easily if bent too tightly.
- Optical transmission is **unidirectional**, so two-way communication requires:  
  - Two separate fibers, **or**  
  - Two frequency bands on one fiber.
- Higher cost of fiber interfaces compared to electrical (copper) interfaces.

---

#### **Use Cases**

| Use Case | Fiber Optics | Copper Wire |
|:----------|:-------------|:------------|
| **Long-distance communication** | Preferred due to low attenuation and high bandwidth | Not suitable due to high signal loss |
| **Backbone networks** | Essential for high-speed backbones (ISPs, data centers) | Rarely used |
| **Last-mile connections** | Increasingly used (e.g., Fiber to the Home) but costly | Common for home/office Ethernet |
| **Harsh environments** | Highly suitable (immune to interference and corrosion) | Less suitable (susceptible to interference/corrosion) |
| **Low-cost, short-distance links** | Overkill; not cost-effective | Ideal (e.g., LAN connections) |

---

#### **Key Takeaways**

- **Fiber** is the future for high-speed, long-distance data communication due to its superior bandwidth, lower attenuation, and security advantages.
- **Copper** remains relevant for shorter distances and cost-sensitive applications where ultra-high speeds are not necessary.

Despite fiber’s clear advantages, its higher costs and installation complexity mean copper will continue to be used for many short-distance connections until fiber becomes more economical across all use cases.

---
