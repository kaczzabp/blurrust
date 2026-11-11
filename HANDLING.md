# Handling research ledger

The car model, parameter locations, exact timestep, assists, tire model, damage coupling, and surface behavior are **UNKNOWN**.

**SOURCED [S12]:** a lead designer described deliberately elevated collision restitution and increasing speed loss during sustained wall grinding. This is design intent, not a numeric model.

**SOURCED [S07] (accessed 2026-10-11):** a first-hand reverse-engineering write-up presents a decoded vehicle record with named gear ratios, tyre radius/grip curves, and friction/downforce fields, and describes speed as emerging from simulated RPM and gearbox behavior. It does not identify this install's `racingphysics.bin` as the source of that record, so its labels guide investigation but are not yet accepted as our file's schema.

**VERIFIED (local archive inspection, bounded KSLF reader, SHA-256 comparison, and read-only executable decompilation, 2026-10-11):** `gamedata/xt/racingphysics.bin` is present in both the local `gamedata.pak` and `gamedata_boot.pak`; the decoded copies have identical SHA-256 values. Both pass the existing KSLF reader: the version/header word is 2, and its eight bounded region counts are `[30, 0, 154, 2460, 81, 81, 269, 65736]` (strides `[12, 4, 8, 1, 8, 6, 4, 1]`). The local executable contains a generic KSLF structural validator, while the named startup list for other `.bin` resources includes `Vehicles.bin` but not `racingphysics.bin`. No direct consumer association for the latter has been established; its contents and relationship to per-car handling remain **UNKNOWN**. The CLI `blur-assets inspect-kslf` reports these bounded descriptors without labeling them.

**VERIFIED (read-only Ghidra schema-string/pointer survey, 2026-10-11):** the local executable has initialized-data references to metadata entries containing `RP_CarFriction`, `RP_CarGears`, `RP_CarTyres`, and fields including `airFriction`, `gearRatios`, `reverseGearRatio`, `rollingFriction`, and `rotationalAirFriction`. These entries have repeated string/offset/descriptor/callback-shaped layouts; one shared callback decompiles to an empty function. A search for code references to the candidate metadata records found none. This ties the labels to static executable metadata, but **does not** map them to a KSLF region/class ID, demonstrate that `racingphysics.bin` is loaded, or provide the values/units needed for handling. Raw addresses and dumps are private under ignored `research/private/` outputs.

No physics constant may be implemented until it is backed by a trace/disassembly observation or marked `HYPOTHESIS` with a falsifying test. Phase 4 will fit parameters from standing acceleration, braking, steering sweeps, drift entry, wall contact, car contact, and jump traces described in `CAPTURE_CHECKLIST.md`.

