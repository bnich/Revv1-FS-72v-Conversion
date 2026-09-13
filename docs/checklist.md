# REVV1 FS → 5,000W / 72V — Full Build Checklist (End to End)

**Vehicle:** Ride1Up REVV1 FS · **Build:** 5,000W 3T hub motor on a 72V system
**Use this as the master step list — DIY or shop.** Work top to bottom; don't skip
verification (✔) steps. ⚠️ = safety-critical. **Updated:** 2026-09-11

> **Golden rules:**
> 1. Battery stays **disconnected** until **first power-up (7.2)** — nothing before that is energized.
> 2. The **XT90-S** is the **only** make/break point for main power. No hot-plugging anywhere
>    else, and **never use the main fuse as the disconnect** — there's no pre-charge there.
> 3. Verify polarity with a meter before **every** main-power connection.
> 4. **Both** Grin V6 torque arms are installed before any power test.
> 5. **The brake cutoff and brake light never depend on firmware.** Each lever pulls the FarDriver
>    `BL` low and switches on the brake lamp through the brake circuit (`revv1-brake-circuit.md`);
>    the ESP32 module only listens. Prove both with the lever tests (brake circuit §7) before riding.
> 6. Torque every fastener to spec and **re-check after the first ride**.

> **Open blockers** (detail in `revv1-build-issues-log.md`):
> - ⚠️ **#12 — M16 axle-nut torque unknown**, and the shop-made axle washers' material is
>   unrecorded. Get the torque from Powerful Lithium (0.3) before 2.6; paint-mark nut and washer;
>   re-checking the nut after the first ride is a hard gate.
> - ⚠️ **#10 — main fuse NOT ordered yet** (checked 2026-09-11): Littelfuse `JLLN125` + Blue Sea
>   `5007100` (0.4), fitted at 4.2. The bike is already moving on the temporary setup below with
>   **no fuse of any kind on the main line**. The ANL 5127 + 5005 pair in the box must not be
>   fitted.
> - ⚠️ **#6 — reverse is still ON in the controller** (`BackEnable = 1`). Fix and verify at 7.3
>   before any ride.
> - ⚠️ **#8 — brake cutoff unproven:** build brake circuit step 1 (`revv1-brake-circuit.md` §6,
>   Phase 5) — check the lever type first (**M3**, §4) — then its level and motor-cut tests (§7.1–§7.2,
>   at 7.3 / Phase 8) are a gate before first ride.
> - **#9 — bracket load path:** the frame side of each printed bracket bears on two 15 mm pads;
>   larger pads or a second contact lower on the frame is undecided (Phase 3).
> - ⏸️ #13 (CAN dash feed) is parked, not a blocker — the dash runs on one-line.

> **Where the bike is (2026-09-11): a temporary running setup.** The FarDriver is bolted to the
> printed brackets, with a **temporary throttle**, a **temporary key switch** and the battery. It
> moves under power. **No fuse of any kind is on the main line.** **No display, no lights and no
> other electronics** are connected. Lighting, signals and horn come from the ESP32 module and the
> brake light from the brake circuit, built incrementally. The phases below describe the finished
> build.
>
> Issues #1 (rotor offset) and #2 (axle washers) closed 2026-09-08 — see Phase 2.

---

## PHASE 0 — Prep, tools, inventory

### 0.1 Tools
- [ ] Metric hex/Allen keys, sockets, combination wrenches
- [ ] **Torque wrench** (small Nm range **and** a larger range that covers the M16 axle nut)
- [ ] **Torx T25** bit/driver (rotor bolts)
- [ ] Multimeter (DC volts, continuity, resistance)
- [ ] **Vernier/digital calipers** (axle stub, washer ID/OD, shim stack, rotor offset)
- [ ] Feeler gauges (rotor/pad gap)
- [ ] High-wattage soldering iron + solder **or** hydraulic lug crimper (for 8–10 AWG)
- [ ] Small crimper for 22–24 AWG signal terminals / bullet connectors
- [ ] Wire strippers, flush cutters, heat-gun
- [ ] Tire irons/levers + valve core tool (moto tire on a 4" rim is stiff — 3 irons help)
- [ ] **Chain tool** + spare master link (chain length may change with the new sprocket position)
- [ ] **Brake bleed kit** matching fluid type (see 6.1) + nitrile gloves
- [ ] Files (round + flat) for torque-arm flat fitment
- [ ] Insulated screwdrivers/pliers for HV work; safety glasses
- [ ] Bike stand / lift, or a way to hold the bike wheel-off-ground
- [ ] Tape measure or string (wheel rolling-circumference measurement)
- [ ] Paint pen (witness marks on the axle nuts, 2.6)
- [ ] ⚠️ **Fire extinguisher** (ABC) within reach for all charging and first-power-up steps

### 0.2 Consumables & small parts
- [ ] Heat-shrink (assorted, incl. large for main leads), zip ties, cable loom
- [ ] Ring terminals / lugs sized to the fuse-block studs and 8 AWG
- [ ] Blue thread locker
- [ ] Dielectric grease
- [ ] Brake fluid (**mineral oil** — see 6.1), brake/contact cleaner, rags
- [ ] 8 AWG silicone wire (if extending main leads)
- [ ] **2× 16" tubes (front + rear) + 1 spare**
- [x] **Axle washers ×2** — fitted 2026-09-08 (issue #2); shop-made, material/hardness unrecorded
      (issue #12). If replaced: **keyed torque washers** for a 16mm axle / 11mm flats (the stock
      washer is this type — D-hole + step into the dropout slot), or on a confirmed-flat dropout
      face **M16 hardened flat washers, DIN 433 (ID 17 / OD 28mm)** or SAE narrow (~27mm), ~3mm.
      ⚠️ **Never ~30mm or ~34mm OD** — it sits cocked. For more clamp spread add thickness, not diameter.
- [x] **6-bolt rotor ring shims, 0.2mm** (M5 ID) — **4 fitted (0.80mm)** 2026-09-08, rotor centred
      (issue #1); 5 were bought
- [ ] **M5×0.8 rotor bolts ~2mm longer** (Torx T25) — only if the shims eat thread engagement
      (⬜ whether they were needed is unrecorded)
- [ ] **R-clip / cotter pin** to suit the cross-drilled axle tip (if used)
- [ ] ⬜ **Brake circuit parts** (`revv1-brake-circuit.md` §3; module BOM group G): **1N4148**
      diodes ×6 (+ spares), **AO3407A** P-channel MOSFET on a SOT-23 adapter, **10 kΩ** ×3,
      **100 nF** ceramic, **1 A mini-blade fuse + inline holder**, 2-pin locking connectors, 22 AWG
      wire. Step 1 (Phase 5) needs only two diodes, the 100 nF and the connectors
- [ ] ⬜ **Controller bracket hardware** — the metal list in Phase 3 (M5/M6 bolts, washers, nylocs,
      limiter tubes, threadlocker)

### 0.3 Resolve open items BEFORE starting
- [x] **Pads identified:** LBN 4-piston = **Shimano Saint/Zee D-type** shape — fit
      **sintered metal (D02S)**, NOT D03S (resin), front + rear
- [x] **Brake fluid type — CONFIRMED MINERAL OIL** (marked on the lever reservoir)
- [x] **Cadmus fits the Center Storage Cage — CONFIRMED** (vendor: Cadmus is "designed to be
      housed" in the Ride1Up Center Storage Cage; cage fits FS; already owned)
- [x] ✔ FarDriver 72450 voltage **CONFIRMED**: 20S/72V-class, max input ~88V — handles the
      84V full charge (its design target; BMS caps at 84V). Don't overvolt beyond 20S.
- [x] Issue #2 (axle washers) and issue #1 (rear rotor ~1mm inboard) — closed 2026-09-08
- [ ] ⬜ **Get the M16 axle-nut torque spec from Powerful Lithium** (issue #12) — ✔ **do not guess
      this number** (see torque table at the end; it's the one value that isn't published here)

### 0.4 Parts inventory (lay everything out, confirm present)
- [ ] 5,000W 3T motor wheel · [ ] FarDriver 72450 (+ throttle, display, enclosure, cable)
- [x] **Cadmus 72V battery — RECEIVED 2026-08-14** · [x] **72V charger — RECEIVED 2026-08-14**
      · [ ] **Littelfuse JLLN125 Class T fuse + Blue Sea 5007100 block** (⬜ to order — issue #10;
        the ANL 5127 + 5005 in the box are under-rated and must not be fitted)
- [ ] XT90-S (main line) · [x] key switch (**purchased 2026-09-07** — "Universal Key Ignition for
      Ebike 12V-96V Anti-Theft") + [ ] ~2A inline fuse · [ ] Grin V6 torque arms ×2 ·
      [ ] Shinko 241 ×2 + tubes
- [ ] XT90-S 5-pack · [ ] brake pads (2 sets) + mineral oil · [ ] wire/heat-shrink/terminals
- [ ] ESP32 module parts and the new bar switch sets (`revv1-module-bom.md`) — for PHASE 5B, as each
      function is built

### 0.5 Battery incoming inspection (received 2026-08-14)
> Do this before the pack goes on a shelf. Shipping damage and wrong/missing parts are far
> easier to claim in the first days. **Nothing here connects the pack to the bike** — golden
> rule #1 still holds.

- [ ] ⚠️ **Inspect before anything else:** dented/deformed case, cracked shell, scorching,
      any smell, loose rattle inside, damaged connectors or chafed leads. **Any heat, swelling,
      hiss, or electrolyte smell → do not charge, isolate outdoors, contact vendor.**
- [ ] **Photograph** the box, packing, pack, label, and every connector **before** unpacking
      further — evidence for a shipping/warranty claim
- [ ] ✔ **Measure resting voltage** at the discharge leads (meter, DC volts). Expect a
      **storage charge ≈ 72–78V** (20S × ~3.6–3.9V/cell). **84.0V = shipped full**;
      **below ~66V (3.3V/cell) = investigate before charging.** → log it at the end
- [ ] ✔ **Confirm polarity and label it** — mark B+/B− on the pack leads now, while it's the
      only thing on the bench
- [ ] ✔ **Identify the main discharge connector type** (and lead gauge / length) — this
      decides the termination into the **125A fuse → XT90-S** line and whether **8 AWG
      extension wire** is needed (still ⬜ to order). Record it in the work-order BOM.
- [ ] ✔ **Identify the charge port / lead** — the plan is to terminate it to **XT90-S**;
      confirm whether the pack already has a connector, and that the charger's plug matches
- [ ] ⚠️ **Test-charge while the charger warranty is alive** (charger warranty is only
      **1 month**): plug charger → pack **only**, on a non-flammable surface, attended,
      extinguisher nearby, no bike involved. ✔ Confirm it terminates at **84.0V** and doesn't
      overshoot. Then bring the pack back down / leave at storage charge if the build is
      weeks out.
- [ ] **Dry-fit the pack in the Center Storage Cage** (bike unpowered, leads not connected) —
      confirm it seats, the cage closes, and the leads exit where the controller/fuse run will
      be. Log any fitment problem in `revv1-build-issues-log.md`.
- [ ] **Storage until PHASE 7:** keep at ~**storage charge (~72–78V), not 84V**, cool and dry,
      leads capped/taped so nothing can short them. Don't store it fully charged for weeks.
- [ ] **File the paperwork:** invoice/serial for the **12-month battery warranty**

---

## PHASE 1 — Teardown (stock removal)

- [ ] **Photograph** the stock wiring, connector routing, and brake-line path (reference)
- [ ] Note the stock throttle/brake-lever connector types (some may be reusable)
- [ ] **Keep the brake levers** — their switches drive the brake circuit; check their type first
      (**M3**, `revv1-brake-circuit.md` §4). The handlebar pods are replaced by the new switch
      sets (plan D20)
- [ ] Disconnect rear motor phase + hall connectors
- [ ] **Drop the pedal chain** off the rear freewheel; then loosen/remove the rear **axle
      nuts** and lift the stock rear wheel/motor out of the dropouts (mind the caliper and rotor)
- [ ] Remove the rear **brake caliper** from its mount (leave the hydraulic line attached;
      hang it so the line isn't kinked) — sets up the rotor transfer
- [ ] ✔ Bag and label all reusable hardware (axle nuts, washers, caliper bolts)
- [ ] Note: the **stock axle washers do not fit the new 16mm axle** (issue #2) and are not
      reused. Keep them anyway until the new wheel is torqued down.

---

## PHASE 2 — Rear wheel / motor (mechanical)

> **Status 2026-09-08:** wheel in the frame with both torque arms and the caliper, rotor centred,
> rear brake works. ⬜ Still open: final axle-nut torque (2.6, issue #12).
>
> Order matters here: **tire first, then rotor, then into the frame.** Levering a stiff moto
> tire onto the rim with the rotor already bolted on is how rotors get bent.

### 2.1 Prep the new motor wheel
- [ ] Inspect the motor, axle threads, and phase/hall cable for shipping damage
- [ ] Identify the **wire side** (phase cable exit) vs the non-wire side
- [ ] ✔ Dry-fit the bare axle in the dropouts — **confirmed 2026-07-09: the axle drops into
      the open-top slot correctly and the flats index.** No filing or dropout modification.
- [ ] ✔ **Measure the threaded axle stub** with calipers (across the threads) and log it with
      the washer fitted (0.2).

### 2.2 Mount the rear tire (wheel out of the bike, no rotor yet)
- [ ] Fit the **Shinko 241 3.00-16** with a fresh 16" tube (respect the rotation arrow —
      it must match the wheel's forward direction once installed)
- [ ] Seat the beads (inflate to the pressure on the sidewall, confirm an even bead line all
      the way round, then set riding pressure)
- [ ] ✔ Confirm the valve stem sits square and isn't being pulled by the tube

### 2.3 Transfer the brake rotor (+ shim for issue #1)
- [ ] Remove the **stock 203mm 6-bolt rotor** from the old hub (note rotor orientation /
      arrow for rotation direction)
- [ ] ⚠️ **Fit the 0.2mm ring shims between the hub flange and the rotor — start with 5
      (~1.0mm)** to push the rotor outboard toward the caliper (issue #1). Ring shims keep all
      6 bolt positions identical, so the rotor stays true.
- [ ] Mount the rotor to the new hub's **6-bolt** flange; **blue thread locker on all 6 bolts**
- [ ] ⚠️ ✔ **Check rotor-bolt thread engagement with the shims in place.** If the bolts
      bottom short, use the **~2mm longer M5×0.8 bolts** (0.2). Do not run partial engagement.
- [ ] Torque rotor bolts in a star pattern (see torque table) ✔ rotor runs true

### 2.4 Mount the wheel in the frame
- [ ] Set the wheel into the dropouts; align so the rotor enters the caliper slot
- [ ] ✔ **Confirm the axle is bottomed in the slot — both sides, 11mm flats fully seated.** A
      washer that sits **cocked** is usually this, not a washer-size problem. It also moves the
      rotor laterally, so it feeds 2.3.
- [ ] ⚠️ **Dry-fit the Grin V6 torque arm (2.5) before finalising the washer.** The stack is
      **`dropout → V6 plate → washer → thick nut → jam nut`**, so the washer bears on the arm's
      machined face, **not the frame**.
- [ ] ⚠️ **Fit the axle washers (0.2)** — one per side, under the nut, seating **flat** and
      bridging the open slot. **Never torque a bare nut against an open-top dropout**, and
      **never torque against a cocked washer** — it point-loads the nut and the torque reading is
      meaningless.
- [ ] ⛔ **Do NOT grind the dropout/frame to make a washer fit.** That weld attaches the tab to
      the highest-stressed joint on the bike; grinding a weld toe starts a fatigue crack, and
      it's the only irreversible option. Fix the **washer** instead: smaller OD (0.2), or grind
      a flat on the washer (D-washer), quenching to preserve its temper.
- [ ] Fit the axle nuts finger-tight (final torque comes after the torque arms, 2.6)
- [ ] ✔ **Spin check + rotor centering:** does the rotor now sit **between** the pads? Adjust
      the shim stack (0.8 / 1.0 / 1.2mm) until it's centered — expect to pull the wheel once
      or twice to dial this in. Log the final stack thickness at the end.
- [ ] ✔ Wheel centered in the frame; tire clears the swingarm/frame on both sides

### 2.5 ⚠️ Install Grin V6 torque arms (BOTH sides) — safety critical
- [ ] Seat the hardened splined insert on the **11mm axle flats** (light file to fit is
      normal — fit should be snug, no slop)
- [ ] Position the arm so the **wire-side arm clears the phase-cable exit**
- [ ] Clamp the arm body to the frame per Grin V6 instructions (frame clamp + hose clamps);
      tighten fully
- [ ] Repeat on the second side
- [ ] ✔ **Both arms installed** — this is golden rule #4; one arm is not acceptable at 5kW

### 2.6 Final axle torque
- [ ] Torque the **primary (thick) axle nut** to spec against the washer/dropout — see torque
      table; ⚠️ **the spec must come from Powerful Lithium (0.3, issue #12)** — never a guessed number
- [ ] Run the **slim jam nut** down hard against the primary nut (double-nut: the jam nut is
      what stops throttle torque and vibration backing it off). Thread locker per the spec.
- [ ] Fit the **R-clip / cotter** through the cross-drilled axle tip if you're using one
- [ ] ⚠️ **Paint-mark each nut and washer** (a line across both) so any movement is visible at
      the post-ride checks (issue #12)
- [ ] ✔ Grab the tire and apply hard forward/back force — **zero axle rotation** in the
      dropout. If it moves, stop and re-anchor.
- [ ] ✔ Re-check rotor centering after final torque (clamping can shift things ~a fraction)

### 2.7 Pedal drive (single-speed freewheel)
- [ ] The motor has a **single-gear freewheel** — reconnect the **pedal chain** to it
- [ ] ✔ Check **chainline** (crank chainring ↔ motor freewheel sprocket align); shim/adjust
- [ ] Set chain length/tension — the new sprocket position may need links added/removed
      (chain tool + master link, 0.1); ✔ ensure the **freewheel is tightened** (it can unscrew
      under pedal torque if loose)
- [ ] ✔ Chain clears the torque arm, phase cable, and tire through its full travel
- [ ] Note: pedaling is functional but vestigial at speed — mainly low-speed start / legal /
      limp-home

### 2.8 Front wheel — tire swap (the second Shinko)
- [ ] Note the **front axle type** (thru-axle vs nutted) and its torque spec before removal
- [ ] Remove the front wheel; fit the second **Shinko 241 3.00-16** + fresh 16" tube
      (rotation arrow!)
- [ ] Seat the beads; set pressure
- [ ] Reinstall the wheel; torque the axle to the **Ride1Up spec**
- [ ] ✔ Tire clears the fork crown, arch, and fender mounts at full compression **and** full
      steering lock — the 3.00-16 is a different profile than stock

---

## PHASE 3 — Controller + motor connections

- [x] **Controller location: under the seat, hung from the four frame lugs on two mirrored
      printed brackets** (one per rail). Model and STLs: `mounts/fardriver-underseat-mount.scad`
      — `bracket_left` / `bracket_right` are **mirror images, not the same part twice**; `plate`
      puts both on one bed. Our ND72450 is the **flat-plate variant (~180 × 120 × 55 mm)**. It
      sits centred across the bike with its **180 mm length spanning both rails**, so each
      bracket takes the **106 mm** hole pair. Plate top **35 mm below the lug centre, touching
      both frame tubes** — the physical minimum, so tyre clearance cannot be found by raising it.
      Centre **9.5 mm rearward** of the lug midpoint (shock clearance); both controller bolts
      land inside the lug span. Full geometry: measurements log.
- [ ] ⛔ **NEVER WELD, DRILL OR GRIND THIS FRAME.** It is heat-treated 6061-class aluminium:
      welding leaves a soft heat-affected zone without post-weld heat treatment, and the
      under-seat area carries the seat-stay and shock loads. **The frame is never the part that
      gets modified — the bracket is.**
- [ ] **Frame side, per bracket:** 2 × M5 into the lugs at **187 mm** centres, **both holes
      round** (a printed shim with two round 5.5 mm holes at 187 fits). ⛔ **Nothing bolts flat to
      this frame** — the lug is recessed in a rounded tube, so every part stands on a **15 mm ×
      6 mm pad** at each hole and touches the frame nowhere else. Stack from the lug face:
      **2 mm metal side panel → 6 mm pad → 10 mm bracket wall**. The metal panel carries the
      clamp, so the frame bolts are 2 mm longer and the limiter tube is **16 mm** (pad + wall).
- [ ] **Controller side, per bracket:** 2 × **M6 × 30** through the baseplate's **plain 7 mm
      holes**: head + washer under the plate → up through a **10 mm limiter** in the 10 mm shelf
      → washer + **nyloc on top of the shelf**. ⛔ **Never thread into the plastic.** Stack ≈
      26.6 mm, so 30 leaves ~3.4 mm spare if the baseplate is 10 mm — ⬜ measure the baseplate at
      the holes. The ribs straddle each controller bolt, leaving **20.5 mm** clear for a 10 mm
      socket.
- [ ] 🔴 **Open (issue #9):** the controller hangs on **both** rails, so each bracket carries
      about half the weight, mostly in shear — but its frame side bears only on its two 15 mm
      pads in line. Decide between larger pads (if the recess allows) or a second contact lower
      on the frame (rubber-faced foot or P-clamp).
- [ ] 🛒 **Metal list for the pair of brackets** (stainless A2, or A4 for corrosion):

      | Qty | Part | Spec | Notes |
      |---|---|---|---|
      | 4 | **M5 bolt** | **× 30 long** (⬜ verify) | Frame. Crosses 19 mm of stack before the thread, so 30 leaves 11 mm of engagement. ⚠️ **Measure the lug's thread depth first** — bottom the bolt out in a blind hole and it never clamps. Drop to 25 if the hole is shallow |
      | 4 | **M5 washer** | plain, under the head | Frame |
      | 4 | **Limiter, M5** | **8.0 OD × 5.0–6 ID × 16 mm** | Frame. ⚠️ A 5.0 nominal bore is zero clearance on an M5 — open it to 5.2 if the bolt will not pass |
      | 4 | **M6 bolt** | **× 30 long** | Controller. Stack ≈ 26.6 mm, 3.4 mm spare |
      | 8 | **M6 washer** | plain, both ends | Controller |
      | 4 | **M6 nyloc nut** | | Controller, on top of the shelf |
      | 4 | **Limiter, M6** | **10.0 OD × 6.4 ID × 10 mm** | Controller, cut to shelf thickness |
      | — | **Threadlocker** | medium strength | ⚠️ **Frame bolts only** — they thread into aluminium with no nyloc to hold them, on an off-road bike |

      Limiters are sold as *unthreaded round spacers/standoffs* (M5 / M6), or cut from tube.
- [ ] ⛔ **Metal compression limiters in every bolt hole are mandatory,** cut **dead flush** — short
      and the plastic takes the clamp, long and the joint cannot pull up. Without them the
      joint goes slack within weeks. **Do not ride it without.**
- [ ] ⚠️ **Material: ASA** (or PC / PETG-CF / PA-CF). **NEVER PLA** — it creeps at room
      temperature and softens near 60 °C, which a summer garage plus a warm controller reaches.
- [ ] ⚠️ **Print orientation is load-bearing:** vertical panel FLAT ON THE BED, shelf standing up,
      so layer lines run along the load path through the corner. Never shelf-down. 5–6 perimeters,
      40–60% infill, solid through the ribs, 100% around every bolt pocket.
- [ ] ✔ **PROOF-LOAD BEFORE TRUSTING IT:** bolt it up and hang **15–20 kg** from the shelf for an
      hour (~8× the controller). If it sags and stays sagged, it will creep in service — reprint
      thicker or in better material.
- [ ] Bolt the controller's baseplate **directly to the two printed brackets** (M6, washer + nyloc
      on top of each shelf). The supplied enclosure is too large and is not used
- [ ] Routing: **wires and connectors pointing down**, **out of the rear tyre's spray**, phase
      wires **short and routed away from signal wires**
- [x] ✔ **Connectors confirmed to mate (2026-08-15)** — no bullet connectors or adapter needed.
      The motor ships with **two hall pigtails in different connector standards** ("Hall-sensor
      (2 sets)"): **one mates the FarDriver harness directly, the other is a spare.**
- [ ] Connect the **3 phase wires** motor→controller (color/label match if marked)
- [ ] Connect the **6 motor-sensor wires** motor→controller — **5 hall + 1 motor-temp**.
      FarDriver colors: **yellow = Hall A · green = Hall B · blue = Hall C · red = Hall+ ·
      black = GND · white = motor temp**
- [ ] ✔ **Cap and insulate the unused second hall pigtail** — exposed pins next to 72V wiring;
      don't leave it bare or loose in the bundle
- [ ] ✔ **Cap and individually insulate every unused wire** in the FarDriver harness (cruise,
      boost, reverse, 3-speed, high brake, etc.) — no bare ends bundled together
- [ ] ✔ Strain-relief and loom the motor cable; keep clear of the tire and rotor
- [ ] Note: exact phase/hall order is corrected in software via **hall self-learn**
      (7.3) — don't agonize over phase order now

---

## PHASE 4 — High-current power system  ⚠️ (battery still disconnected)

### 4.1 Plan & mount
- [ ] Mock up the layout: **Battery → 125A Class T fuse → XT90-S → Controller B+**, Battery → Controller B−
- [ ] Mount the **5007100 Class T block** (four-stud bolt-down) in a dry, secured spot. Mount the
      **XT90-S where it can be reached and visually inspected without disassembly** — it's both
      the disconnect and a wear item to watch for heat
- [ ] ⛔ **The Flipsky FSESC 75200 is NOT installed** (issue #3 — it's a motor controller, not a
      switch). Leave it boxed. **Never land its blue/green/yellow leads on the FarDriver's
      phase studs** — that shorts two controller output stages together
- [ ] Keep main-lead runs short; use **8 AWG** if extending

### 4.2 Build the main line (no battery connected)
- [ ] Crimp/solder ring terminals; make the **B+** chain: battery+ → fuse block → **XT90-S** →
      controller B+
- [ ] ⚠️ ✔ **XT90-S orientation — the FEMALE half goes to the battery/fuse side.** The
      **5.6Ω pre-charge resistor lives in the female housing** and must be fed from the battery
      to work. Male half → controller B+. Backwards = no pre-charge at all. (Female-to-battery also
      keeps the always-live side recessed, with no exposed pins.)
- [ ] Make the **B−** lead: battery− → controller B−
- [ ] ⚠️ **Fit only the `JLLN125` + `5007100`** (issue #10). The Blue Sea 5127 ANL link and 5005 block
      in the parts box are rated **80V / 32V DC**, below the 84.0V pack — never fit them. ⛔ Never a
      175/200A link either — over the block's 160A rating.
- [ ] Install the **125A Class T fuse** in the block (leave it out until first power-up if you
      prefer an extra safety gap)
- [ ] ✔ **Torque the fuse-block studs to 72 in-lb max** (5007100 spec); no lug can be turned by
      hand. A loose joint is a fire, not a fault.
- [ ] ✔ **Refit and secure the block's cover** — the 5007100 is ignition-protected only with its
      cover in place
- [ ] Build the **KEY circuit:** tap B+ **downstream of the XT90-S** → **~2A inline fuse** →
      **2-wire key switch** → FarDriver **KEY** wire
      - ℹ️ **Later, with the module:** the key switch feeds the module and the **start latch**, and the
        controller comes up only with the run switch ON and the start button held ~2 s
        (`revv1-esp32-module-plan.md` §3.2.5a, D24). Wire it straight through for now
      - **Identifying KEY:** FarDriver calls it **60VKEY** — an **orange wire in a 1-pin
        housing**, usually the **only single-pin lead in the harness**
      - ⚠️ **KEY carries FULL battery voltage (~72–84V), not 12V.** Jumpering the wrong wire to
        B+ puts 84V on a signal input and destroys the controller. ✔ Before connecting, meter
        the candidate to B− with the pack live: it must read **~0V**. If it doesn't, stop
      - ✔ Key switch must be rated for **84V** (the spec'd unit is 12–96V). A 12V automotive
        key switch will arc and weld on this line
      - 💡 **Bench/config shortcut:** to power the controller before the key switch arrives,
        jumper **KEY → B+ through a ~2A fuse**. Nothing wakes up — and Bluetooth never
        appears — until KEY sees battery voltage
- [ ] ⚠️ ✔ **The ~2A inline fuse is not optional** — the KEY tap is a thin wire hanging off an
      otherwise unfused 84V line. If it chafes without that fuse, it's a fire
- [ ] ✔ Confirm the key switch is **maintained** (stays on when turned), not momentary
- [ ] ✔ ⚠️ **Count the key switch's wires.** The build assumes a **2-wire** switch, but many
      *"anti-theft"* ebike ignitions are **3- or 4-wire** (battery / ignition / accessory, or a
      separate lock-alarm circuit). ⬜ The purchased unit's wire count is unverified. If it is not
      a plain 2-wire, identify each wire with an ohmmeter before connecting anything — only the
      pair that closes on key-ON is used; **cap the rest**
- [ ] ✔ Heat-shrink every main-lead joint; no exposed conductor; correct polarity labeled

### 4.3 Battery mounting + charge port (still not connected)
- [ ] Mount the **Cadmus** in the Center Storage Cage; secure against vibration
- [ ] Build the battery-side termination to match the fuse/XT90-S line (polarity ✔ with
      meter) — but **leave the final battery connector UNMATED and capped.** "Terminate" here
      means make the connector up, not plug it in.
- [ ] Terminate the **charge port** with an **XT90-S** (battery side); confirm it matches the
      72V charger's plug
- [ ] ✔ Do NOT connect the battery yet — that happens at first power-up (**7.2**)

---

## PHASE 5 — Controls

- [x] **Throttle: the FarDriver-bundled twist throttle is INSTALLED** (2026-09-06). It mates the
      harness throttle lead directly. Bar controls come from the new switch sets (plan D20)
      - **FarDriver harness throttle lead (3-pin):**
        **red-white = ACC+ (power) · green-white = SV (signal) · black = GND**
      - **The throttle's 2-pin RED BUTTON lead → ESP32 module → FarDriver boost input** (plan **D4**,
        hold / toggle modes). Capped until the module exists
      - ⚠️ **Identify every harness lead by COLOUR, not pin number** — FarDriver's published
        diagrams for the ND72450 **disagree on pin numbers** (throttle SV is pin 14 in one, 27
        in the other). The colours agree; the numbers don't. Full colour table: work order §3.0
      - ⚠️ **Wire by FUNCTION, not by color.** The FarDriver throttle uses **yellow** for signal
        where the harness uses green-white
      - ⚠️ Do **not** plug the red-button lead into whatever 2-pin fits: brake, cruise, boost,
        reverse and 3-speed all share the same housing. Match the **labeled** lead
- [ ] ⚠️ **Brake circuit, step 1 — motor cut** (`revv1-brake-circuit.md` §2, §6):
      - ✔ **First check each lever's switch type (M3, §4).** A normally-open two-wire switch works as
        drawn; a normally-closed switch or a three-wire sensor needs the §4 notes before wiring
      - Wire each lever through its own **1N4148** (D1L / D1R, cathode toward the lever) to the
        FarDriver **`BL`** (yellow/green); lever return to **B−**. Fit the **100 nF** from `BL` to B−
        at the controller. Heat-shrink the diodes in-line
      - The grey **`BH`** stays capped
      - Tests §7.1 (levels) and §7.2 (motor cut) come at 7.3 / Phase 8 — **issue #8's gate before
        riding**. Steps 2 (brake lamp) and 3 (module sense) come with the module (PHASE 5B)
- [ ] **Keep the stock brake levers** (hydraulic masters + integrated cutoff switches) as-is
- [ ] Mount and connect the **FarDriver display** (none is fitted on the temporary setup)
      - **Display = Powerful Lithium 3"** (Chaojie `CJ-V3-01`, 376×960, 1000 cd/m², rated
        **DC 12–120 V** so it runs straight off pack voltage — no DC-DC).
        https://powerfullithium.com/products/3-e-bike-display
      - ⚠️ **It does NOT plug into the FarDriver harness** (issue #4). The 9-pin
        **DJ7091A-2.8-11** is a *vehicle-harness* connector, not a controller connector — it
        expects switched power, a controller signal, CAN, and lighting telltales from a complete
        bike loom. **Break it out wire by wire per the table below.**

      **DJ7091A-2.8-11 pinout (vendor diagram) — and what each does in THIS build:**

      | Pin | Wire | Vendor function | This build |
      |---|---|---|---|
      | 1 | Orange | Left turn signal (0–15V) | ⚠️ **CAP — do not connect** |
      | 2 | Black | "Electric door lock" (= **ignition/key switch**) positive, 0–150V | **POWER IN** → switched B+, downstream of the key switch |
      | 3 | Green | Battery negative (0V) | **Ground** → B− |
      | 4 | Light blue | Right turn signal (0–15V) | ⚠️ **CAP — do not connect** |
      | 5 | Blue | Headlight signal (0–15V) | ⚠️ **CAP — do not connect** |
      | 6 | Red | Reserved / TBD | **CAP** |
      | 7 | Green-black on our harness (manual: green-yellow) | **CAN L** (0–5V) — a FarDriver-CAN pair (Chaojie: the panel supports the FarDriver CAN 18 protocol). Terminates at 132 Ω | ⚠️ **LEAVE UNCONNECTED.** Nothing drives it on this bike — the FarDriver is a non-CAN SKU. ⏸️ Reserved for the ESP32 module's CAN feed, which is **parked** (issue #13 / plan D19) |
      | 8 | Red-black | **CAN H** (0–5V) — same pair | ⚠️ **LEAVE UNCONNECTED** — same |
      | 9 | Purple | **Motor controller ONE LINE** (0–15V) | **→ FarDriver BROWN lead (2-pin plug) — proven 2026-09-06, issue #7.** Only wire going to the controller. ⚠️ **NOT purple-to-purple** (`SPA`, analog) and **not the light-blue `SPD`** (dead on this unit — cap it) |

      - ⛔ **NEVER match display↔controller by wire colour.** All nine display wires have a
        same-colour FarDriver counterpart and **none is the correct match** (full table: work
        order §3.1). Worst two: **orange↔orange** puts 84V on a 15V input and **destroys the
        display**; and the manual's **green-yellow** for pin 7 (CAN L) matches the FarDriver's
        **yellow-green `BL` Low Brake** — joining them **silently disables the brake cutoff**. Only
        three connections exist: **9 → BROWN lead**,
        **2 → switched B+**, **3 → B−**
      - 🔑 **Power (pin 2) and the FarDriver KEY wire share the same source** — both hang off
        the switched side of the key switch. Give the display its own small inline fuse
        (separate from KEY's — a shared fuse means a display short also kills ignition)
      - 🔧 **Build a break-out adapter** using a mating **DJ7091A-2.8-11**; **populate only
        positions 2, 3, 9** and leave the rest **empty**. Empty cavities can't be miswired, and
        the display stays unmodified
      - ⚠️ **Pins 1, 4, 5 stay capped** until the ESP32 module's 12V lamp feeds exist; then each
        is fed from the lamp feed it mirrors through ~1 kΩ (plan §6). They are **0–15V** inputs —
        never connect them to anything at pack voltage
      - ℹ️ The display is **readout only** (speed, volts, amps, fault codes). **All configuration
        is via Bluetooth in the FarDriver app** — see 7.3
      - ✔ **Link test (issue #7):** once powered, the display must show the controller's gear
        (**D**, not N), and speed must respond when the raised wheel is hand-spun. Voltage/% alone
        proves nothing — the display measures those itself — and **C°/M° stay 0 on the one-line**
        (it carries no temperatures; 3" pinout doc §4). If it shows N / 0: check pin 9 is on the **BROWN**
        lead (not light blue — dead on this unit) with a real crimp/solder joint, and
        **`Special Frame` = 21** on the app's One Line page (246 = RS485 PC mode = no output).
        Proven working 2026-09-06
      - ✅ **Speed scale (issue #7b) — settled:** `Speed pulse` **1** was the whole fix; the
        tire-page transmission ratio stays **1.000**. Final check vs GPS in PHASE 9
      - ✔ Weatherproof the connector if it sits exposed; leave slack for full steering lock
- [ ] Route all control wiring tidily; loom and zip-tie clear of moving parts
- [ ] ✔ No cable tension at full steering lock (turn bars lock-to-lock)

---

## PHASE 5B — Lighting, signals, horn and brake light (ESP32 module, built incrementally)

> The bike's lamps, turn signals and horn are driven by the **ESP32 module**
> (`revv1-esp32-module-plan.md`), added one function at a time as the module is built. Bar controls
> come from the new switch sets (plan D20–D22). The brake lamp comes from the brake circuit, not
> from firmware (golden rule #5).

- [ ] Wire each module output as it is built — running light, low beam, high beam, tail, turn
      signals, hazard, horn (plan §6). ✔ Each passes the Phase 8 lighting sweep before the next
      is added
- [ ] ⚠️ **Brake circuit, step 2 — brake lamp** (`revv1-brake-circuit.md` §6), once the module's
      12 V rail is in: add **Q1 (AO3407A)**, **R1 10 kΩ**, **D2L / D2R** and the **1 A fuse F1** →
      tail STOP (red) wire. ✔ Test §7.3: each lever lights the lamp; release turns it off
- [ ] **Brake circuit, step 3 — module sense**, once the module is in: add **D3L / D3R** → IN-05 /
      IN-06 with **10 kΩ** pull-ups. ✔ Test §7.4
- [ ] (Optional) a **standalone FarDriver 3-speed mode switch** for bar-selectable ride modes — or
      change modes in the app

---

## PHASE 6 — Brakes  ⚠️

### 6.1 Fluid type — CONFIRMED MINERAL OIL
- [x] Reservoir cap reads **MINERAL OIL**
- [ ] ✔ Use bicycle **mineral oil** + a mineral-compatible bleed kit (never DOT)

### 6.2 Pads + rotor
- [ ] Install **Shimano Saint/Zee D-type sintered pads (D02S** or equiv, NOT D03S resin) —
      **one set front, one set rear** (same caliper type both ends)
- [ ] Reinstall the **rear caliper** to its mount; center it over the 203mm rotor
- [ ] ✔ **Rear rotor/pad alignment (issue #1):** the rotor should sit centered between the
      pads thanks to the 2.3 shim stack. Fine-tune with the caliper mount first; add/remove a
      0.2mm ring only if the caliper can't take it out.
- [ ] ✔ Pads contact the rotor squarely with an even gap; rotor spins free, no rub
- [ ] ✔ Front caliper/rotor untouched and still aligned after the 2.8 wheel removal

### 6.3 Bleed (front and rear)
- [ ] Bleed each circuit per the brake maker's procedure (caliper→lever, no air)
- [ ] (Optional) fit **braided steel lines** before bleeding for a firmer lever
- [ ] ✔ Lever is firm (no sponginess); holds pressure; no leaks — **both ends**
- [ ] ✔ Both **brake cutoffs** will be verified to cut motor power in **PHASE 8**

---

## PHASE 7 — First power-up + FarDriver configuration  ⚠️

> **This is where the battery gets connected for the first time.** Wheel **off the ground**
> for everything in this phase. The FarDriver app talks to the controller over Bluetooth, so
> the controller must be powered — that's why 7.1/7.2 come before the config in 7.3.
> Do this outdoors or in a ventilated space, extinguisher within reach.

### 7.1 Pre-power inspection (battery still disconnected)
- [ ] ✔ Meter the main leads for **shorts** (B+ to B−) — should read open/high resistance
- [ ] ✔ Confirm polarity end to end one more time
- [ ] ✔ Brake circuit step 1 fitted (Phase 5); grey `BH` capped
- [ ] ✔ All fasteners torqued; nothing contacting the tire/rotor; wiring loomed
- [ ] ✔ **Both torque arms on**, axle nuts at final torque (golden rule #4)
- [ ] ✔ Wheel **off the ground** and the bike stable in the stand

### 7.2 First energize
- [ ] ⚠️ Confirm the **key switch is OFF** and the **XT90-S is unmated**
- [ ] Insert the **125A Class T fuse** if you left it out — **fuse goes in BEFORE the XT90-S is mated**,
      never the other way round
- [ ] **Mate the XT90-S** (pre-charge handles inrush — expect a small click, not a bang)
- [ ] Turn the **key switch ON** — listen/watch for issues
- [ ] ✔ Display powers up; **no fault codes**; no smoke/heat/odor — if anything is wrong,
      **key OFF and unplug the XT90-S immediately**
- [ ] ✔ Meter the pack voltage at the controller B+/B− and confirm it matches the pack

### 7.3 FarDriver configuration (Bluetooth app, wheel off the ground)
- [ ] Install the app: **iOS = "FarDriver"** · **Android = "Nanjing FarDriver" (南京远驱)**.
      Grant all permissions and enable Bluetooth. Registration uses a **phone number +
      a password you choose** — ✔ record the password in the handover log
- [ ] ✔ **Check whether the controller has built-in Bluetooth or needs the BT dongle.** Some
      FarDrivers need an external module; if one came in the loaded bundle, that's what it is
- [ ] Pair the app over Bluetooth (**controller must be powered — that's why 7.2 comes first**)
- [ ] **Import `fardriver/ND72450_AKHA86_20260908_revv1-import-noreverse.heb`** (app main menu →
      open heb → **Save** → key-cycle → re-export). It is the 2026-09-08 post-session export with
      only `BackEnable` changed to 0 (**reverse OFF**, issue #6). The steps below are *verify*
      steps — don't re-type values.
      ⚠️ **Do not import the 2026-09-06 files** (`…20260906_revv1-import-v2.heb` and earlier) — they
      predate the 2026-09-08 changes and would put `Brake` back to `7-Disabled` and `SpeedPulse`
      back to 11
- [ ] ✔ Verify **72V (20S) rated / 34Ah / Lithium** (full charge 84.0V)
- [ ] ✔ Verify **LVC = 60.0V** (+2V derate). **Over-voltage protect stays at the factory 90.7V /
      88.7V** — ⚠️ do **NOT** set "~84V": a full pack sits at 84.0V and would trip it
- [ ] ⚠️ ✔ Verify **battery-current limit ("MAX LINE CURR") = 80A** — **before spinning anything.**
      It's the motor-protection cap **and** what keeps the ~90A XT90-S inside its rating
- [ ] ✔ Verify **phase-current limit = 200A** (of the 72450's 450A) — conservative, leave it
- [ ] ⚠️ ✔ Verify **Reverse / REGear = OFF** — the pedals are chain-linked through the freewheel;
      powered reverse spins the cranks backwards. ⚠️ **Currently still ON** (issue #6): `BackEnable
      = 1` in every controller export, and an in-app change on 2026-09-08 did not take. The import
      above turns it off — confirm it in the fresh export. If the import does not take, set
      `Backward Pin` → `13-Invalid` (`revv1-fardriver-nd72450-pinout.md` §3). **No riding until
      this is OFF**
- [ ] ✔ Verify **throttle 1.1V = 0% / 3.9V = 100%** and that it reads **~0.8V at rest** = 0% (avoids a
      startup over-throttle fault). Sweep confirmed with the FarDriver throttle
- [x] ✅ **Brake = `0-StopWhenGround`** (set 2026-09-08, issue #8) — confirmed in the export.
      Braking grounds **`BL`** (yellow/green) through the brake circuit (`revv1-brake-circuit.md`
      §5); the grey **`BH`** stays capped. ⛔ Never a **`P+`** variant (bundles Park) and never
      **`1-StopWhenFloat`** (inverts it)
- [ ] ⚠️ ✔ **STILL OWED — brake circuit tests §7.1 and §7.2.** Levels: `BL` ≈ 3.3 V released,
      **below 0.8 V** with each lever pulled. Motor cut: each lever stops the motor (the temporary
      throttle can drive the wheel for it). **This is a gate before first ride**: until it passes,
      the levers slow the bike mechanically while the motor keeps driving against them
- [ ] ✔ Verify **`Speed pulse` = 1** on the One Line page, with **`Special Frame` = 21** (issue #7)
- [ ] **Hall self-learn is already done** (export shows phase offset **212.0°**, learned). Re-run
      **only** if the motor or phase wiring was disturbed: "test angle" / "self-study" on the angle
      page — wheel off the ground, nothing near the tire, ⚠️ **current caps already set**
      - The controller beeps **2 short + 1 long, repeating**, while in the self-learn state —
        that cue means it's running, not faulting. Follow the app's prompts to completion
- [ ] ✔ Verify **motor-temperature protection**: `NTC_PTC = 5-KTY83-122`, protect **120°C** / restore
      **90°C** (MOS 100/80°C). The sensor is the white wire in the hall connector (issue #5)
- [ ] ✔ Confirm the motor temp **reads plausible ambient**, not a rail value. A rail reading
      means the sensor isn't being seen → **don't rely on the protection** until it's fixed
- [ ] ✔ Verify wheel = **80 / 100 / 16** (tire width / aspect / rim = 3.00-16), transmission ratio
      **1.000**. The app has **no circumference field**; the speedo gets checked against GPS in
      PHASE 9. (Still measure the rolling circumference — it's the number you compare against)
- [ ] ✔ Save config; **export a fresh .heb into `fardriver/`** and write every value into the log
      at the end for handover

---

## PHASE 8 — Commissioning / acceptance test (wheel off the ground)  ⚠️

- [ ] Gentle throttle → wheel spins **smoothly, correct direction**, no cogging/noise
- [ ] ✔ If the wheel spins **backwards**: re-run hall self-learn or reverse in the app —
      **do NOT just swap phase wires blindly**
- [ ] ⚠️ ✔ **Each brake lever cuts motor power** (brake circuit §7.2, owed from 7.3) — until it
      passes, the cutoff is unproven: **no first ride**
- [ ] ✔ **Each brake lever lights the brake lamp** (brake circuit §7.3) — once step 2 is in
- [ ] ✔ **The module reads each lever** (brake circuit §7.4) — once step 3 is in
- [ ] ✔ Throttle returns to zero cleanly; no runaway; no cutout at part throttle
- [ ] ✔ Lighting sweep for each module output as it is built: running light, low beam, high
      beam, tail/brake, turn signals, hazard, horn
- [ ] Check the controller/motor for excess heat after a minute of light spinning
- [ ] ✔ **Key switch OFF drops the whole system cleanly** and the display goes out
- [ ] ✔ **Feel the XT90-S** after the spin test — it should be cool. Any warmth here is an
      early warning that the ~90A connector is near its limit

---

## PHASE 9 — Road test (progressive)

- [ ] ⚠️ Gear: helmet + protection. First ride in a safe, open, private/off-road area.
- [ ] **Walking-speed** throttle test; confirm brakes haul it down
- [ ] Low-speed laps; test both brakes hard from ~15 mph (bed in pads)
- [ ] Gradually increase speed; feel for wobble, brake fade, cutout, or heat
- [ ] ✔ Confirm the **speedo against a GPS app**; if it's off, correct the app's **transmission
      ratio** (tire fields stay 80/100/16 — there is no circumference field)
- [ ] ✔ Return; **immediately re-check** axle-nut torque (paint marks unbroken — issue #12),
      torque arms, rotor bolts, main-lead connections, and feel the motor/controller/battery for
      hot spots
- [ ] Log anything odd in `revv1-build-issues-log.md` while it's fresh

---

## PHASE 10 — Final & handover

- [ ] Re-torque **all** fasteners after the shakedown ride
- [ ] Tidy/secure any wiring that moved; final loom + zip-tie
- [ ] Confirm charger works on the bike: plug the 72V charger, verify it charges and tapers
      at ~84V
- [ ] Record final config: **battery-current cap**, LVC/HVC, wheel size, app password — and
      **export the final .heb into `fardriver/`** and diff it against the 2026-09-08 import file
- [ ] File: this is a warranty-voiding, non-stock high-power build — note off-road /
      registration status per local law
- [ ] If the bike will sit for weeks, leave the pack at **storage charge (~72–78V), not 84V**
- [ ] **Storage isolation — order matters.** Disconnect: **key OFF → unplug XT90-S →
      (optional) take the main fuse out.** Reconnect in reverse: **fuse in → mate XT90-S →
      key ON.** ⚠️ Never make/break at the fuse with the XT90-S still mated — that bypasses
      pre-charge and arcs the fuse contacts. Never break either one under load.
- [ ] (Optional upgrade) if the XT90-S shows heat or daily unplugging gets old, fit a proper
      **anti-spark master switch** (e.g. Flipsky Antispark Switch Pro V3.0 200A, ~$90) — ⚠️ check
      its DC voltage rating against the 84.0V pack first

---

## Torque reference

> ⚠️ These are **starting points, not gospel** — the part's own documentation always wins.
> Confirm each against Grin's torque-arm sheet, the rotor-bolt maker, and Ride1Up's manual.

| Fastener | Torque | Source / note |
|---|---|---|
| **M16 rear axle nut** | 🔴 **CONFIRM WITH POWERFUL LITHIUM** (issue #12) | The one number not to guess. It's a 16mm/M16 hub-motor axle, not a bicycle nut — bike-forum numbers don't apply. Ask before final assembly (0.3). |
| Axle jam (slim) nut | run down hard against the primary nut | double-nut; it's a lock, not a clamp |
| 6-bolt rotor bolts (M5) | **~4–6 Nm**, star pattern, blue loctite | Shimano specs **2–4 Nm** for their own bolts — follow whichever brand you're using |
| Caliper mount bolts | ~6–10 Nm typical | per Ride1Up / caliper maker |
| Grin V6 clamp hardware | per **Grin's instructions** | ships with the arms — don't substitute |
| Fuse-block studs | **72 in-lb max**, snug + no hand movement | Blue Sea 5007100 spec; re-check after first ride |
| Front axle | per **Ride1Up spec** (differs thru-axle vs nutted) | note it before removal (2.8) |

---

## Measurements log — fill this in as you go

| What | Value | Date |
|---|---|---|
| Pack resting voltage on arrival (expect 72–78V) | | |
| Charger termination voltage (expect 84.0V) | | |
| Pack main discharge connector type / lead gauge | | |
| Threaded axle-stub diameter (calipers) | | |
| Axle washer fitted: stock/material, hardened? (ID / OD / thickness) — issue #12 | shop-made (angle grinder) | 2026-09-08 |
| Rotor shim stack used (0.2mm × ?) | **4 × 0.2mm = 0.80mm** | 2026-09-08 |
| Rotor bolt length used (stock / +2mm) | | |
| Axle nut torque used (and where the spec came from) | | |
| Brake levers (M3, brake circuit §4): wire count, NO / NC, released / squeezed Ω, own pair per lever? | | |
| `BL` level, released / each lever pulled (brake circuit §7.1 — expect ~3.3 V / below 0.8 V) | | |
| Controller location | under the seat, on the four frame lugs, two printed brackets | |
| Mount boss: thread size (fwd / aft — may differ) | **M5** (screw major dia 4.84 mm) | 2026-09-06 |
| Mount boss: centre-to-centre spacing, one side | **187 mm** (both holes round; a printed shim with round 5.5 mm holes at 187 fits) | 2026-09-07 |
| Mount boss: lug is **RECESSED in a rounded tube** — no part can bear flat; every part stands on a **15 mm × 6 mm pad** per hole | **6 mm standoff** | 2026-09-07 |
| Mount boss: lug OD (consistent with a 16 mm lug carrying an M5 thread) | 15.98 | 2026-09-06 |
| Mount boss: pair mirrored on the other side? | **YES** | 2026-09-06 |
| Mount boss: front triangle or moving rear triangle? | **FIXED frame — does not move with suspension** | 2026-09-06 |
| FarDriver baseplate: plate size | 180 × 120 mm | 2026-09-07 |
| FarDriver baseplate: hole pattern | **106 mm along the bike × 168 mm across** (holes 7 mm in from the long edges, 6 mm from the ends; corroborated by the rig's front reach and by fit) | 2026-09-08 |
| FarDriver baseplate: holes are **plain 7 mm through-holes**, not threaded → M6 bolt + metal limiter + nyloc on top of the shelf. ⛔ never thread into the plastic | 7 mm | 2026-09-07 |
| Controller orientation | **180 mm ACROSS the bike (spanning both rails) · 120 mm along the rail** — each bracket takes the **106 mm** hole pair | 2026-09-08 |
| Controller mounting | **TWO mirrored brackets, one per rail; controller bolts to BOTH**, sits centred, protrudes equally past each side | 2026-09-07 |
| **Lug span, FRONT pair** | **105 mm** (tape, lug face to lug face) | 2026-09-08 |
| **Lug span, REAR pair** | **93.52 mm** — rails narrow going rearward, so reach is 31.50 mm front, 37.24 mm rear | 2026-09-08 |
| **Side panels the bracket bears on** | **2 mm metal, each side.** Stack from the lug face: **panel 0–2, standoff pad 2–8, bracket wall 8–18.** Frame measurements were taken with the panels off, so the panel adds to the stack. Frame bolts 2 mm longer; limiter **16 mm** (pad + wall) | 2026-09-08 |
| **Contact at drop 35** | The controller's alloy plate **touches both frame tubes — zero gap**. The shelf sits outboard of the tubes' widest point with clear air above; no duct over the plate except the open channel between the tubes | 2026-09-08 |
| **Drop** — lug centre line to controller top face | **35 mm** — the physical minimum (plate hard up under the frame); tyre clearance cannot be found by raising the controller | 2026-09-08 |
| ⬜ **Controller baseplate thickness at the bolt holes** — sets the M6 bolt length | | |
| **Controller fore/aft shift** | **9.5 mm REARWARD of the lug midpoint** — both controller bolts land inside the lug span (50 mm behind the front lug, 31 mm ahead of the rear) | 2026-09-08 |
| Rear suspension travel (spec 50 mm — verify, non-catalogue shock fitted) | | |
| Tyre-to-controller gap, static | | |
| Tyre-to-controller gap at FULL compression (must stay positive) | | |
| Tire rolling circumference (mm) | | |
| FarDriver: battery current cap / phase current | 80A / 200A | 2026-09-06 |
| FarDriver: LVC / HVC | 60.0V / 90.7V factory | 2026-09-06 |
| FarDriver: reverse gear | ⚠️ **still ON in the controller** (`BackEnable = 1`) — must be OFF, issue #6 | 2026-09-08 |
| First-ride re-torque completed | | |

---

## Quick reference — build specs

| Item | Spec |
|---|---|
| Motor | 5kW 3T hub · 16mm axle / 11mm flats · 6-bolt rotor · hall · tubed · single-speed freewheel (pedals work) |
| Controller | FarDriver 72450 · 48–72V (20S) · 200A line / 450A phase |
| Battery | 72V/20S Cadmus · Molicel P42A 34Ah · 84.0V full · smart BMS |
| **Current cap** | **80A** battery current · 200A phase — **set** (export 2026-09-06); do not raise |
| **FarDriver config** | import `fardriver/ND72450_AKHA86_20260908_revv1-import-noreverse.heb` (fw HA86; never the 2026-09-06 files) · **reverse must be OFF — still ON in the controller (issue #6)** · brake `0-StopWhenGround` · KTY83-122 120/90°C · LVC 60V · OVP 90.7V factory · hall learned 212.0° · wheel 80/100-16 · `Special Frame` 21 · `Speed pulse` 1 |
| Fuse | **Littelfuse `JLLN125` Class T 125A** (125 V DC, 20 kA DC) + **Blue Sea `5007100`** block (160 V DC, bolt-down) — ⬜ to order (issue #10). Not the 5127/5005 ANL pair (under-rated) |
| Main switch | **XT90-S** (main-line make/break + anti-spark pre-charge; female half → battery side) · daily on/off via **2-wire key switch on the FarDriver KEY wire** (+ ~2A inline fuse). ⚠️ XT90 ~90A cont. vs the 80A cap — watch for heat |
| Torque arms | Grin **V6** ×2 · clamp-mount · both sides |
| Rotor | stock **203mm 6-bolt**, reused · **+~1mm ring shims** (issue #1) |
| Axle hardware | shop-made washers ×2 (issue #2; material unrecorded — issue #12) · double-nut · R-clip · torque ⬜ from Powerful Lithium |
| Tires | Shinko 241 **3.00-16** ×2 (front + rear) + 16" tubes |
| Brake fluid | **MINERAL OIL** (confirmed) |
| **Brake circuit** | `revv1-brake-circuit.md` — each lever, through 1N4148 steering diodes: pulls FarDriver `BL` low (motor cut, `Brake: 0-StopWhenGround`) · lights the brake lamp via P-FET `AO3407A` (no firmware) · signals the module (IN-05/06) · grey `BH` capped |
| **Lighting** | ESP32 module, built incrementally — lamps, signals, horn (plan §6); brake lamp from the brake circuit |
| **Throttle** | **FarDriver-bundled twist throttle installed** (2026-09-06) · red button → ESP32 module → boost, hold/toggle (plan D4) · bar controls from the new switch sets (D20) |
| **Controller mount** | two mirrored printed brackets (ASA, never PLA) on the four under-seat lugs · M5 × 4 frame side, M6 × 30 × 4 controller side · metal limiters in every hole |
