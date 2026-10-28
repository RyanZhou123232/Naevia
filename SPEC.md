# Naevia — Demo 1 Implementation Specification

Version: 0.3 | Status: Implementation baseline with acceptance checklist

## 1. Goal and Scope

Build the Godot frontend first, then connect the API backend and automatic local memory. This document specifies the tools, components, interactions, and implementation order needed to start development.

Demo 1 includes one fixed home, a small mining area, furniture placement, the witch and blacksmith, clickable speech bubbles, independent persistent memories, and a three-person conversation.

Keepsakes, expansion, birthdays, official gifts, local inference, and large-world simulation are deferred. Numeric values below are adjustable implementation defaults, not measured results.

## 2. Tools and Responsibilities

- **Client:** Godot 4.7 stable, standard build, GDScript, Compatibility renderer. Use matching Windows export templates. Record the exact editor version in README.
- **Backend:** Python 3.12, FastAPI, Uvicorn, Pydantic, HTTPX. Lock dependencies when scaffolding.
- **Model adapter:** HTTP chat-completions transport for an OpenAI-compatible provider. This names the protocol, not the chosen model vendor. Endpoint, model ID, and credentials are configuration.
- **Persistence:** Local JSON saves and character memories; JSONL dialogue history. No SQL, vector database, or embedding model for Demo 1.
- **Assets:** AI image generation for initial concepts if useful; LibreSprite for pixel cleanup, animation alignment, and PNG export; Godot TileSet editor for tile collision and assembly.
- **Verification:** pytest for backend and memory contracts; manual Godot playthrough for visual interaction.

Godot owns gameplay, dialogue coordination, and all local file reads/writes. FastAPI composes prompts, calls the model, normalizes responses, and returns proposed memory updates. The cloud model never searches the player's disk.

During development, run the backend on localhost. For external testers, point the same client to an HTTPS deployment with a revocable demo access token. Keep provider credentials on the backend; no accounts or cloud-save database are needed for Demo 1.

### Repository structure

```text
Naevia/
  PRD.md
  SPEC.md
  README.md
  client/
    project.godot
    scenes/
    scripts/
    assets/
    data/characters/
    data/furniture.json
  server/
    app/main.py
    app/schemas.py
    app/provider.py
    app/prompts.py
    tests/
    .env.example
    requirements.lock
```

## 3. Frontend Foundation

### 3.1 Scene and modules

Create World.tscn containing:

- TileMapLayer nodes for background walls, solid terrain, and mineable resources.
- A CharacterBody2D player with CollisionShape2D, AnimatedSprite2D, and Camera2D.
- Separate witch, blacksmith, and pigeon scenes, each with an Area2D click target.
- A furniture container with individual placed-object nodes.
- A CanvasLayer for bubbles, reply input, inventory, and contact controls.

Implement these scripts with clear ownership:

- PlayerController: movement and animation.
- MiningController: mining progress and resource removal.
- FurnitureController: purchasing, placement, moving, storing.
- ConversationCoordinator: participants, speaker order, and conversation IDs.
- BubbleController: speaker anchors, wrapping, segments, and input focus.
- ApiClient: ordinary requests and streamed replies.
- MemoryStore: per-character files and queued updates.
- SaveManager: world state and safe file replacement.

### 3.2 Scale and camera

Use Terraria's base proportions: a tile is 16 × 16 game pixels, representing approximately 0.61 meters / 2 feet. The standard player collision box is 20 × 42 pixels; standing clearance is roughly three tiles. Match furniture and architecture to these proportions. Sprite canvases may extend beyond collision bounds.

Start with a 640 × 360 world viewport, nearest-neighbor texture filtering, integer display scaling, and letterboxing. Initial window: 1280 × 720. Render dialogue UI at display resolution with a bundled font covering English and Chinese.

### 3.3 Art and animation

First create one scene composition establishing palette, character appearance, house, mining area, and bubble placement. Then separate it into reusable assets.

Prepare ground, grass, stone, flooring, walls, door, table, chair, lamp, and two background layers. Prepare player, witch, blacksmith, and pigeon sprites. Start with placeholders so art completion does not block interaction work.

Initial animation targets: four-frame player walking and mining, two-frame pigeon flight, character idle poses, and short walk cycles for visitors. Keep foot anchors consistent. Export PNG sprite sheets; use AnimatedSprite2D for playback. Small debris can use GPUParticles2D. No prerecorded video is needed.

### 3.4 Gameplay controls

- A/D moves; Space jumps.
- Hold left mouse on a resource tile within four tiles to mine. Start at one second per tile; each tile yields one stone.
- Mining removes that resource cell and its collision, exposing the background. Only marked resources are mineable; the house stays fixed.
- An inventory menu offers a table, chair, and lamp. Initially each costs three stone.
- Selecting an item enters placement preview. Left click places; right click cancels. Snap placement to the tile grid for Demo 1.
- Arrange mode selects existing furniture to move or store. Stored items can be placed again without paying twice. Furniture does not block walking in this prototype.
- Provide a development action to reset resources.

UI consumes clicks before gameplay. Text focus disables movement shortcuts. Saving follows material and furniture changes; player position saves on a short debounce.

## 4. Conversation Interaction

### 4.1 Contact and invitations

After initial play, a timer brings a pigeon near the player. A separate timer can bring the blacksmith to the door. Allow only one unsolicited invitation at a time, with configurable intervals. Ignoring an invitation causes no relationship penalty.

Click the pigeon to talk remotely with the witch; anchor her replies to the pigeon. Click the blacksmith to talk locally; anchor his replies above him. A small Contacts control offers Witch, Blacksmith, and Invite Both. Invite Both brings the two characters into the house scene and starts group mode.

Add development buttons to trigger contacts immediately.

### 4.2 Input and bubble sequence

1. Click a messenger or character to open the conversation and reply control.
2. Click the reply field to type; click Send to submit. Enter is not required.
3. Save the player message, release text focus, and show a waiting indicator.
4. Stream the response into a bubble while movement and other gameplay remain available.
5. Wrap text automatically and split long replies at sentence boundaries. Click to advance overflow segments; retain the last segment until dismissal or a new reply.
6. An on-demand History control shows prior dialogue. Do not create a permanent chat sidebar.

Bubble font size, padding, width, height, and segment length live in one configurable UI resource. Start around 320 display pixels maximum width and 160 maximum height. Short messages shrink to fit.

Convert speaker world positions into CanvasLayer coordinates each frame. Clamp bubbles to screen bounds and offset overlaps. Only the current speaker needs a full-size reply bubble.

Allow one pending player submission per conversation. Closing or changing conversations cancels remaining AI turns after the current response. Use conversation IDs so a late reply cannot appear in a different conversation.

### 4.3 Character profiles and group turns

Create editable JSON profiles with personality, speaking style, background, current personal situation, and starting relationship.

Starter defaults:

- Witch: elderly, observant, dry humor, values small pleasures; currently tending an unusual garden.
- Blacksmith: practical, warm, direct; currently restoring an old workshop tool.

They begin as familiar acquaintances. Do not invent past shared events. These writing defaults can change without altering code. Replies usually contain one to three sentences and may include the character's own life. Avoid repetitive praise.

ConversationCoordinator chooses turns. In private mode, use the selected character. In group mode, prefer a character explicitly addressed by the player; otherwise alternate the first responder. A reply can nominate another present character to answer. Allow at most three character turns per player message, then wait for the player. A new player message cancels queued turns after the current turn.

Each speaker receives only their own eligible memory and the dialogue they can hear. Characters can address each other or decline to add anything. The announcement example must emerge from these rules, not a fixed script.

## 5. Backend and Model Integration

### 5.1 Configuration

Server environment:

- NAEVIA_PROVIDER_BASE_URL
- NAEVIA_MODEL
- NAEVIA_PROVIDER_API_KEY
- NAEVIA_MODEL_MODE=mock or live
- NAEVIA_DEMO_ACCESS_TOKEN for hosted testing

Client configuration specifies backend URL and, for hosted tests, the demo access token. Keep real provider credentials out of client files and the repository. Mock mode requires no provider account.

ProviderAdapter exposes stream_reply(context) and update_memory(context). Use HTTPX for the external HTTP connection. Unsupported provider-specific protocols require an adapter; do not assume every provider implements the selected transport identically. Validate the selected provider/model with a smoke test before live integration.

### 5.2 Endpoints

- GET /health: returns service status and mock/live mode.
- POST /v1/chat: streams one character turn.
- POST /v1/memory/update: returns a complete validated replacement memory for one character.

Use Pydantic schemas for request and response bodies. The service remains stateless with respect to game saves; no file-access endpoint is needed.

A chat request contains request_id, conversation_id, speaker_id, participants, profile, memory, recent_messages, and scene_context. Messages carry event_id, speaker, addressee, audience, and text. Audience is assigned by game logic.

Compose prompts in this order: character instructions, profile, authorized memory, observable scene context, recent dialogue. Private requests never include the other character's private history. Group requests include the shared gathering transcript.

### 5.3 Streaming contract

Normalize provider output into newline-delimited JSON using FastAPI StreamingResponse:

```json
{"type":"text_delta","request_id":"r1","text":"That sounds"}
{"type":"text_delta","request_id":"r1","text":" like good news."}
{"type":"done","request_id":"r1","speaker_id":"witch","addressee":"player"}
```

A terminal error event contains the request ID and a displayable message. In Godot, poll HTTPClient and read_response_body_chunk without blocking the game loop. Buffer raw bytes until complete newline-delimited records arrive, then decode UTF-8 and parse JSON. Use HTTPRequest for health and memory requests.

Ask the model for an optional first-line recipient marker such as @to:blacksmith. Buffer and remove this line before displaying text. Validate it against participants; missing or invalid markers default to the player. Normalize nonstreaming provider replies into the same event format if necessary.

Mock mode returns deterministic streamed replies and valid memory snapshots. On failures preserve the player's message, offer retry, and keep gameplay running. Only commit a completed character turn to history.

## 6. Automatic Local Memory

### 6.1 Storage

Godot reads and writes files under user://saves/default/:

```text
world.json
characters/witch.memory.json
characters/blacksmith.memory.json
history/witch.jsonl
history/blacksmith.jsonl
history/group.jsonl
```

Static character profiles stay in res://data/characters/. Separate history from concise current memory. No SQL queries or vector search are required in this demo.

Memory shape:

```json
{
  "schema_version": 1,
  "character_id": "witch",
  "revision": 2,
  "last_processed_event_id": "e12",
  "facts": [],
  "ongoing": [],
  "experiences": []
}
```

Each entry has id, text, source_event_ids, learned_from, and share_scope (private or shared). Facts describe stable knowledge; ongoing describes current situations; experiences summarizes meaningful exchanges. Initially cap combined memory text at approximately 3,000 characters per character and consolidate during updates. Retain raw history separately.

### 6.2 Read, generate, update

1. Load the speaking character's memory and last 12 relevant messages. Await any pending memory update for that character before assembling context; gameplay remains active.
2. Send that context to /v1/chat and display the streamed reply.
3. Append the completed exchange to history with stable event IDs.
4. In the background, send that character's current memory, revision, and newly heard events to /v1/memory/update.
5. The model proposes a replacement snapshot, preserving valid facts, updating changed situations, and merging duplicates. It must retain source links and cannot invent events.
6. Validate schema, character ID, base revision, sources, and text limits. Write a temporary file and replace the old snapshot after validation.

Memory processing is automatic, with no routine prompts or explicit “remember this” command. Model generation proposes the changes; code controls files and persistence.

Serialize updates per character. Reject stale revisions, retain the old file on error, and retry unprocessed events. Event IDs and processing cursors prevent duplicate application. Persist history before requesting an update so a restart can recover pending work.

### 6.3 Group knowledge

After group turns finish, update each participant from their own existing memory plus the events they heard. Never copy the witch's memory into the blacksmith's update input.

Private entries inform private conversations. For Demo 1 group prompts, withhold private entry content and supply shareable memory plus the current public transcript. Information spoken publicly can become shared knowledge for listeners. Keep the original speaker as the knowledge source.

## 7. World Persistence

world.json stores schema_version, player position, remaining resource tiles, material counts, stored furniture, and placed object IDs/positions. Use stable IDs, temporary-file replacement, and the same save-directory ownership as memory.

Local storage does not imply local AI processing. The backend and provider receive the selected context sent for a request. Do not log full dialogue or memory by default on the hosted service.

## 8. Implementation Milestones

### A. Frontend with mock dialogue

Create the Godot project, scene layout, visual references, placeholder assets, movement, mining, furniture controls, and bubbles. Use a local mock reply source first. Complete when a player can gather, decorate, click a pigeon, type, and receive replies without live AI.

### B. Backend framework and live model

Implement FastAPI schemas, mock/live ProviderAdapter, health, streaming chat, and configuration examples. Connect Godot's ApiClient. Complete when a live reply displays progressively without freezing the world.

### C. Local memory

Implement per-character JSON, dialogue event history, memory updates, and queued file writes. Add the blacksmith with isolated state. Complete when both characters maintain different knowledge across restarts.

### D. Group conversation

Implement Invite Both, speaker coordination, recipient markers, shared history, and listener memory updates. Complete when one character can learn through another character's speech without access to their private file.

### E. Playable Windows handoff

Replace essential placeholders, finish required animation and saving, and export Windows. Deliver the Godot project, backend source, locked dependencies, example configuration, Windows build, and README.

README must explain editor setup, backend setup, mock/live modes, model configuration, running locally, hosted endpoint configuration, and export. List placeholders and known issues.

## 9. Verification

Add focused checks for request schemas, mock event output, isolated character contexts, memory revisions, and duplicate events. Manually verify bubble positioning, input focus, streamed text, mining, placement, and save recovery. Record actual results; the document itself is not proof of implementation.

Acceptance playthrough:

1. Gather stone, obtain furniture, and place it.
2. Privately tell the witch about preparing for exams.
3. Restart: the witch remembers; the blacksmith has not learned that exchange.
4. Invite both and announce making the Dean's List.
5. Let them respond and talk to each other, without scripting a required reaction.
6. Restart and verify updated individual memories and the saved room.

Model vendor, exact model ID, final character writing, and final visual palette remain tuning/configuration choices. They do not block starting frontend and backend mock work. Select and record a working cloud model before live milestone B is complete.

## 10. Demo 1 Acceptance Checklist

This is the product acceptance checklist for the first playable demo. Pass items by playing the Windows build with a live model, not just by reading code or watching mock replies. The developer should record pass/fail and any remaining issue for each item. Natural wording may vary; exact scripted sentences are not required.

### 10.1 A playable, coherent pixel world

- [ ] I can open the Windows build using the supplied instructions and enter the home and adjacent mining area.
- [ ] The world uses a horizontal 2D view with consistent Terraria-style tile and character proportions. Characters, terrain, furniture, and backgrounds form a coherent pixel-art scene.
- [ ] I can move and jump. Walking and mining have visible animation; mining is more than a static image with an inventory counter.
- [ ] I can mine stone, obtain at least three furniture types, and place, move, and store them. The result is visible in the room, and furniture can be placed again after being stored.

### 10.2 Click-to-talk bubbles

- [ ] A pigeon can arrive and the blacksmith can visit. I can click either to start the corresponding conversation without pressing Enter.
- [ ] Replies appear in bubbles attached to the correct character or messenger, not in a permanent chat sidebar.
- [ ] Short text produces a compact bubble; longer text wraps or splits into readable segments. Bubbles do not run off screen or become unreadable when the camera moves.
- [ ] I can click the reply field, type, and click Send. Typing does not move the player; clicking UI does not mine or place items behind it.
- [ ] I can resume movement and gathering while waiting for a reply, then read it and respond. Previous dialogue can be revisited.

### 10.3 Real AI conversation and distinct characters

- [ ] Both characters answer new, unscripted messages through the live API. The core demo is not limited to predefined responses.
- [ ] The witch and blacksmith have recognizably different voices and can discuss their own current lives as well as mine.
- [ ] Ordinary replies are usually brief. Characters respond to what I actually say, rather than turning every exchange into generic praise.
- [ ] I can ignore a contact invitation or stop chatting and continue playing without guilt messages or relationship penalties.

### 10.4 Automatic, independent memory

- [ ] I tell the witch one specific detail, such as an upcoming exam date or a favorite food. Later, she can use that detail when relevant without asking me to save it.
- [ ] After closing and reopening the game, the witch can still recall the detail. No explicit memory command or routine confirmation was needed.
- [ ] Before I share it elsewhere, the blacksmith does not claim knowledge of the detail told only to the witch. Inspecting request context confirms that he was not given her private memory.
- [ ] When I tell the witch that the exam is now finished, her later responses reflect the update instead of repeatedly treating it as upcoming.

### 10.5 Shared conversation and learning from others

- [ ] Invite Both brings the witch and blacksmith into a shared interaction. I can announce something, such as making the Dean's List, and receive context-appropriate responses.
- [ ] Characters can address and answer each other through correctly attributed bubbles. At least one live exchange demonstrates this, without a hard-coded dialogue script.
- [ ] A character learns a new, shareable detail spoken by the other character and can recall it later, including after restarting. A character's own life update can be used to test this without exposing my private conversation.
- [ ] Group participation does not grant access to either character's full private memory. The conversation returns control to me rather than running indefinitely.

### 10.6 Continuity and basic reliability

- [ ] After restarting, my materials, furniture inventory, room arrangement, conversation history, and character memories remain available.
- [ ] The world stays responsive during live replies and background memory processing. An ordinary play session can include gathering, decoration, private chat, and group chat without crashing.
- [ ] If the API is unavailable, I see a clear failure state, retain my message for retry, and can continue basic gameplay. Retrying does not duplicate a completed exchange.

### 10.7 Handoff

- [ ] Another developer can follow README to open the client, run the backend in mock mode, configure live AI, and produce the Windows build.
- [ ] The delivered source includes configuration examples, dependency versions, development contact triggers, and a clear list of placeholders and known issues. Real provider credentials are absent from source and client builds.

### Acceptance session

Use one short session to connect the checks: gather and decorate; privately tell the witch an exam detail; restart and check separate knowledge; update the exam status; invite both and announce Dean's List; observe a character-to-character exchange; restart and check what each character remembers.

Run this once from a fresh save and once with an existing save. Record the machine and model configuration, noticeable reply delays, and any failed items. Automated checks support this session but do not replace the actual playthrough.

Demo 1 is accepted when these checks pass and the owner is satisfied with the visual coherence, character distinction, and comfort of alternating play and conversation. Final art polish, keepsakes, expansion, celebrations, local inference, and a larger world are not acceptance conditions for this demo.

## 11. Technical References

- [Godot 4.7 stable](https://godotengine.org/download/archive/4.7-stable/)
- [Godot local data paths](https://docs.godotengine.org/en/stable/tutorials/io/data_paths.html)
- [Godot HTTPClient](https://docs.godotengine.org/en/stable/classes/class_httpclient.html)
- [FastAPI streaming responses](https://fastapi.tiangolo.com/advanced/custom-response/)
- [Terraria unit conversion](https://terraria.wiki.gg/wiki/Template%3AReal-life_size)
- [Terraria player dimensions](https://docs.tmodloader.net/docs/stable/class_player.html)

