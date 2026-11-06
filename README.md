# Stacks

A chord-function build game. A function is shown — hit the chord that is that function in the shown key. Rounds alternate direction, pads shuffle every round, the key changes every 5 correct.

Single-file web game: WebGL effects layer + WebAudio beat-clock engine (music follows the game key). No dependencies, no build step — open `index.html` or serve it.

Controls: D F G H J K L = pads, P = pause. Touch works on mobile.

## Melody mode

The prompt notes trace a real tune instead of a random walk: `tunes.json` ships 55 classic piano melodies as scale-degree heads (1-7 against the game scale, `SCL` mapping, looping cursor). Per prompt the game plays a short piano tone on the tune note; inverted rounds are suppressed in melody mode. Pads show the interval number and stay in a fixed 1-7 order (no shuffle) so the layout is learnable; the prompt shows the note name in the current key. If `tunes.json` fails to load the game falls back to classic mode (logged once); if it arrives after the player already started, melody mode activates from the next prompt. 7 heads (Clair de Lune, Arabesque No. 1, Gymnopédie No. 1, Gnossienne No. 1, Consolation No. 3, Widmung, Träumerei) are verified against Mutopia MIDI top-voice (`source_ref` + `confidence: high` per tune); the rest are knowledge-encoded (`confidence: medium`) and are the first candidates for fixes if one sounds wrong.

Deploy note: `tunes.json` must ship next to `index.html` (static, fetched at load).

## Score server

Tiny arcade high-score server (plain bun stdlib, no deps, no build step): `PORT=18010 bun run scores` (or `bun run scores`; env: `PORT`, `DATA_DIR`, data persisted as JSONL at `$DATA_DIR/scores.jsonl`). Endpoints: `GET /scores` (top 20, `count`) and `POST /scores` with `{name, score}` (name exactly 3-4 chars A-Z0-9, score integer 0..99999999); plus `GET /healthz`. CORS origin: `https://stacks.mrzk.io`.
