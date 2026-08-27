# Conductors & Wiring
Part of the **[hardware eXPerience](https://github.com/gom9000/xp-hardware)** collection: reusable engineering knowledge built through practical experimentation.

## Background
When prototyping on stripboards or perforated PCBs, it is customary for hobbyists to use surplus component leads (typically resistors and diodes) as conductors. These leads conveniently come mainly in two recurring diameters, $0.5\text{ mm}$ and $0.8\text{ mm}$, which this document treats as the two reference gauges. These notes document the practical design rules and wire gauge choices adopted to ensure low voltage drops and structural reliability under load when using these leads.


## Design Basis
Two quantities control the voltage drop per meter of a conductor: the material's resistivity ($\rho$) and the current density you choose to run through it ($J$, current per unit cross-section). Together they set the drop directly:

$$\Delta V_m = I \cdot R_m = (J \cdot A)(\rho/A) = \rho \cdot J$$

**Resistivity ($\rho$)**: Since circuits are often inside closed or poorly ventilated enclosures, these notes deliberately use the temperature-corrected resistivity of copper at $50^\circ\text{C}$ rather than the standard $20^\circ\text{C}$ value. This assumes a conservative operating condition (a warm chassis, sustained load), giving the numbers below a built-in safety margin without having to track ambient temperature case by case:

$$\rho_{50} = \rho_{20} \times [1+\alpha(50 - 20)]$$

with: $\rho_{20} = 0.0175\ \Omega\cdot\text{mm}^2/\text{m}, \quad \alpha = 0.00393^\circ\text{C}^{-1}$

$$\rho_{50} \approx 0.02\ \Omega\cdot\text{mm}^2/\text{m} \quad (20\ \text{m}\Omega\cdot\text{mm}^2/\text{m})$$

**Current density ($J$)**: Unlike $\rho$, which is fixed by the material, $J$ is a design choice. The value adopted here is intentionally conservative, prioritizing low voltage drop, modest self-heating, and robustness when using surplus component leads of uncertain quality:

$$J = 5\text{ A/mm}^2$$

This value is well below the current densities commonly tolerated by bare copper conductors in free air. Keeping $J$ constant across every gauge provides two practical advantages:

- **Thermal margin**: combined with the $50^\circ\text{C}$ resistivity value, it provides ample thermal margin for conductors used inside enclosed chassis.
- **Predictable losses**: since $\Delta V_m$ depends only on $J$ and $\rho$ (not on the wire's section), fixing $J$ also fixes the voltage drop to a predictable $\approx 100\text{ mV}$ per meter across all wire sizes, regardless of which of the two gauges is used for a given trace.


## In Plain Terms
The formulas above translate into three simple proportions:

* **Wider conductor** → less voltage drop, more current capacity (larger section, lower resistance).
* **Longer conductor** → more total voltage drop (resistance scales directly with length; all values here are *per meter*).
* **More current** → more voltage drop *and* more self-heating, since both depend on current density ($J = I/A$), not current alone.

Fixing $J$ at a conservative and constant value is the trick that makes the following tables comparable: it makes $\Delta V_m$ depend only on the material, not on the specific gauge, so a $0.5\text{ mm}$ lead, an AWG 24 wire, and a $20\text{ mil}$ PCB trace all drop a predictable voltage per meter, and can be substituted for one another without recalculating from scratch.


## Reference Tables
Calculated with $J = 5\text{ A/mm}^2$ and $\rho_{50} = 0.02\ \Omega\cdot\text{mm}^2/\text{m}$. *Geometric profiles and electrical values are rounded for better bench-work readability.*

### Component Leads (Metric Gauge)
| Diameter ($\varnothing$) | Section ($A$) | Reference Current ($I$) | Resistance / Meter ($R_m$) | Voltage Drop / Meter ($\Delta V_m$) |
| :---: | :---: | :---: | :---: | :---: |
| **$0.5\text{ mm}$** | $\approx 0.20\text{ mm}^2$ | 1.0 A | $100\text{ m}\Omega/\text{m}$ | **$100\text{ mV/m}$** |
| **$0.8\text{ mm}$** | $\approx 0.50\text{ mm}^2$ | 2.5 A | $40\text{ m}\Omega/\text{m}$ | **$100\text{ mV/m}$** |

### Equipment Wires (AWG Gauge - American Wire Gauge)
| AWG | Diameter ($\varnothing$) | Section ($A$) | Reference Current ($I$) | Resistance / Meter ($R_m$) | Voltage Drop / Meter ($\Delta V_m$) |
| :---: | :---: | :---: | :---: | :---: | :---: |
| **AWG 14** | $\approx 1.63\text{ mm}$ | $\approx 2.08\text{ mm}^2$ | 10 A | $10\text{ m}\Omega/\text{m}$ | **$100\text{ mV/m}$** |
| **AWG 16** | $\approx 1.29\text{ mm}$ | $\approx 1.31\text{ mm}^2$ | 6.5 A | $15\text{ m}\Omega/\text{m}$ | **$100\text{ mV/m}$** |
| **AWG 18** | $\approx 1.02\text{ mm}$ | $\approx 0.82\text{ mm}^2$ | 4 A | $25\text{ m}\Omega/\text{m}$ | **$100\text{ mV/m}$** |
| **→ AWG 20** | **$\approx 0.81\text{ mm}$** | **$\approx 0.52\text{ mm}^2$** | **2.6 A** | **$40\text{ m}\Omega/\text{m}$** | **$100\text{ mV/m}$** |
| **AWG 22** | $\approx 0.64\text{ mm}$ | $\approx 0.32\text{ mm}^2$ | 1.6 A | $60\text{ m}\Omega/\text{m}$ | **$100\text{ mV/m}$** |
| **→ AWG 24** | **$\approx 0.51\text{ mm}$** | **$\approx 0.20\text{ mm}^2$** | **1 A** | **$100\text{ m}\Omega/\text{m}$** | **$100\text{ mV/m}$** |
| **AWG 26** | $\approx 0.40\text{ mm}$ | $\approx 0.13\text{ mm}^2$ | 0.65 A | $150\text{ m}\Omega/\text{m}$ | **$100\text{ mV/m}$** |
| **AWG 28** | $\approx 0.32\text{ mm}$ | $\approx 0.08\text{ mm}^2$ | 0.40 A | $250\text{ m}\Omega/\text{m}$ | **$100\text{ mV/m}$** |
| **AWG 30** | $\approx 0.25\text{ mm}$ | $\approx 0.05\text{ mm}^2$ | 0.25 A | $400\text{ m}\Omega/\text{m}$ | **$100\text{ mV/m}$** |

### Bench Interconnections
| Accessory | Section  ($A$) | Reference Current ($I$) | Practical Note |
| :--- | :---: | :---: | :--- |
| **Standard Pin Headers** (Rigid 2.54mm) | $\approx 0.41\text{ mm}^2$ | 1.0 A / pin | High-quality gold-plated pins can reach 2.0 A; for a conservative design using economy or surplus parts, 1.0 A per pin is adopted as the safe limit to mitigate localized contact resistance heating and voltage drops. Parallel multiple pins to reduce contact resistance overhead on power rails. |
| **Solid-Core Breadboard Jumpers** (AWG 22) | $\approx 0.32\text{ mm}^2$ | 1.0 A | While the AWG 22 wire can theoretically handle 1.6 A, the internal spring clips of standard breadboards degrade rapidly and increase contact resistance; keeping current at or below 1.0 A prevents localized voltage drops and heating. |
| **Flexible Jumper Wires** (AWG 24 - 26) | $0.13$ - $0.20\text{ mm}^2$ | 0.5 A | Although the copper cross-section supports up to 1.0 A, the crimped leaf-connections inside the plastic housings represent a critical point of failure; restricted to 0.5 A to ensure signal and low-power integrity. |
| **Dupont Lab Cables** (AWG 28 - 30, often CCA) | $0.05$ - $0.08\text{ mm}^2$ | 0.1 A | Cheap sets use copper-clad aluminum (CCA), which has a resistivity $1.6\times$ higher than pure copper. Due to high bulk resistance ($\Delta V_m > 160\text{ mV/m}$) and fragile crimping, they are strictly rated at 100 mA for digital logic signals only. |


## PCB Copper Traces (1 oz / 35 µm Standard Foil)
Applying a constant current density ($J = 5\text{ A/mm}^2$) across both round component leads and planar PCB traces reveals a structural physical disparity:

* **Aspect Ratio & Heat Dissipation:** A $35\,\mu\text{m}$ (1 oz) PCB trace features an extremely high surface-to-volume ratio compared to a cylindrical $0.5\text{ mm}$ lead. Consequently, PCB traces dissipate thermal energy into the ambient air and the underlying FR4 substrate significantly more efficiently.
* **Practical Design Limits:** While a $0.5\text{ mm}$ lead ($A \approx 0.20\text{ mm}^2$) carries $1.0\text{ A}$ at $J = 5\text{ A/mm}^2$, strictly replicating this cross-section on a 1 oz PCB would require an impractical trace width of $\approx 5.7\text{ mm}$ ($225\text{ mil}$).

According to the **IPC-2221** standard, the maximum current $I$ tolerated on PCB layers for a given temperature rise $\Delta T$ (in °C) and cross-sectional area $A$ (in $\text{mil}^2$) is expressed by the empirical formulation:

$$I = k \cdot (\Delta T)^b \cdot A^c$$

Where for external layers, $k = 0.048$, $b = 0.44$, and $c = 0.725$.

Due to the sub-linear exponent $c = 0.725$ ($I \propto A^{0.725}$), the allowable current density $J = I / A$ is non-constant and scales inversely with the cross-sectional area:

$$J(A) = \frac{k \cdot (\Delta T)^b \cdot A^c}{A} = k \cdot (\Delta T)^b \cdot A^{c-1} \propto A^{-0.275}$$

For a fixed temperature rise $\Delta T = 10^\circ\text{C}$, the corresponding density is roughly $100\text{ A/mm}^2$ at $10\text{ mil}$ and $48\text{ A/mm}^2$ at $140\text{ mil}$.

To maintain the conservative design philosophy adopted throughout these notes, a baseline density of **$J = 20\text{ A/mm}^2$** is selected as a conservative design limit. This choice avoids trace-by-trace recalculations while satisfying two strict operational constraints:

1. **Thermal Operating Window:** For standard signal widths ($10\text{ mil}$ to $40\text{ mil}$), operating at $20\text{ A/mm}^2$ keeps trace self-heating well below the standard $10^\circ\text{C}$ IPC limit, providing thermal margin in unventilated enclosures.
2. **Ohmic Drop Control ($\Delta V_m = 400\text{ mV/m}$):** On wider power paths ($> 40\text{ mil}$), the design limit shifts from thermal dissipation to voltage drop prevention. At $J = 20\text{ A/mm}^2$, a typical $50\text{ mm}$ PCB run drops less than $20\text{ mV}$, protecting logic noise margins and analog precision.

Both $J$ values are chosen with the same conservative intent, and the difference reflects the underlying physics (round conductor vs. planar copper foil).

Under $J = 20\text{ A/mm}^2$ and $\rho_{50} = 0.02\ \Omega\cdot\text{mm}^2/\text{m}$, the reference voltage drop scales predictably to **$\Delta V_m = 400\text{ mV/m}$**:

| Width (mil) | Width (mm) | Copper Area ($A$) | Reference Current ($I$) at $J=20\text{ A/mm}^2$ | Resistance / Meter ($R_m$) | Voltage Drop / Meter ($\Delta V_m$) |
| :---: | :---: | :---: | :---: | :---: | :---: |
| 10 mil | 0.254 mm | $0.0089\text{ mm}^2$ | **0.18 A** | $2.25\ \Omega/\text{m}$ | 400 mV/m |
| 15 mil | 0.381 mm | $0.0133\text{ mm}^2$ | **0.27 A** | $1.50\ \Omega/\text{m}$ | 400 mV/m |
| 20 mil | 0.508 mm | $0.0178\text{ mm}^2$ | **0.36 A** | $1.12\ \Omega/\text{m}$ | 400 mV/m |
| 30 mil | 0.762 mm | $0.0267\text{ mm}^2$ | **0.53 A** | $0.75\ \Omega/\text{m}$ | 400 mV/m |
| 40 mil | 1.016 mm | $0.0356\text{ mm}^2$ | **0.71 A** | $0.56\ \Omega/\text{m}$ | 400 mV/m |
| 50 mil | 1.270 mm | $0.0445\text{ mm}^2$ | **0.89 A** | $0.45\ \Omega/\text{m}$ | 400 mV/m |
| → 60 mil | 1.524 mm | $0.0533\text{ mm}^2$ | **1.07 A** | $0.38\ \Omega/\text{m}$ | 400 mV/m |
| 100 mil | 2.540 mm | $0.0889\text{ mm}^2$ | **1.78 A** | $0.225\ \Omega/\text{m}$ | 400 mV/m |
| → 140 mil | 3.556 mm | $0.1245\text{ mm}^2$ | **2.49 A** | $0.161\ \Omega/\text{m}$ | 400 mV/m |

> Routing a continuous 140 mil (3.56 mm) trace on a compact single-layer board is often geometrically constraining. To preserve layout compactness while maintaining the $2.5\text{ A}$ design current of a **0.8 mm lead**, a practical prototyping approach is to use **50/60 mil trace reinforced with a bare 0.8 mm copper wire soldered directly along the trace**, effectively augmenting the 1 oz copper foil section.