# Hardware projects 2027

Eight projects to build from 2027 on. They're ordered so each one reuses the tools and skills of the one before. Prices are US retail, checked in November 2026. Use them to size the budget, then confirm each price before ordering.

| # | Project | Main new skill | Big new purchase |
|---|---|---|---|
| 1 | Analog guitar pedal | Soldering, transistor biasing, enclosures | Shared bench core |
| 2 | Class D amplifier | Switching power, LC filters, audio measurement | Audio interface, dummy load |
| 3 | Geometric RF filter | Microstrip design, EM simulation, S-parameters | VNA |
| 4 | High-current BLDC controller | FOC (field-oriented control), gate drive, thermal | High-power bench PSU, 3D printer |
| 5 | Balancing double pendulum | Modelling, LQR (a standard state-feedback controller), real-time control | Rail and encoders (reuses #4 driver) |
| 6 | Flight controller | IMU fusion, ESC protocols, embedded firmware | Quad kit, LiPos, radio |
| 7 | Small CNC | Steppers, motion firmware, CAM | 3018-class machine |
| 8 | Autonomous drone missions (Nectar SDK) | PX4 SITL, ROS 2, Gazebo, vision (ArUco, YOLO), OpenTelemetry | None to start (Crazyflie optional later) |

The CNC can move earlier. Its lasting value is making enclosures, mounts, heatsinks and the pendulum frame, more than PCBs. Project 8 needs no hardware to start — it runs in simulation via Docker and can happen anytime.

## Buying from Brazil

- **Under US$50 on a Remessa Conforme platform** (e.g. AliExpress): 0% federal import tax since May 2026. State ICMS (17–20%) still applies. Split small parts into orders under this limit.
- **Above US$50:** sources disagree on whether the import tax is 60% minus US$30 or 30%. Check Receita Federal before any big order. Instruments land at roughly 1.5–1.9× their US price, so compare against Mercado Livre stock already in Brazil.
- **Buy locally:** LiPo batteries (AliExpress often won't ship them), the 3D printer (Bambu A1 mini is sold locally), aluminium extrusion (heavy to ship), and MDF or copper-clad board.
- **Mains is 220 V** in Florianópolis. Pick 220 V variants (e.g. a VFD spindle).

## Shared bench

Buy once. Every project uses it.

| Item | Why | Pick | ~USD | When |
|---|---|---|---|---|
| Oscilloscope | Class D switching, gate drive, PWM, bus decoding | Rigol DHO804 (70 MHz, 4 ch, 12-bit) or Siglent SDS804X HD | 400–460 | Project 1 |
| Multimeter | Bias, continuity, supply checks | UNI-T UT61E+ | 50–70 | Project 1 |
| Soldering iron | Through-hole and SMD | Pinecil V2 + 65 W USB-C PD charger. Add a T12 station later for XT60 and thick wire | 26–40 | Project 1 |
| Bench power supply | Current limiting is the main safety feature. BLDC work needs 48–60 V / 15–20 A | Riden RD6018 + AC-DC brick + case. It can't sink current, so add a brake resistor for motor regen | 150–300 | Project 1 (one PSU for everything) |
| Logic analyzer | SPI/I2C/UART for IMU and encoder debugging | 24 MHz 8-ch clone + PulseView | ~10 | Project 4 |
| 3D printer | Motor mounts, test stands, encoder holders, pendulum arms, enclosures | Bambu Lab A1 mini (bought locally, ~R$2,100) | ~400 | Project 4 |
| Consumables | Solder, flux, wick, tweezers, fume fan, breadboard, XT60/XT30, silicone wire, heat-shrink, fuses | — | ~60 | Project 1 |
| Thermal camera | Hot FETs, shunts, joints | InfiRay P2 Pro or Topdon TC001 | 175–300 | Later (before pushing BLDC past 20 A) |
| Function generator | Test signals | FeelTech FY6900, or a Rigol DHO914S scope with one built in | 80–100 | Later (an audio interface + REW covers audio) |

**Safety rule for the whole bench:** the scope's ground clip is tied to mains earth. Never clip it onto a bridged amp output, a switch node, or a high-side point. Use two channels with A−B math, a battery-floated circuit, or a differential probe.

## 1. Analog guitar pedal

Lowest risk (9 V), so it's the best place to learn soldering and debugging.

- **First kit:** a Fuzz Face-style kit (StompBoxParts, ~$55) or a simple boost. Next, bare PCBs from PedalPCB or Aion FX, with parts from Tayda.
- **Enclosure:** Hammond 1590B or 125B (Tayda drills them). Footswitch, jacks and pots are about $15 per pedal.
- **Test:** a 9 V center-negative supply, an amp or headphone amp, and a DIY audio probe (a capacitor into an amp).
- **Software:** LTspice, with ElectroSmash's circuit analyses as a reference, and KiCad. Avoid stripboard for the first build, because it's a common cause of pedals that don't work.
- **Protection:** add a reverse-polarity diode.

## 2. Class D amplifier

Its output is a fast square wave. Measuring it like a normal amp gives wrong numbers or damages gear.

- **Path:** a TPA3116D2 board (cheap, 24 V) to learn. Then a 3E Audio TPA3255 PFFB module (~$65) as the reference. Then your own PCB following the TI EVM layout guidance.
- **Supply:** a closed-frame 48 V supply (e.g. Mean Well LRS-350-48, ~$40) with a fuse.
- **Test:**
  - Dummy load: 2× 4–8 Ω aluminium-clad resistors, 100–300 W, on a heatsink.
  - USB audio interface (Focusrite Scarlett Solo, ~$140) with REW.
  - A DIY passive measurement filter: an LC low-pass copied from the Audio Precision AUX-0025, flat to 20 kHz. Without it, distortion readings are meaningless.
- **Software:** LTspice (TI publishes TPA3xxx models), KiCad, REW.
- **PCB:** at JLCPCB, 2 oz copper for the high-current paths.
- **Safety:** both speaker terminals are live (the output is bridged, or BTL). Enclose the mains side and earth the chassis.

## 3. Geometric RF filter

At RF, the copper shape is the component, and a scope can't show a 2 GHz response. A VNA (vector network analyzer) is the core tool.

- **VNA:** LiteVNA-64 (50 kHz–6.3 GHz, ~$150–210, from the official store; clones are common). The NanoVNA-H4 stops at 1.5 GHz, below where hairpin and interdigital filters usually sit.
- **Accessories:** the bundled calibration kit, two good SMA cables, through adapters, and SMA edge-launch connectors (~$1 each).
- **Software:**
  - Qucs-S 25.1+: filter synthesis and microstrip models.
  - openEMS: free 3D EM solver, scriptable in Python.
  - Sonnet Lite: free planar EM solver, with a size cap.
  - KiCad for layout.
  - scikit-rf: compare measured and simulated `.s2p` files in Python.
- **Order of builds:** a 5th-order stepped-impedance lowpass at 1–2 GHz first, because it's the easiest. Then a hairpin bandpass.
- **PCB:**
  - Start on plain FR-4 at JLCPCB, using their impedance calculator. FR-4's dielectric constant shifts the response, which is fine for learning up to about 3 GHz.
  - Move to Rogers RO4350B later, when you want simulation and measurement to match closely.
  - Controlled impedance at JLCPCB needs 4+ layers.
- **Later:** tinySA Ultra (spectrum analyzer; also useful for class D EMI).

## 4. High-current BLDC controller

Buy a working low-current board, then a working high-current one, then design your own. Each step adds one new risk.

- **Learning board:** ST B-G431B-ESC1 (~$20–40, works with SimpleFOC and ST MCSDK). Or SimpleFOC Mini for gimbal motors, which also suits the pendulum.
- **High-current reference:** Flipsky 75100 (VESC-based, ~$100). VESC hardware is open, so read its schematics before designing yours. ODrive S1 is the alternative, but it was sold out when checked.
- **Test motors:** Flipsky 6374 190KV (~$90) for current, and a GM4108-class gimbal motor for low current. Clamp them down.
- **Power:** the RD6018 during development (its current limit saves boards), with a brake resistor for regen. Use a fused 6S pack only for sustained tests above 18 A.
- **Measurement:** an inline DC wattmeter (~$20) first. Later, a Hantek CC-65 current clamp (it shows phase current, not PWM ripple).
- **Differential probe:** a Micsig DP750-100 (~€176) before any gate-drive debugging.
- **Load:** two motors back to back, with the second VESC in current-brake mode, plus a load cell on a printed lever arm for torque. Needed before claiming "tens of amps continuous".
- **Safety:**
  - An inline fuse above the test current, an anti-spark XT90-S connector, and a physical kill switch on the DC bus.
  - FETs usually fail short.
  - Bus capacitors stay charged after power-off.
  - Wear eye protection on first power-up.
- **Software:** VESC Tool, SimpleFOC Studio, STM32CubeIDE, LTspice for gate-drive and snubbers, KiCad.

## 5. Balancing double pendulum

Milestones: single pendulum, then swing-up, then double. A hobby double-pendulum build is hard to stabilise without good system identification and a 500 Hz–1 kHz loop.

- **Actuator:**
  - **Reaction-wheel pendulum:** the quickest win. It reuses the SimpleFOC hardware from #4 and has a ready reference project.
  - **Cart on a linear rail:** the classic double-pendulum platform. Use a BLDC-driven belt, because a stepper loses steps on fast reversals and gives no torque control.
- **Encoders:** AS5600 (I2C, ~$3) for a single pendulum. AS5048A (SPI, faster) or an optical quadrature encoder for the double. The middle joint needs a slip ring or a cable that can twist.
- **Mechanics:**
  - ~1 m of 2020/2040 V-slot extrusion or an MGN12 rail (buy locally).
  - GT2 belt, pulleys and an idler.
  - 608 bearings and 8 mm shaft.
  - Limit switches and end bumpers.
- **MCU:** a Nucleo-G431/F446 or Teensy 4.x with hardware quadrature timers and a fast serial link for logging.
- **Software:**
  - Derive the model in SymPy.
  - Design the controller with `python-control` (`lqr`/`dlqr`) in SciPy.
  - CasADi or do-mpc for MPC and swing-up.
  - MuJoCo for simulation checks, PlotJuggler for telemetry.
  - Write the host-side logger in Go.
- **References:** the SimpleFOC reaction-wheel pendulum, the PythonRobotics inverted-pendulum module, and the Hackaday "double pendulum cart".

## 6. Flight controller

Fly a stock Betaflight board first. Then run your own firmware on that same board, then design your own PCB. The known-good board separates firmware bugs from hardware faults.

- **Stack:** SpeedyBee F405 V4 (STM32F405 + ICM-42688-P IMU + 55 A 4-in-1 ESC, ~$70–86). F405 is the best-documented target. Skip RP2040 for a real FC, because its timer/DMA support for DShot is thin.
- **Airframe:** a 5" carbon frame, 4× 2207 ~1950KV motors, and several sets of 5" props.
- **Power:**
  - 3–4× 4S 1300–1500 mAh LiPos, bought on Mercado Livre.
  - A ToolkitRC M7 charger (runs from the RD6018) or an ISDT 608AC.
- **Radio:** RadioMaster Pocket ELRS + an RP1/RP2 receiver. ELRS uses the CRSF protocol over UART, which is easy to parse in your own firmware. Hobby radios formally need ANATEL approval.
- **Debug:** ST-Link V3MINIE (~€10) or a Raspberry Pi Debug Probe (~$12). Commercial FCs have tiny SWD pads, so expect fine soldering.
- **Testing:**
  - Props off on the bench, always.
  - A tether or gimbal rig for prop-on tests.
  - A smoke stopper on first power-up.
  - A DIY HX711 load-cell thrust stand later.
- **Firmware references:** madflight, dRehmFlight, PinkFluffyUnic0rn/stm32flight. Use Betaflight and INAV for production behaviour.
- **Toolchain:** STM32CubeIDE, or Rust with Embassy and probe-rs. Betaflight Blackbox Explorer for logs.
- **LiPo safety:**
  - Charge only in a LiPo bag or ammo can, never unattended.
  - Storage-charge to 3.8 V/cell.
  - Discard puffed packs.
  - Add an arm switch and failsafe before your firmware's first prop-on test.

## 7. Small CNC

One cheap machine mills PCBs well and aluminium badly. Rigid aluminium machines cost 3–5× more, so plan for two machines over time.

- **Phase 1, the PCB machine (~$350–450 landed):**
  - Machine: a 3018-class machine with limit switches, a probe and an E-stop. Genmitsu 3018-PROVer V2 (~$250) or a local Mercado Livre 3018 Pro (~R$1,100–1,500).
  - Tooling: 20° 0.1 mm V-bits, 0.8/1.0 mm drills, 1/8" end mills.
  - Material: FR1 copper-clad (kinder to bits than FR4), an MDF spoilboard, double-sided tape.
- **Phase 2, the firmware work (~$80–150):**
  - Replace the stock GRBL board with an ESP32 FluidNC board (6-Pack, Jackpot, PiBot) and TMC2209 drivers.
  - Or design your own FluidNC controller in KiCad.
  - Then upgrade to a 500 W ER11 spindle under firmware PWM, and try grblHAL on the same hardware.
- **Phase 3, the aluminium machine (~$700–1,500):**
  - Build a Root 4 Lite (smaller, ballscrews) or a PrintNC (steel tube, the stiffest hobby option).
  - Use TMC5160 or external drivers, 48 V, and a 1.5 kW 220 V VFD spindle over Modbus.
  - Keep the 3018 for PCBs.
  - Skip the LowRider: it cuts sheet goods, which none of these projects need.
- **Software:**
  - FreeCAD (grbl post-processor) or Fusion Personal for CAD/CAM.
  - KiCad → pcb2gcode (scriptable, Makefile-friendly) or FlatCAM for PCB milling.
  - bCNC or UGS for PCB auto-levelling.
- **Workshop:**
  - A shop vac at the cut and a PFF2 mask when milling FR4, because fibreglass dust irritates the lungs.
  - Safety glasses and earmuffs.
  - An enclosure early if you live in an apartment.
- **Mill or order?**
  - Mill single-sided through-hole boards and same-day RF test coupons.
  - Order from JLCPCB ($2 for 5) for fine-pitch, multi-layer, high-current or keep-forever boards: the flight controller, BLDC, final amp and final RF filter.

## 8. Autonomous drone missions (Nectar SDK)

Starts with zero hardware, prioritizing the Linux Foundation / Dronecode stack (**PX4**, **Pixhawk**, **MAVLink**, **MAVSDK**, **QGroundControl**, **Zephyr**, **OpenTelemetry**) paired with ROS 2, Gazebo and [Black Bee Drones' Nectar SDK](https://black-bee-drones.github.io/nectar-sdk/).

Full stack choices, sim-vs-hardware test boundaries, phased plan and Prof. Bruno contribution notes live in [autonomous-drone/README.md](./autonomous-drone/README.md).

## Budget (US prices, before Brazilian tax)

| Bucket | Must-have to start |
|---|---|
| Shared bench (scope, PSU, multimeter, iron, consumables) | ~$700–900 |
| 3D printer + logic analyzer (from project 4) | ~$400 |
| Pedal | ~$75 (assumes you have an amp) |
| Class D | ~$100 + ~$140 audio interface |
| RF filter | ~$250 |
| BLDC | ~$230–300 |
| Pendulum | ~$80–150 |
| Flight controller | ~$350–450 |
| CNC phase 1 | ~$350–450 landed |
| Autonomous drone missions (Nectar SDK) | $0 in simulation (Crazyflie optional later) |
| Later (diff probe, current clamp, thermal camera, dyno, CNC phases 2–3) | ~$1,300–2,300 |

Expect the instruments to cost about 1.6–1.9× once Brazilian taxes are paid.

## Still to verify before buying

- The import tax above US$50 and the ICMS rate in Santa Catarina.
- Mercado Livre prices in R$ for the scope, PSU and CNC.
- Whether JLCPCB prepays tax for Brazil, and its shipping cost and time.
- Current stock: ODrive S1 was sold out, and the Micsig DP10013 is discontinued at one retailer.
