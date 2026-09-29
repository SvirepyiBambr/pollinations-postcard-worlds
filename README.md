# 🗺️ Postcard Worlds

**Try it:** https://svirepyibambr.github.io/pollinations-postcard-worlds/

Type a scene, step inside, then click the glowing spots (doors, paths, windows, objects) to walk deeper into the world — one generated postcard at a time. Submitted for **quest #15728**.

## How it works
1. **First view** — `POST /v1/images/generations` (model `black-forest-labs/flux.1-kontext-pro`) paints the world you typed.
2. **Clickable spots** — the same image goes to a vision model (`openai`, image input) via `POST /v1/chat/completions`, which returns exactly 3 spots as JSON with `x/y` coordinates, a label and where each leads. The spots are drawn on the picture by *our code*.
3. **Step through** — clicking a spot calls `POST /v1/images/edits` with the **current view as the reference image**, so the next postcard keeps the same world and art style.
4. **Breadcrumb strip** — every visited scene is a thumbnail; click to jump back.
5. **Replay link** — your path (seed + step descriptions) is packed into a URL; anyone can replay your exact journey.
6. **Bring your own Pollen** — the key stays in localStorage; no secrets embedded.

## Live check (real API runs, 2026-09-29)
- `POST /v1/images/generations`, model `flux.1-kontext-pro` ("cozy painter's cottage interior") → HTTP 200, JPEG returned.
- Vision call (`openai` + image input) → HTTP 200, valid JSON with 3 spots, e.g. `{"x":760,"y":240,"label":"Window","lead":"Looks out to greenery beyond the room…"}`.
- `POST /v1/images/edits` with the first view as reference ("walk through the open door into the garden") → HTTP 200, 286 KB JPEG — continuity confirmed.

All calls are paid with the player's own Pollen key.
