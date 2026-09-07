# ECG12 Portable2 — Revision B design specification

Status: routed engineering prototype; native ERC has 0 errors/6 reviewed warnings, DRC has 0 violations/0 unconnected/0 parity differences. System acceptance requires bench verification. Not medically certified.

## Requirements and decisions

- Ten electrodes: RA, LA, LL, V1–V6 and RL. ADS1298IPAG measures I=LA−RA, II=LL−RA and six chest leads relative to its internal Wilson central terminal. Firmware derives III=II−I, aVR=−(I+II)/2, aVL=I−II/2, aVF=II−I/2.
- 500 samples/s/channel, simultaneous 24-bit acquisition; low-power converter mode, internal oscillator. Nominal 0.5–40 Hz monitoring display, with raw samples retained. The converter sinc response is about -3 dB at 131 Hz at 500 SPS; a flat 150 Hz diagnostic bandwidth is not claimed. A 1 kSPS mode requires a revised data/power budget.
- 3.0 V analog supply, 2.4 V internal reference, nominal PGA gain 6: differential ADC full-scale ±400 mV, subject to input common-mode headroom. Electrode DC offset and saturation recovery require prototype verification.
- Input impedance target ≥10 MΩ at 10 Hz; CMRR target ≥80 dB at 50/60 Hz with RLD (system targets, not proven results). Matched input RC filtering, low-leakage clamps, current limiting and internal lead-off detection. Internal RLD amplifier, compensated feedback, two series patient-bias resistors. Component/parasitic capacitance matching must be characterized on prototypes.
- RF module with nRF52832, 64 KiB RAM, 512 KiB flash, integrated high-frequency crystal, matching and chip antenna. Internal low-frequency RC clock with calibration. No external data flash.
- 8×500×24=96 kbit/s raw ECG; including 24-bit status each frame gives 13,500 byte/s. Allocate 32 KiB circular acquisition buffer. Packetize every 250 ms, require ≥200 kbit/s measured application payload throughput. A 32-byte application record plus 10% overhead gives 4,400 bytes per 250 ms batch, draining in 176 ms at that service rate; nominal oldest-sample delivery is 426 ms plus connection/retry delays. ≤1 s is an acceptance target under a specified compatible BLE central; outage cannot guarantee latency.
- 1S protected rechargeable Li-polymer battery, minimum 500 mAh, 10 kΩ NTC coupled to cell. USB-C is 5 V charging only, two independent 5.1 kΩ Rd resistors, data pins unconnected. Charger with power-path and cell temperature monitoring; conservative default USB current limit.
- Independent low-noise analog 3.0 V and digital 3.0 V LDOs. Battery low-voltage stop target 3.3 V; usable-capacity derating accounts for cutoff, temperature and aging.
- Preliminary battery-current budget: AFE 4 mA, MCU 3 mA, BLE 4 mA average, regulators 0.1 mA, miscellaneous 1 mA =12.1 mA. Use 20 mA engineering design envelope; 500 mAh×0.70/20 mA=17.5 h estimated runtime, over 2× eight-hour requirement. Must be measured with final firmware, cell and RF link.
- Board 45×45 mm maximum, four copper layers: front components/signals, continuous inner GND, inner power/slow signals, back signals/GND. 1.0 mm nominal stack, 35 μm outer copper; fabricator must confirm final dielectric stack.
- Initial routing limits: 0.15 mm trace/clearance, 0.6/0.3 mm through vias; power wider where space permits. Inner GND reserved for return continuity. No copper, vias or traces in antenna keepout on any layer.
- Interfaces: keyed ten-electrode connector, keyed battery/NTC connector, USB-C, SWD programming/test pads, status LED and battery voltage measurement.
- Charging is a non-patient-connected mode: no galvanic USB isolation is included. Enclosure/workflow must prevent patient connection while USB or a grounded programmer is attached. Hardware is not defibrillator protected or suitable for electrosurgery. Simulator-only testing precedes any patient use.

## Acceptance evidence required

Editable local KiCad symbols/footprints; native schematic-to-board net correspondence; complete routing; filled planes; native ERC and DRC reports; BOM/positions/Gerber/drill exports; schematic and fabrication/assembly PDFs; explicit remaining prototype tests for noise, CMRR, lead-off, RLD stability, antenna, ESD/EMC, charging thermal behavior and runtime.

## Revision B implementation

Three fiducials per side; 14 test contacts including six new analog/power nodes; USB locating-hole-to-edge datum 5.79 mm; 8.66 kΩ RISET; 9.08 h nominal timer; 3.3% USB100 termination; 1% C0G input capacitors; upgraded MLCCs with documented bias margins; local RLD and source-SPI routing; four filled/capped charger EP vias. See REV_B_OPTIMIZATION_REPORT.md for measurements and remaining acceptance work.
