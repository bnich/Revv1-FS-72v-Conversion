# REVV1 — Brake circuit (motor cutoff + brake light)

**Created:** 2026-09-11 · **Status:** design adopted by the owner 2026-09-11. ⬜ **M3** (§4) confirms
the lever switch type before anything is built.

---

## 1. What it does

Either brake lever does three things at once, all in hardware:

1. **Cuts the motor** — pulls the FarDriver **`BL`** (yellow/green) to ground. `Brake` =
   `0-StopWhenGround` (§5).
2. **Lights the brake lamp** — a P-channel MOSFET (Q1) switches +12 V to the tail's STOP feed; on the
   module it commands a current-limited, diagnosed driver channel instead (§6). No firmware in the
   path, so the brake light still works if the ESP32 module's firmware hangs (plan **D23**). It is fed
   from the module's 12 V rail, so it is dark when that rail is down; the motor cut is not affected.
3. **Tells the ESP32 module** — the same signal reaches IN-05 (left lever) / IN-06 (right lever) for the boost
   safety-release and telemetry. The module only listens.

Everything on the bike shares the 72 V pack's negative (B−), so no isolation is needed. **Steering
diodes** let one lever switch pull three inputs low at once without connecting their pull-ups to each
other — without them, the 12 V gate pull-up would back-feed into the FarDriver's 3.3 V logic input.

Only a few milliamps flow through the lever switch, so a small microswitch, reed or sensor output
can drive it. The hard-brake flash is not possible: the lamp is on whenever a lever is pulled,
whatever the module's firmware does. It does need the module's 12 V rail (§9).

---

## 2. Circuit

One lever shown. **The right lever is identical** — its own node and its own three diodes (D1R, D2R,
D3R); D1L and D1R share the `BL` wire, D2L and D2R share Q1's gate, and D3R goes to IN-06.

```
                                  ┌── D1L ──|◄── FarDriver BL (yellow/green)   [FarDriver's own pull-up]
    left lever (normally open)    │
 B− ────────o/  o──── NODE ───────┼── D2L ──|◄── Q1 gate ── R1 10 kΩ ── +12 V
                                  │
                                  └── D3L ──|◄── ESP32 IN-05 ── R3L 10 kΩ ── 3.3 V

 +12 V ── Q1 source      Q1 drain ── 27 kΩ ──┬── driver channel IN (TPS4H160B) ── OUT ── tail STOP (red)
          (AO3407A, P-channel)             10 kΩ + 5.1 V zener to B−          ── lamp ── common (B−)

 At the controller:  C1 100 nF  BL ↔ B−
```

`|◄` is a diode with its cathode (the bar) on the node side. Lever pulled → the node goes to B− →
`BL`, Q1's gate and IN-05 are each pulled to ~0.6 V through their own diode → motor cut, Q1 on and
the driver channel lights the lamp, module sees it. Lever released → each line returns to its own
pull-up; the diodes block current between them.

### 2.1 Run/off toggle (right pod) — the secondary kill

The right pod's **run/off toggle** cuts drive the same way the levers do, by holding `BL` low. It
can't use a diode like a lever, because it is **closed in RUN and open in OFF** (confirmed on the pod,
owner 2026-09-12) — the inverse sense — so one small N-FET inverts it:

```
 ACC+ 5.1 V (throttle supply) ── R4 1 kΩ ───┬── RED (pod) ──o/ o── BLUE (pod ground) ── B−
                                            │                 run = closed
                                      R5 100 Ω
                                            │
                                      Q2 gate (AO3400A)   ── R6 100 kΩ ── B−
                                      Q2 drain ── BL        Q2 source ── B−
```

RUN: the toggle holds the node at ground → Q2 off → `BL` free → drive allowed. OFF: the node rises
to 5 V → Q2 on → `BL` held low → the controller refuses to drive, exactly as if a lever were held.

- ⭐ **It fails safe.** A broken or unplugged bar wire leaves the node pulled up, which reads as OFF.
- The pull-up runs from the **throttle's `ACC+`**, not from a module rail, so the toggle works with
  the module unpowered — and, at step 1b, with no module fitted. **1 kΩ** gives the toggle's contacts
  5.1 mA of wetting current (tin and silver contacts want 1–10 mA); the node stays at RUN while the
  contact measures under ~146 Ω.
- Q2's drain sits on `BL` in parallel with the lever diodes; any one of them cuts the motor.
- The toggle does **not** light the brake lamp: nothing connects it to Q1's gate.
- The module senses the same node on **IN-11** through a **100 k / 180 k divider** into its input
  expander — **3.24 V at the pin** with the node at 5.03 V (`ACC+` 5.1 V, loaded by Q2's gate network
  and the divider), against a 2.64 V input-high threshold. Sense only; it never drives the cut.
- ⚠️ **A short fails dangerous.** The node shorted to ground (a failed clamp, a pinched pod cable)
  reads as RUN, and nothing can tell it from RUN. The toggle is the *secondary* kill; the key switch
  is the primary one.
- The pod's `red` serves the toggle alone; the lighting slider shares the pod ground (`blue`), so
  nothing else on the pod rides on this node.

---

## 3. Parts

| Ref | Qty | Part | Notes |
|---|---|---|---|
| D1L D1R D2L D2R D3L D3R | 6 (+4 spare) | **1N4148** small-signal silicon diode | Not Schottky — see below |
| Q1 | 1 (+1 spare) | **AO3407A** P-channel MOSFET, −30 V, **±20 V gate**, SOT-23 | Same SOT-23 adapter as the module's `AO3400A`. A ±12 V-gate part is marginal on a 12 V rail |
| R1 | 1 | 10 kΩ | Q1 gate pull-up to +12 V; also the lever's wetting current (~1.2 mA) |
| R3L R3R | 2 | 10 kΩ | Module pull-ups on IN-05/06 to 3.3 V — **not** the 1 kΩ used on the bar switches: R1 already wets the contact, and 10 kΩ keeps the low level near 0.55 V |
| Q2 | 1 (+1 spare) | **AO3400A** N-channel MOSFET, SOT-23 | Inverts the run/off toggle (§2.1). Already on the BOM as D3 |
| R4 R5 R6 | 3 | 1 kΩ · 100 Ω · 100 kΩ | Toggle pull-up from `ACC+` (the wetting current, §2.1) · Q2 gate series · Q2 gate-to-ground |
| C1 | 1 | 100 nF ceramic | `BL` ↔ B− at the controller (EMI) |
| — | — | 22 AWG, heat-shrink | steps 1 and 1b: the diodes and Q2 in-line in the harness |

On the module (step 2 onward) these parts are on its **DRV** board, with its own protection: the
lever nodes (idle ~11.4 V) and `BL` take a **15 V** TVS array (`SMS15T1G`) — never a 5 V array,
which would clamp the node and hold the lamp on. Q1 then drives a driver channel's input instead of
the lamp: the channel limits the lamp's current, so no fuse is fitted.

**Why 1N4148 and not Schottky.** With the levers released, each node floats up to ~11.4 V through D2
and R1, so D1 and D3 sit reverse-biased by ~8 V. A Schottky's reverse leakage — up to milliamps when
hot — would then push current into the FarDriver's and the ESP32's 3.3 V inputs; a 1N4148 leaks
nanoamps. The cost is a higher low level (~0.5–0.6 V at `BL` instead of ~0.3 V), still well under a
3.3 V logic threshold; §7.1 verifies it.

---

## 4. Lever type — M3 (unpowered, ~10 minutes)

The circuit works as drawn for a **normally-open dry contact** or a **sinking open-collector sensor
output**. Check each lever before building ([ESP32 module
`docs/inputs-bench-session.md`](https://github.com/bnich/fardriver-esp32-body-module/blob/main/docs/inputs-bench-session.md)
sitting B):

1. **Count the wires from each lever.** Two = a switch. Three = a powered sensor (supply, ground,
   signal). The lever wires run in the handlebar loom — **M2** identifies them.
2. **Two wires:** ohmmeter across the pair, released vs squeezed.
   - Open → ~0 Ω: **normally open** ✅ — works as drawn.
   - ~0 Ω → open: **normally closed** ⚠️ — inverts the logic (motor cut and lamp on at rest). ⬜ Needs
     a different arrangement; don't wire it as drawn.
3. **Three wires:** the signal must be an **open-collector / open-drain output that sinks, rated for
   ≥15 V** — then it replaces the switch in §2 and needs a supply (the module's 5 V rail, or the
   throttle's 5.1 V `ACC+`). ⚠️ A **push-pull 5 V output does not work**: its high level holds Q1
   part-on and the lamp glows at rest. ⬜ Don't connect a three-wire sensor until its datasheet or a
   bench test shows the output type.
4. Record: wire count, colours, released/squeezed resistance, and whether each lever has its own pair.

---

## 5. FarDriver setting

**`Brake` = `0-StopWhenGround`** (set 2026-09-08, confirmed in the controller export — issue #8):
`BL` pulled to ground = motor cut. ⛔ Never a `P+` variant (they bundle Park, which stays Disabled)
and never `1-StopWhenFloat` (inverts the logic — the motor cuts when *not* braking). Keep **High Brake
(`BH`, grey) capped and unused**, so a floating high-brake can't appear.

---

## 6. Build order — it grows with the bike

| Step | When | What | Test |
|---|---|---|---|
| **1** | **Now** — the temporary setup | Levers → D1L / D1R → `BL`, with C1 at the controller. The diodes sit in-line in the lever-to-`BL` harness under heat-shrink | §7.1, §7.2 — **issue #8's gate before riding** |
| **1b** | **Now**, with the right pod | Run/off toggle → Q2 inverter → `BL` (§2.1). Needs only `ACC+` and `BL`, so it does not wait for the module | §7.5 |
| **2** | When the module's 12 V rail is in | Add Q1, R1, D2L / D2R → brake lamp | §7.3 |
| **3** | When the module is in | Add D3L / D3R → IN-05 / IN-06 with R3L / R3R | §7.4 |

From step 2 the whole circuit — the diodes, Q1 and R1, and Q2 with R4–R6 — moves onto the module's
**DRV** board ([ESP32 module](https://github.com/bnich/fardriver-esp32-body-module), plan §9.2),
which carries it on its own terminals: the levers in (a 4-way 5.08 mm terminal: `LEVER_L`, GND,
`LEVER_R`, GND); `BL` out with `ACC+` in (a 3-way 5.08 mm terminal: `BL`, GND, `ACC+`); the STOP lamp
out on the tail terminal. There Q1 commands a TPS4H160B channel through a divider — the lamp gets a
current limit and an open-load and fault report, still with no firmware in the path. `BL` never
passes through the module's logic board. The wiring of the earlier steps does not change.

---

## 7. Tests

All with the key ON, **wheel off the ground**, hands clear of the wheel.

1. **Levels.** Meter `BL` to B− at the controller: released ≈ the FarDriver's pull-up (~3.3 V, and
   never above ~3.6 V — higher means D1 is leaking or reversed); each lever pulled → **below 0.8 V**.
2. **Motor cut.** Gentle throttle; pull **each** lever in turn → the motor stops; release → throttle
   works again. **This is issue #8's gate — pass it before riding.**
3. **Brake lamp.** Each lever lights the STOP lamp; release → off. Q1 gate: released ≈ 12 V, pulled
   below 1 V.
4. **Module.** IN-05 / IN-06 read low with their lever; the WiFi page shows it.
5. **Run/off toggle (§2.1).** RUN: `BL` sits at its pull-up and the throttle drives the motor. OFF:
   `BL` is below 0.8 V and the throttle does nothing. Then **unplug the pod connector with the toggle
   in RUN** — drive must stop, proving the fail-safe.

---

## 8. Troubleshooting

| Symptom | Likely cause |
|---|---|
| Motor never cuts | D1 reversed or open; wrong wire (must be yellow/green `BL`, not grey `BH`); `Brake` not `0-StopWhenGround` |
| Motor always cut | lever switch is normally closed (§4); D1 shorted; `BL` chafed to B− |
| Lamp stays on | a lever node held low (NC switch, pinched wire); Q1 shorted; a push-pull sensor output (§4) |
| Lamp never lights | D2 reversed; Q1 backwards (its source goes to +12 V); no 12 V rail (module unpowered, tap fuse blown, converter tripped); on the module, the driver channel in fault (its FAULT line reports it) |
| `BL` above ~3.6 V with levers released | D1 leaking or reversed |
| Random cutoffs | EMI — keep C1 at the controller, twist each lever pair, route the lever wires away from the phase leads |

---

## 9. Safety

- The brake cutoff is a **secondary** safety — the primary is releasing the throttle plus the
  hydraulic brakes. Test wheel-off-ground first, and on every commissioning.
- Take the circuit's ground from the **controller's B− stud**, where the module's ground stars too.
- Neither the cutoff nor the brake lamp depends on firmware; the module only listens (plan D23).
- ⚠️ **The brake LAMP needs the module's 12 V rail.** A blown module tap fuse, an open key tap or a
  tripped converter means a dark brake lamp. The motor cut does not depend on it.
- The lever wiring carries at most ~12 V and a few milliamps.
