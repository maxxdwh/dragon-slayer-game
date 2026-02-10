# DragonMap — Corti API Roadmap Quest

A 2D pixel art adventure map showcasing the Corti clinical dictation API roadmap. Players guide **Bard the Bowman** through an epic quest from the already-conquered foundation to the dragon's lair.

## Quick Start

```bash
# Serve locally
python3 -m http.server 8765
# Open http://localhost:8765
```

Or simply open `index.html` in a browser (works as a standalone file).

## How It Works

### Game Flow
1. **Title Screen** — Shows core positioning: "The best foundation for building clinical dictation products, delivered as an API."
2. **Foundation Zone** (bottom-left, forest) — 3 shipped features represented as conquered castle towers with green glow and checkmarks
3. **Milestone: "Empowered to Build a Dragon Replacement"** — Triggered when the player walks near the flag
4. **Power-Ups Zone** (mid-map) — 4 upcoming features represented as treasure chests with golden glow
5. **Dragon Lair** (top-right, volcanic) — The dragon encounter and slaying animation

### Controls
- **WASD / Arrow Keys** — Move character
- **Space / Click** — Interact with roadmap items
- **Click on map** — Move character to that location
- **ESC** — Close info panel

### Roadmap Items
| # | Item | Status | Icon |
|---|------|--------|------|
| 1 | Clinical-grade STT Models | Shipped | Castle Tower |
| 2 | Command Framework | Shipped | Castle Tower |
| 3 | Output Formatting | Shipped | Castle Tower |
| — | EMPOWERED TO BUILD A DRAGON REPLACEMENT | Milestone | Flag |
| 4 | Advanced Formatting | Upcoming | Treasure Chest |
| 5 | Advanced Speed | Upcoming | Treasure Chest |
| 6 | Advanced Commands | Upcoming | Treasure Chest |
| 7 | Advanced Customization | Upcoming | Treasure Chest |
| — | SLAY THE DRAGON ONCE AND FOR ALL | Final | Dragon |

## Customizing Sprites

All sprites are currently generated procedurally via the `PixelArt` class. To swap with custom image assets:

### Character Sprites (8 directions, 4 frames each)
The character sprite system expects 8 directions × 4 animation frames = 32 sprites.

```javascript
// In the Character class, replace the loadSprites method:
async loadCustomSprites(basePath) {
  const directions = ['n','ne','e','se','s','sw','w','nw'];
  this.sprites = {};
  for (const dir of directions) {
    this.sprites[dir] = [];
    for (let frame = 0; frame < 4; frame++) {
      const img = new Image();
      img.src = `${basePath}/char_${dir}_${frame}.png`;
      await new Promise(resolve => img.onload = resolve);
      this.sprites[dir].push(img);
    }
  }
}
```

**Sprite specs:**
- Size: 32×32 pixels (rendered at 1:1 with TILE_SIZE)
- 8 directions: `n`, `ne`, `e`, `se`, `s`, `sw`, `w`, `nw`
- 4 frames per direction: idle (0), walk1 (1), walk2 (2), walk3 (3)

### Decoration Sprites
Replace entries in the `decos` object returned by `PixelArt.generateDecorations()`:

```javascript
// Example: Replace tower sprite with custom image
const towerImg = new Image();
towerImg.src = 'assets/tower.png';  // 64×64 px
decos.tower = towerImg;
```

**Sprite sizes:**
- Trees: 32×64 px (1 tile wide, 2 tiles tall)
- Tower: 64×64 px (2×2 tiles)
- Chest: 32×32 px (1 tile)
- Flag: 32×64 px
- Dragon: 96×96 px (3×3 tiles)
- Rock, flower, skull, campfire, lava: 32×32 px

### Tile Sprites
Replace entries in the `tiles` object from `PixelArt.generateTiles()`:

```javascript
// Example: Replace grass tiles
for (let v = 0; v < 4; v++) {
  const img = new Image();
  img.src = `assets/grass_${v}.png`;  // 32×32 px
  tiles[`grass${v}`] = img;
}
```

### Map Layout
Edit `ROADMAP_ITEMS` array to change item positions (`mapX`, `mapY` in tile coordinates). Edit `WorldMap.generate()` to change terrain layout.

## Architecture

Single HTML file with embedded CSS and JavaScript:
- **PixelArt** — Procedural sprite generation
- **WorldMap** — Tile map with terrain zones and decorations
- **Character** — Player character with 8-directional movement
- **ParticleSystem** — Visual effects (glows, sparkles, explosions)
- **Game** — Main game loop, camera, input handling, UI

## Browser Support

Works in all modern browsers. Uses HTML5 Canvas, ES6+, and the "Press Start 2P" Google Font.
# dragon-slayer-game
