# Happy Birthday Mummy!! 🎂

A small interactive 3D scene built as a birthday gift — a table set with photo frames, a candle, flowers, a Snoopy figurine, and a handwritten note, rendered in the browser and walkable in first person.

![Scene preview](preview.png)

## What it is

This is a personal, walk-around 3D birthday card. Instead of a flat image or video, the birthday message lives inside a small 3D room you can explore:

- A wooden table holding the "gift" scene
- Two picture frames — one a static photo, one a **looping video** of a memory together
- A hand-written note (rendered onto a canvas texture) with a birthday message
- A flickering candle with a real point light
- A flower model and a Snoopy figurine sitting on the table
- Background music that plays once you click into the scene
- First-person movement (WASD + mouse look) so you can walk up and look around

## Tech Stack

- **[Three.js](https://threejs.org/)** — 3D rendering
- **[Vite](https://vitejs.dev/)** — dev server / bundler
- **GLTFLoader** — loading `.glb` 3D models (Snoopy, frames, flowers, candle, arc/room)
- **PointerLockControls** — first-person mouse-look navigation
- **HTML5 `<video>` + `THREE.VideoTexture`** — video played back onto a picture frame mesh
- **`THREE.Audio` + `AudioLoader`** — background music with autoplay-safe handling
- **Canvas 2D API** — the note's text is drawn onto a `<canvas>` and used as a texture

## Project Structure

```
├── main.js          # Scene setup, lighting, models, controls, animation loop
├── style.css        # Page/start-screen styling
├── index.html        # Entry point with #start-screen overlay
├── models/           # .glb 3D models (snoopy, frames, arc, candle, flowers)
├── videos/           # Video clips played inside picture frames
└── audio/            # Background music (about_you.m4a)
```

## How It Works

### Scene Setup
The scene uses a dark blue-grey background with matching fog (`createScene`), a perspective camera positioned at the table (`createCamera`), and a `WebGLRenderer` with shadows enabled.

### Lighting
A layered lighting rig gives the scene mood and depth:
- Ambient light for base fill
- A directional light + a dimmer rim light for contrast
- An overhead point light acting as the main "room light"
- A separate warm ceiling light for a cozy glow

### Models
Models are loaded via `GLTFLoader` and placed using per-object configs (position, scale, rotation) defined at the top of `main.js` in the `CONFIG` object — this makes it easy to nudge any object without touching the loading logic.

### Video Frames
Two custom loaders (`loadVideoFrame`, `loadVideoFramez`) each:
1. Create an HTML `<video>` element and play it muted (to satisfy browser autoplay policies)
2. Wrap it in a `THREE.VideoTexture`
3. Find the frame's inner mesh by material name and swap in the video as its texture, with manual zoom/offset tuning so the video fills the frame correctly regardless of aspect ratio

### The Note
`createNote` draws the birthday message onto an off-screen `<canvas>` (custom font, line-wrapping by `\n`), turns that canvas into a texture, and maps it onto a `PlaneGeometry` positioned flat on the table.

### Candle
`loadCandleWithLight` loads the candle model, marks the flame mesh as emissive, and attaches a real `PointLight` above it. In the animation loop, the light's intensity is modulated with a `sin()` wave for a flickering effect.

### Controls
- **Click** the screen to lock the pointer and start looking around
- **WASD** — move
- **Mouse** — look
- **Space / Shift** — move up / down
- **Esc** — release pointer lock

### Background Music
Music loads via `AudioLoader` and is played once the audio context resumes (required by browsers), triggered from the start-screen click handler.

## Running Locally

```bash
npm install
npm run dev
```

Then open the local dev URL Vite prints in your terminal.

## Notes

- Model/video/audio paths are relative to the `public/` folder (`/models`, `/videos`, `/audio`) as expected by Vite.
- Some config entries (`framez`, `flowers`) are placeholders/leftovers from earlier iterations — only the objects loaded in `init()` actually appear in the final scene.

---

*A little 3D room, built to say happy birthday.* 🎈
