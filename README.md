# Atlantic International Challenge — Live Scoreboard

Custom chroma-key scoreboard for the AIC 2026 live stream (Belmont Bowling Club, Belfast).
Two static pages, no build step, no server code.

## Files
- **display.html** — runs fullscreen on the streamed device (added to Switcher as a camera source, chroma-keyed on pure green `#00FF00`). Listener only; no controls appear on stream. Tiny status dot top-right: green (invisible when keyed) = connected, red = link down.
- **controller.html** — runs on any other device with internet (the scorer's/commentator's phone). Big-button controls for scores, sets, end, set number, match setup, and the match timer.

## Sets play
Tick **Sets play** in Match Setup to add the per-side set counters and the SET stepper. During a deciding tiebreak, hit the **TIEBREAK** toggle: the display shows `TB` instead of `SET n`, and the SET stepper hides until you toggle it back off.

Take **END** down to `0` (the controller shows `OFF`) to hide the end readout entirely — for a one-end tiebreak, where counting ends says nothing. It works outside sets play too; there the chip collapses to a plain divider between the scores.
- **index.html** — landing page linking both.

## Hosting (GitHub Pages)
1. Push these files to the repo root.
2. Settings → Pages → Deploy from branch → `main` / root.
3. Pages URL serves `index.html`; open `display.html` on the stream device and `controller.html` on the scorer's device.

On the stream device: open display.html in Safari → Share → **Add to Home Screen** → launch from the icon (chromeless). Enable **Guided Access**, disable auto-lock, max brightness.

## How they sync
MQTT over secure WebSocket via public brokers (broker.emqx.io, fallback test.mosquitto.org), retained state, unique channel ID baked into both files. Both devices just need internet — not the same network. Timer is timestamp-based and re-syncs every 20s; it survives sleep/refresh on either device.

## Display gestures (fallback only)
The display keeps local tap gestures (tap score +1, double-tap −1, tap END, tap clock start/pause) as an emergency fallback — but controller state overwrites local changes on the next update, so score from the controller.

## Security note
This is a public repo, so the channel ID is visible in source. Anyone who finds it could alter the scoreboard. For a bowls score the risk is modest; if it bothers you, keep the repo name obscure, or regenerate with a fresh channel ID before each event.

## Switcher setup
Source → Edit Properties → Overlay → Chroma Key Enable → eyedropper the green → tune Range/Softness/Edge Desaturation. Note: chroma-keyed angles don't composite inside Multiviews.
