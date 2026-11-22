# ESP 3.2 Roadmap

Improve flight transitions, Odyle gathering, FP management, and route playback.

> Status: Planned, with completed source fixes noted below. Live validation and packaging are tracked separately.

## P0 — Core Fixes

### Skip nodes that disappear during approach

- [x] Implemented in ESP 3.1: keep F9 running when the selected node becomes invalid during approach or immediately before F.
- [x] Defer the unavailable node for 30 seconds and select another without counting a gathering attempt.
- [x] Preserve flight cleanup and existing stops for input failures or unavailable player state.

**Validation:** Controller tests and MSVC syntax checks passed. Live reproduction with another player taking the node and an updated release executable are still pending; this fix is not yet included in the published 3.1 archive.

### Record and replay mid-air flight cancellation

- [ ] Record V separately from confirmed touchdown.
- [ ] Prevent falling segments from being saved as ordinary Fly waypoints.
- [ ] Resume walking after touchdown is confirmed.
- [ ] Preserve normal descent recorded by flying down to the ground.
- [ ] Assess compatibility with older routes without guessing missing V actions.

**Acceptance:** Walk → Takeoff → Fly → V mid-air → Descend → Land → Walk completes without freezing or unintended takeoff.

### Correct Odyle approach distance

- [ ] Port the relevant approach-distance correction from Better A2.
- [ ] Test approaches from below, from the side, and after FP recovery.
- [ ] Keep ground-resource interaction distances unchanged.

**Acceptance:** The character reaches interaction range instead of stopping underneath the node.

### Land when F9 has no eligible target

- [ ] Allow a short repeat-search window after collection.
- [ ] Request landing when no next target is found.
- [ ] Confirm touchdown and continue searching from the ground.
- [ ] Bound failed landing attempts and display a clear status.

**Acceptance:** The character does not exhaust FP while hovering without a target.

## P1 — FP Management and Detection

### Separate landing and recovery reasons

- [ ] After no-target landing, allow takeoff when a target appears and minimum takeoff FP is reached.
- [ ] After low-FP landing, wait for the configured recovery threshold.
- [ ] After a node-batch pause, require the timer and applicable FP conditions.
- [ ] Display the current waiting or recovery reason.
- [ ] Keep recorded route flight actions separate from nearby F9 recovery rules.

### Investigate inconsistent F9 target detection

- [ ] Compare initial selection with selection during sustained operation.
- [ ] Log rejection reasons: range, filters, area, allowed locations, FP, height, deferral, and stale data.
- [ ] Change selection rules only where a discrepancy is confirmed.

## P1 — Gathering Features

### Support Odyle in Gather Along Route

- [ ] Support airborne nodes using XYZ coordinates, corridor limits, route order, and FP.
- [ ] Resume the route without skipping required recorded actions.
- [ ] Validate mixed ground and air routes.

### Handle Odyle-to-ground-resource transitions

- [ ] Approach the next ESP-selected ground resource.
- [ ] Request V at a full 3D XYZ distance of 4 meters or less.
- [ ] Apply no separate safe-height check.
- [ ] Confirm touchdown before gathering.
- [ ] Bound unsuccessful transition attempts.

## Validation

- [ ] Mid-air V cancellation and normal descent to the ground.
- [ ] Ground gathering, Odyle, and mixed routes.
- [ ] Area and selected-location restrictions.
- [ ] Target disappearance and reappearance.
- [ ] Low FP, recovery, no-target landing, and failed landing.
- [ ] Characters with different FP reserves under equivalent conditions.
- [ ] Focus loss, pause, resume, and stop.
- [ ] MOBFARM and counterattack regression checks.

## Documentation and Packaging

- [ ] Update the English manual, USAGE guide, changelog, and language packs.
- [ ] Prepare a compact update requiring the full ESP 3.1 base installation.
- [ ] Include changed binaries, matching PDB, and required supporting files.
- [ ] Exclude unchanged assets.
- [ ] Verify preservation of settings, routes, and data-format compatibility.

## Out of Scope

- Mandatory walking between every node.
- Immediate takeoff for every target.
- A complete new terrain pathfinder.
- Underwater gathering.
- New maps.
