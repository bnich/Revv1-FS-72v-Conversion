# REVV1 5kW Build — Problems & Resolutions Log

Running log of fitment/install problems found during the physical build, with root cause and
resolution. Newest issues at the top. Update **Status** as each is worked; when a resolution
changes a build fact, **propagate it to the three canonical docs** (build sheet / checklist /
work order) per `CLAUDE.md`.

**Status key:** 🔴 Open · 🟡 Diagnosing / partly resolved · 🟢 Resolved · ⏸️ Parked

## Index

| # | Issue | Found | Status |
|---|-------|-------|--------|
| 14 | ⚠️ **Both rims are far too wide for the 3.00-16 moto tyres** — front 32 % / rear 52 % over the tyre’s widest approved rim; the stretched carcass is the prime suspect for the bumpy ride | 2026-09-17 | 🟡 **Replacement tyres bought 2026-09-17** — BDGR 20×4.5 rear, Huntsman 20×4.0 front. ⚠️ ⬜ Rear needs **> 38 mm** tyre-to-controller gap at full compression — measure before mounting |
| 13 | **The 3" dash will not take the CAN telemetry feed** — no ACK at 17 bitrates, and the CAN 18 byte map it expects is undocumented. No motor temp, controller temp or bus current on the glass | 2026-09-09 | ⏸️ **Parked 2026-09-10 by the owner** (plan D19) — module continues without it; dash keeps its one-line feed; temps move to the WiFi page. Re-opens on a CAN 18 map or a replacement panel |
| 12 | ⚠️ **M16 axle-nut torque is unknown and the wheel is already assembled**; the washers are shop-made, material/hardness unrecorded | 2026-09-08 | 🔴 **Open — safety item.** Golden rule #6 forbids a guessed number; needs the figure from Powerful Lithium |
| 11 | **Measuring rig would not assemble — controller orientation was backwards in the model** | 2026-09-08 | 🟢 Resolved — long axis runs **across** the bike, so each bracket takes the **106 mm** hole pair |
| 10 | ⚠️ **Main-line ANL fuse (80 V DC) and block (32 V DC) are rated below the 84.0 V pack** | 2026-09-07 | 🟡 **Replacement approved 2026-09-08** — Littelfuse `JLLN125` Class T (125 V DC, 20 kA DC) + Blue Sea `5007100` block (160 V DC), **125 A**. ⬜ Not yet ordered; the ANL pair must not be fitted |
| 9 | **Nothing bolts flat to the under-seat lugs** — recessed lug in a rounded tube; spacing at 186 mm barely fitted | 2026-09-07 | 🟡 Partly resolved — **15 mm × 6 mm pad** per hole, spacing **187 mm**; ⬜ the load-path decision for the structural bracket is open |
| 8 | ⚠️ **Brake input was `7-Disabled`** — levers cut no power | 2026-09-06 | 🟡 **Set 2026-09-08 to `0-StopWhenGround`** (in the export). `BL` is driven by the brake circuit (`revv1-brake-circuit.md`). ⚠️ ⬜ Its powered cut test (§7.2) has not run — **gate before riding** |
| 7 | **3" display powered but showed no controller data** | 2026-09-06 | 🟢 **Resolved 2026-09-08** — `SpecialFrame` **21**, one-line on the **brown** lead (light-blue `SPD` dead), `SpeedPulse` **1**, `RateRatio` **1.000**. ⬜ GPS trim in Phase 9 |
| 6 | ⚠️ **Reverse gear enabled** on a pedal bike (cranks chain-linked) | 2026-09-06 | 🔴 **Open — reverse is still ON** (`BackEnable = 1` in every export). ⬜ Import the 2026-09-08 no-reverse file and verify |
| 5 | Motor temp read **197°** on a cold motor → fault "7. Motor Temp Protect" | 2026-08-16 | 🟢 Resolved — `NTC_PTC` `0-None` → `5-KTY83-122`. ⬜ Sensor type to confirm with Powerful Lithium |
| 4 | 3" display's 9-pin plug **does not mate** anything on the FarDriver harness | 2026-08-16 | 🟢 Resolved — it's a *vehicle-harness* connector; broken out wire by wire |
| 3 | **Wrong part ordered: the "Flipsky anti-spark switch" is a VESC motor controller** | 2026-08-15 | 🟢 Resolved — design revised (Option B), unit not installed. ⬜ Confirm price paid; return/resell |
| 2 | New hub axle hardware didn't fit the stock dropouts (washers) | 2026-07-09 | 🟢 **Resolved 2026-09-08** — shop-made washers fitted, wheel mounted with both torque arms. Material and torque tracked in **#12** |
| 1 | Rear brake: rotor sat ~1 mm inboard of the caliper pads | 2026-07-09 | 🟢 **Resolved 2026-09-08** — **4 × 0.2 mm ring shims (0.80 mm)**, rotor centred, brakes work. ⬜ Record whether longer M5 rotor bolts were needed |

**Rules this log has produced — they apply to every step on this bike:**
- **Verify by function before connecting — physical fit and vendor defaults prove nothing** (#3,
  #4, #5). This bike's non-stock wiring breaks assumptions that hold on every other build.
- **Check the VOLTAGE line on every part, not just the current line** (#10). The 84.0 V pack sits
  above where most 12/24/48 V-market hardware is rated.
- **A part line whose description and source link disagree is the warning sign** (#3). Reconcile
  them before ordering, and check the price paid against the price budgeted.
- **Printed parts that bolt face to face must clear the heads of the fasteners already in that
  face** (#11).
- **Check every dimension of a spec, not just the one that made the part look compatible**
  (#14). A bead seat diameter that matches says the tyre will go on — not that it fits. The
  same shape as #10’s voltage line.

---

## #14 — ⚠️ Both rims are far too wide for the 3.00-16 moto tyres

**Status:** 🟡 **Replacement tyres bought 2026-09-17; rear clearance unverified** · **Found:**
2026-09-17 (owner, chasing a bumpy ride at moderate speed)

**Symptom.** The ride goes **very bumpy at moderate speed**. The first hypothesis was wheel
imbalance — which a hub motor makes untestable by the usual method. A gravity spin test **cannot**
work here: the motor’s cogging torque (23 pole pairs, ~1–2 N·m of magnetic detent) is 20–50× the
gravity torque of any realistic imbalance. It would take **~750 g** at the rim before the wheel
stopped where the mass says instead of where the magnets say. Unplugging the phase leads removes
eddy braking but does nothing to cogging.

**Root cause — the rims are 32 % and 52 % wider than the tyre’s widest approved rim.** Measured
flange to flange with calipers, 2026-09-17:

| Wheel | Flange to flange | Internal (less 2–4 mm/side) | Nominal | vs. 3.00-16 max (54.6 mm) |
|---|---|---|---|---|
| **Front** — stock Ride1Up | **77.86 mm** | ~70–74 mm | 70 mm fat rim | **~32 % over** |
| **Rear** — Powerful Lithium motor wheel | **89.08 mm** | ~81–85 mm | 80 mm fat rim | **~52 % over** |

The widest rim approved for a 3.00-16 is **2.15" = 54.6 mm** (application table: 1.60 / 1.85 /
2.15). Both wheels are past the end of that chart, not near its edge. The rear rim is **wider than
the tyre’s own nominal section** — 83 mm rim against a 76 mm tyre = **109 %**, where a healthy
rim-to-tyre ratio is 60–75 %. The flange-thickness uncertainty does not change the verdict: both
extremes land in the same place.

Spread a tyre’s beads ~28 mm wider than design and the carcass is pulled taut between them. The
sidewalls stand near-vertical and lose the compliance that normally absorbs road texture; the crown
flattens and the contact patch squares off. ⭐ **That is a harsh ride by construction, and it is
independent of balance.** It also produces the uneven bead line and radial runout that carry roughly
**20× the force** of any plausible imbalance — 2 mm of runout into a ~100 N/mm carcass ≈ 200 N,
against ~10 N from 30 g of imbalance at 40 km/h.

⚠️ **These are bicycle rims**: no motorcycle bead safety hump, a bicycle load rating, and a moto bead
stretched 28 mm past its maximum design width sitting on them at 84.0 V and 5 kW. Bead retention is
the failure mode.

**Why it was missed.** Both `parts-list.md` and `build-sheet.md` recorded the fitment as *"the
‘20×4’ rim is ISO 406 = motorcycle 16-inch bead seat, so 3.00-16 moto tyres fit with no
conversion."* That is **true about bead seat diameter and silent about width** — and bead seat
diameter only says the tyre will go on. Neither rim’s width was ever measured, and neither vendor
publishes it: Powerful Lithium’s spec table says *"Rim Size: 20" x 4""* and stops; Ride1Up
publishes no rim specs at all.

**Resolution — back to correctly-sized fat rubber (owner decision, 2026-09-17).**

| | Part | Rim ratio | Outer Ø | Radius vs. 3.00-16 |
|---|---|---|---|---|
| **Rear** | **Super73 BDGR Override 20×4.5** — ✅ bought | **73 %** on 83 mm | ~634 mm | **+38 mm** |
| **Front** | **Super73 Huntsman Override 20×4.0** — ✅ bought | **71 %** on 72 mm | ~608 mm | **+25 mm** |
| Tubes | Super73 fat tube, spec **20 × 4 / 4.5 / 5** — one part covers both ends | ⬜ to order | | |

Both are Vee Tire Co **Override** construction (3× puncture protection), so the carcass matches
front to rear even though the tread differs — BDGR is big-block all-terrain, Huntsman a road/loose
hybrid. **20×4.0 is the stock diameter**, so the front’s fork crown, arch, fender-mount and
steering-lock clearances are already proven. The rear is the clearance-limited end, so 4.5 was taken
over the available 5.0: a 73 % rim ratio for 13 mm less radius.

⛔ **Moto tyres were ruled out, not merely passed over.** The only 16" moto tyres approved for an
83 mm rim are the 130-section ones (130/80-16, 130/90-16 — approved 64–89 mm), at **+27 mm per side
at the swingarm** and +28 to +41 mm of radius. **This rim wants a tyre the frame cannot take.**

**Load and speed ratings were never the reason for the moto tyres.** `build-sheet.md` records the
241 choice as tread pattern, 16" availability and *"better grip and wear at ~45 mph"*. The 45P
marking (165 kg, 150 km/h) is about double the headroom this vehicle needs.

**Outstanding.**
1. ⚠️ ⬜ **Measure the tyre-to-controller gap at FULL COMPRESSION before mounting the rear tyre.** It
   must exceed **38 mm** for the BDGR 4.5 to clear. The controller sits at the **35 mm** drop, which
   is its physical minimum — *tyre clearance cannot be found by raising the controller*. Measure it
   with the 3.00-16 still fitted (zip tie on the shock shaft to capture max travel); an unmounted
   tyre is far easier to return than a mounted one. ⛔ **If the gap is under 38 mm, the controller
   has to move or the motor gets relaced to a 47–55 mm rim.**
2. ⚠️ ⬜ **Re-derive the FarDriver wheel setting.** `80 / 100 / 16` **is** the 3.00-16 and is now
   wrong: rear rolling circumference goes from ~1753 mm to ~1992 mm (**+13.6 %**), so the speedo
   would read that much low. Candidate starting point **110 / 100 / 16** (OD 626 mm, 1.3 % under),
   then trim the transmission ratio against GPS in Phase 9.
3. ⬜ Measure and log the **rear rolling circumference** once the new tyre is on.
4. ⬜ Check **swingarm clearance** each side with the 4.5 fitted.
5. ⬜ Order tubes and fresh rim strips.
6. ⬜ Record a **riding pressure** — none is written down anywhere.
7. ⬜ Decide what happens to the two Shinko 241s (~$100): return, or sell on.
8. ⬜ **Re-assess the bumpy ride once the new tyres are on.** If it survives, the remaining suspects
   are rear sag and rebound (50 mm of travel, non-catalogue shock, now carrying the pack and a 5 kW
   hub) and tyre pressure. Also still open from this investigation: issue **#12**’s post-ride
   axle-nut re-check, which is a hard gate and directly able to produce this symptom.

---

## #13 — ⏸️ 3" dash will not carry the CAN telemetry feed (no temps, no bus current on the glass)

**Status:** ⏸️ **Parked 2026-09-10 by the owner** — plan **D19** · **Found:** 2026-09-09 (bench).
Deliberately deferred — not a fault, not a wiring error.

**Symptom.** The module was to impersonate a CAN-enabled FarDriver so the Chaojie would show
**motor temperature, controller temperature and bus current** — data the one-line frame has no
room for. On the bench the panel **never acknowledges a CAN frame**: 17 bitrates from 10 K to 1 M,
standard and extended IDs, single-shot, every one error-passive with TEC pinned at 128 and
**0 ACKs** (`esp32/m16b-can-console/`).

**Root cause.**
1. **Not a dead link.** The bus is one electrical node at **68 Ω** measured at the breakout,
   grounds bonded, and **the display's own transceiver is powered and biasing both lines** with our
   board unpowered. The rig self-tested 8/8 correct / 0/8 disconnected / 0/8 reversed. Method traps
   (including a bus the display was not connected to, which gives a confident, reproducible false
   negative): `revv1-m16-can-bench-test.md` §4.1.
2. **The vendor (Peri, `hzboxinte.com`, 2026-09-10):** *"Currently display default only support
   fardriver CAN18 protocol."* CAN is enabled and the protocol is **FarDriver CAN 18**. With 0 ACKs,
   the consistent reading is a **receive-only CAN node** — it never ACKs or transmits, but receives
   normally once the transmitter is error-passive. Confidence MEDIUM. So the module transmits in
   `TWAI_MODE_NO_ACK`, and exactly one unknown remains: **the CAN 18 byte map**, which no document we
   hold defines and which the controller does not export (`CAN` is a firmware-internal instruction
   number, not a table — M12).

**Why it is parked.** Every remaining route to the map is open-ended — wait on the vendor, ask
SiAECOSYS, set `CAN` = 18 on our controller and decode (⛔ collides with D7 on A11/A12), or ask a
working owner to sniff his bus — and **nothing else in the module depends on it**: the BOM, the pin
map and every build step are display-independent. Owner: finish the module and come back to the
dash later, *"either figure it out or replace it."*

**Consequences (full list in plan §8, D19):**
- **The dash is not dead.** The one-line feed on the brown lead (issue **#7**) gives **speed, the
  panel's own V/SOC, gear and faults**. Lost: temperatures and bus current.
- **Those move to the module's WiFi page**, which is the module's primary readout.
- **The CAN hardware stays fitted** — transceiver in hand, 2 of 6 spare GPIO — so a later answer
  needs no rework.
- **D11 and board F (the display power switch) park too**; the dash keeps feeding from switched B+.
- **D17** (no screen on the module) re-opens if the panel is *replaced* rather than fixed.

**What re-opens it:** a CAN 18 map arriving from any source, or a decision to replace the panel.
⬜ Do not chase the vendor reply; capture it in `revv1-m16-can-bench-test.md` §8 if it comes.

---

## #12 — ⚠️ M16 axle-nut torque unknown; washers are shop-made of unrecorded material

**Status:** 🔴 Open — safety item · **Found:** 2026-09-08 (recording the Phase 2 close-out)

**What happened.** Issue #2 was resolved by **fabricating the axle washers with an angle grinder**
rather than buying either catalogued class. The wheel is in the frame with both Grin V6 torque arms
and the caliper, and the rear brake works — **but the nut was tightened to an unrecorded torque,
and the washer material/hardness was never recorded.**

**Why it stays open.** Golden rule **#6**: the M16 axle-nut torque must come from Powerful Lithium,
never a guessed number. This is **the highest-stressed joint on the bike** — axle reaction torque at
5 kW can spin the axle and shear the phase wires (golden rule #4). A generic M16 table value does
not know this axle's thread pitch, material, flat geometry or the vendor's clamp intent.

**Two unknowns:**
1. ⚠️ **Torque value.** The wheel is assembled at *some* torque.
2. ⚠️ **Washer material/hardness.** The nut clamps against these washers, which is why **hardened**
   M16 flats were specified — a soft washer embeds and creeps, losing clamp force over heat cycles
   and vibration. Grinding hardened stock can draw the temper at the cut edge.

**The risk is real but bounded.** The **V6 torque arms** take the anti-rotation load, and the stack
is `dropout → V6 plate → washer → nut`, so the washer bears on the arm's machined face and is a
**clamping** part, not the torque-reaction part. A soft washer threatens *clamp retention*, not
immediate axle spin.

**Actions.**
1. ⬜ **Ask Powerful Lithium for the M16 axle-nut torque spec** (same message as #5's sensor type).
   Until then the figure stays unknown in every document.
2. ⬜ **Record what the washers were made from** — stock type and whether it was hardened.
3. ⚠️ **Re-check axle-nut tightness after the first ride — a hard gate, not routine.** Mark the nut
   and washer with a paint line so any movement is visible.
4. ⬜ Consider replacing the shop-made washers with catalogued hardened parts (spec in #2) — cheap
   insurance on a joint this loaded.

---

## #11 — Measuring rig would not assemble; the controller's orientation was backwards

**Status:** 🟢 Resolved · **Found:** 2026-09-08 (owner, offering the printed rig up to the bike)

**Symptom:** the rig would not go together. The carrier's two controller slots were 168 mm apart
along the bike, which only works if the FarDriver bolts to each rail along its long edges. It does
not.

**Root cause:** an assumption in the model, not a measurement. The plate is 120 × 180 mm with holes
106 × 168 mm, and the model had the 180 mm length running fore/aft. It runs **across the bike**,
spanning between the two rails, so the pair of holes landing on any one bracket is the **106 mm**
pair.

**Resolution:** the model now reads 106 as "along the bike" and 168 as "across", and every
downstream value derives from those two. Carrier slots are at 106 mm spacing and stamped with it;
rig and bracket rebuilt. With 168 across, the controller's hole row lands well outboard of the
bracket's pad and wall (with 106 it landed inside the bracket), and the rear controller bolt sits in
front of the rear lug, so the bracket needs no tail. Current figures:
`mounts/fardriver-underseat-mount.scad`.

A second interference, found by rendering the rig assembled: the carrier's upright lay flat against
the spine's outer face 10–15 mm out from the lug face, exactly where the M5 frame-bolt heads sit.
Fixed by stopping the upright short of the lugs — it spans 26 to 161 mm and clears each bolt head
by 21 mm.

---

## #10 — ⚠️ Main-line ANL fuse and block are rated below the pack voltage

**Status:** 🟡 **Replacement approved 2026-09-08; parts not yet ordered** · **Found:** 2026-09-07
(converter parts research — a paper finding, not on the bench)

**Symptom:** none on the bike. Both parts were ordered 2026-06-29; neither has been installed.

| Part | Rated | Pack |
|---|---|---|
| **Blue Sea 5127** ANL 150A link | **Maximum Voltage 80V DC**, Interrupt Capacity 6000A @ 80V DC | **84.0V** at full charge |
| **Blue Sea 5005** ANL block | **Maximum Voltage 32V DC** (no amperage or interrupt rating published for the block) | **84.0V** |

Sibling links 5128 / 5129 / 5133 carry the identical 80V DC / 6000A pair — a family rating.

**Why it matters.** These are the **only overcurrent protection on the 72V traction line**, between
a pack that can deliver **~360A** and the rest of the bike. Above 80V the link's arc-interrupting
capability is **unspecified**: DC arcs do not self-extinguish, so a fuse asked to clear a fault
outside its rated voltage may sustain an arc instead of breaking it. The block's 32V DC rating
covers insulation and creepage. The exposure is worst at **top of charge** — when the pack has the
most energy to deliver into a fault. The fuse's job is unchanged: the **XT90-S remains the only
make/break point** and the fuse is never the primary disconnect (golden rule #2).

**Resolution — Class T, 125 A (owner approved 2026-09-08; propagated to all four docs).**

| | Part | Rating | Notes |
|---|---|---|---|
| **Holder** | **Blue Sea `5007100`** Class T block, 110–200 A | **160 V DC** (1.9× the pack) | four-stud **bolt-down**, 1/4-20 stainless studs, 72 in-lb, lugs to 4/0 AWG, **ignition-protected** ISO 8846 / SAE J1171, UL 94-V0. Max operating **160 A** |
| **Link** | Littelfuse **`JLLN125`** | 300 VAC / **125 V DC**, **20 kA @ 125 V DC**, fast-acting | JLLN150 is $77.12 at DigiKey, in stock; the 125 A sibling shares the price band |
| Alternative link | Bussmann **`JJN-125`** | 300 VAC / **160 V DC** | most margin, ~2× the price (JJN-150 = $155.31) |

**Why 125 A, not 150 A.** With the FarDriver capped at **80 A**, a 150 A link can never open on
anything the controller can do, leaving the 8 AWG cable (~80–90 A in free air) and the thin-margin
**XT90-S (~90 A)** unprotected against any sustained fault below 150 A. **125 A gives 1.56× over
the 80 A cap** — no nuisance clearing — and puts the protection point under the XT90-S rating.
Not 100 A: it lands exactly on Blue Sea's 80 %-of-rating guidance with zero margin. Capacitor
inrush is not a factor — the XT90-S pre-charge pin handles it.

**⚠️ Traps:**
1. **Class T's headline 200 kA is AC-only.** The DC figure is **20 kA @ 125 V DC** — ten times
   smaller. The UL 248-15 listing is an AC listing; the DC numbers are manufacturer-declared.
2. **Littelfuse's own companion holder for the JLLN, the `LFT30200`, publishes no voltage rating
   anywhere** — the same trap as the 5005. That is why the Blue Sea block (explicit 160 V DC) is used.
3. **Never fit a 175 A or 200 A link** — they exceed the 5007100's 160 A operating rating, even
   though Blue Sea lists 5115/5116 as accessories. **Do not order the `5502`** — that is the
   225–400 A block.

**Rejected (so they are not re-researched):** Littelfuse MEGA HP `0888`/SF56 — 120 V DC, under the
125 V bar, and 2500 A breaking is ~8× below Class T · Mersen `MEV50A150-20G` — aR partial-range
fuse, minimum breaking capacity 300 A · Mersen M-fuse — 100 V DC, not procurable · Mersen HP10M
gPV — 1000 VDC but catalogued 1–32 A only · Blue Sea `5113` (A3T) — DC rating unverified.

**Outstanding.**
1. ⬜ **Order the `JLLN125` + `5007100`** (~$110–150 est.; firm prices exist only for the 150 A
   links).
2. ⬜ Decide what happens to the ordered 5127 + 5005 (~$45): return, or keep for a sub-80 V use.
   **Never fit them on this bike.**

---

## #9 — Nothing bolts flat to the under-seat lugs (recessed lug in a rounded tube)

**Status:** 🟡 Partly resolved · **Found:** 2026-09-07 (owner, offering up the printed jig spine)

**Symptom:** found by fitting a printed part to the frame rather than measuring it.
1. A flat plate **will not sit flush**. The lug face is slightly **recessed**, and the frame around
   it is a **rounded tube**, so the plate fouls the tube before it reaches the lug.
2. The spine's two holes at **186 mm** only **just** went over both lugs.

**Why it matters:** there is no flat back face on the rail to carry the controller's tipping
moment. And a two-hole plate tolerates only about half a millimetre of spacing error before it
binds, so "barely fits" is evidence.

**Resolution so far:**
- **Standoff pads.** Every part that bolts to these lugs stands on a **15 mm diameter × 6 mm proud**
  pad at each hole and touches the frame **nowhere else** (the lug measures 15.98 mm OD, so 15 sits
  on it like a washer).
- **Spacing 187 mm**, canonical for every part. If still tight, open **one** hole to 6 mm rather
  than reprint — one only, or the fore/aft datum floats.
- ⚠️ **Limiter tubes are 16 mm** (6 mm pad + 10 mm panel), so the metal bears on the lug face
  directly and no plastic sits in the clamp path to creep.

⬜ **Still open — the structural load path.** Two 15 mm pads in a line resist tipping far less well
than a flat face would; the couple arm is about 7 mm instead of the panel's full height. Before the
structural bracket is trusted, decide between larger pads (if the recess allows them) and a
**second contact lower down the frame** (rubber-faced foot, or a P-clamp around the tube). The
current design — two brackets with the controller spanning both rails
(`mounts/fardriver-underseat-mount.scad`) — carries the load mostly in shear, which reduces this
but does not close it.

---

## #8 — Brake input was `7-Disabled` in the controller; the levers cut no power

**Status:** 🟡 **Set 2026-09-08; not yet verified** · **Found:** 2026-09-06 (app screenshot of the
parameter page)

**Symptom:** the app's function page read **`Brake: 7-Disabled`**. With the brake input disabled
the controller ignores the **`BL`** (yellow/green, low-brake) wire — the input the brake circuit
pulls to ground when either lever is squeezed. The levers would slow the bike mechanically while the
motor kept driving against them.

**Resolution:** `Brake` set to **`0-StopWhenGround`** (app label; confirmed in the 2026-09-08
export). `BL` is driven by the **brake circuit** (`revv1-brake-circuit.md`): each lever pulls it to
ground through a 1N4148 steering diode, and the same lever lights the brake lamp and signals the
module. ⛔ Never a **`P+`** variant (they bundle Park, which stays Disabled) and never
**`1-StopWhenFloat`** (inverts the logic — the motor cuts when *not* braking). Never use the
high-brake input (grey `BH`, 12V) — keep it capped. `Follow` stays Disabled (no regen).

**Outstanding:**
1. ✅ **M3 — the lever type** (`revv1-brake-circuit.md` §4): Magura MT5, a 2-wire normally-open
   switch per lever — step 1 is built as drawn.
2. ⬜ ⚠️ **Powered cut test — gate before riding:** `revv1-brake-circuit.md` §7.1 (levels at `BL`)
   and §7.2 (each lever cuts the motor, wheel off the ground). The temporary throttle on the
   current setup can drive the wheel for it.

---

## #7 — 3" display powers up but shows no controller data

**Status:** 🟢 **Resolved 2026-09-08** · **Found:** 2026-09-06

**Symptom:** the display lit and read its own voltage and SOC (**70.5V, 43 %** — so pins 2/3 power
and the voltage setting were right), but everything that has to come from the controller was blank
with the throttle held open: gear **N**, **0 km/h**, **0 rpm**, **C 0°**, **M 0°**.

**Root cause — two faults:**
1. **`SpecialFrame` shipped at 246**, which FarDriver defines as *"In use 485: special frame must
   be 246 — convenient for computer 485 join"* — the RS485 PC-link mode, **which sends nothing on
   the one-line** (FarDriver app parameter description §6.2.5). FarDriver's *"general first-line
   pass = 21"* (16 + DATA9 "power" + DATA10 "current %") is what Chaojie resellers (E-Conic,
   Rennovations) specify for this display family.
2. **On this ND72450 (hardware H, code K, fw A86) the one-line output is the BROWN lead**
   (BOOST/ALARM, 2-pin plug with the blue/red XH wire); the light-blue `SPD` carries nothing. Same
   finding as ND72680 / KR7280A owners.

**Resolution (current settings, confirmed in the 2026-09-08 export):** `SpecialFrame` **21**; the
display's purple (pin 9) on the **brown** lead with a proper crimped or soldered joint, no exposed
strands; light-blue `SPD` capped. DATA0 **8** / DATA1 **97**, ByteOption 3, Step **0.9 ms**, Stop
**2** (app label 124 ms), SQH 0, PULSE 0. **Speed: `SpeedPulse` 11 → 1 was the whole fix; `RateRatio`
stays 1.000** — a hand-spin ruled out the 23-pole-pair factor. ⬜ Final trim after the GPS check in
Phase 9 (display `SPEED COEF`).
⛔ Never land the display's purple on the controller's purple **`SPA`** — a 72V PWM output, against
a 0–15V input. With the one-line in use the RS485 PC link is unavailable; configuration goes through
the Bluetooth app.

**What the one-line cannot carry.** FarDriver's general one-line (frames 16–31) is a fixed layout:
header ×2, a status byte (P / side-stand / grip / anti-theft bits), fault byte, speed, **one**
current byte (DATA6), **one** voltage-or-power byte (DATA9) and **one** power-or-current%-or-voltage
byte (DATA10). There is **no motor-temp / controller-temp pair** — the only "Centigrade" option
(DATA9 option 12 + ByteOption 2) gives up the voltage byte for a single temperature. Chaojie
resellers say so directly: *"All functions will not work on display with normal fardriver
controller, CanBus models will show most functions"* (Electric Moto Family). **Temperatures need
the CAN link** → issue #13 (parked). Meanwhile they are monitored in the app, and **the protection
itself lives in the controller** (`NTC_PTC` KTY83-122, 120/90 °C) whether or not the dash shows it.
Dash options for full controller data: build sheet, *Decisions logged*.

**Display side.** The 3" (`CJ-V3-01`) has no protocol selector. Its backend is entered by **holding
`+` and `−` together within 15 s of power-on**:
`DEBU · UNIT SET · V SET · V COEF · SPEED COEF · SPEED R · F TIRE · B TIRE · W UNIT`.
**`V SET` (full-pressure limit) and `V COEF` (voltage coefficient)** set the battery %. ⬜ Set them
on the panel and record the values here.

**Untried one-line trials** — for more dash fields; none carries both temperatures. Each: Save →
key-cycle → note which fields populate; return to the settings above if speed disappears.

| # | SpecialFrame | DATA0 / DATA1 | Stop | other | why |
|---|---|---|---|---|---|
| 1 | **247** | 0 / 0 | **3 (216 ms)** | — | FarDriver "group 26": 15-byte one-line, the only documented frame long enough for two temps |
| 2 | **247** | 7 / 0 | 2 | — | FarDriver "group 5" |
| 3 | **248** | 84 / 83 | 2 | — | FarDriver "group 31": 13-byte one-line |
| 4 | 247 / 248 | as 1–3 | as 1–3 | **SQH 255** | manual ties SQH=255 to these two frames |
| 5 | **28** | 8 / 97 | 2 | **ByteOption 2** | the one documented way to put a temperature ("Centigrade", DATA9 option 12) into the general frame |
| 6 | 31 | 8 / 97 | 2 | ByteOption 2 | same, DATA10 = rated voltage |
| 7 | 250 · 253 · 249 · 242 · 243 · 244 · 241 | 8 / 97 | 2 | ByteOption 3 | the other named one-line variants (YJ, DY, no-SQH, "30", 0x52/0x51 header, F2, ATN) |
| 8 | 21 | 8 / 97 | 2 | **P 1 / BC 8 / Hbar 8 / FD 8**, then **P 3 / BC 2 / Hbar 0 / FD 8**, then **P 1 / BC 1 / Hbar 8 / FD 0** | FarDriver's three documented status-byte position sets — the only knob for a gear indicator stuck on N (current: P 1 / BC 0 / Hbar 0 / FD 8) |
| 9 | 16 · 17 · 20 · 24 · 25 | 8 / 97 | 2 | ByteOption 3 | DATA9/DATA10 content variants within the general family — for amps / power fields, not temps |
| 10 | 48 · 64 · … · 208 | ignored | 2 | — | manufacturer-encrypted variants (manual step 6) — last |

Sources: FarDriver "APP Parameter description" PDF
(ae01.alicdn.com/kf/Ae20138f0bf574217b60607874d951bd12.pdf), econiccycles.com Chaojie 3.13" page,
endless-sphere.com "Nanjing far driver controllers" p.39, `fardriver/reference/` manuals.

---

## #6 — Reverse gear enabled on a pedal bike

**Status:** 🔴 **Open — reverse is still ON** · **Found:** 2026-09-06 · **Reopened:** 2026-09-08

**Symptom:** none on the bike. Found by decoding the controller's own parameter export
(`ND72450_14_AKHA86_20260906_110032.heb`, firmware HA86, serial `JSWXOJ260303D1ECD1FD`) field by
field against the build spec. Layout from github.com/jackhumbert/fardriver-controllers; decoder
`fardriver/heb_decode.py`. The filename's `AKHA86` matches the ParaIndex / SpecialCode / hardware /
software bytes inside the file, which confirms the mapping.

**Root cause:** the FarDriver ships configured as an e-moto controller: **reverse gear enabled**,
mapped to the RE input (brown/white, `Backward Pin: 4-PIN8`). On this bike the hub's
**single-speed freewheel is chain-linked to the cranks**, so powered reverse turns the pedals
backwards into the rider's legs. The RE wire is capped, so it takes a fault (chafe to frame ground,
water in the loom) to trigger it — but nothing in the build needs reverse, so leave no path to it.

**Current state:** `BackEnable = 1` in every controller export (2026-09-06 and both 2026-09-08). An
in-app attempt on 2026-09-08 did not take.

**Outstanding:** ⬜ Turn reverse off and confirm it in a fresh export.
- **File to import: `fardriver/ND72450_AKHA86_20260908_revv1-import-noreverse.heb`** — the
  2026-09-08 post-session export with only `BackEnable` changed to 0 (checked by decoding and
  diffing, 2026-09-11). App main menu → open heb → **Save** → key-cycle → re-export.
- ⚠️ **Do not import `…20260906_revv1-import-v2.heb` now.** It was built from the 2026-09-06 export,
  so it would put `Brake` back to `7-Disabled` (issue #8) and `SpeedPulse` back to 11 (issue #7).
- If the import does not take: set `Backward Pin` → `13-Invalid` (`revv1-fardriver-nd72450-pinout.md`
  §3), which disconnects the RE wire from reverse.

**Also found in the 2026-09-06 export (controller not changed):**
- **The spec targets are set:** line current **80A**, phase **200A**, `NTC_PTC` KTY83-122 with
  **120°C / 90°C**, LVC **60.0V** (+2V derate), 34Ah, rated 72V, wheel **80/100-16**, throttle
  **1.1–3.9V** reading 0.79V at rest, hall self-learn done (phase offset **212.0°**). Regen / e-brake
  off (`Follow = Invalid`).
- **Over-voltage protect stays at the factory 90.7V / restore 88.7V.** Never set it to ~84V — a full
  pack sits at 84.0V and would trip it.
- **There is no circumference field.** The app takes tire width / aspect / rim plus a transmission
  ratio (1.000); verify against GPS and trim.
- **`Park: 2-Disabled`** — no P gear, no auto-park.
- **Pole pairs = 23** (vendor-programmed, not on any spec sheet) — on the Powerful Lithium question
  list with the axle-nut torque and the temp-sensor type.
- `Special Frame = 246` as shipped → issue #7. Brake code 7 = `Brake: 7-Disabled` → issue #8. The
  `MOE` bit is undecoded; leave it.

---

## #5 — Motor temperature read 197° on a cold motor ("7. Motor Temp Protect")

**Status:** 🟢 Resolved (config) — ⬜ vendor confirmation outstanding · **Found:** 2026-08-16

**Symptom:** first controller power-up. Controller beeping; app showed **"7. Motor Temp Protect"**
and **Motor Temperature 197°** on a motor at room temperature. MOS temperature read **27°** on the
same screen, so only the motor channel was wrong. `MTPA` showed a red X.

**Root cause:** `NTC_PTC` shipped set to **`0-None`**. With no sensor curve selected the channel
read a rail value and tripped the protection.

**Resolution:** `Paras → NTC_PTC → 5-KTY83-122 → Save`. Motor temp then read **25°** against MOS
27°, status **System OK**, `MTPA` green. The setting is a readout mapping, safe to iterate: with the
motor at known room temperature, the correct option lands near the MOS reading. Options:
`0-None · 1-PTC · 2-NTC230K · 3-KTY84-130 · 4-Cacu · 5-KTY83-122 · 6-NTC10K · 7-NTC100K`.

⬜ **Still open:** matching at 25° proves the curve only *at ambient*; two sensors can agree at room
temperature and diverge badly at 120°, where the protection matters. **Confirm the sensor type with
Powerful Lithium** — same message as the M16 axle-nut torque (#12).

**Fallback if the vendor names a sensor with no preset:** `4-Cacu` estimates winding temperature
from load — enter the **motor's** rated power (**5000W**), not the controller's nameplate (the app
header shows 72V3000W). Prefer this to disabling protection: a hub motor cooks from the windings
outward, where the MOS sensor can never see it.

---

## #4 — 3" display's 9-pin plug doesn't mate anything on the FarDriver harness

**Status:** 🟢 Resolved — wiring defined · **Found:** 2026-08-16

**Symptom:** the bundled Powerful Lithium 3" display's 9-wire plug has no mating connector anywhere
in the FarDriver harness fan-out.

**Root cause — not a missing part.** The connector is a **DJ7091A-2.8-11**, a *vehicle-harness*
connector. It expects switched ignition power, a controller signal, CAN, and **lighting telltales**
from a complete bike loom — a single plug on a normal FarDriver e-moto build. This bike has no
such loom, so no single connector can match.

**Resolution:** break out wire by wire. **Full pinout: work order §3.1, checklist Phase 5,
`revv1-display-chaojie-3in-pinout.md` §3.** Summary: **pin 2 (black) = power in** from switched
B+ · **pin 3 (green) = B−** · **pin 9 (purple) = one-line to the FarDriver brown lead** (the only
wire to the controller, issue #7) · **pins 7/8 = CAN, left unconnected** (issue #13) · **pins 1, 4,
5, 6 = capped.** The display is rated **DC 12–120 V**, so it runs directly off pack voltage; no
DC-DC needed.

**⚠️ The trap.** Pins 1/4/5 are turn-signal and headlight telltale inputs rated **0–15V**. They
stay capped until the ESP32 module's 12V lamp feeds exist; then each is fed from the lamp feed it
mirrors through ~1 kΩ (plan §6). Never connect them to anything at pack voltage.

---

## #3 — "Flipsky anti-spark switch" was mis-specified; the part is a VESC motor controller

**Status:** 🟢 Resolved — design revised · **Found:** 2026-08-15 · **Ordered:** 2026-06-29

**Symptom:** the part received as the build's "anti-spark master switch" is labelled **FSESC 75200
Pro V2.0 with Alu. PCB** — a complete **VESC-based motor controller**. Spotted when the owner asked
whether its blue/green/yellow leads land on the **same FarDriver phase studs as the motor wires**.

**⚠️ Near-miss.** Those three wires are the FSESC's **motor phase outputs**. Bolting them to the
FarDriver's phase terminals wires two controller output stages together; on power-up each drives
hundreds of amps into the other's MOSFETs — a dead short across a 34Ah pack, destroyed controllers,
arc flash, credible fire risk. **Nothing was connected; nothing was damaged.**

**Root cause:** the build-sheet line described *"Flipsky 75200 Pro anti-spark switch (84V/200A, alu
PCB) — button, key-swappable, ~$90"*, but its **source link** pointed to
`…based-on-vesc-for-electric-skateboard-scooter-ebike-speed-controller`. Flipsky sells a **"75200
Pro V2.0"** (ESC) and an **"Antispark Switch Pro V3.0"** (the switch) — both 200A, both "Pro", both
"with Aluminum PCB", and the ESC ships in a "with Key Switch" variant. The budget was a second
signal: ~$90 against the FSESC's typical **$180–260**.

**Resolution — Option B (owner's decision, 2026-08-15).** No anti-spark switch:
- The **XT90-S is the main-line make/break point and the anti-spark device** (its 5.6Ω pre-charge
  resistor does the job). **$0 — owned.** ✔ **Female half → battery/fuse side** (the resistor side).
- Daily on/off = a **2-wire key switch on the FarDriver KEY wire only** — low-current logic, fed from
  B+ downstream of the XT90-S through a **~2A inline fuse**.
- Topology: `Battery(+) → 125A Class T fuse (issue #10) → XT90-S → Controller B+`.
- **The FSESC 75200 is not installed anywhere.** Return or resell.

**⚠️ Accepted tradeoff (owner's call):** XT90 is ~**90A continuous** against an **80–90A**
battery-current cap — essentially no margin, where a real anti-spark switch would have given 200A.
Mitigations: **cap set to 80A**, **mount the XT90-S where it can be inspected without disassembly**,
and **check it for heat / discoloration after the spin test and after the first hard rides**.

**Storage isolation — order matters.** The XT90-S must always be the part that makes and breaks,
because the pre-charge lives there.
- Disconnect: **key OFF → unplug XT90-S → (optional) take the main fuse out**
- Reconnect: **fuse in → mate XT90-S → key ON**
- ⚠️ Never make/break at the fuse with the XT90-S mated — that bypasses pre-charge and puts the full
  controller-capacitor inrush across the fuse contacts. Never break either under load.

⬜ **Confirm what was actually paid** for the FSESC — it affects the build-sheet totals and how hard
to push the return.

---

## #2 — Axle washer sizing mismatch (old washers vs. new 16mm axle)

**Status:** 🟢 **Resolved 2026-09-08** (owner) — washers fitted, wheel mounted in the frame with both
Grin V6 torque arms and the caliper; Phase 2 complete · **Found:** 2026-07-09

**Symptom:** neither washer worked. The stock washers' **ID** was too small for the new axle; the
motor-provided washers' **OD/profile** would not seat flush against the dropout.

**Root cause:** the new axle is **16mm (M16) threads with 11mm flats**, a hair larger than stock.
The original part is a **keyed torque washer**, not a flat washer: **D-hole keyed to the axle
flats + a raised step that drops into the dropout slot**, ~20–22mm OD, bored for the stock axle. The
axle itself drops into the open-top slot correctly — its flats index into the slot — so this was
purely a washer-fit problem; **no dropout modification is needed**.

**Resolution:** the owner **made the washers with an angle grinder** (2026-09-08); neither
catalogued class was bought. ⚠️ **Material/hardness and the M16 nut torque are unrecorded —
tracked as issue #12.**

**Assembly as fitted:** `dropout → V6 plate → washer → thick nut → jam nut`. The washer bears on the
V6 arm's machined face, not the frame. **Double-nut:** the thick nut is the clamp, the slim nut is
run down hard against it as a jam nut. The axle tip is cross-drilled for an R-clip. The nuts and
washers are not the anti-spin device — the axle flats and the **Grin V6 torque arms** are.

**Replacement spec, if the shop-made washers are replaced (#12 action 4):**
1. **Torque washer ×2 for a 16mm axle / 11mm flats** — restores the OEM design and seats in the slot.
2. **M16 hardened flat washer** (ID ~17mm, **OD 27–28mm = DIN 433 / SAE narrow**, ~3mm) — only if
   the dropout face is confirmed flat. ⚠️ **Never ~30mm or ~34mm fender washers** — they overlap a
   boss and weld fillet on the dropout and sit cocked. For more clamp spread add thickness, not
   diameter.

**Rules:**
- ⛔ **Never grind the frame or dropout to make a washer seat.** That weld attaches the tab at the
  bike's highest-stressed joint; grinding a weld toe starts a fatigue crack. Fix the washer instead.
- ⛔ **Do not bore the OEM torque washer out to 17mm** — it destroys the D-key and leaves ~2mm of
  wall on a torque-carrying part.
- ⚠️ **Never torque against a tilted washer** — it point-loads the nut and the torque reading is
  meaningless.

---

## #1 — Rear brake: rotor sits inboard of the caliper pads (lateral misalignment)

**Status:** 🟢 **Resolved 2026-09-08** — rotor centred in the pads, rear brake works · **Found:**
2026-07-09

**Symptom:** with the new hub wheel in the frame, the rear rotor did not sit between the brake pads:
from the centre outward the order was **wheel → rotor → pads**. The miss was **lateral**, not
radial.

**Root cause:** the new hub's disc face sits **~1mm** further inboard than the caliper needs.
Wheel position is not the lever — shifting the wheel as far as it goes widens the offset. **Not a
brake problem.** The brakes are now **Magura MT5** front + rear; the MT5 caliper dropped in on the
same 0.80 mm shim stack.

**Resolution:** **6-bolt ISO ring shims, 0.2mm each, M5 ID — 4 fitted = 0.80mm**, between the hub
flange and the rotor. Ring shims keep all six bolt positions identical, so the rotor stays true;
add or remove one to fine-tune. **Blue threadlocker on all rotor bolts.** ⚠️ Shims reduce rotor-bolt
thread engagement — verify full engagement, else use **M5×0.8 rotor bolts ~2mm longer** (Torx T25).

⬜ Record whether longer M5 rotor bolts were needed (at 0.80mm the engagement is slightly better
than the planned 1.00mm).

The front rotor needs no shims.
