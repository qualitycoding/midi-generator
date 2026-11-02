# midi-generator

A generator script for a single-page web app that creates instrumental MIDI backing tracks in rock,
metal, punk and jazz styles, plays them in the browser and exports them as `.mid` files.

> **Status: the repository holds a setup script, not the app, and the app it generates has a bug.**
> There is no `src/` directory or `package.json` in the repository. Running `midi-generator.sh` writes
> them. I ran the script and its tests in a scratch directory: three of the four test files pass, and the
> end-to-end test fails with `ReferenceError: chordNotes is not defined`. That means generating a song
> throws, so no MIDI can be exported yet.

## What is here

| Path | Contents |
|---|---|
| `midi-generator.sh` | Bash script that writes the whole React app, its tests and a GitHub Pages workflow into the current directory |
| `plan/`, `premortem/`, `research/`, `reviews/` | Planning documents and review records. The reviews cover only the planning documents; the app code has not been reviewed |
| `HANDOFF.md`, `EXECUTION_LOG.md`, `DEVIATIONS.md` | Run records |

## What the generated app does

- 26 style profiles: 10 rock styles, 10 jazz styles and 6 artist-inspired profiles (The Exploited, Igorrr,
  Sex Pistols, Motörhead, Mötley Crüe, Judas Priest). Each sets a BPM range, scales, a chord progression, a
  rhythm pattern, a bass style and a drum groove.
- Generators for drums, chords, rhythm guitar and bass, a humaniser (timing jitter and velocity variance),
  a MIDI builder (`@tonejs/midi`) and an in-browser synth player (Tone.js).
- A React 18, Vite and Tailwind interface: style selector, parameter controls, audio player, a MIDI
  visualiser and an export panel.

## Known problems in the generated app

- `generateSongMidi` in `src/engine/index.js` uses `chordNotes` outside the loop that defines it, so it
  throws. This is what the failing integration test shows.
- The Lead track is created but never given any notes.
- Only the first scale and the first chord progression of a profile are used.
- The script does not generate a `package-lock.json`, but the generated deploy workflow runs `npm ci`, which
  needs one. Run `npm install` once and commit the lock file.

## Using the script

Use an empty directory and Node 20 or later:

```bash
mkdir app && cd app
bash ../midi-generator.sh
npm install
npm test        # Vitest
npm run dev     # Vite dev server
```

The generated Vite config sets the base path to `/midi-generator/` for GitHub Pages.

## Licence

Apache License 2.0. See [LICENSE](LICENSE).
