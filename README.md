# BILAX 1.8 — A Point & Click Adventure

A browser-based horror point-and-click adventure game. You are trapped in a dark house. A shadowy monster named Bilax roams the rooms. Find a flashlight, find a key, and escape through the front door.

---

## How to run

**Locally (quickest)**
Just open `index.html` in any modern browser. No build step, no dependencies.

**Local HTTP server (recommended — avoids browser CORS restrictions on asset loading)**
```bash
# Python 3
python3 -m http.server 8080
# then open http://localhost:8080
```

**GitHub Pages**
Push the repo to GitHub, enable Pages on the branch. The game loads at `https://<user>.github.io/<repo>/`.

---

## How to replace placeholder art with real assets

Every asset tries to load from `/assets/<filename>`. If the file is missing the game draws a labeled colored rectangle instead — the game is fully playable with placeholders.

**Steps:**
1. Create an `/assets/` directory next to `index.html`.
2. Place your PNG file in that directory with the **exact filename** listed in the manifest below.
3. Reload the page — the asset loads automatically. **No code changes needed.**

Transparent backgrounds are required for all sprites.

---

## ASSET_MANIFEST — Complete reference

All prompts use these shared Stable Diffusion settings unless noted:
- **Model:** DreamShaper v8 or Deliberate v3
- **Steps:** 30
- **CFG:** 7
- **Negative prompt:** `text, watermark, signature, blurry, photorealistic, 3d render, anime`

---

### Backgrounds — 960 × 420 px

| Asset key       | Filename                | In-game role                                    | SD Prompt |
|-----------------|-------------------------|-------------------------------------------------|-----------|
| `bg_living_room`  | `bg_living_room.png`  | Starting room. Has sofa and coffee table hiding spots; exits to hallway, kitchen, front door. | `point and click adventure game, 2d painted background, dark horror night, living room interior, worn sofa, moonlight through cracked window, cinematic lighting, LucasArts style, detailed` |
| `bg_hallway`      | `bg_hallway.png`      | Central hub. Contains coat closet hide spot; exits to living room, master bedroom, attic stairs. | `point and click adventure game, 2d painted background, dark horror night, narrow hallway, coat closet door, stairs up, shadows, moonlight, LucasArts style, detailed` |
| `bg_master_bedroom` | `bg_master_bedroom.png` | Contains the Note item; has bed and wardrobe hiding spots; exit to hallway. | `point and click adventure game, 2d painted background, dark horror night, master bedroom, rumpled bed, wardrobe, moonlight through window, LucasArts style, detailed` |
| `bg_attic_dark`   | `bg_attic_dark.png`   | Attic without flashlight — nearly pitch black. Trunk not visible. | `point and click adventure game, 2d painted background, dark horror night, dark attic, boxes old furniture, pitch black, LucasArts style, detailed` |
| `bg_attic_lit`    | `bg_attic_lit.png`    | Attic with flashlight — trunk visible. Key is inside trunk. | `point and click adventure game, 2d painted background, dark horror night, attic lit by flashlight, old trunk visible, dusty beams, LucasArts style, detailed` |
| `bg_kitchen`      | `bg_kitchen.png`      | Contains kitchen drawer (examine to reveal flashlight); exit to living room and pantry. | `point and click adventure game, 2d painted background, dark horror night, kitchen, dirty dishes, drawer, moonlight, LucasArts style, detailed` |
| `bg_pantry`       | `bg_pantry.png`       | Small room off kitchen with shelving hide spot. Dead end. | `point and click adventure game, 2d painted background, dark horror night, cramped pantry, shelves canned goods, dark shadows, LucasArts style, detailed` |
| `bg_win_screen`   | `bg_win.png`          | Win screen background — exterior night scene. **960 × 540 px** (full canvas). | `point and click adventure game, 2d painted, exterior house at night, person escaping through front door, moonlight, freedom, LucasArts style` |

---

### Sprites

#### Player sprites

| Asset key        | Filename           | Dimensions    | In-game role | SD Prompt |
|------------------|--------------------|---------------|--------------|-----------|
| `sp_player_idle` | `player_idle.png`  | 64 × 96 px    | Player standing still (default state). Front-facing. | `point and click adventure game sprite, person standing idle, front view, dark clothes, scared expression, transparent background, pixel art style` |
| `sp_player_walk` | `player_walk.png`  | 256 × 96 px   | Walk animation — **4-frame horizontal strip**, each frame 64 × 96 px. Side-facing. Flipped horizontally when moving left. | `point and click adventure game sprite sheet, 4 frame walk cycle, side view, dark clothes, transparent background, pixel art` |
| `sp_player_hide` | `player_hide.png`  | 64 × 64 px    | Player crouching in hiding spot. Rendered semi-transparent. | `point and click adventure game sprite, person crouching hiding, transparent background, pixel art` |

#### Bilax sprites

| Asset key          | Filename            | Dimensions     | In-game role | SD Prompt |
|--------------------|---------------------|----------------|--------------|-----------|
| `sp_bilax_idle`    | `bilax_idle.png`    | 128 × 192 px   | Bilax present in the room. Rendered with pulsing red glow. | `point and click adventure game sprite, tall shadowy monster facing camera, glowing eyes, horror, transparent background, painted style` |
| `sp_bilax_doorway` | `bilax_doorway.png` | 128 × 192 px   | Bilax appearing at the doorway (grace-turn warning). Pulsing opacity. | `point and click adventure game sprite, shadowy monster silhouette in doorway, backlit, horror, transparent background, painted style` |

#### Item sprites — 32 × 32 px each

| Asset key            | Filename               | In-game role | SD Prompt |
|----------------------|------------------------|--------------|-----------|
| `sp_item_key`        | `item_key.png`         | Brass Key — found in attic trunk after examine. Required to unlock front door. | `point and click adventure game item sprite, old brass key, transparent background, painted` |
| `sp_item_flashlight` | `item_flashlight.png`  | Flashlight — found in kitchen drawer after examine. Required to see attic. | `point and click adventure game item sprite, compact flashlight, transparent background, painted` |
| `sp_item_note`       | `item_note.png`        | Note — visible from the start in the master bedroom. Hints at key location. | `point and click adventure game item sprite, crumpled note paper, transparent background, painted` |

---

### Spritesheet format

The walk animation (`player_walk.png`) is a **horizontal strip of 4 frames**:

```
|  frame 0  |  frame 1  |  frame 2  |  frame 3  |
|  64 × 96  |  64 × 96  |  64 × 96  |  64 × 96  |
```

Total image size: **256 × 96 px**. The game samples `frameIndex * 64` as the X offset.

---

## Audio requirements

All sounds are currently stub `console.log()` calls. Replace each with:
```js
const snd = new Audio("/sounds/<filename>.ogg");
snd.play();
```

| Sound name          | Description |
|---------------------|-------------|
| `ambient_house`     | Looping background ambience — wind, creaking floorboards. Low volume. |
| `bilax_footstep`    | Single heavy footstep. Plays whenever Bilax moves. |
| `bilax_enter`       | Bilax enters the player's room — low growl or door creak. |
| `bilax_leave`       | Bilax passes by while player hides — retreating footsteps. |
| `bilax_found_you`   | Jump-scare sting. Plays on game over. |
| `player_hide`       | Soft rustling sound when player hides. |
| `player_unhide`     | Soft movement when player stops hiding. |
| `item_pickup`       | Short pick-up chime. |
| `door_unlock`       | Key in lock + click. |
| `door_open`         | Heavy wooden door opening. |
| `room_transition`   | Brief whoosh during fade-out between rooms. |
| `game_over`         | Sting or silence. Plays with bilax_found_you. |
| `game_win`          | Relief sting — cool night air, distant chime. |

---

## Optional polish ideas

- **Parallax layers:** Split each background into foreground/mid/background layers and offset them slightly on mouse move.
- **Attic dust particles:** Particle emitter in `renderBackground()` when attic is lit — random small white dots drifting downward.
- **Footstep triggers:** Play `bilax_footstep` every N frames while `S.bilax !== S.room` as ambient dread.
- **Room-specific ambient sounds:** dripping in pantry, wind in attic, fridge hum in kitchen.
- **Difficulty modes:** Adjust `CONFIG.BILAX_MOVE_CHANCE` (default 0.55). Higher = harder, lower = easier.
- **Bilax pathfinding:** Replace random walk with BFS toward player's room for a harder mode.
- **Item inspection screen:** Click inventory item to show a large view with the note's text legible.
- **Footstep particles:** Small dust puffs under player sprite when walking.
- **Screen shake:** Brief camera shake on game over before fade.
