# Universal Bridgeless Nonisolated Battery Charger with Wide Output Voltage Range

Open-loop LTspice and closed-loop MATLAB/Simulink simulation of a 1.5 kW universal bridgeless nonisolated battery charger for EV on-board charging, with a dual-loop CC-CV controller.

Based on: S. Karneddi and D. Ronanki, *Universal Bridgeless Nonisolated Battery Charger with Wide Output Voltage Range*, IEEE Transactions on Power Electronics, 2023.

---

## Overview

Battery chargers for electric vehicles must accept a wide range of grid voltages and charge battery packs across a wide voltage window, while drawing clean, near-unity power factor current from the supply. This project studies and simulates a bridgeless, nonisolated charger topology that supports:

- Universal AC input, 85-265 V at 50/60 Hz
- Wide DC output range, 48-450 V
- Rated power of 1.5 kW
- Switching frequency of 40 kHz
- Constant-current / constant-voltage (CC-CV) charging using a dual-loop controller with an AHM filter

The converter is modelled in two environments:

- **LTspice:** open-loop circuit model of the power stage, used to study the converter's switching behaviour and waveforms without a controller.
- **MATLAB/Simulink:** the complete closed-loop model of the charger, including the dual-loop CC-CV controller with AHM filter, for regulated charging operation.

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
│   └── projectendetret.asc      # LTspice open-loop circuit of the charger
├── simulink/
│   └── UC2_fixed.slx            # Simulink closed-loop model with CC-CV controller
└── README.md
```

## Requirements

- **LTspice** (any recent version) for the `.asc` schematic
- **MATLAB with Simulink** and the Simscape Electrical (Specialized Power Systems) toolbox for the `.slx` model

## How to Run

**LTspice (open loop)**
1. Open `ltspice/projectendetret.asc` in LTspice.
2. Run the transient simulation as configured in the schematic. The circuit runs open loop, with no controller.
3. Probe the input voltage/current and output voltage/current nodes to view the waveforms.

**MATLAB/Simulink (closed loop)**
1. Open MATLAB and navigate to the `simulink/` folder.
2. Open `UC2_fixed.slx`. This is the full circuit with the dual-loop CC-CV controller.
3. Run the model and open the scopes to view input voltage, input current, output voltage and output current.

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
