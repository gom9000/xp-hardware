# Layout Design
Part of the **[hardware eXPerience](https://github.com/gom9000/xp-hardware)** collection: reusable engineering knowledges built through practical experimentation.

## Background
In hobbyist and bench electronics, prototyping typically starts on a perforated board. Yet layout design is often treated as a downstream, process-dependent step.  
Circuits are typically routed differently depending on the target manufacturing medium: point-to-point wiring on perforated boards (stripboards/perfboards), home photo-etching, or industrial PCB fabrication.  
This fragmented approach introduces significant drawbacks:
* It forces the redundant re-verification of parasitic behaviors (trace resistance, loop areas, capacitive coupling) whenever moving from bench prototype to final board.
* It increases the likelihood of introducing functional discrepancies between the prototype and the production unit.
* It often ignores basic power and thermal constraints of thin PCB copper traces.
To overcome this, the design methodology adopted across these repositories enforces a unified, medium-agnostic layout paradigm.


## Topological and Geometric Layout Isomorphism
Topological and geometric isomorphism is the practice of designing a single, 1:1 physical layout that is natively compatible across all target fabrication methods without requiring topological or geometric modifications.  
This kind of isomorphic layout is designed to keep parasitic effects, thermal dissipation pathways, and signal/power ground returns within the same conservative design margins across every target medium, from hand-wired bench prototype to manufactured PCB.


## Core Layout Guidelines
To preserve native topological isomorphism across perfboards, photo-etched boards, and industrial PCBs, every layout adheres to the following core constraints:

- **Grid and Pitch:**
   * All component footprints, pad centers, and trace intersections, where possible, must align to a standard $2.54\text{ mm}$ ($100\text{ mil}$) grid.
   * Passive components (resistors, axial capacitors) and semiconductors use standard lead spacings to allow direct insertion into perforated boards.

- **Native Single-Layer Topography:**
   * Routing is performed on a single bottom layer.
   * Top-layer connections are minimized and treated exclusively as straight physical wire jumpers routed between grid nodes on the component side.

- **Pad & Clearance:**
   * Pad diameters are sized conservatively to tolerate manual column drilling without stripping the copper ring during home fabrication or bench assembly.
   * Minimum trace-to-trace and trace-to-pad clearances are maintained above conservative limits to prevent under-etching or bridging during home chemical etching (toner transfer / photo-etching).

- **Trace Width & Current Capacity:**
   * High-current paths must be as short and wide as possible.
   * A standard PCB trace is only a thin, two-dimensional copper layer (typically $35\ \mu\text{m}$ thick). Because it has a smaller section than a perfboard solid wire trace (requiring an impractical $\approx 5.7\text{ mm}$ width to replicate the cross-section of a $0.5\text{ mm}$ lead), the PCB trace defines the maximum current capacity of the entire design.
   * To increase current trace capability and preserve layout compactness, best practice dictates reinforcing the traces with a bare copper wire soldered directly along the path.
   * Hand-wiring on perfboards replicates these exact path geometries but builds them using copper wires or component leads (e.g., $0.5\text{ mm}$ to $0.8\text{ mm}$ diameter). This allows the perfboard realization to easily handle the target current of the design. See **[Conductors & Wiring](../conductors-and-wiring)**.


## Routing Guidelines for Analog
Analog precision relies heavily on controlling parasitic voltage drops and magnetic/capacitive coupling. The following routing guidelines govern all analog signals within isomorphic layouts:

- **Star Grounding & Power Distribution:**
   * High-current return paths (e.g., load connections, power stage emitters) are routed through dedicated, wide conductors back to the primary power input terminal.
   * Sensitive Small-signal references connect to ground via separate traces that meet at a single "star" point, preventing heavy load currents from introducing ohmic offset errors.

- **Trace Geometry & Frequency Domain:**
   * While high-frequency RF design demands mitered corners or curved traces to prevent impedance discontinuities, these parasitic effects are negligible in low-frequency, baseband analog circuits. Maintaining an orthogonal layout keeps grid alignment without compromising signal integrity.


## On Routing a Nice Layout
*Routing is not a mechanical, secondary task to be delegated to an automated tool*, it is an integral phase of electronic design. A schematic dictates logical connectivity, but the physical layout defines the real-world physics of the circuit, governing parasitic coupling, thermal gradients, and current dynamics.  
A good layout combines technical functionality and visual design:

- **Functional Floorplanning:** Components are arranged by functional blocks, and/or sequentially following the natural signal path, giving the physical board an intuitive, readable structure. Obviously, external mechanical constraints (enclosure outline, fixed connector positions) may locally increase routing density.

- **Aesthetic Geometry:** Visual elegance in PCB design, characterized by clean parallel traces, consistent component alignment, symmetrical spacing, and logical group floorplanning, is rarely just cosmetic. 

- **Maintainability and Clarity:** An aesthetically ordered board simplifies bench probing, reduces assembly errors, and reflects a deep architectural clarity of the underlying circuit.