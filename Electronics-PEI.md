### Based on practical Electronics for Inventors

For 90% of *Practical Electronics for Inventors*, you need DC measurements, audio-frequency waveforms, and slow digital signals. A ₹12,000 scope is not required for that.

**One hard rule: stay at or below 24 V DC for the first 6 months. No mains projects.** India’s 230 V plus a ₹1,000 meter is how people get hurt. Read the mains chapters; don’t build them yet.

---

## 1. The ₹10,000 bill of materials

Prices are indicative Indian retail (Robu, Sunrom, ThinkRobotics, ElectronicsComp, Zbotic, Amazon, local market). Buying loose components from Lamington Road/SP Road/Lajpat Rai/Ritchie Street can cut 30–50%.

| Category | What to buy | ₹ |
|---|---|---:|
| **Meter** | Aneng AN8008 (features) or UNI-T UT33C+ (brand/safety) | 1,100 |
| **Soldering** | Soldron 25 W iron, stand, 63/37 solder 50 g, flux, wick, desoldering pump | 1,000 |
| **Hand tools** | Side cutters, long-nose pliers, tweezers, screwdriver set, safety glasses | 550 |
| **Power** | 12 V 2 A sealed adapter, 2× LM2596 buck modules, LM317 + passives, fuse holders + 1 A fuses, terminal blocks | 650 |
| **Prototyping** | 2× 830-point breadboards, jumper wires, 5× perfboards, headers, heatshrink | 600 |
| **Instruments** | 8-channel USB logic analyzer (24 MHz clone), USB power meter | 700 |
| **Components** | Resistor kit, capacitor kit, LED/diode/transistor kit, IC pack, pots, sensors, motors, modules | 3,200 |
| **Bench** | Storage boxes, silicone mat/cardboard, labels, pegboard | 400 |
| **Reserve** | Shipping, spare fuses, mistakes, one spare ESP32 later | 1,800 |
| **Total** | | **10,000** |

**Components breakdown (₹3,200):**
- Resistors: 1% metal film, E12, 10 Ω–1 MΩ — ₹300
- Ceramic + electrolytic capacitors, 25 V — ₹400
- LEDs, 1N4148, 1N4007, zeners, BC547, 2N2222, 2N3906, 2N7000, IRLZ44N — ₹500
- ICs: NE555 ×3, LM358, LM324, TL072, LM386, 7805, LM317, 74HC00, 74HC14, 74HC595, CD4017, ULN2003 — ₹650
- 10k/100k pots and trimmers — ₹150
- LDR, NTC thermistor, DHT11, HC-SR04, PIR, 0.96" I²C OLED — ₹600
- SG90 servo, 28BYJ-48 + ULN2003, small DC motor — ₹350
- Relay module, TP4056 with protection, 4-channel level shifter — ₹250

If you already have basic components, move that money to a **spare ESP32 DevKit (₹400)** and an **AD9833 function-generator module (₹450)**.

---

## 2. The bench (“homelab”)

Your table, left to right:

- **Left/rear:** power strip, 12 V adapter, DIY bench supply. Keep mains cables at the back edge, never across the work area.
- **Center:** breadboard on a silicone mat or thick cardboard. DMM in front.
- **Right:** laptop, logic analyzer, sound-card probe box.
- **Back:** pegboard/tools, storage boxes labeled by type.
- **Front:** paper lab notebook. Always.

Add a bright desk lamp — shadows cause wiring errors. Keep a metal tin for hot parts, and tape a sign to the table: **“UNPLUG BEFORE REWIRING.”**

---

## 3. Turn the laptop into the instrument rack

### Software (all free)
- **Falstad CircuitJS** — browser circuit simulator; use it before every build.
- **KiCad** — schematic + PCB + ngspice.
- **PulseView/sigrok** — logic analyzer.
- **xoscope** or a Python `sounddevice` + `matplotlib` script — sound-card scope.
- **Arduino IDE 2 / PlatformIO + esptool** — ESP32.
- **Wokwi** — ESP32 simulator when parts are missing.
- **Logisim Evolution** — digital logic.
- Python: `numpy`, `scipy`, `matplotlib`, `pyserial`.
- `picocom`/`minicom` — serial terminal.

### Three cheap “probes”

**A. Sound-card oscilloscope (20 Hz–20 kHz, AC only)**
Build a small box:
- Probe tip → 100 kΩ series → 10 kΩ to ground (≈10:1 divider, but your sound card’s ~10 kΩ input loads it, so calibrate).
- Then 1 µF/63 V coupling cap → 1 kΩ series → line-in.
- Two BAT85 diodes anti-parallel from line-in to ground as clamps.
- Verify with a DMM before plugging into the laptop. Run the laptop on battery while probing to avoid ground loops.

This is enough for filters, oscillators, audio, and RC/RL experiments.

**B. ESP32 oscilloscope / data logger (DC–~100 kHz)**
- Use **ADC1** pins only (GPIO32–39); ADC2 conflicts with Wi-Fi.
- Input divider: 100 kΩ / 20 kΩ (÷6) for 0–20 V, plus a 3.3 V zener clamp and 1 kΩ series resistor.
- ESP32 ADC is nonlinear; calibrate against the DMM.
- Stream over USB or Wi-Fi WebSocket and plot in Python/browser.

**C. ₹450 logic analyzer**
The best-value instrument in the list. 8 channels, 24 MHz, works with PulseView on Linux. It decodes UART, I²C, SPI — exactly what Chapters 11–12 need. This alone replaces a ₹4,000+ Saleae.

### What you can measure without a scope

| Need | Tool |
|---|---|
| DC voltage/current/resistance | DMM |
| AC RMS, frequency, duty cycle | DMM + sound card |
| Waveform 20 Hz–20 kHz | Sound-card scope |
| Waveform DC–100 kHz | ESP32 ADC scope |
| Digital timing, UART/I²C/SPI | ₹450 logic analyzer |
| Diode/transistor curves | ESP32 curve tracer |
| Noise/THD/spectrum | Sound card + Python FFT |
| Rise time <1 µs | Borrow a scope / simulate |

---

## 4. Book roadmap: 24 weeks at 8–10 h/week

Cadence per week: **3 h reading, 4 h building, 1 h simulation, 1 h notes.** If you have 15 h/week, compress to ~14 weeks.

| Phase | Weeks | Read | Build / measure |
|---|---|---|---|
| **0. Bench + Ohm’s law** | 1 | Ch 1 | DMM practice, LED + 330 Ω from 5 V, measure V/I, breadboard rails |
| **1. Theory + passives** | 2–4 | Ch 2–3 | Voltage dividers loaded/unloaded; RC charging logged with ESP32 (10 kΩ + 100 µF, τ = 1 s); RC filter Bode plot with sound card; small transformer/inductor experiments at low voltage |
| **2. Semiconductors + opto** | 5–8 | Ch 4–5 | Diode I–V curve with pot + DMM; half/full-wave rectifier + 100 µF, measure ripple; zener regulator; BC547 switch driving relay/LED; 2N7000 PWM LED; LDR/IR/7-segment |
| **3. IC basics + timers** | 9–11 | Ch 6, 9 | 555 astable at 1 Hz and 1 kHz (R1=10k, R2=47k, C=10µF ≈ 1.4 Hz); CD4017 LED chaser; relay driver with flyback diode; logic analyzer capture |
| **4. Op-amps, filters, power** | 12–14 | Ch 7–8, 10 | LM358 non-inverting gain 11; comparator with hysteresis; Sallen-Key low-pass, sweep with sound card; Wien-bridge oscillator; LM317 bench supply with load test |
| **5. Digital + MCU** | 15–17 | Ch 11–12 | 74HC gate truth tables; 74HC595 shift register; flip-flops/debounce; ESP32 GPIO/PWM/ADC/UART/I²C/SPI; capture and decode with PulseView |
| **6. Applied + capstone** | 18–24 | Motors, audio, sensors/robotics chapters (titles vary by edition) | Pick two: bench PSU enclosure, ESP32 weather station with OLED/web dashboard, line-following robot, LM386 amplifier, diode/transistor curve tracer |

### How to read the book without drowning
- **Three passes per chapter:** skim → build 2 circuits → read deeply → write a one-page summary.
- **Skip long derivations on pass one.** Learn the result, simulate it, come back for the math.
- **Simulate every schematic in Falstad before breadboarding.** Prediction before measurement is the whole game.
- **Keep a lab notebook:** date, schematic, predicted values, measured values, % error, what surprised you.
- **Datasheets beat the book for pinouts and limits.** The book has known errors; cross-check.
- **Don’t read cover-to-cover.** It’s a reference. The goal is: understand concepts, build from schematics, know where to look things up.

---

## 5. Build these instead of buying

| Readymade | Retail India | Your version | Cost |
|---|---:|---|---:|
| Bench power supply | ₹2,500–5,000 | 12 V adapter → fuse → LM2596 → panel meter → binding posts | ₹700 |
| Function generator | ₹2,000–4,000 | AD9833 + ESP32 + op-amp buffer | ₹700 |
| Oscilloscope | ₹12,000+ | Sound-card probe + ESP32 web scope | ₹300 |
| Logic analyzer | ₹4,000+ | 8-ch clone + PulseView | ₹450 |
| Component tester | ₹1,000–1,500 | LCR-T4 kit | ₹600 |
| Electronic load | ₹2,000+ | MOSFET + op-amp + 10 W resistor + heatsink | ₹500 |
| Fume extractor | ₹1,000+ | 120 mm PC fan + carbon filter | ₹300 |
| Audio signal tracer | ₹1,000+ | LM386 + probe | ₹150 |

**Build what teaches. Buy what protects you.** Buy sealed adapters, fuses, buck modules, and the DMM. Don’t DIY mains power supplies, Li-ion chargers, or anything where failure means fire.

---

## 6. Safety rules (non-negotiable)

1. **24 V DC ceiling.** No mains projects for 6 months. Use sealed adapters only.
2. **Fuse every bench rail.** 1 A inline fuse or polyfuse on the positive output.
3. **One power source per circuit, common ground.** Unplug before rewiring.
4. **Discharge big capacitors** with a 1 kΩ/2 W resistor before touching.
5. **Li-ion:** protected cells only, TP4056 with protection, never charge unattended, store in a LiPo bag.
6. **Soldering:** fan + open window, iron stand, safety glasses, wash hands, no food at the bench. Flux fumes are the concern; lead doesn’t vaporize at soldering temperatures.
7. **ESD:** touch grounded metal before handling ICs/MOSFETs; keep them in foam; avoid carpet and synthetic clothes.
8. **Eye protection** when clipping leads or desoldering.
9. **No liquids, no metal jewelry, no tired work.** If it smells hot, unplug first, then investigate.
10. **Never probe mains with a cheap DMM.** CAT ratings are real. If you ever must, use a CAT III meter, one-hand rule, and supervision.

**ESP32-specific traps:** 3.3 V only, not 5 V tolerant; use ADC1 with Wi-Fi; don’t power motors/servos from the 3.3 V pin; use a separate 5 V supply and common ground; avoid strapping pins (GPIO0/2/12/15) for buttons; use a level shifter for 5 V sensors; use logic-level MOSFETs like IRLZ44N, not IRF540.

---

## 7. First 7 days

- **Day 1:** Order the BOM. Set up the table. Install Falstad, KiCad, PulseView, Arduino IDE/PlatformIO.
- **Day 2:** DMM practice: AA/9 V batteries, resistors, continuity, diode test, your body resistance.
- **Day 3:** LED + 330 Ω from 5 V. Calculate 10 mA, measure it, measure the LED drop.
- **Day 4:** 10k/10k divider from 5 V; measure unloaded and loaded with 10 kΩ. Explain the difference.
- **Day 5:** Simulate the same divider in Falstad; compare.
- **Day 6:** Solder practice on a perfboard: resistor leads, header pins. Make a 5 V LED board.
- **Day 7:** Read Ch 1, write a one-page summary, plan week 2.

---

## 8. What not to buy

- A cheap DSO138/DSO150 as your first scope — limited and frustrating; use the laptop first.
- 37-in-1 sensor kits, “super starter kits,” and most IoT kits — 80% junk.
- A retail bench PSU, hot-air station, or 3D printer.
- Any mains-powered DIY kit.
- Unbranded Li-ion cells or chargers.
- Expensive “precision” meters — one decent handheld is enough for now.

**India-specific:** AliExpress no longer ships to India. Consolidate orders from 1–2 Indian stores to save shipping. Use a BIS-marked adapter and a good spike guard. In monsoon, store parts with silica gel. If you’re near a college or makerspace, borrow their scope for the 2–3 experiments that truly need one — that’s the ultimate budget hack.

---

The constraint is actually an advantage: people with scopes often stare at traces instead of understanding the circuit. You’ll be forced to predict, simulate, and measure — which is how you actually learn.

If you want, I can generate next: **(a)** a printable BOM/checklist with exact part numbers, **(b)** the Python sound-card oscilloscope script, **(c)** ESP32 scope/logger firmware, or **(d)** a week-by-week checklist with experiments and expected values.
