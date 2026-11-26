# Universal Bridgeless Nonisolated Battery Charger with Wide Output Voltage Range

LTspice and MATLAB/Simulink simulation of a 1.5 kW universal bridgeless nonisolated battery charger for EV on-board charging, with a dual-loop CC-CV controller.

Based on: S. Karneddi and D. Ronanki, *Universal Bridgeless Nonisolated Battery Charger with Wide Output Voltage Range*, IEEE Transactions on Power Electronics, 2023.

---

## Overview

Battery chargers for electric vehicles must accept a wide range of grid voltages and charge battery packs across a wide voltage window, while drawing clean, near-unity power factor current from the supply. This project studies and simulates a bridgeless, nonisolated charger topology that supports:

- Universal AC input, 85-265 V at 50/60 Hz
- Wide DC output range, 48-450 V
- Rated power of 1.5 kW
- Switching frequency of 40 kHz
- Constant-current / constant-voltage (CC-CV) charging using a dual-loop controller with an AHM filter

The converter is modelled and verified in both LTspice (circuit-level) and MATLAB/Simulink (system-level with controller).

## Design Specifications

| Parameter | Value |
|---|---|
| Rated power | 1.5 kW |
| Input voltage | 85-265 V AC, 50/60 Hz |
| Output voltage | 48-450 V DC |
| Switching frequency | 40 kHz |
| Control scheme | Dual-loop CC-CV with AHM filter |
| Topology | Bridgeless, nonisolated |

## Repository Structure

```
.
├── ltspice/
│   └── projectendetret.asc      # LTspice schematic of the charger
├── simulink/
│   └── UC2_fixed.slx            # MATLAB/Simulink model with CC-CV controller
├── results/                     # Simulation plots (see Results)
├── docs/                        # Reference paper notes, design calculations
└── README.md
```

## Requirements

- **LTspice** (any recent version) for the `.asc` schematic
- **MATLAB with Simulink** and the Simscape Electrical (Specialized Power Systems) toolbox for the `.slx` model

## How to Run

**LTspice**
1. Open `ltspice/projectendetret.asc` in LTspice.
2. Run the transient simulation as configured in the schematic.
3. Probe the input voltage/current and output voltage/current nodes to view the waveforms.

**MATLAB/Simulink**
1. Open MATLAB and navigate to the `simulink/` folder.
2. Open `UC2_fixed.slx`.
3. Run the model and open the scopes to view input voltage, input current, output voltage and output current.

## Results

Simulation waveforms from the Simulink and LTspice models:

| Waveform | Plot |
|---|---|
| Input voltage | `results/input_voltage.png` |
| Input current | `results/input_current.png` |
| Output voltage | `results/output_voltage.png` |
| Output current | `results/output_current.png` |

<!-- Replace the table above with embedded images once the plots are added, e.g.:
![Input voltage and current](results/input_voltage_current.png)
-->

### Reference paper results (for comparison)

The reference paper reports the following figures for its own prototype. These are **the paper's results, not results of this project**:

- Peak efficiency of 95.6%
- Third-harmonic reduction of 86% and 76% (as reported in the paper)

## Tools Used

- LTspice
- MATLAB / Simulink

## Team

- Devansh Sharma
- Aditya R. Shinde
- Vineet Pal

## Reference

S. Karneddi and D. Ronanki, "Universal Bridgeless Nonisolated Battery Charger with Wide Output Voltage Range," *IEEE Transactions on Power Electronics*, 2023.

## License

This repository is shared for academic and educational purposes. Please credit the authors and the reference paper if you use or build on this work.
