# Souboj vlajek (Flag Battle)

A Czech-language multiplayer flag guessing quiz game built as a single-file PWA.

## Architecture

- **Single-file app**: Everything lives in `index.html` — HTML, CSS, JavaScript, PWA manifest, and service worker
- **No build tools**: Vanilla JS, no frameworks, no bundlers, no npm dependencies
- **PWA**: Offline-capable with inline service worker and manifest (data URI)

## Tech Stack

- Vanilla JavaScript (ES6+)
- CSS3 with custom properties (CSS variables)
- REST API: `https://restcountries.com/v3.1/all` for country/flag data
- Flag images: `https://flagcdn.com` (cached by service worker)

## Game Rules

- 2–6 players, turn-based rotation
- 15 questions per player, 30 seconds per question
- 4 multiple-choice options per question
- +1 point for correct answer, 0 for wrong/timeout
- All text is in Czech (including Czech country name overrides via `CZ_OVERRIDES`)

## Code Structure (index.html)

| Section | Description |
|---------|-------------|
| `<head>` | Meta tags, PWA manifest, iOS PWA meta, inline CSS |
| CSS variables (`:root`) | Theme colors, backgrounds, borders, player colors |
| `.setup-screen` | Player setup UI — name inputs, rules info box |
| `.game-screen` | Scoreboard, flag display, answer choices, timer |
| `.results-screen` | Results table with medals, replay buttons |
| `app` object | All game state and logic (IIFE, `window.app`) |
| Service Worker | Inline blob-based SW for flag image caching |

## Key Patterns

- **State machine**: `app.gameState` = `'setup'` | `'playing'` | `'results'`
- **Screen switching**: `.active` class toggled on screen containers
- **Player rotation**: `currentPlayerIndex` cycles through players, skipping finished ones
- **Timer**: `setInterval` with visual warning at ≤10 seconds
- **Responsive**: Mobile-first CSS with breakpoints at 360px, 480px, 700px

## Development

No build step required. Open `index.html` in a browser or serve with any static server:

```bash
# Python
python3 -m http.server 8000

# Node.js
npx serve .
```

## Testing

Manual testing only. Key flows to verify:
1. Country data loads from API on startup
2. Player add/remove works (min 2, max 6)
3. Game starts, players rotate correctly
4. Timer counts down and expires properly
5. Correct/incorrect answer feedback displays
6. Results screen shows correct rankings
7. PWA installs and works offline (flag images cached)
8. Mobile layout renders correctly on small screens (320px+)

## External APIs

- **restcountries.com** — Country names, codes, flag URLs. Rate-limited; no auth required.
- **flagcdn.com** — Flag PNG images. Cached by service worker after first load.
