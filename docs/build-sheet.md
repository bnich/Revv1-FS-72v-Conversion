# REVV1 FS → 5,000W 3T Build Sheet

**Bike:** Ride1Up REVV1 FS (full-suspension moped-style e-bike)
**Goal:** Replace stock drivetrain with the Powerful Lithium 5,000W 3T hub motor
**Config:** 72V drivetrain conversion (vendor-recommended max-power path)
**Running on a temporary setup (2026-09-11):** FarDriver on the printed brackets, temporary throttle and key switch, battery — it moves under power; no display, lights or other electronics, and **no fuse of any kind on the main line**. Lighting, signals, horn and brake light come from the ESP32 module, built incrementally.
**Status:** Ordered ✅ — motor, controller, battery + charger, torque arms, key switch, brake pads · **New tyres bought 2026-09-17** (20×4.0 front / 20×4.5 rear — the Shinko 241 pair is out of the build, issue #14) · Cage owned ✅ · ⬜ to order: Class T fuse + block, tubes, brake consumables, brake circuit parts (module BOM group G)
**Battery + charger RECEIVED 📦 2026-08-14** — incoming inspection pending (see note below)
**Created:** 2026-06-28 · **Updated:** 2026-09-17

> Prices marked ✓ are vendor-confirmed (Powerful Lithium, June 2026).
> Prices marked *(est.)* are estimates for parts the vendor doesn't sell.

---

## Order tracker

| Item | Status | Cost |
|------|--------|-----:|
| Tires — **Huntsman Override 20×4.0** (front) + **BDGR Override 20×4.5** (rear) | ✅ **BOUGHT** 2026-09-17 (issue #14) | ⬜ **record actual** |
| Tubes — Super73 fat tube, **20 × 4 / 4.5 / 5** ×2 (one spec covers both ends) + rim strips | ⬜ **TO ORDER** | ~$30 *(est.)* |
| ⛔ Tires — Shinko 241 3.00-16 (pair) | ⚠️ Ordered 2026-06-28 — **not in the build: both rims are far too wide for a 3.00-16 (issue #14).** ⬜ Return or sell on | ~$100 *(est.)* |
| Center Storage Cage | ✅ owned | — |
| 5,000W 3T motor | ✅ **ORDERED** 2026-06-28 | $760 |
| FarDriver 72450 controller (loaded) | ✅ **ORDERED** 2026-06-28 | $550 |
| ↳ throttle — included w/ controller | ✅ ordered | *(in $550)* |
| ↳ display — included w/ controller | ✅ ordered | *(in $550)* |
| ↳ enclosure — included w/ controller | ✅ received — **not used** (too large; the controller bolts directly to the printed brackets) | *(in $550)* |
| ↳ power / controller cable — included w/ controller | ✅ ordered | *(in $550)* |
| 72V Cadmus battery | 📦 **RECEIVED** 2026-08-14 | $1,900 ✓ |
| ↳ 72V charger — bundled add-on (+$100 w/ battery) | 📦 **RECEIVED** 2026-08-14 (same box) | $100 ✓ |
| Torque arms — Grin **V6** ×2 (clamp-mount) | ✅ **ORDERED** 2026-06-29 | ~$100 |
| **Electrical — Littelfuse `JLLN125` Class T fuse, 125A** (125 V DC · 20 kA @ 125 V DC) — Zoro listed **$43.16** (2026-09-08); also Mouser `JLLN125.XXP` | ⬜ **TO ORDER — approved 2026-09-08, issue #10** | ~$70 *(est.)* |
| **Electrical — Blue Sea `5007100` Class T block** (160 V DC · 160 A max operating · four-stud bolt-down, 1/4"-20 studs · ignition-protected **only with the cover secured** · lugs to 4/0) — street $44.98–76.11, ~$68 list (2026-09-08) | ⬜ **TO ORDER — approved 2026-09-08, issue #10** | ~$50 *(est.)* |
| Electrical — Blue Sea 5127 150A ANL fuse | ⚠️ Ordered 2026-06-29 — **not used: rated 80V DC, below the 84.0V pack (issue #10). Do not fit.** ⬜ Return or keep as a sub-80V spare | ~$20 |
| Electrical — Blue Sea 5005 ANL block | ⚠️ Ordered 2026-06-29 — **not used: rated 32V DC (issue #10). Do not fit.** ⬜ Return or keep as a sub-32V spare | ~$25 |
| Electrical — Flipsky FSESC 75200 Pro V2.0 (ordered as an "anti-spark switch") | ❌ **Wrong part — a VESC motor controller (issue #3). Not installed.** ⬜ Return/resell | ~$90 budgeted (⬜ **verify what was actually paid** — this unit lists $180–260) |
| Electrical — key switch → **FarDriver KEY wire** (+ ~2A inline fuse) — *"Universal Key Ignition for Ebike 12V-96V Anti-Theft"* | ✅ **PURCHASED** 2026-09-07 | ~$12 *(est. — ⬜ confirm actual)* |
| Electrical — XT90-S (5-pack) — **main-line make/break + anti-spark** (Option B) | ✅ owned | ~$15 |
| Electrical — 8 AWG main wire (if leads need extending) | ⬜ to order | ~$15 |
| **Brakes — Magura MT5, front + rear, with a brake switch per lever** | ✅ **FITTED** — hoses cut to length, refilled | ⬜ price not recorded |
| ⛔ Brakes — pads, Shimano D02S sintered ×2 sets — **not in the build** (Shimano shape; the MT5 takes Magura pads) | ✅ **ORDERED** 2026-06-29 · ⬜ return or sell on | ~$60 |
| **Axle washers ×2** — issue #2 | ✅ **FITTED 2026-09-08** — shop-made by the owner; material and nut torque open (issue #12). ⬜ actual cost | ~$10–20 *(est.)* |
| **Rotor — 0.2mm 6-bolt ring shims + M5×0.8 bolts** — issue #1 | ✅ **FITTED 2026-09-08** — 4 shims (0.80 mm), rotor centred, brakes work. ⬜ actual cost | ~$15 *(est.)* |
| **Brake circuit parts** (1N4148 steering diodes, `AO3407A` P-FET, 10 kΩ, 100 nF, 1 A fuse) — `revv1-brake-circuit.md` §3; part list in the module BOM, group G | ⬜ to order | *(counted in the module BOM, ~$5)* |
| Professional install | ⬜ to schedule | ~$250–400 |

**Committed so far: ~$3,557** · **Remaining: ~$380–670** · **All-in: ~$3,937–4,227** — breakdown
and caveats under **Totals**.


---

## Why this is a full conversion, not a bolt-on

The bike's original controller caps at ~1,800W, so a 5kW motor on it does almost nothing — and
risks damage. Powerful Lithium states that the motor needs an aftermarket controller and that its
connectors do not fit the original plugs. The motor is the trigger to replace the controller,
battery, charger, display, and wiring.

---

## Tier 1 — Core electrical (required to function)

| # | Part | Price | Running total |
|---|------|------:|--------------:|
| 1 | [5,000W 3T hub motor](https://powerfullithium.com/products/5-000w-3t-high-power-hub-motor) — ✅ **ORDERED** (base, no hall adapter) | $760 ✓ | **$760** |
| 2 | [FarDriver 72450 controller](https://powerfullithium.com/products/fardriver-controller-72v) — ✅ **ORDERED**, loaded *(see breakdown below)* | $550 ✓ | **$1,310** |
| 3 | [72V "Cadmus" battery](https://powerfullithium.com/products/72v-cadmus-battery-ride1up-revv-1) (Molicel P42A, 34Ah) — 📦 **RECEIVED** 2026-08-14 | $1,900 ✓ | **$3,210** |
| 4 | 72V (20S) charger — 📦 **RECEIVED** 2026-08-14 (bundled +$100 w/ battery) | $100 ✓ | **$3,310** |
| — | Vendor order discount (applied at checkout) | −$200 | **$3,110** |

**Controller (line 2) breakdown — $550, all options selected:**
- Base FarDriver 72450 (200A line / 450A phase) — $200
- Throttle & Display bundle — $200 → **throttle + display**
- Enclosure & Power Cable bundle — $150 → **enclosure + power/controller cable**

> ✅ All four accessories (throttle, display, enclosure, power cable) are **ordered as part
> of the controller** — already counted in the $550, so don't buy them separately.

## Tier 2 — Safety & mechanical (strongly recommended)

*Costs are ranges for quality parts. Stock 4-piston/203mm brakes carry over, so brakes are
service + upgrade, not full replacement.*

**Torque arms — Grin V6 clamp-mount ×2 — ✅ ORDERED**
- [Grin Torque Arm V6](https://www.amazon.com/Grin-Technologies-Universal-Ebike-Torque/dp/B0CCZ36RXP)
  (hardened 17-4 splined insert, **frame-clamp** mount, 12/14/16mm axle) — **~$100/pair**
- **V7 was preferred but isn't made in 16mm**, so V6 is the next-best — and it still skips
  the weak **M5 fender eyelet** by clamping to the frame. Don't enlarge that eyelet; it's
  not the anchor at 5kW.
- Insert fits the motor's 16mm axle; confirm it seats on the 11mm flats. The wire-side arm
  must clear the phase-wire exit.

**Electrical safety**
- ⚠️ **Main fuse: Littelfuse `JLLN125` Class T 125A in a Blue Sea `5007100` block** (issue #10) —
  link **125 V DC / 20 kA @ 125 V DC**, block **160 V DC**, bolt-down, ignition-protected. Class T's
  headline 200 kA is **AC-only**. **Never fit a 175A or 200A link** — over the block's 160A rating.
  ⛔ **Not the ordered Blue Sea 5127 ANL link / 5005 block** — rated 80V / 32V DC, below the 84.0V
  pack; above 80V the link's arc interruption is unspecified.
- ❌ **NO Flipsky anti-spark switch — issue #3.** What arrived is a **Flipsky FSESC 75200 Pro V2.0**,
  a complete **VESC motor controller**. ⚠️ **Do not install it anywhere** — its blue/green/yellow
  leads are *phase outputs*; landing them on the FarDriver's phase studs shorts two output stages
  together and destroys both controllers.
- ✅ **Main switch + anti-spark — DECIDED 2026-08-15 (Option B):** the owned **XT90-S carries
  the main line and is the anti-spark device** (its pre-charge pin handles controller inrush).
  Wires **Battery+ → 125A Class T fuse → XT90-S → controller B+**. **$0** — already owned.
  - 🔑 **Daily on/off = 2-wire key switch on the FarDriver KEY wire only** — a low-current
    logic input, fed from B+ downstream of the XT90-S through a **~2A inline fuse**. It does
    **not** switch main current. Purchased 2026-09-07: its **12–96 V** rating covers the 84.0 V
    pack, and per **ESP32-plan D13** it gates a soft-started MOSFET rather than carrying module
    current, so **no current rating is required**. ⚠️ **Wire count unverified** — the build
    assumes **2-wire**, and many *"anti-theft"* ignitions are 3- or 4-wire (checklist Phase 7).
  - **Storage isolation:** key OFF → unplug XT90-S → optionally take the main fuse out.
    Reconnect in reverse. ⚠️ **Never make/break at the fuse with the XT90-S mated** — that
    bypasses pre-charge and puts full capacitor inrush across the fuse contacts.
  - ⚠️ **Accepted tradeoff — thin current margin:** XT90 is ~90A continuous against an 80–90A
    battery-current cap. The FarDriver cap is set at **80A**; **inspect the connector for
    heat/discoloration after the first hard rides**. A $90 anti-spark switch would have given
    200A of margin; the owner chose ~$12 instead.
- ⚠️ **84V note:** the 20S Cadmus hits 84.0V full. XT90 is rated 500V, so **current**, not
  voltage, is the XT90-S's limit.
- **Main wire: 8 AWG silicone** if leads need extending — ~$15.
- 🔑 **Key config — SET (controller export 2026-09-06): battery-current limit = 80A**, phase
  200A; **125A Class T** fuse = short-circuit backstop, Cadmus BMS = finer limit. Settings table:
  work order §6. ⚠️ **Import file: `fardriver/ND72450_AKHA86_20260908_revv1-import-noreverse.heb`**
  (the 2026-09-08 export with reverse off). **Not `…20260906_revv1-import-v2.heb`** — it predates the
  2026-09-08 changes and would reset `Brake` to `7-Disabled` and `SpeedPulse` to 11 (issues #6/#8).

**Brakes — Magura MT5, front + rear; stock 203mm rotors carry over**
- ✅ **Magura MT5** 4-piston, front + rear, with Magura's pads. **Chosen mainly for form factor:
  the MT5 levers sit better beside the new left and right switch pods on the bar** (plan D20).
  Hoses cut to length (new barb + olive) and refilled. ⬜ Price not recorded
- ✅ Brake fluid: **mineral oil — Magura Royal Blood**. Never DOT
- ✅ **Brake switches:** one per lever, **2-wire, normally open** (M3) — the brake circuit works as
  drawn
- ✅ **Rotor: 203mm, 6-bolt ISO** (stamped "203") — transferred to the new 6-bolt hub, shimmed
  0.80 mm to centre it in the caliper (issue #1); the MT5 caliper dropped in on the same shims
- ⛔ **Shimano D02S pads ×2 sets (ordered, ~$60) are not used** — a Shimano D-type shape for the old
  LBN calipers, which the MT5 does not take

**Tier 2 subtotal: ~$180–240** *(excludes the MT5 brakes — ⬜ price not recorded; the D02S pads in it are unused)*

## Tier 3 — Labor

| # | Item | Price | Running total |
|---|------|------:|--------------:|
| — | Professional install (req'd for motor warranty) | ~$250–400 *(est.)* | **(see Totals)** |

---

## Totals

- ✅ **Committed (ordered): ~$3,557** — tires, motor, loaded controller, battery + charger (net
  **$1,800** after −$200 discount), torque arms, Blue Sea ANL fuse + block, Flipsky FSESC, key
  switch, D02S pads, axle washers, rotor shims, XT90-S (owned). Cage owned.
  - ⚠️ **The ANL pair (~$45, issue #10) and the Flipsky (~$90, issue #3) are not in the build.**
    Committed drops to **~$3,467** if the Flipsky is returned, or **~$3,512** if the ANL pair is.
  - ⚠️ **The Shinko 241 pair (~$100, issue #14) is also out of the build**, replaced by the
    20×4.0 / 20×4.5 fat pair bought 2026-09-17 — ⬜ price not yet recorded, so the total is not
    yet restated.
  - **The Flipsky's actual price is unconfirmed** — the FSESC 75200 typically lists **$180–260**,
    so Committed may be understated by ~$90–170. Actual prices for the key switch, washers and
    shims are unconfirmed too. **Re-run these totals once receipts and return outcomes are known.**
- ⬜ **Remaining: ~$360–550** — **Class T fuse + block (issue #10) ~$110–150** (estimate; listings
  seen 2026-09-08: `JLLN125` $43.16 at Zoro, `5007100` $44.98–76.11 street) · **~2A inline fuse** ·
  install ~$250–400. Excludes the 8 AWG
  wire — only needed if the pack's leads are too short (decide at the 0.5 incoming inspection) —
  and the brake circuit parts, which the module BOM counts (group G, ~$5).
- 🏁 **All-in estimate: ~$3,917–4,107** (before any returns; ⚠️ excludes the Magura MT5 brakes, price not yet recorded)

---

## Open items

- ⬜ **Order the Class T fuse + block** (issue #10) — the main line currently has no fuse at all.
- ⬜ **Brake circuit step 1** (levers → steering diodes → FarDriver `BL`, `revv1-brake-circuit.md`
  §6) — the MT5 switches are normally open (M3 ✅, §4); its motor-cut test (§7.2) is issue #8's gate
  before riding.
- ⬜ **Battery incoming inspection** (checklist 0.5) and a **test charge** before the charger
  warranty runs out (~2026-09-14 from delivery).
- ⬜ **Returns:** the Flipsky FSESC (issue #3) and the Blue Sea 5127 + 5005 (issue #10, or keep as
  low-voltage spares). Confirm what was actually paid.
- ⬜ **Install labor:** placeholder — get a local shop quote.

## Decisions logged

- **Stay with the 3" Chaojie dash; Gen 4 touchscreen parked (2026-09-09).** The owner has a
  **Chaojie Gen 4 5" touchscreen** (`CJ-V5-04`, rear-camera variant; E-Conic $175, $195 with camera;
  dual-core, phone mirroring, TPMS) in hand and evaluated it as a replacement. **Parked — on this
  controller it shows no more than the 3".** Its CAN advantage is conditional: E-Conic's instruction
  reads *"**if using CANBUS controller**, set to CAN18 and 0-250 kbps"*, and the **ND72450 is not a
  CANbus SKU**. Converting one is a hardware job that lands a transceiver on **A11/A12 — the Hi/Low
  speed sense lines** — which this build has already ruled out. So out of the box the Gen 4 would run
  the same one-line `SpecialFrame` 21 and show the same blank **C°/M°**, against a 3" live on one-line
  since 2026-09-06. Open if it is revisited: it is rated **DC 12–96 V** — 12 V of headroom on the
  84.0 V pack, against the 3"'s 36 V — and its **brightness is unpublished** against the 3"'s
  **1000 cd/m²**, a real question on an open moped in daylight. ⭐ The research also produced the
  FarDriver CAN setting a Chaojie expects — **`CAN` 18 @ 250 kbps**, the 18 since confirmed by
  Chaojie — a direct input to the ESP32 module's D8. The dash question as a whole was parked on
  2026-09-10 (plan D19). Full evaluation: `revv1-display-chaojie-gen4-touchscreen.md`.
- **Reverse gear OFF (issue #6).** The hub's freewheel is chain-linked to the cranks, so powered
  reverse spins the pedals backwards into the rider; nothing in the build needs reverse. ⚠️ **It is
  still ON in the controller** (`BackEnable = 1` in every export) — issue #6 has the fix.
- **ESP32 body module (decided 2026-09-06).** It takes over lighting, signals, horn and boost; it has
  **no screen of its own (D17)** — the Chaojie 3" is the only display
  (`revv1-esp32-module-plan.md` §2.1).
- **Throttle: the FarDriver-bundled throttle is installed (2026-09-06).** It mates its own harness
  lead. Its **red button grounds blue/red `XH` directly for hold-to-boost until the ESP32 module
  exists**; then it feeds the module, which drives boost with hold/toggle modes and safety clears
  (plan D4). Bar controls come from new aftermarket switch sets (plan D20; work order §5.2 / checklist
  Phase 5).
- **Display one-line (issue #7):** `SpecialFrame` **21** with the display's purple on the
  FarDriver's **brown** lead and `SpeedPulse` **1**. Shipped at 246, FarDriver's RS485 PC-link mode,
  which sends nothing to a one-line display; 21 is FarDriver's "general one-line" and what Chaojie
  resellers specify for this display.
- **Dash data: temperatures need CAN, and the CAN feed is parked (issue #13, plan D19).** Over the
  one-line, *every* FarDriver-compatible dash receives the same short frame — speed, volts, one
  current-or-power byte, gear/brake/light flags, faults — **no temperatures** (Qulbix: one-line
  "transmits only speed, speed mode and battery voltage"; CAN adds "current, motor phase current,
  motor temperature"). Standard ND controllers ship **without a CAN transceiver**; CAN units are a
  separate SKU (Qulbix ND72450 "BT + CAN" €190 ex-VAT; E-Conic "limited supply").
  **Decision:** the **ESP32 body module feeds the Chaojie over CAN, impersonating a CAN FarDriver**
  (`revv1-esp32-module-plan.md` §7.2) — ⏸️ **parked 2026-09-10 (owner, plan D19)**: the module is
  being built without it, and the display question re-opens later (solve the **CAN 18** byte map, or
  replace the panel). The CAN hardware (~$2 transceiver, in hand) stays fitted, so a later answer
  needs no rework. **Cost consequence: none.** Meanwhile motor and controller temperature and bus
  current live on the module's **WiFi page** and in the FarDriver app; the Chaojie keeps speed, its
  own V/SOC, gear and faults.
  If the map never turns up, the alternatives are a **factory CANbus ND72450** (~$180–200; under the
  current receive-only reading of our panel it would work) or a **CAN dash + CAN controller swap**
  (Chaojie SCJ313-CAN ~$47 at QS Motor or a DKD TFT, plus a CAN-enabled ND72450 → ~$250–300 and a
  full re-commission). The **FarDriver app or the third-party GDriver app** on a phone shows
  temperatures for $0 + a bar mount. Not a **Grin Cycle Analyst V3-HC** (~$166 + external shunt) — it
  never sees the FarDriver's fault codes or the KTY83 winding temp, and adds two more lugs in the 80A
  main line.
- **Over-voltage protect left at the factory 90.7V / 88.7V.** Never set it to ~84V — a full 20S
  pack sits at 84.0V and would trip it. Regen is off, so the pack can't exceed 84.0V anyway.
- **Speedo is set by tire size, not circumference.** The app takes width / aspect / rim plus a
  transmission ratio; `SpeedPulse` 1 for the one-line dash. ⚠️ **`80 / 100 / 16` is the 3.00-16 and
  is now wrong** — the 20×4.5 rear adds **+13.6 %** of rolling circumference (~1753 → ~1992 mm), so
  the speedo would read that much low. ⬜ Re-derive it (candidate **110 / 100 / 16**, OD 626 mm),
  then trim the transmission ratio against GPS (issue #14).
- **Controller: 72450, not 72680.** A 5kW motor draws ~70A at 72V; the 72450's 200A
  battery current is ample. 72680 would be unusable overkill (and the 34Ah BMS can't feed
  it). Saved $100.
- **Hall sensor adapter: skipped** — it's for the Super73 + HandlWorks BAC2000 only. ✅ **Verified
  by fit 2026-08-15:** the motor's hall pigtail mates the FarDriver harness directly, no adapter and
  no splicing. The motor ships with **two hall pigtails in different connector standards**; the
  **spare gets capped**. The lead carries **6 wires — 5 hall + 1 motor-temp** (yellow/green/blue =
  Hall A/B/C, red = Hall+, black = GND, **white = motor temp**), so **motor-temp protection is
  enabled** in the FarDriver app.
- **Tires: fat, not moto — Huntsman Override 20×4.0 front, BDGR Override 20×4.5 rear**
  (2026-09-17, issue #14). The Shinko 241 3.00-16 pair is **out of the build**: both rims are far
  too wide for it (front ~72 mm, rear ~83 mm internal against a **54.6 mm** maximum), which stands
  the sidewalls up, kills carcass compliance and is the prime suspect for the bumpy ride. No 16"
  moto tire suited to an 83 mm rim clears the frame — the 130-section sizes need **+27 mm per side**
  at the swingarm. Both new tires are Vee Tire **Override** carcasses, so front and rear match; and
  **20×4.0 is the stock diameter**, so the front's fork and steering-lock clearance is already proven.
- **Moto load/speed ratings were never the point.** The 241 was chosen on tread pattern, 16"
  availability and grip/wear at ~45 mph. Its **45P** marking (165 kg, 150 km/h) is roughly double
  the headroom this vehicle needs, so moving to fat rubber gives up nothing that mattered.
- **⚠️ Wheel sizing fact — bead seat diameter is NOT fitment.** The Revv1's "20×4" rim is ISO 406
  (~406 mm bead seat) = a motorcycle "16-inch", so a 16" moto tire will *mount* on both the stock
  rims and the 20×4 motor wheel. **Width is a separate question, and it fails.** Measure the rim
  before believing a tire fits (issue #14).
- **Torque arm: Grin V6 (not V7).** V7 is the high-power ideal but isn't made in 16mm; V6
  clamp-mount is next-best and skips the weak M5 fender eyelet by clamping to the frame.
- **Power switch — Option B (2026-08-15, issue #3).** The **XT90-S stays on the main line** as
  make/break + anti-spark, and a **2-wire key switch gates the FarDriver KEY wire only** (~$12 vs ~$90
  for a real anti-spark switch). Accepted cost: XT90's ~90A rating is thin against the 80A cap — check
  the connector for heat after the first hard rides.
- **Cage: the owned Center Storage Cage.** Powerful Lithium's Cadmus page states the pack "requires
  the Revv 1 Center Storage Cage accessory... designed to be housed and stored in the official storage
  cage." It fits the FS frame — the vendor-intended mount, no extra purchase.
- **Pedals retained:** the motor has a **single-speed freewheel**, so the pedal chain
  reconnects and pedaling drives the wheel (verify chainline at install). Vestigial at
  speed; useful for legal "functional pedals" / low-speed / limp-home.
- **Incremental build (owner, 2026-09-11).** *"we will build incrementally."* The bike runs on its
  temporary setup (FarDriver, temporary throttle and key switch, battery), and lighting, signals,
  horn and brake light are added from the ESP32 module as each is built. Detail: work order §5 /
  checklist PHASE 5B.
- **Brake circuit (2026-09-11) — `revv1-brake-circuit.md`.** One lever signal, three consumers.
  Through 1N4148 steering diodes, each lever pulls the FarDriver `BL` low (motor cut), pulls the gate
  of an `AO3407A` P-FET low so it switches +12 V to the tail STOP lamp (hardware brake light, plan
  D23), and signals the module on IN-05/06. **No firmware sits in the brake path**, so a hung module
  still cuts the motor and lights the lamp; only a few milliamps flow through the lever switch, so a
  microswitch, reed or sinking sensor output can drive it (the MT5 switches are 2-wire normally
  open, M3). It is built in steps
  with the bike: levers → `BL` now, the lamp when the 12 V rail is in, the module input when the
  module is in. Parts ~$5 (module BOM group G).

## Trade-offs baked into this build

- **Voids Ride1Up warranty** on all affected systems.
- **Loses stock pedal-assist (PAS) / class system** — throttle-driven. (Pedals still
  *mechanically* drive the wheel via the motor's single-speed freewheel — there's just no
  motor pedal-assist.)
- **Not a legal e-bike** at 5kW/72V — off-road use, or register as a motor vehicle
  (most US jurisdictions cap e-bikes at 750W).
- **Motor is non-returnable**; warranty requires documented professional-shop install.

---

## Sources (Powerful Lithium, verified June 2026)

- 5,000W 3T motor — https://powerfullithium.com/products/5-000w-3t-high-power-hub-motor
- FarDriver 72V controller — https://powerfullithium.com/products/fardriver-controller-72v
- 3" FarDriver display — https://powerfullithium.com/products/3-e-bike-display
- FarDriver `.heb` layout + parameter map (used to decode the controller exports) —
  https://github.com/jackhumbert/fardriver-controllers (`HEB.bt`, `fardriver.hpp`, `MANUAL.md`);
  local decoder `fardriver/heb_decode.py`
- 72V Cadmus battery — https://powerfullithium.com/products/72v-cadmus-battery-ride1up-revv-1
- Ride1Up parts collection — https://powerfullithium.com/collections/ride1up

## Tier 2 part links

- Grin Torque Arm V6 (clamp-mount) — ✅ ORDERED — https://www.amazon.com/Grin-Technologies-Universal-Ebike-Torque/dp/B0CCZ36RXP
- Grin Torque Arm V7 (heavy-duty) — N/A in 16mm — https://www.amazon.com/dp/B0D1LX21BP
- Grin Torque Arm V5 (light, M5 eyelet — not for 5kW) — https://www.amazon.com/Grin-Technologies-Universal-Ebike-Torque/dp/B0CCXKT228
- **Littelfuse JLLN125 Class T 125A** — ⬜ to order (issue #10) — DigiKey
- **Blue Sea 5007100 Class T Fuse Block 110–200A** — ⬜ to order (issue #10) — https://www.bluesea.com/products/5007100
- Blue Sea 5127 ANL Fuse 150A — ordered, not used (issue #10) — https://www.bluesea.com/products/5127/ANL_Fuse_-_150_Amp
- Blue Sea 5005 ANL Fuse Block — ordered, not used (issue #10) — https://www.bluesea.com/products/5005/ANL_Fuse_Block_with_Insulating_Cover_-_35_to_300A
- ❌ Flipsky 75200 Pro V2.0 — a **VESC speed controller**, the wrong part (issue #3); do not re-order —
  https://theflipsky.com/product/flipsky-75200-pro-v2-0-84v-200a-with-aluminum-pcb-with-key-switch-based-on-vesc-for-electric-skateboard-scooter-ebike-speed-controller/
- Flipsky Antispark Switch Pro V3.0 200A — the actual switch product; **not ordered** (Option B
  chosen instead) — https://flipsky.net/collections/anti-spark-switch
- Key switch 12V–96V 2-wire (**gates the FarDriver KEY wire**; needs a ~2A inline fuse on the
  B+ tap) — https://www.amazon.com/Universal-Ignition-Superior-Anti-Theft-Replacement/dp/B0D8J3MW85
- AMASS XT90-S (5-pack) — ✅ owned (main-line make/break + anti-spark) — https://www.amazon.com/XT90-S-Female-Connector-Battery-Charge/dp/B00RVM8U5W
- Magura MT5 brakes (pads, Royal Blood, bleed and hose procedure: Magura's own service documents)
