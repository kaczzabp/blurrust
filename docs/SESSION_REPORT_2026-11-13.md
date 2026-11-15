# Session report — 2026-11-13

## Result

- Corrected the AID height probe: the lower and upper stored fields are array indices 9 and 10. The earlier claim based on index 8 was invalid; that field is a side-vector component.
- Surveyed the complete Tokyo `_4 MainLoop` candidate against bounded decoded render regions. With reversed Z, 942 of 1,035 points have a render-surface hit within one Y unit of the stored height midpoint; a +100 X control matches 195. The 93 mismatches form nine runs. The two longest runs have geometry at every sampled XZ, but at differing heights.
- Cross-checked four Brighton `_1 mainLoop` regions: midpoint matches were 150/150, 150/157, 135/138, and 144/147. Wide surfaces also give many shifted-control matches, so these results do not identify a driving lane.
- Traced the executable's AID path selection. It looks up the active route ID in a runtime route registry and uses the matching record's debug tag as the AID basename. The similar AID names in the executable's UI/event registry do not establish route IDs.
- Fixed the project-side collision preview so a vehicle starting inside a wall clearance radius can move out of that overlap.

## Verification

The focused install-backed AID checks passed. `cargo fmt --all --check`, `cargo clippy --workspace --all-targets -- -D warnings`, `cargo test --workspace --quiet`, and `tools/publish-check.ps1` passed after the changes.

## Remaining blockers

The original event-to-AID mapping, drivable surface layer, collision rules, start positions, and race gates are unverified. **0/18 installed race directories are verified playable.** The next useful step is to recover route-registry contents or an original event selection, then validate the selected AID against geometry and a complete driven lap.

## Commits

- `c9cdcc0` — AID height probe and collision overlap escape
- `a944daf` — corrected AID probe and Tokyo route alignment survey
- `2d1ecca` — Brighton cross-check
- `c23e4ba` — runtime AID route lookup evidence

Work stopped when the five-hour usage window reached 20% remaining, as requested.
