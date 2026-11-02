# Naevia — Phase 1 visual and design handoff

Created by **RyanZhou123232**. Naevia is a companionship-focused pixel game: gather resources, furnish a home, and talk with a witch and a blacksmith who have independent persistent memories.

## What this repository delivers

This repository intentionally contains **the design specification and visual assets, not a playable game or backend**. Separate local development is not part of this submission and has not been uploaded. The next developer should implement the game using [SPEC.md](SPEC.md) and the existing art.

The original October 28 handoff snapshot is commit `6bb0698`. This README and [prompt_log.md](prompt_log.md) are **late documentation supplements added on November 1, 2026**. They do not represent code or documents committed during Wednesday's class. Earlier history is preserved, including the spec-only first commit and the later consolidation that removed the previous README. The source snapshot and original SPEC/art are unchanged by this supplement.

## Open and inspect the handoff

1. Download the repository ZIP from GitHub, extract it, and keep the folder structure intact.
2. Read [SPEC.md](SPEC.md), especially sections 3.3 (asset integration), 8 (implementation order), 10 (26 product checks), 11 (planned setup), and 12 (actual delivery status).
3. Open [the scene preview](art/previews/scene_direction.png) and [animation contact sheet](art/previews/animation_contact.png) in an image viewer. These previews are references, not playable scenes.
4. Use [art/export/manifest.json](art/export/manifest.json) to import individual runtime PNGs. Frame sizes, anchors, animation rates, and stable visual IDs are recorded there.

**There is no executable to launch, client project to open, or backend to install from this repository yet.** SPEC section 11 describes the intended implementation/setup contract; it is not a verified command sequence for files currently supplied. No API key is needed to inspect the artwork. The planned game uses Godot 4.7, Python/FastAPI, local JSON/JSONL persistence, and a configurable cloud model; exact implementation and dependency versions remain the implementing developer's responsibility.

## Intended controls

These are requirements for the next developer, not working controls in this handoff:

- A/D: move; Space: jump.
- Hold left mouse on an in-range resource: mine.
- Inventory and arrangement UI: acquire, place, move, and store table/chair/lamp; right click cancels placement.
- Click the pigeon or blacksmith to talk; click the reply field and Send to reply.
- Contacts UI: Witch, Blacksmith, Invite Both.

## File map

- `SPEC.md`: product vision, technical design, modules, API/memory contracts, implementation milestones, planned setup, and acceptance checklist/status.
- `art/export/manifest.json`: IDs, file paths, frame layout, playback rates, and anchors for 35 runtime PNGs.
- `art/export/characters/`: ten character/action sheets, including player walk/mine/jump, visitors, and pigeon flight.
- `art/export/tiles/`: eleven terrain/building tiles on a 16×16 grid.
- `art/export/props/`: door, window, and house trim.
- `art/export/furniture/` and `icons/`: three furniture images and four inventory icons.
- `art/export/backgrounds/` and `effects/`: two backgrounds, mining cracks, and stone chips.
- `art/previews/`: assembled scene, palette, object/animation sheets, UI direction, and repeat previews; enlarged copies are for inspection.
- `art/references/`: source scene and character visual references.
- `art/source/palette.json`: shared colors; `validation-report.json`: asset file-contract checks and hashes. These are art inputs/evidence, not gameplay source.
- `prompt_log.md`: recovered original prompts, tool provenance, and the scope/limits of this documentation reconstruction.

The module/file names listed in SPEC's proposed repository structure are a development plan. `client/`, `server/`, local launchers, and a Windows build are not supplied in this GitHub handoff.

## Acceptance status of the submitted snapshot

The visual package is delivered. The existing validation report records 35 runtime assets and zero file-contract errors; those facts do **not** establish runtime or artistic acceptance. No game/backend is submitted and no gameplay acceptance session can run from this snapshot. The IDs below correspond to SPEC section 10 and retain its requirements.

- **10.1-1 — Not met:** no Windows build to enter the home/mining area.
- **10.1-2 — Partially met:** coherent visual references and separate runtime artwork exist; the actual interactive world is not assembled.
- **10.1-3 — Partially met:** walk/mining animation sheets exist; movement, jumping, and runtime animation are not implemented here.
- **10.1-4 — Not met:** mining, inventory, and furniture placement/move/store require implementation.
- **10.2-1 — Not met:** timed pigeon/blacksmith visits and click-to-contact are not implemented.
- **10.2-2 — Not met:** anchored dialogue bubbles are not implemented; UI images only guide styling.
- **10.2-3 — Not met:** adaptive wrapping, segmenting, and camera-safe bubbles require runtime work.
- **10.2-4 — Not met:** reply input/Send and UI/gameplay input isolation are not implemented.
- **10.2-5 — Not met:** asynchronous play during replies and dialogue history are not implemented.
- **10.3-1 — Not met:** no live model adapter or unscripted NPC conversation is delivered.
- **10.3-2 — Not met:** distinct characters are specified, but their live voices/lives are not verified.
- **10.3-3 — Not met:** concise, context-sensitive replies cannot be assessed without implementation.
- **10.3-4 — Not met:** penalty-free contact handling is specified but not implemented.
- **10.4-1 — Not met:** automatic personal-detail memory is not implemented.
- **10.4-2 — Not met:** memory persistence across restarts is not implemented.
- **10.4-3 — Not met:** request-context isolation between NPCs is not implemented or tested.
- **10.4-4 — Not met:** replacing outdated memory is not implemented.
- **10.5-1 — Not met:** Invite Both and group conversation are not implemented.
- **10.5-2 — Not met:** live NPC-to-NPC exchange and attribution are not implemented.
- **10.5-3 — Not met:** learning/recalling shareable facts from another NPC is not implemented.
- **10.5-4 — Not met:** group privacy and bounded turn control are not implemented.
- **10.6-1 — Not met:** world, inventory, layout, history, and memory persistence are not implemented.
- **10.6-2 — Not met:** live-response responsiveness and complete-session reliability are not tested.
- **10.6-3 — Not met:** API failure/retry behavior is not implemented.
- **10.7-1 — Not met:** SPEC supplies the planned setup path, but no client/backend/build exists to follow it end to end.
- **10.7-2 — Partially met:** design contracts, art manifest, and pending-status notes exist; executable configuration examples, locked dependencies, and developer triggers are not delivered.

These are Phase 1 progress statements, not a claim that the final Demo 1 acceptance checklist has passed. SPEC section 12's NOT READY decision remains accurate for this repository.

## Next steps, in priority order

Follow SPEC section 8: (A) integrate the supplied art into a playable Godot world and mock conversation UI; (B) add the local backend and live adapter; (C) implement automatic independent local memory; (D) implement bounded group conversation and knowledge sharing; (E) export Windows and run the full fresh-save and existing-save acceptance sessions. Preserve the original visual direction and all 26 product requirements.

## Known traps and shortcuts

- Scene/UI previews are flattened illustrations. They cannot substitute for independent world objects, collision, controls, or speech bubbles.
- Import native runtime exports with nearest-neighbor filtering. Enlarged previews and filename guesses are not runtime metadata.
- The tiles cover a limited straight-edge/platform set, not arbitrary autotiling. The door image provides no collision/passability behavior.
- No game source, layered Aseprite source, or redistributable font is included. A properly licensed font supporting English/Chinese must be chosen during implementation.
- Some SPEC prose still calls for a single handoff document or assumes preparation scripts exist in `art/source/`; the actual submission is SPEC plus art plus the two course-required documentation files. The manifest and validation JSON are the supplied asset records.
- The handoff focuses on design and reusable art. Game integration and runtime validation were deferred; art file validation must not be reported as successful game testing.

## Course submission caveat

The course asks for a Phase 1 code foundation and a repository frozen at the end of Wednesday's class. This repository's original handoff is art/spec only, and these two documents were added later. The supplements improve completeness and honesty of documentation but do not retroactively satisfy the missing-code or original freeze-time requirements. No dates or history have been rewritten to suggest otherwise.
