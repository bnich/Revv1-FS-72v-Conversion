# Parts list

Named products, as actually bought. Prices are what was paid or quoted in 2026 and are here because
they are useful to anyone costing the same build — not because they are current.

The live order tracker with status and running totals is in [build-sheet.md](build-sheet.md).

---

## Drivetrain and power

| Part | Product | Cost |
|---|---|---|
| **Hub motor** | Powerful Lithium **5,000 W 3T** hub motor | $760 |
| **Controller** | **FarDriver ND72450**, ordered "loaded" — throttle, 3" display, enclosure and power/controller cable bundled | $550 |
| **Battery** | Powerful Lithium **72 V "Cadmus"** for Ride1Up Revv 1 — 20S, Molicel P42A, 34 Ah, 84.0 V full, smart BMS | $1,900 |
| **Charger** | 72 V, bundled add-on with the battery | $100 |

⚠️ The controller's bundled **enclosure is not used** — it is too large. The controller bolts directly
to printed brackets ([`revv1fs-fardriver72450`](https://github.com/bnich/revv1fs-fardriver72450)).

## Wheel and tyres

| Part | Product | Cost |
|---|---|---|
| **Tyres** | **Shinko SR241** 3.00×16 (45P), tube type, trials — ×2 | ~$100 |
| **Tubes** | 16" | — |
| **Torque arms** | **Grin Technologies V6** universal ebike torque arm — ×2, clamp-mount | ~$100 |
| **Brake pads** | **Shimano D02S sintered** — ×2 sets | ~$60 |
| **Rotor shims** | 0.2 mm 6-bolt ring shims ×4 (0.80 mm total) + M5×0.8 bolts | ~$15 |
| **Axle washers** | Shop-made ×2 | ~$10–20 |

⚠️ The "20×4" rim is **ISO 406 = motorcycle 16-inch** bead seat, so 3.00-16 moto tyres fit with no
conversion. Grin **V6**, not V7 — V7 is not made in 16 mm.

⚠️ Brake fluid is **mineral oil**, never DOT. Pads are **D02S sintered**, not D03S resin.

## Controls

| Part | Product | Cost |
|---|---|---|
| **Bar switches** | **Anxingo** universal 7/8" 22 mm handlebar control switches — turn signal, low/high beam, warning light, flameout | — |
| **Key switch** | Universal key ignition for ebike, **12 V–96 V**, anti-theft | ~$12 |
| **Throttle** | FarDriver-bundled twist throttle | *(in $550)* |

The key switch feeds the FarDriver **KEY** wire only — a low-current logic input through a ~2 A inline
fuse. **It does not switch main current.**

## Main-line protection and switching

| Part | Product | Cost |
|---|---|---|
| **Main fuse** | **Littelfuse `JLLN125`** Class T link, 125 A — 125 V DC, 20 kA @ 125 V DC | ~$70 |
| **Fuse block** | **Blue Sea `5007100`** Class T block — 160 V DC, bolt-down, ignition-protected | ~$50 |
| **Main connector** | **XT90-S** (anti-spark) — the make/break point | ~$15 |
| **Main wire** | 8 AWG, if leads need extending | ~$15 |

⚠️ **125 A, not 150 A.** With the controller capped at 80 A a 150 A link never opens, leaving the
8 AWG cable (~80–90 A) and the XT90-S (~90 A) unprotected against sustained sub-150 A faults. 125 A
keeps 1.56× over the cap.

⚠️ Class T's headline 200 kA is **AC only — the DC figure is 20 kA.** Never fit a 175/200 A link; it
is over the block's 160 A rating.

⚠️ **Do not use** Blue Sea 5127 ANL / 5005 block — rated 80 V / 32 V DC, below the 84.0 V pack. Two
were bought before this was caught.

## Displays

| Part | Product |
|---|---|
| **3"** | Chaojie **`CJ-V3-01`**, DC 12–120 V — bundled with the controller |
| **5" touchscreen** | Chaojie **`CJ-V5-04`** Gen 4, rear-camera variant, DC 12–96 V — ~$195 |

Which panel the bike ends up with is open. Mount for the 5":
[`chaojie-touchscreen-gen4`](https://github.com/bnich/chaojie-touchscreen-gen4).

## Not used

| Part | Why |
|---|---|
| **Flipsky FSESC 75200 Pro V2.0** | Ordered as an "anti-spark switch". It is a complete VESC motor controller — wrong part. Not installed anywhere |
| Blue Sea 5127 ANL fuse | 80 V DC rating, below the pack |
| Blue Sea 5005 ANL block | 32 V DC rating, below the pack |
| FarDriver enclosure | Too large; printed brackets used instead |

---

## Still to source

- Mineral oil and bleed kit (optional braided lines)
- Brake-circuit components — 1N4148 steering diodes, `AO3407A` P-FET, 10 kΩ, 100 nF, 1 A fuse
- ESP32 body module — see [`fardriver-esp32-body-module`](https://github.com/bnich/fardriver-esp32-body-module)
