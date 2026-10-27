# NASA messages used here

Read from the F1/F2 bus with `custom_sensor` (indoor unit = address `20.00.00`, outdoor = `10.00.00`).
Checked on an AE260RNWSGG (EHS Mono ClimateHub, 9 kW).

| Message | Unit | Entity | Scale | Notes |
|---|---|---|---|---|
| `0x4235` | indoor | DHW Target Temperature | × 0.1 °C | |
| `0x4237` | indoor | DHW Current Temperature | × 0.1 °C | |
| `0x4087` | indoor | DHW Booster | 0/1 | 1 while the electric booster heater runs |
| `0x4238` | indoor | Water Flow Temperature | × 0.1 °C | leaving the heat pump |
| `0x4236` | indoor | Water Return Temperature | × 0.1 °C | coming back |
| `0x4067` | indoor | 3-Way Valve | 0/1 | **0 = space heating, 1 = DHW tank** (flow was 51-55 °C while it read 1) |
| `0x402E` | indoor | Defrost State | 0/… | non-zero during defrost |
| `0x42E9` | indoor | Water Flow Rate | × 0.1 L/min | ~20 L/min on this install |
| `0x4426` | indoor | Heat Output Last Minute | W | matched flow × ΔT: 20.4 L/min × 4.9 K ≈ 6.97 kW vs 6.86 kW reported |
| `0x4427` | indoor | Heat Output Total | Wh | lifetime; +7.5 kWh over 54 min at ~8.4 kW average |
| `0x8238` | outdoor | Compressor Frequency | Hz | |

Built-in sensors of the component that are also used: outdoor instantaneous power (W) and cumulative
energy (kWh), DHW mode select (Eco / Standard / Power / Force), water outlet target (space-heating
water setpoint).

## DHW modes and the booster

There is no direct switch for the electric booster heater. The DHW mode decides when it may run:

| Mode | Booster |
|---|---|
| Eco | heat pump only (booster only if the heat pump cannot run at all) |
| Standard | heat pump first, booster after the controller's delay (about 20 min here) |
| Power / Force | booster together with the heat pump straight away |

So starting DHW earlier does **not** avoid the booster in Standard mode; Eco does.
