# Installer Work Order — Ride1Up REVV1 FS → 5,000W / 72V Conversion

**Prepared for:** installing technician / shop
**Vehicle:** Ride1Up REVV1 FS (moped-style e-bike, 20×4 / ISO 406 wheels)
**Job:** Replace the stock drivetrain with a 5,000W 3T hub motor on a 72V system, with
new controller, battery, high-current safety gear, torque arms, tires, and brake service.
**Date:** 2026-06-29 · **Updated:** 2026-09-11

> **Scope note:** This is a high-power aftermarket conversion. It voids the Ride1Up
> warranty, exceeds e-bike power limits (off-road or registered-vehicle use only), and
> involves 72V (84V peak) wiring. Qualified installation only.

---

## 1. Bill of materials (all owner-supplied, assumed on hand)

| # | Part | Key specs |
|---|------|-----------|
| 1 | Powerful Lithium **5,000W 3T hub motor** (complete rear wheel) | 20×4 rim · **6-bolt ISO** rotor mount · **16mm axle / 11mm flats** · Hall-sensor (2 sets) · tubed tire · **single-speed freewheel (pedal drive)** |
| 2 | **FarDriver 72450** controller (loaded) | 48–72V (20S), 200A line / 450A phase · incl. throttle, 3" display, enclosure (too large — **not used**; the controller bolts directly to the printed brackets), controller/power cable · **bundled throttle installed** (§5.2); its red button → ESP32 module → boost (plan D4) |
| 3 | **72V "Cadmus" battery** — 📦 on hand (received 2026-08-14) | 20S, Molicel P42A, 34Ah · smart BMS · **84.0V full charge** · mounts in Center Storage Cage · **main discharge connector type: TBD — record at install** (drives the B+/B− termination into the fuse/XT90-S line) |
| 4 | **72V (20S) charger** — 📦 on hand (received 2026-08-14) | adjustable; charge lead terminated to XT90-S |
| 5 | **Littelfuse `JLLN125` Class T fuse, 125A** (125 V DC · 20 kA @ 125 V DC) + **Blue Sea `5007100` Class T block** (160 V DC · 160 A max operating · four-stud bolt-down · ignition-protected **only with its cover secured** · 72 in-lb) — ⬜ **to order** (issue #10) | main-line fault protection. ⛔ **Do not fit the Blue Sea 5127 ANL 150A link + 5005 block that are in the parts box** — rated 80 V / 32 V DC, below the 84.0 V pack, and DC arcs do not self-extinguish. ⛔ **Never fit a 175/200 A link** — over the block's 160 A rating |
| 6 | **XT90-S** — main-line make/break **and** anti-spark (owner-supplied) | Pre-charge pin handles controller inrush · **the only** make/break point · ⚠️ ~90A cont. rating vs. the 80A cap — see §3 notes |
| 6b | **2-wire key switch** + **~2A inline fuse** | Gates the **FarDriver KEY wire only** — low-current logic, **not** main current |
| 6c | ⛔ **Flipsky FSESC 75200 Pro V2.0** — **NOT IN THIS BUILD** | A **VESC motor controller** mis-ordered as a switch (issue #3). **Do not install.** Leave boxed / return |
| 7 | **Grin V6 torque arms ×2** | clamp-mount · hardened 17-4 splined insert · 12/14/16mm axle |
| 8 | **Shinko 241** tires (pair) + 16" moto tubes | 3.00-16 (fits the 20×4/ISO-406 rim) · tubed |
| 9 | **AMASS XT90-S** connectors (5-pack) | for charger lead / battery-removal plug |
| 10 | **Brake service parts** | stock 203mm 6-bolt rotor **reused** · pads = **Shimano Saint D-type, sintered (D02S)** · fluid = **mineral oil** |
| 11 | 8 AWG silicone wire, heat-shrink, ring terminals (as needed) | main-lead extension / terminations |
| 11a | **Axle washers ×2** — ✅ fitted 2026-09-08 (issue #2) | Shop-made; ⚠️ **material/hardness unrecorded** (issue #12, open). If ever replaced: a **keyed torque washer** for a 16mm axle / 11mm flats (the stock washer is this type — D-hole + raised step into the dropout slot), or, on a flat dropout face, an **M16 hardened flat washer, DIN 433 / SAE narrow** (ID ~17 · OD 27–28mm · ~3mm). ⚠️ **Never ~30mm or ~34mm OD** — it sits cocked at the dropout. Axle takes a **double-nut** (thick = clamp, slim = jam) and has a cross-drilled tip for an R-clip |
| 11b | **6-bolt rotor ring shims, 0.2mm ×5** (M5 ID) + **M5×0.8 rotor bolts ~2mm longer** if the shims cost thread engagement | ✅ fitted 2026-09-08: **4 × 0.2mm = 0.80mm** (planned 5), rotor centred, rear brake works (issue #1 — the rear rotor sat ~1mm inboard of the caliper on the new hub). ⬜ Record whether the longer bolts were needed |
| 11c | **Brake circuit** — 1N4148 steering diodes ×6, P-FET `AO3407A`, 10 kΩ ×3, 100 nF, 1 A mini-blade fuse + holder | Each lever pulls the FarDriver `BL` low (motor cut), switches on the brake lamp (no firmware) and signals the ESP32 module — **circuit, parts and tests in `revv1-brake-circuit.md`** (§2, §3, §7). Built in three steps (§6 there); step 1 (motor cut) now. See §5.3 |
| 12 | Center Storage Cage | already fitted to bike (battery mount) |

---

## 2. ⚠️ Critical warnings — read before starting

1. **Torque arms are mandatory.** At 5kW the axle reaction torque can spin the axle in the
   dropout and shear the motor phase wires (dead short / wheel lock). Install **both** Grin
   V6 arms before any power test.
2. **High voltage:** the pack reaches **84V**. De-energize by **unplugging the XT90-S** (key
   OFF first) before any wiring work. **Anti-spark is the XT90-S's pre-charge pin** — it is the
   **only** make/break point; do **not** hot-plug the main battery anywhere else, and do **not**
   use the main fuse as the disconnect (no pre-charge there).
   - ⛔ **A Flipsky "FSESC 75200 Pro V2.0" may be in the parts box. It is NOT part of this
     build — do not install it.** It is a VESC motor controller (issue #3); its blue/green/yellow
     leads are **phase outputs**, and bolting them to the FarDriver's phase studs shorts two
     output stages together and destroys both units.
3. **Brake fluid is MINERAL OIL** (marked on the lever reservoir). Use bicycle mineral oil only;
   never DOT (it destroys the seals).
4. **Battery-current cap:** the FarDriver battery-current limit is set to **80A** (§6) — verify
   it, do not raise it. Sustained current above that overheats the hub motor.
5. **Controller voltage — in spec:** the ND72450 is a 20S/72V-class controller with max input
   ~88V, designed for an 84V-full pack. The Cadmus's 84.0V is its design voltage and the BMS caps
   there. Headroom is modest (~4V) but in spec — the battery vendor's matched pairing. **Do not
   overvolt beyond 20S** on this controller.
6. **The brake cutoff and brake light never depend on firmware.** Each lever pulls the FarDriver
   `BL` low and switches on the brake lamp through the brake circuit (`revv1-brake-circuit.md`);
   the ESP32 module only listens. Prove both with the lever tests (brake circuit §7) before
   riding. See §5.3.

---

## 3. Wiring architecture

```
                    ┌────── 125A CLASS T fuse ──────┐
   BATTERY (+) ●────┤ (JLLN125 + Blue Sea 5007100)  ├────●  XT90-S  ●──────┬──────● CONTROLLER B+
                    └───────────────────────────────┘     (anti-spark      │
                                                           pre-charge)     │
   BATTERY (−) ●───────────────────────────────────────────────────────────┼──────● CONTROLLER B−
                                                                           │
                          ┌── ~2A inline fuse ──● 2-wire KEY SWITCH ●──────┘
                          └──● ESP32 module supply
                             └──● START LATCH (run switch + start button, 2 s) ──● FarDriver "KEY" wire

   XT90-S       = the ONLY main-power make/break point (handles inrush via pre-charge)
   KEY SWITCH   = daily ON/OFF. LOW-CURRENT LOGIC ONLY — it does NOT switch main current.
                  Tap B+ downstream of the XT90-S, through the ~2A inline fuse, to KEY.

   MOTOR → CONTROLLER:  3× phase wires  +  6× sensor wires (5 hall + 1 motor-temp)
                        connectors MATE directly — motor has 2 hall pigtails, 2nd is a SPARE (cap it)
   CONTROLLER harness:  throttle, brake circuit → BL, display
   CHARGE PORT:         72V charger via XT90-S (battery side)
```

**Notes**
- Main leads (battery → fuse → XT90-S → controller) must carry the full current — use
  **8 AWG silicone minimum** if extending; keep runs short.
- Fuse value **125A** sits 1.56× over the 80A cap, so it won't nuisance-blow. Don't upsize: a 150A
  link never opens on a sustained sub-150A fault, which would leave the 8 AWG (~80–90A) and the
  XT90-S (~90A) unprotected (issue #10).
- ⚠️ **XT90-S current margin is thin.** XT90 is ~90A continuous and carries the full main line
  against the **80A** cap (§6) — do not raise the cap, and **inspect the connector for heat or
  discoloration after the first hard rides** (§9). Mount it where it can be reached and inspected
  without disassembly.
- The key switch is on a **logic input only**. Do not route main current through it, and do not
  omit the **~2A inline fuse** on its B+ tap — it's a thin wire off an otherwise unfused 84V line.
- **Storage isolation order matters:** key OFF → unplug XT90-S → *then* optionally take the main
  fuse out. Reconnect in reverse (fuse in → XT90-S → key ON). ⚠️ Never make/break at the fuse
  with the XT90-S mated: that bypasses pre-charge and arcs the fuse contacts. Never break
  either under load.
- Lighting, signals and horn come from the ESP32 module as it is built (**§5**); the brake lamp
  from the brake circuit (**§5.3**).

### 3.0 FarDriver ND72450 harness — wire-colour reference

📄 **Full pinout (pin numbers from all three FarDriver maps, electrical levels, sub-plugs, serial):
`revv1-fardriver-nd72450-pinout.md`.** This table is the short form.

⚠️ **Identify leads by WIRE COLOUR, never by pin number.** FarDriver's own published diagrams for
this controller **disagree on pin numbers** (throttle SV appears as pin 14 in one and 27 in the
other). **The colours agree; the numbers do not.**

| Function | Wire colour | Signal |
|---|---|---|
| **KEY / ignition** ("e-lock switch", 电门锁) | **ORANGE** (1-pin) | KEY |
| Throttle power | **red-white** | ACC+ |
| Throttle signal | **green-white** | SV |
| Throttle ground | **black** | GND |
| Speed pulse / one-line (**DEAD on this unit — cap it**, issue #7) | LIGHT BLUE | SPD |
| Analog speed meter (up to battery voltage) | **purple** | SPA |
| **Low brake** (the one this build uses) | **yellow-green** | BL |
| High brake (**NOT used**) | **grey** | BH |
| Reverse | brown-white | RE |
| Cruise / P gear | blue-red | XH |
| **One-line to the display** (display purple lands here — proven 2026-09-06, issue #7) | **BROWN** (2-pin plug, paired with the blue-red XH) | labelled BOOST (official diagram) / ALARM-SPD (NS series) |
| High-speed gear | yellow-white | SDH |
| Low-speed gear | blue-white | SDL |
| Motor Hall A / B / C | **yellow / green / blue** | HA / HB / HC |
| Motor Hall power | **red** | HALL+ |
| Motor temp | **white** | TEMP |
| Motor ground | **black** | GND |

⚠️ **PURPLE-TO-PURPLE IS A TRAP.** The display's one-line wire is **purple**; the controller's
**purple** is the *analog* output (SPA). Never join them (§3.1 pin 9). The one-line is the
**BROWN** lead — not the light-blue `SPD` either, which is dead on this unit (issue #7).

🔑 **The brake circuit lands on the yellow-green BL (low brake) wire** — the input the §6 setting
`Brake: 0-StopWhenGround` acts on (§5.3). The grey BH wire stays capped.

---

### 3.1 Display wiring — **DJ7091A-2.8-11, 9-pin (issue #4)**

📄 **Full pinout, cavity map, backend menu and required controller settings:
`revv1-display-chaojie-3in-pinout.md`.**

⚠️ **The display does NOT plug into the FarDriver harness.** Its 9-pin is a *vehicle-harness*
connector expecting a complete bike loom. **Break it out wire by wire:**

| Pin | Wire | Vendor function | Connect to |
|---|---|---|---|
| 1 | Orange | Left turn signal (0–15V) | ⚠️ **CAP — DO NOT CONNECT** |
| 2 | Black | "Electric door lock" = **ignition/key**, 0–150V | **POWER IN** → switched B+ (key-switch side), own small inline fuse |
| 3 | Green | Battery negative (0V) | **B−** |
| 4 | Light blue | Right turn signal (0–15V) | ⚠️ **CAP — DO NOT CONNECT** |
| 5 | Blue | Headlight signal (0–15V) | ⚠️ **CAP — DO NOT CONNECT** |
| 6 | Red | Reserved / TBD | **CAP** |
| 7 | Green-black on our harness (the manual says green-yellow) | **CAN L** (0–5 V) — a FarDriver-CAN pair: Chaojie says the panel supports the FarDriver CAN 18 protocol, and its `CJ-V3-01` manual documents a *Controller information* page. Display end terminates at 132 Ω | ⚠️ **LEAVE UNCONNECTED.** Nothing on this bike drives it — the FarDriver is a non-CAN SKU and the Cadmus BMS is not on it. ⏸️ Reserved for the **ESP32 module's CAN feed**, which is **parked** (issue #13 / plan D19) |
| 8 | Red-black | **CAN H** (0–5 V) — same pair | ⚠️ **LEAVE UNCONNECTED** — same |
| 9 | Purple | **Motor controller ONE LINE** (0–15V) | **→ FarDriver BROWN lead (2-pin plug) — proven 2026-09-06, issue #7.** The *only* wire going to the controller. ⚠️ **NOT the controller's purple** (`SPA`, analog) and **not the light-blue `SPD`** (dead on this unit) |

### ⛔ NEVER MATCH DISPLAY ↔ CONTROLLER BY WIRE COLOUR

**All nine display wires have a same-colour counterpart on the FarDriver side. NONE of them is
the correct match.** Colour is the right way to identify leads *within* the FarDriver harness
(§3.0) — and an actively dangerous way to join the display to it.

| Display wire | Same-colour FarDriver wire | Result if matched |
|---|---|---|
| Orange (left turn, 0–15V) | **ORANGE = KEY, 84V** | ⚠️ **DESTROYS THE DISPLAY** — 84V into a 15V input |
| Black (**POWER IN**, 0–150V) | **BLACK = GND** | Polarity inverted; display never powers |
| Light blue (right turn) | **LIGHT BLUE = SPD** | Speed data into a telltale input; pin 9 left dead |
| Purple (**one-line**) | **PURPLE = SPA** (analog) | Wrong protocol; no data |
| Green-yellow (pin 7 CAN L as the manual colours it) | **YELLOW-GREEN = BL Low Brake** | ⚠️ **SILENTLY DISABLES THE BRAKE CUTOFF** |
| Green (batt −) | GREEN = Hall B | wrong |
| Blue (headlight) | BLUE = Hall C | wrong |
| Red (reserved) | RED = Hall+ | wrong |
| Red-black (CAN H) | RED-BLACK = 485A/RXD | wrong |

**The only three connections that exist:** **9 → BROWN lead**, **2 → switched B+**, **3 → B−**.
Everything else stays unpopulated.

**🔧 BUILD IT AS AN ADAPTER — populate only pins 2, 3 and 9.** Source a mating
**DJ7091A-2.8-11** and make a break-out pigtail rather than cutting the display's plug off.
**Leave positions 1, 4, 5, 6, 7, 8 EMPTY.** Then there is no loose telltale wire to cap, and
nobody servicing this bike later can connect one by mistake — empty cavities can't be miswired.
It also keeps the display unmodified and returnable.

**Fusing:** pin 2 gets its **own ~2A fuse, separate from the KEY circuit's.** Sharing one means
a display short also kills ignition — i.e. the bike shuts down mid-ride.

⚠️ **Pins 1, 4 and 5 stay capped** until the ESP32 module's 12V lamp feeds exist; then each is fed
from the lamp feed it mirrors through ~1 kΩ (ESP32 plan §6). They are **0–15V** inputs — never
connect them to anything at pack voltage.

---

## 4. Installation sequence

1. **Strip stock drivetrain:** remove the stock rear wheel/motor and disconnect the stock
   wiring. **Keep the brake levers** — their switches drive the brake circuit (§5.3); check their
   type first (`revv1-brake-circuit.md` §4, M3). The handlebar pods are replaced by new switch sets
   (ESP32 plan D20).
2. **Fit the new motor wheel** in the rear dropouts. Orient the phase-wire exit, route the
   motor cable forward.
3. **Install both Grin V6 torque arms** (one per side), per Grin's instructions:
   - Hardened insert must seat on the **11mm axle flats** (light file to fit is normal).
   - The **wire-side arm must clear the phase-wire exit.**
   - Clamp securely to the frame; this is the primary anti-spinout anchor.
4. **Transfer the stock 203mm 6-bolt rotor** to the new hub — ⚠️ **with ~1.0mm of 0.2mm ring
   shims between the hub flange and the rotor** (issue #1). Blue thread locker; **verify full
   rotor-bolt thread engagement** with the shims fitted (use the +2mm bolts if short). Then check
   caliper alignment and fine-tune the shim count for centered pad contact.
   ⚠️ **Axle hardware:** washers (BOM 11a) under the nuts — never torque a bare nut against the
   open-top dropout, and never against a cocked washer (false reading). Stack is `dropout → V6
   plate → washer → nut`, so the washer bears on the arm's machined face, not the frame. Confirm
   the **11mm axle flats are bottomed in the slot** on both sides — a tilted washer is often an
   unseated axle. Double-nut the axle (thick = clamp, slim = jam), R-clip the cross-drilled tip.
   **Get the M16 axle-nut torque spec from Powerful Lithium — do not guess it** (issue #12).
   ⛔ **Do NOT grind, file, or dress the dropout/frame to make a washer seat.** A welded tab sits
   there, at the highest-stressed joint on the bike; dressing a weld toe starts a fatigue crack.
   Correct the **washer** instead.
5. **Mount tires — BOTH wheels:** Shinko 241 3.00-16 with 16" moto tubes (motor specifies
   tubed tires); **the front wheel gets the second tire.** Fit the tire **before** the rotor
   goes on the rear hub (tire irons bend rotors). Balance if practical. Check front-tire
   clearance at the fork arch under full compression and full steering lock. Then **reconnect
   the pedal chain** to the motor's single-speed freewheel — check chainline + tension (chain
   length may need adjusting); ensure the freewheel is tightened.
6. **Mount the controller** directly on the two printed under-seat brackets (the supplied enclosure
   is too large and is not used); route and connect **3 phase + 6 sensor
   (5 hall + 1 motor-temp)** leads to the motor. **Connectors mate directly — no adapter.** The
   motor has **two hall pigtails**; use the one that fits and **cap the spare**.
   Mount the **3" display** — ⚠️ **it does NOT plug into the FarDriver harness; break it out
   per §3.1 (issue #4).** **Throttle: the FarDriver-bundled throttle is installed** on its own
   harness lead (§5.2).
7. **Build the main power line:** Battery(+) → **125A Class T** fuse → **XT90-S** → Controller B+;
   Battery(−) → Controller B−. Then wire the **KEY** circuit: tap B+ **downstream of the
   XT90-S** → **~2A inline fuse** → **2-wire key switch** → the ESP32 module's supply **and** the
   **start latch** → FarDriver **KEY** wire. The key powers the module; the controller starts only
   with the run switch ON and the start button held ~2 s (ESP32 plan **D24**, §3.2.5a). Until the
   module and latch are built, the key switch feeds the KEY wire directly.
   ⚠️ The key switch carries logic current only — never main current.
8. **Mount the Cadmus battery** in the Center Storage Cage (fit confirmed, §7); connect
   through the fuse/XT90-S line.
9. **Connect controls:** **FarDriver throttle → its harness throttle lead** (§5.2) and the
   **FarDriver display**. Brake cutoff: brake circuit step 1 (step 10a, §5.3).
10. **Brake service:** install **Shimano D02S sintered** pads (front + rear), fresh
    **mineral oil** + full bleed, optional braided lines. Verify firm lever, no fade.
10a. **Brake circuit, step 1 — motor cut** (§5.3): check each lever's switch type first
    (`revv1-brake-circuit.md` §4, M3), then wire each lever through its own **1N4148** to the
    FarDriver **`BL`** (yellow/green), lever return to B−, with **100 nF** `BL` ↔ B− at the
    controller (brake circuit §2, §6). The level and motor-cut tests (brake circuit §7.1–§7.2,
    and §8 below) gate the first ride.
11. **Charge port:** terminate the 72V charger lead / battery charge plug with XT90-S.
12. **Lighting & accessories (§5):** lights, signals and horn come from the ESP32 module as it is
    built; add brake circuit steps 2 (brake lamp) and 3 (module sense) with it (§5.3).

---

## 5. Lighting & accessories (ESP32 module)

Lighting, signals and horn come from the **ESP32 module** (`revv1-esp32-module-plan.md`), built
incrementally — one function at a time. Bar controls come from new switch sets (plan D20–D22).
The brake lamp comes from the brake circuit (§5.3).

### 5.1 Architecture
The ESP32 module is fed from the key-switched 72V node and drives the 12V lamps (plan §3.2, §6).
Its ground stars at the controller's B− stud.

### 5.2 Throttle — FarDriver-bundled throttle installed (2026-09-06)
- The **FarDriver-bundled twist throttle is on the bar.** Its 3-pin lead mates the harness
  throttle lead directly (harness: red-white ACC+ 5V · green-white SV · black GND — the
  throttle-side signal wire is **yellow**; wire by function, not colour). All three reference
  FarDriver ground.
- Its **2-pin red-button lead** goes to the **ESP32 module**, which drives the FarDriver boost
  input with hold / toggle modes (ESP32 plan **D4**, §7.1 there). Until the module exists, leave
  it **capped** — ⚠️ do not plug it into whatever 2-pin fits: brake, cruise, boost, reverse and
  3-speed share the same housing.
- Bar controls come from the new switch sets (ESP32 plan D20).
- Calibration (set, §6): **1.1V = 0% · 3.9V = 100%**, reads 0.79V at rest ✓.

### 5.3 Brake cutoff and brake light — the brake circuit
📄 **Circuit, parts, lever check, build order and tests: `revv1-brake-circuit.md`.**

Each lever, through its own **1N4148** steering diodes, does three things in hardware:
- **Motor cut:** pulls the FarDriver **Low Brake** (`BL`, yellow/green) to B− —
  `Brake: 0-StopWhenGround` (§6). The grey `BH` stays capped.
- **Brake lamp:** pulls P-FET **Q1 (`AO3407A`)**'s gate low, so Q1 switches +12 V to the tail STOP
  lamp through a 1 A fuse — **no firmware in the path** (ESP32 plan **D23**).
- **Module:** signals the ESP32 module on IN-05 / IN-06; the module only listens.

Everything shares the pack's B−, so no isolation is needed; the diodes keep the three pull-ups
apart. Built in three steps (brake circuit §6): **step 1 now** — levers → diodes → `BL`, 100 nF at
the controller; **step 2** with the module's 12 V rail — brake lamp; **step 3** with the module —
sense. ✔ Check each lever's switch type first (brake circuit §4, M3). The motor-cut test gates the
first ride (§8).

### 5.4 ⚠️ Electrical rules
- **Never** feed a 12V lamp or a 0–15V display input from the 72V pack — 84V destroys them.
- Take the brake circuit's and the module's ground from the **controller's B− stud**.

### 5.5 Verify
Brake circuit tests (`revv1-brake-circuit.md` §7) as each step goes in; each module output in the
§8 lighting check.

### 5.6 Optional — ride-mode switch
For bar-selectable eco/normal/sport, add a **standalone FarDriver-compatible 3-speed mode
switch**, or just change modes in the **FarDriver app.**

---

## 6. FarDriver configuration (Bluetooth app)

**File to import: `fardriver/ND72450_AKHA86_20260908_revv1-import-noreverse.heb`** — the
2026-09-08 post-session export (firmware **HA86**, serial `JSWXOJ260303D1ECD1FD`) with only
`BackEnable` changed to 0 (**reverse OFF**, issue #6). App main menu → open heb → **Save** →
key-cycle → re-export, and confirm the rows below. Do not re-type values by hand. Decoder:
`fardriver/heb_decode.py`.
⚠️ **Do not import the 2026-09-06 files** (`…20260906_revv1-import-v2.heb` and earlier) — they predate
the 2026-09-08 changes and would put `Brake` back to `7-Disabled` (issue #8) and `SpeedPulse` back to
11 (issue #7).

| Setting | Current value (verify, don't re-enter) |
|---|---|
| Battery | **72V (20S)** rated · **34Ah** · type Lithium · full charge 84.0V |
| **Battery-current limit** (`MAX LINE CURR`) | **80A** ✓ — motor-protection cap **and** what keeps the ~90A XT90-S inside its rating. **Do not raise.** |
| Phase-current limit | **200A** (the 72450's product max is 450A) — conservative; leave |
| Motor | **5000W** rated · **23 pole pairs** (vendor-programmed — not yet confirmed by Powerful Lithium) · max 3967 rpm in the app's units |
| Hall sensors | ✓ **self-learn done** — phase offset **212.0°**. Re-run only if the motor or phase wiring is disturbed |
| **Motor temperature** | ⚠️ **`NTC_PTC` = `5-KTY83-122`** ✓ · protect **120°C** / restore **90°C** (MOS 100/80°C). **Never `0-None`** — it reads a bogus **197°** and faults "7. Motor Temp Protect" (issue #5). ✔ Re-confirm plausible ambient at every power-up; a rail reading means the sensor isn't seen. ⬜ *Sensor type still to be verified with Powerful Lithium — matching at 25° does not prove the curve at 120°.* |
| Throttle (**FarDriver-bundled twist, installed**) | **1.1V = 0% · 3.9V = 100%**, response Linear. Reads **0.79V at rest** ✓ = 0% (below the 1.1V floor → no startup over-throttle fault). Its red button → ESP32 module → FarDriver boost input (plan D4) |
| Brake cutoff | ✅ **`0-StopWhenGround`** (set 2026-09-08, confirmed in the controller export, issue #8). Braking pulls **`BL`** (yellow/green) to B− through the brake circuit (§5.3). ⛔ Never a **`P+`** variant (they bundle Park, which stays Disabled) and never **`1-StopWhenFloat`** (inverts the logic → motor cuts when *not* braking). ⚠️ **NOT YET VERIFIED — the powered cut test (brake circuit §7.2) has not run**. **GATE BEFORE FIRST RIDE.** **E-brake / regen stays OFF** (`Follow: Invalid`) |
| Low-voltage cutoff | **60.0V** (3.0V/cell); power derate starts **+2V** (62V) |
| Over-voltage protect | **90.7V / restore 88.7V — factory internal default for a 72V-rated unit. Leave it.** ⚠️ Do **not** set "~84V": a full pack sits at 84.0V and would trip it. With regen off the pack can never exceed 84.0V anyway |
| **Reverse gear** | **Must be OFF** — the pedals are chain-linked to the hub through the freewheel; a powered reverse spins the cranks backwards into the rider's legs. ⚠️ **Still ON in the controller** (issue #6, open): `BackEnable = 1` in every controller export (2026-09-06 and both 2026-09-08); an in-app change on 2026-09-08 did not take. ⬜ Import the noreverse file above and confirm reverse OFF in a fresh export. If the import does not take, set `Backward Pin` → `13-Invalid` (`revv1-fardriver-nd72450-pinout.md` §3), which disconnects the RE wire from reverse. The RE wire stays capped |
| Speedo / wheel | the app takes **tire width / aspect / rim = 80 / 100 / 16** (= 3.00-16) and **transmission ratio 1.000** — there is **no circumference field**. ✔ Check against GPS on the road test; correct via the ratio if off |
| Park (P) | **`Park: 2-Disabled`** in the app — no P gear, no auto-park |
| Display one-line | **Working:** `Special Frame` **21** (not 246 — RS485 PC mode, sends nothing) with the display purple on the **BROWN** lead; the light-blue `SPD` is dead on this unit (issue #7). DATA0/1 = 8 / 97, byte option 3, 0.9 ms / Stop 2 (app label 124 ms), **`Speed pulse` 1**. Speed scale settled (issue #7b): `Speed pulse` 1 was the whole fix; transmission ratio stays **1.000**. ⬜ Final trim vs GPS on the road test |

---

## 7. Owner confirmations (resolved)

- [x] **Pads:** LBN 4-piston caliper = **Shimano Saint/Zee D-type shape** (Ride1Up brake chart).
  Fit **sintered metal — Shimano D02S** or equiv (NOT D03S, which is resin), front + rear. Stock
  rotor (203mm 6-bolt) reused.
- [x] **Brake fluid: MINERAL OIL** (marked on the lever). Use mineral only.
- [x] **Cadmus fits the Center Storage Cage** — confirmed by the battery vendor ("designed to be
  housed and stored in the official storage cage accessory"). Cage fits the FS frame and is
  already owned.

---

## 8. Commissioning / acceptance test

1. **Pre-power:** visual check all connections; confirm correct polarity; meter B+ to B− for
   shorts; torque-check all fasteners (axle nuts, torque-arm clamps, rotor bolts, caliper).
   Brake circuit step 1 fitted (§5.3); grey `BH` capped.
2. **First power-up:** plug the **XT90-S** (pre-charge handles inrush), then **key ON** →
   confirm display lights, no faults.
3. **Throttle spin test** with the wheel **off the ground** (hall self-learn is already done —
   re-run only if the motor or phase wiring was disturbed).
4. **Brake test** (brake circuit §7): `BL` below 0.8 V with each lever pulled, and **each lever
   kills motor power** — gate before first ride; levers firm; no air in lines. Once steps 2–3 are
   in: each lever lights the brake lamp (§7.3) and the module reads it (§7.4).
5. **Torque-arm check:** no axle movement under load.
6. **Low-speed road test** first; recheck fastener torque and for any heat/odor after.
7. Confirm speedo against GPS (if off, adjust the app's **transmission ratio**; tire fields stay 80/100/16).
8. **Lighting** (as each ESP32 module output is built): running light, low beam, **high beam**,
   tail/brake, **turn signals**, hazard and **horn** work. The **throttle drives the FarDriver**
   correctly (0% at rest, smooth sweep, brakes cut power).

---

## 9. Sign-off

| Check | Tech initials | Date |
|---|---|---|
| Torque arms installed, both sides | | |
| Main line: fuse + **XT90-S** correct, polarity verified | | |
| KEY circuit: ~2A inline fuse fitted, key switch gates KEY wire only | | |
| **XT90-S checked for heat/discoloration after road test** | | |
| Flipsky FSESC 75200 left **uninstalled** (not part of this build) | | |
| Config loaded; hall self-learn complete; current cap **80A**; **reverse OFF** | | |
| Brakes: correct fluid, bled, pads bedded | | |
| Brake circuit step 1 fitted (§5.3); both levers cut motor power (brake circuit §7.2) | | |
| Brake lamp lit by each lever (brake circuit §7.3) — once step 2 is in | | |
| Road test passed | | |
| Lighting/controls built so far work; throttle calibrated | | |

---

*High-power aftermarket build. Not a stock e-bike — voids Ride1Up warranty; off-road or
registered-vehicle use per local law. Hand back the FarDriver app credentials and note the
final battery-current cap to the owner.*
