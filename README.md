# Universal Bridgeless Nonisolated Battery Charger with Wide Output Voltage Range

Open-loop LTspice and closed-loop MATLAB/Simulink simulation of a 1.5 kW universal bridgeless nonisolated battery charger for EV on-board charging, with a dual-loop CC-CV controller and active harmonic minimization (AHM).

Based on: H. Karneddi and D. Ronanki, *Universal Bridgeless Nonisolated Battery Charger With Wide-Output Voltage Range*, IEEE Transactions on Power Electronics, vol. 38, no. 3, March 2023.

---

## Overview

Electric vehicles use battery packs of different voltages and are charged from different grid supplies around the world, so each vehicle normally needs its own charger. This project simulates a single charger that handles both ends of that range:

- Universal single-phase AC input, 85-265 V at 50/60 Hz
- Battery voltage range of 48-450 V
- Rated power of 1.5 kW
- Unity power factor and sinusoidal input current, with no front-end diode bridge
- No galvanic isolation (a nonisolated design), which removes the transformer and saves losses and volume

The converter is modelled in two environments:

- **LTspice:** open-loop circuit model of the power stage, used to study switching behaviour and waveforms without a controller.
- **MATLAB/Simulink:** the complete closed-loop model, including the dual-loop CC-CV controller, for regulated charging.

## Design Specifications and Component Values

| Parameter | Value |
|---|---|
| Input supply | 1-phase, 85-265 V |
| Supply frequency | 50/60 Hz ± 1% |
| Maximum power | 1.5 kW |
| Output voltage | 48-450 V |
| Switching frequency | 40 kHz |
| Output voltage ripple | 20% |
| Boost inductor L1 | 0.5 mH |
| Buck inductor L2 | 0.4 mH |
| Capacitor C1 | 44 µF |
| Output capacitor Co | 0.475 mF |
| Holding time (t_hold) | 20 ms |
| Mode transition band (Δv) | 1 V |

## Topology and Operating Modes

The charger uses three pairs of semiconductor devices (S1-S6) with L1, L2, C1 and the output capacitor Co.

- S3 and S4 switch at line frequency. The remaining switches operate at the 40 kHz switching frequency.
- S1-S4 with L1 form a boost-derived AC-DC converter. S5, S6, C1 and L2 form the buck stage.
- The operating mode follows the instantaneous input and battery voltages:
  - **Boost mode:** input voltage is below the battery voltage. The C1-L2 arm acts as an output filter and reduces ripple.
  - **Buck mode:** input voltage is above the battery voltage. The boost inductor L1 acts as an input filter.
  - **Transition mode:** a small band around the battery voltage where the converter runs at unity gain for a smooth changeover.
- Overall duty cycle: `d_charger = d_buck * (1 - d_boost)`

## Control Strategy

The controller has a common outer loop and separate inner current loops for boost and buck operation.

- **Outer loops:** a CC (battery current) loop and a CV (battery voltage) loop generate the reference for the inner loop.
- **Inner loops:** distinct boost and buck current controllers regulate the input current and keep the power factor at unity.
- **Active harmonic minimization (AHM):** the second-order power ripple on the output capacitor puts a third-harmonic component in the reference current. A band-stop filter removes it, with unity gain away from the corner frequency:

  `G_AHM(s) = (s^2 + (2*pi*fc)^2) / (s^2 + 2*zeta*(2*pi*fc)*s + (2*pi*fc)^2)`

  with the corner frequency fc set to 150 Hz (50 Hz supply) or 180 Hz (60 Hz supply).
- **Charging method:** CC-CV, with CC charging independent of the battery voltage and CV charging used across the 48-450 V range.

### PI controller transfer functions

| Controller | Transfer function |
|---|---|
| Outer current (CC) | 0.81 + 5.67 / s |
| Outer voltage (CV) | 0.257 + 41.1134 / s |
| Inner boost current | 0.06578 + 83.3184 / s |
| Inner buck current | 0.357 + 456.74 / s |

## Simulation

Simulation conditions follow the reference paper: universal AC supply (85-264 V RMS, 50/60 Hz), with a 110 V, 60 Hz input for the plotted cases.

| Case | Description |
|---|---|
| CC charging | Battery charged at a constant 10 A |
| CV charging | Battery held at a constant 400 V |
| Wide output range | Output stepped across the 48-450 V range to check regulation and mode transitions |

In each case the signals to check are the input voltage, input current, output (battery) voltage and output current. The input current should be sinusoidal and in phase with the input voltage (unity power factor), and the output should stay regulated at its reference.

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

## Reference

H. Karneddi and D. Ronanki, "Universal Bridgeless Nonisolated Battery Charger With Wide-Output Voltage Range," *IEEE Transactions on Power Electronics*, vol. 38, no. 3, pp. 2816-2820, March 2023.

## License

This repository is shared for academic and educational purposes. Please credit the authors and the reference paper if you use or build on this work.
