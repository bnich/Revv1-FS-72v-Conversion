# Ride1Up REVV1 FS — 72 V, 5 kW conversion

Converting a **Ride1Up REVV1 FS** from its stock drivetrain to a **5,000 W 3T hub motor on a 72 V
system**, with a FarDriver controller, printed mounts, and an ESP32 body module taking over lighting,
signals, horn and boost.

This repository is the **build itself** — procurement, the phase-by-phase checklist, the shop work
order, the issues log and the safety rules. The pieces that stand alone live in their own
repositories, linked below.

> ⚠️ **This is a deliberately non-stock, warranty-voiding, >750 W build.** It is not a legal e-bike in
> most US jurisdictions — off-road or registered-vehicle use. It loses stock PAS and class behaviour
> and is throttle-driven.

---

## The repositories

| | |
|---|---|
| **this repo** | Build sheet · checklist · work order · issues log · parts list · brake circuit |
| [**revv1fs-fardriver72450**](https://github.com/bnich/revv1fs-fardriver72450) | Printed under-seat mount for the controller. **Printed and in service** |
| [**chaojie-touchscreen-gen4**](https://github.com/bnich/chaojie-touchscreen-gen4) | Printed handlebar mount and rear cowl for the 5" touchscreen. Fit gauge printed and proven; the shaped parts are modelled and verified but **not yet printed** |
| [**fardriver-esp32-body-module**](https://github.com/bnich/fardriver-esp32-body-module) | ESP32 body module — lighting, signals, horn, boost |

---

## Where the build is

A **temporary running setup**: controller on the printed brackets, a temporary throttle, a temporary
key switch, and the battery. **It moves under power.**

No display, no lights, no other electronics — and **no fuse of any kind on the main line**; the
Class T fuse is not yet ordered. The build proceeds incrementally: lighting, signals and horn arrive
from the ESP32 module as each is built, and the brake circuit grows in three steps.

Open blockers are tracked in [docs/issues-log.md](docs/issues-log.md) and at the head of
[docs/checklist.md](docs/checklist.md).

---

## Documents

| | |
|---|---|
| [**docs/parts-list.md**](docs/parts-list.md) | Named products as actually bought |
| [**docs/build-sheet.md**](docs/build-sheet.md) | Order tracker, costs, running totals, and the rationale for each choice |
| [**docs/checklist.md**](docs/checklist.md) | Phases 0→10, prep through handover. **The document to print and follow step by step** |
| [**docs/work-order.md**](docs/work-order.md) | Shop-facing: bill of materials, warnings, wiring diagram, FarDriver config table, sign-off sheet |
| [**docs/issues-log.md**](docs/issues-log.md) | Problems found during the build, with root cause and resolution |
| [**docs/brake-circuit.md**](docs/brake-circuit.md) | Levers → motor cutoff and brake lamp, in hardware |

---

## Core specification

| | |
|---|---|
| **Motor** | 5,000 W 3T hub · 16 mm axle, 11 mm flats · 6-bolt ISO rotor · tubed · single-speed freewheel |
| **Controller** | FarDriver **ND72450**, 20S / 72 V, **80 A line** (capped) / 200 A phase |
| **Battery** | 72 V / 20S, Molicel P42A, 34 Ah, **84.0 V full**, smart BMS |
| **Wheels** | "20×4" rim = **ISO 406 = moto 16-inch** bead seat — 3.00-16 tyres fit with no conversion |
| **Brakes** | Stock 203 mm 6-bolt rotor · **mineral oil, never DOT** · Shimano D02S sintered pads |
| **Main line** | `Battery(+) → 125 A Class T fuse → XT90-S → Controller B+` |

Sensors: the motor's lead carries **6 wires = 5 hall + 1 motor temp**. Motor-temp protection is
enabled, `NTC_PTC` = `5-KTY83-122`.

---

## ⚠️ Safety rules

These are load-bearing throughout the checklist and work order.

1. **The battery stays disconnected until first power-up.** FarDriver configuration needs the
   controller powered, so the energise step comes *before* app config — do not reorder them.
2. **The XT90-S is the only make/break point** for main power. The pre-charge resistor lives in it.
   **The main fuse is never the primary disconnect** — there is no pre-charge there. Break main power
   only with no load, key OFF.
3. **Verify polarity with a meter** before every main-power connection.
4. **Both Grin V6 torque arms are mandatory** before any power test. Axle reaction torque at 5 kW can
   spin the axle and shear the phase wires.
5. **The brake cutoff and brake light never depend on firmware.** Each lever pulls the FarDriver `BL`
   low and switches the brake lamp through hardware; the ESP32 module only listens.
6. **Torque every fastener to spec and re-check after the first ride.**

### Storage isolation — order matters

The XT90-S must always be the part that makes and breaks, because the pre-charge resistor is in it.

- **Disconnect:** key OFF → unplug XT90-S → *(optional)* remove the main fuse.
- **Reconnect:** fuse in → plug XT90-S → key ON.

⚠️ Never make or break at the fuse with the XT90-S mated — that bypasses pre-charge entirely and puts
the full controller-capacitor inrush across the fuse contacts.

---

## Licence

[CC BY-SA 4.0](LICENSE).

## Disclaimer

A 5 kW, 84 V vehicle built by an amateur. Nothing here has been tested to any standard, and several
items are open blockers rather than finished work. **Do not treat this as instructions.** You are
responsible for anything you build.
