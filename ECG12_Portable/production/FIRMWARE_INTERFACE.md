# Firmware integration contract — ECG12 Portable2 Rev B

This is a hardware interface/acceptance specification, not tested firmware.

| Function | nRF52832 GPIO / module pin |
|---|---|
| AFE CS, SCK, MOSI | P0.11/25, P0.12/26, P0.13/27 via 33 ohm series resistors |
| AFE MISO, DRDY | P0.14/28, P0.15/29 |
| AFE RESET, PWDN | P0.16/30, P0.17/31 |
| USB PGOOD, charge status | P0.25/2, P0.26/3; active low, external 10k pull-ups |
| Battery ADC | P0.31/8; 1M/1M divider, 10nF filter; Vbat=2×Vadc |
| Status LED | P0.22/38, active-low sink through fitted resistor/LED |
| Reset, SWDCLK, SWDIO | P0.21/35, dedicated pins 36 and 37 |
| UART TX/RX | P0.06/19, P0.07/20 |

TP1..TP8: DVDD, GND, SWDCLK, SWDIO, MCU_RESET, UART_TX, UART_RX, AFE_CLK.

TP9..TP14 (back side): VBAT, VSYS, AVDD, VREF, RLD_REF, RLD_OUT. Use high-impedance, low-capacitance probes on analog nodes.

Rev B charging: USB100 is a 100 mA total input limit. RISET=8.66 kΩ programs 102.77 mA nominal; USB100 default termination is about 3.39 mA (3.3%). RTMR=68.1 kΩ gives a nominal 9.08 h fast-charge safety timer; static factor/resistor range 6.742–11.464 h, with dynamic timer behavior to be verified.

1. Use the module in LDO mode. External DCC network is absent. Configure the calibrated internal low-frequency RC oscillator; the module's shipped factory test firmware must be replaced. Its internal 32 MHz oscillator and DEC4 bypass are already fitted.
2. Configure ADS1298 for low-power 500 SPS, internal clock, 2.4 V internal reference, gain 6 and eight active normal-input channels. Verify readback of all registers. START pin is low: issue SPI START/STOP commands. Read the complete 27-byte status+sample frame per DRDY. Use approximately 1 MHz SPI initially.
3. WCT1=0x09 and WCT2=0xC2 enable RA=CH1N, LA=CH1P, LL=CH2P. CH3–8 negative inputs are physically tied to WCT. Internal augmented-lead switches remain disabled. Derive III/aVR/aVL/aVF digitally.
4. RLD uses external half-supply reference and external compensation. Select limb electrodes without repeatedly counting shared RA. Start with lead-off disabled for noise/CMRR characterization; enable only after verifying currents and thresholds on the simulator.
5. U1 PACE/test pins 17/18 are tied to AVDD; keep their output drivers disabled. GPIO1–4 are tied to GND and must remain inputs. DAISY_IN and RESV1 are grounded.
6. Stop acquisition whenever USB_PGOOD is low. D24 provides a hardware pull-down path to AFE_PWDN; it does not provide galvanic isolation. Use high-impedance MCU behavior compatible with this clamp rather than actively driving PWDN high against it. Disconnect electrodes for USB/debug operations.
7. Reserve 32 KiB for a bounded ring buffer. Budget a 32-byte application record (24 sample bytes, 3 status bytes, timestamp/flags). Flush every 250 ms. Verify BLE stack RAM, stack/heap headroom and linker placement on the actual 64 KiB device.
8. Negotiate suitable MTU/data length; require measured application throughput >=200 kbit/s. Timestamp and sequence every batch, account for CRC/protocol overhead, reject stale data, report sequence gaps. Under the validated connected-link test condition, maximum delivered sample age must stay <=1 s. No such guarantee is possible during disconnection.
9. Observe battery voltage, stop around 3.3 V under load with hysteresis, and enter low power. Test cell recovery, reconnect, brownout and interrupted-transfer behavior.
10. Verify at least 8 hours runtime with full-rate ECG, worst intended BLE use, chosen cell, representative aging/temperature and final enclosure.
