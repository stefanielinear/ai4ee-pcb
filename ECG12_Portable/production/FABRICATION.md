# ECG12 Portable2 Rev B - Fabrication and assembly

Engineering prototype. Native DRC: 0 violations, 0 unconnected, 0 schematic parity differences. Perform bare-board opens/shorts testing with bare_board_test.d356.

- Outline 45.00 × 45.00 mm; nominal finished thickness 1.0 mm; ENIG. Four layers: F.Cu / In1.Cu / In2.Cu / B.Cu. Outer Cu 35 µm, inner Cu 18 µm.
- Nominal stack in mm: mask 0.010 / Cu 0.035 / prepreg 0.110 / Cu 0.018 / core 0.654 / Cu 0.018 / prepreg 0.110 / Cu 0.035 / mask 0.010. Review any factory equivalent before fabrication.
- Minimum track/clearance 0.15/0.15 mm; via land/drill 0.60/0.30 mm; copper-to-edge ≥0.25 mm. No blind or buried vias.
- **28 vias require resin filling, planarization and copper capping.** Use filled_capped_vias.csv and native flags; do not substitute open holes. This includes all four GND thermal vias inside U3's 1.6 × 1.6 mm exposed pad. Other vias are tented on both sides.
- Datum: lower-left board corner, X right/Y up in top view. Gerber, drill, position and fiducial CSV share this datum. Bottom positions remain top-view coordinates; translate rotation to machine convention. Assembly_back.pdf is mirrored for bottom-side viewing.
- BOM.csv has 110 purchased/placed parts: 100 front, 10 back. BOM_ALL.csv additionally contains 14 PCB test contacts. Six board-only optical fiducials and all test contacts are excluded from placement purchase operations. See fiducial_coordinates.csv: three non-collinear marks per side, 1 mm copper, 2 mm mask opening. No solder paste on fiducials or test pads.
- J2 TYPE-C-31-M-12 locating-hole centerline is 5.79 mm from the lower PCB edge. Preserve the manufacturer's hole/slot geometry. Verify the actual connector, plug and enclosure mating before production release. The 3D J2 envelope is illustrative.
- U1 TQFP-64 has no exposed pad. U3 QFN has segmented paste and four filled/capped ground vias. Use supplied front/back paste Gerbers and qualify stencil/reflow for the module, 0402, TQFP, QFN and SOT-23 packages.
- D11–D19 BAV199: pin1 anode/GND, pin2 cathode/AVDD, pin3 junction/signal. D21 is on the back in Rev B. Check USB/JST orientation and all pin1/polarity indicators against fabrication and assembly drawings.
- Retain 0.1% input resistors and **1% C0G C1–C9**. C32/C33 are **22 µF/25 V TDK C2012X5R1E226M125AC**, not the examined 16 V candidate. C31/C34–C37 are 4.7 µF/16 V TDK C1608X5R1C475K080AC. Alternative MLCCs require effective-capacitance validation at bias/temperature.
- R38=8.66 kΩ 1%; R39=68.1 kΩ. USB100 limits total input nominally to 100 mA; 102.77 mA is the nominal current programming value, not a guarantee of actual charging current. Default USB100 termination is about 3.39 mA; nominal timer is 9.08 h.
- No additional copper or metal in the RF keepout on any layer. Keep enclosure, cell and harness clear of the module antenna. Main board has no external RF feedline.
- J3: pin1 VBAT, pin2 GND, pin3 NTC, pin4 NTC return. The protected cell, cell-bonded 10k NTC, patient harness and insulating carrier are external assemblies listed separately. Verify every harness polarity and continuity.
- Never reflow an attached battery or electrode harness. Initial test uses a current-limited supply and ECG simulator. Disconnect electrodes before USB or grounded programming. This board includes no medical USB isolation or defibrillator protection.

Read REV_B_OPTIMIZATION_REPORT.md and DESIGN_REPORT.md for the remaining prototype tests. KiCad 3D package approximations are not controlling mechanical drawings.
