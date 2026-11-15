# Session report — 2026-11-14

## Result

- Added explicit AID-line corridor loading over decoded source draws. Tokyo and Brighton produced full-line diagnostic scenes and completed 120 generic drive steps each.
- An install-backed sweep loaded nonempty candidate geometry for all 33 decoded AID files, spanning 15 of 18 race directories. This is geometry coverage, not race playability.
- Reduced large candidate-surface startup cost through median BVH partitioning, cached bounds, and moving decoded geometry into the surface mesh after render conversion. A Tokyo 3.8-million-triangle run retained identical final position and pixels; an LA Downtown 11-million-triangle scene built and ran one step.
- Found layered LA Downtown geometry at one AID point: midpoint Y=113.525, decoded render-surface hits Y=114.032 and 118.784 at the same reversed-Z XZ. The midpoint does not identify a unique drivable layer.
- Executable inspection showed `Amax.bin` registered at startup, but did not establish its relation to the route registry.

## Verification

Synthetic corridor and BVH tests, installed Tokyo/Brighton diagnostic runs, and the 33-AID installed geometry sweep passed. The LA Downtown ignored install-backed surface-height test passed. `cargo fmt --all --check`, `cargo clippy --workspace --all-targets -- -D warnings`, `cargo test --workspace --quiet`, `tools/publish-check.ps1`, and `git diff --check` passed.

## Remaining blockers

The original event-to-AID mapping, drivable layer, collision rules, start positions, gates, and full lap behavior remain unverified. The three multiplayer race directories without decoded AIDs remain separate. **0/18 installed race directories are verified playable.**

Work stopped when the five-hour usage window fell below 20% remaining, as requested.

## Follow-up route registry probe — 2026-11-15

- Decompiled the route setup and script-facing route accessor in the installed executable. The active route ID matches an runtime route record; its name drives the level bundle and AID path, and the accessor exposes the ID, another integer, thumbnail/debug tag, and point-to-point flag.
- Confirmed the section handler parses `Routes` containers with `Route` children. The installed Amax field layout and its exact runtime mapping are still undecoded; `RLDatabase.bin` has no demonstrated role in route selection.
- Updated `CONTEXT.md` and `docs/MAP_COVERAGE.md` with this verified relationship. No code or format interpretation was added because the Amax bytes are still opaque.
- Map milestone remains **0/18 race directories verified playable**. Event-to-route mapping, route count, starts, checkpoints, drivable layer, collision, and complete laps remain unknown.

## Installed route/AID inventory cross-check

The 33 extracted AID files across 15 race folders and their suffixes are recorded in `docs/ASSET_INVENTORY.md`; suffixes range from `_1` to `_6`, with gaps. This inventory can be compared with the Amax route names after that payload is structurally decoded, but suffixes alone do not imply variation IDs or original playable-event counts. The three multiplayer folders without installed AIDs remain without a route candidate.

The five-hour usage window was 97% remaining at the latest read. Work continues toward the requested 20% stop threshold.

### Next concrete route-decoding step

1. Follow the section child iterator/read helpers called by the `Routes` handler in Ghidra and identify which operations read strings, scalars, and nested children.
2. Trace the `Route` child handler to the runtime constructor and label only fields that flow into the route ID, level path name, AID tag, or the two copied runtime fields.
3. Use that field map to decode the corresponding `Amax.bin` rows, then compare the recovered tags with the 33 installed AIDs. Keep the Amax KSLF tables opaque until the table/field mapping is byte-validated.
4. After IDs/tags are known, inspect the level/event selector to recover which route IDs are available to each race configuration. An ID/name table alone still does not prove a full drivable route.

**CORRECTION (evidence review, 2026-11-15):** The inspected level setup performs separate lookups for a level name and route tag. Their backing resource has not been established as Amax. The inspected Routes/Route section reader is already associated with CPPhysic in FORMATS.md; it must not be described as a KSLF/Amax reader without a caller/data-flow link. The interrupted follow-up probe identified object cleanup, not a route deserializer. Follow the KSLF magic consumer and runtime registry initialization to establish provenance before decoding fields.

**VERIFIED follow-up:** Following the actual KSLF magic consumer reaches its schema loader and version-2 string transform. A private executable-led probe decodes the bounded Amax schema-string region into coherent names for 209 descriptors, including Route (21 fields), City (19 fields), and AmaxGameDataObject. Route includes City, RouteNumber, Thumbnail, MinimumYPosition, OnlyAllowSingleLap and CircuitType; the base includes Name, Index and LocString. Record values, city links and event mapping still require decoding and independent checks.

**VERIFIED (private executable-led Amax record census, 2026-11-15):** Decoding each record string tail separately recovers 70 Route records and 22 City records. The descriptor traversal consumes all 8,333 group rows and ends exactly at file EOF. Thirty route names match the 33 installed AID basenames; Brighton fifth, LA Downtown third, and LA Downtown multiplayer figure-of-eight remain unmatched. All 70 serialized City references resolve to City records. The candidate Index values repeat and must not be promoted to global route IDs; event selection and runtime ID provenance remain unresolved. Raw names and record values remain in ignored private JSON.

**VERIFIED (reproducible schema tool and span fix):** Added tools/inspect-kslf-schema.py with three synthetic tests. It successfully inspected all 76 decoded KSLF files across the full archive cache, including all 51 gamedata files. Corrected Rust region strides 5/6 to 16/6 and updated the synthetic span fixture. Four focused KSLF tests, three Python tests, formatting, clippy, the workspace suite (229 passed, 112 ignored), and publish-check passed. No original assets or decoded schema outputs are committed.

**VERIFIED (event and multiplayer route references):** The schema tool now exposes checked root record references. All 86 SinglePlayer event Route references resolve to 30 Amax routes matching installed AID names. Multiplayer has 439 root Route references spanning 33 distinct Amax routes; 29 names match installed AIDs and four lack an exact match. In particular, the referenced figure-of-eight tag differs from the installed multiplayer AID basename. No alias is introduced. All 76 KSLF files still parse; four Python tests pass. Nested references, menu eligibility, runtime route ID conversion and actual playable configurations remain unresolved.

**VERIFIED (configured selection and scalar extension):** The schema diagnostic now reads inherited fields, supported scalar values and root reference arrays. It recovers the configured 30-route/four-arena split and online/split-screen/system-link playlist lists. The configured routes equal the 30 SinglePlayer-referenced routes and all have exact installed AID basename matches. Seven synthetic tests pass. Graphify navigation queries were run against the existing graph, whose contents predate the new diagnostic. Usage last read 53% remaining; work continues to the requested 20% threshold.

## Final checkpoint — 2026-11-15

Usage reached 20% remaining in the five-hour window, so work stopped as requested. Commits this continuation: 472c5e3 (KSLF schema strings and span correction), af300fc (event/route references), and 678863b (inherited values and configured route/arena lists). Seven Python tests and all 76 decoded KSLF files pass; the Rust stride change passed formatting, clippy, 229 workspace tests and publish-check. Graphify queries were used for project navigation; the graph is older than the new tool.

The final read-only probe identifies the registered Route post-load callback: it copies the database record ID into the runtime lookup-ID field before route registration. This connects the serialized record IDs used by the event/config references to the runtime route lookup path and distinguishes them from serialized per-city `Index` values. Callback/metadata output remains ignored and private. Registry insertion details and live event selection remain to be checked. The current phase has a decoded configured route inventory and serialized event associations, but starts, gates, collision and full laps remain unverified; 0/18 race directories are verified playable.

### Handoff status

- **Done:** bounded KSLF schema/string/record decoding; Amax Route/City inventory; serialized single-player event and multiplayer configuration route references; record-ID provenance into runtime lookup; 30 configured route names joined to installed AID basenames.
- **Still unknown:** the exact runtime registry insertion/selection sequence, event/menu eligibility, the three unmatched AIDs, arena-to-level resolution, and route-specific starts, gates, drivable collision and lap completion.
- **Next useful step:** trace the route registration helper and a live event-selection consumer, then verify one configured route end to end (record ID → runtime record → level/AID path). Keep that separate from marking it playable until starts, gates, collision and a full lap are verified.
- **Map milestone:** 0/18 installed race directories verified playable. Route discovery is substantially better; full map rendering and race reconstruction are still open.
