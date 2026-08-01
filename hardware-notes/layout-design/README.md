# Layout Design
Part of the **[hardware eXPerience](https://github.com/gom9000/xp-hardware)** collection: reusable engineering knowledges built through practical experimentation.

## Background
In hardware prototyping, layout design is often treated as a downstream, process-dependent step.  
Circuits are typically routed differently depending on the target manufacturing medium: point-to-point wiring on perforated boards (stripboards/perfboards), single-sided home photo-etching, or multi-layer industrial PCB fabrication.  
This fragmented approach introduces significant drawbacks:
* It forces the redundant re-verification of parasitic behaviors (trace resistance, loop areas, capacitive coupling) whenever moving from bench prototype to final board.
* It increases the likelihood of introducing functional discrepancies between the prototype and the production unit.

To overcome this, the design methodology adopted across these repositories enforces a unified, medium-agnostic layout paradigm.


## Layout Isomorphism
Layout Isomorphism is the practice of designing a single, 1:1 physical layout that is natively compatible across all target fabrication methods without requiring topological or geometric modifications.  
An isomorphic layout guarantees that parasitic distribution, thermal dissipation pathways, and signal/power ground returns stay within the same conservative design margin across every target medium, from hand-wired bench prototype to manufactured PCB.


## Core Layout Guidelines
To guarantee native isomorphism across perfboards, photo-etched boards, and industrial PCBs, every layout adheres to the following core constraints:

- **Grid and Pitch:**
   * All component footprints, pad centers, and trace intersections must strictly align to a standard $2.54\text{ mm}$ ($100\text{ mil}$) grid.
   * Passive components (resistors, axial capacitors) and semiconductors use standard lead spacings to allow direct insertion into perforated boards.

- **Native Single-Layer Topography:**
   * Routing is performed on a single bottom layer.
   * Top-layer connections are minimized and treated exclusively as straight physical wire jumpers routed between grid nodes on the component side.

-  **Pad & Clearance:**
   * Pad diameters are sized conservatively to tolerate manual column drilling without stripping the copper ring during home fabrication or bench assembly.
   * Minimum trace-to-trace and trace-to-pad clearances are maintained above conservative limits to prevent under-etching or bridging during home chemical etching (toner transfer / photo-etching).


## Routing Guidelines for Analog
Analog precision relies heavily on controlling parasitic voltage drops and magnetic/capacitive coupling. The following routing guidelines govern all analog signals within isomorphic layouts:

- **Star Grounding & Power Distribution:**
   * High-current return paths (e.g., load connections, power stage emitters) are routed through dedicated, wide conductors back to the primary power input terminal.
   * Small-signal references connect to ground via separate, isolated traces that meet at a single "star" point, preventing heavy load currents from introducing ohmic offset errors.

- **Power Bus Dimensioning:**
   * High-current paths are kept as short and wide as possible. When hand-wiring on perfboards, these paths are reinforced using copper component leads (e.g., $0.5\text{ mm}$ / $0.8\text{ mm}$ diameter leads), ensuring millivolt-level parasitic drops across the entire operating range. See **[Conductors & Wiring](../conductors-and-wiring)**.


## On Routing a Nice Layout
*Routing is not a mechanical, secondary task to be delegated to an automated tool*, it is an integral phase of electronic design. A schematic dictates logical connectivity, but the physical layout defines the real-world physics of the circuit, governing parasitic coupling, thermal gradients, and current dynamics.  
A well-executed layout bridges functional engineering with structural aesthetics:

- Components are arranged by functional blocks, and/or sequentially following the natural signal path, giving the physical board an intuitive, readable structure. Obviously, external mechanical constraints (enclosure outline, fixed connector positions) may locally increase routing density.

- Visual elegance in PCB design, characterized by clean parallel traces, consistent component alignment, symmetrical spacing, and logical group floorplanning, is rarely just cosmetic. 

- An aesthetically ordered board simplifies bench probing, reduces assembly errors, and reflects a deep architectural clarity of the underlying circuit.