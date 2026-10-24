# notKirby

A small Kirby-style platformer that runs in the browser, built with [Kaboom.js](https://kaboomjs.com), TypeScript and Vite. Float around, inhale enemies and spit them back out as stars.

**[Play it](https://not-kirby.vercel.app)**

## Controls

| Key | Action |
| --- | --- |
| <kbd>←</kbd> <kbd>→</kbd> | Move |
| <kbd>X</kbd> | Jump. Press again in mid-air to keep floating upward (up to 10 jumps before landing). |
| <kbd>Z</kbd> (hold) | Inhale. Enemies in front of you get pulled in and swallowed. |
| <kbd>Z</kbd> (with a mouthful) | Spit the enemy out as a star that destroys any enemy it hits. |

## Gameplay

- **Flames** hop in place about once a second.
- **Walkers** pace back and forth.
- **Birds** fly right to left, a new one every 10 seconds, at a random speed.

Touching an enemy without swallowing it costs health, and Kirby flickers. Get hit with no health left, or fall off the map, and the level restarts.

## Running locally

You need Node.js 20.19+ or 22.12+ (Vite 8's minimum).

```bash
git clone https://github.com/ymahrous/notKirby.git
cd notKirby
npm install
npm run dev
```

Then open the URL Vite prints, usually http://localhost:5173.

| Script | What it does |
| --- | --- |
| `npm run dev` | Starts the dev server with hot reload |
| `npm run build` | Type-checks with `tsc`, then builds a static site into `dist/` |
| `npm run preview` | Serves the production build locally |

`vite.config.ts` sets `base: "./"`, so the build works from any path. Upload `dist/` to Vercel, Netlify, GitHub Pages or any other static host.

## Project structure

```
src/
  main.ts        Loads the sprite sheet, sets up the level scene, camera and enemy spawns
  entities.ts    The player, keyboard controls, inhaling, and the three enemy types
  utils.ts       makeMap(): turns a Tiled map into colliders and spawn points
  kaboomCtx.ts   The Kaboom instance (256×144 pixels, scaled ×4, letterboxed)
  state.ts       Which scene is current and which comes next
  constants.ts   The global pixel scale
public/
  kirby-like.png Sprite sheet (9×10 grid of 16px frames)
  level-N.png    Level art
  level-N.json   Level map data from Tiled
docs/            README images
```

## Making a level

Levels are drawn in [Tiled](https://www.mapeditor.org) (16px tiles) and shipped as two files in `public/`:

- `level-N.png`: the level art, exported as an image
- `level-N.json`: the map, exported as JSON

`makeMap()` only reads two object layers from the JSON. The tile layers are just for drawing the PNG.

| Layer | Objects | Becomes |
| --- | --- | --- |
| `colliders` | Rectangles. Name one `exit`. | Solid platforms, plus the level exit |
| `spawnpoints` | Points named `player`, `flame`, `guy` or `bird` | Where Kirby and each enemy start |

To add the level to the game, load its sprite and map in `main.ts`, register a scene for it, and add the scene name to `scenes` in `state.ts`.

## Status

Level 1 is playable, but it can't be finished yet:

- The exit door doesn't lead anywhere. `makeMap()` tags every collider as a platform, so the player's `exit` collision never fires, and `nextScene` is never set.
- The art and map for level 2 are already in `public/`, but no scene loads them.
- The `end` scene is empty.

## Credits

A non-commercial fan project. Kirby is a trademark of Nintendo / HAL Laboratory, and this project isn't affiliated with or endorsed by them.
