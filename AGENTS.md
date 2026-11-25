# AGENTS.md — Kids Learning Games Repository

This document describes the purpose, architecture, and strict requirements for building and maintaining educational mini-games in this repository.

## 1. Project Purpose & Context

- **Goal:** A collection of fun, educational mini-games designed to help two kids learn while playing together with their parents.
- **Language:** **100% Dutch (`lang="nl"`)**. Every title, button, instruction, encouragement, reward, and spoken phrase must be written in natural, child-friendly Dutch.
- **Primary Play Setup:** Played on a **smartphone touchscreen** while **Chromecasted / screen-mirrored to the living-room TV** so parents can watch along, cheer, and help from the couch.
- **Zero-Build Architecture:** Every page/game is a **single, self-contained HTML file** in the repository root. No bundler, no npm build step, no external local assets — everything must run directly via `file://` or static hosting (GitHub Pages).

## 2. Repository Structure

- `index.html` — The **begin-screen / game-picker** with two sides:
  - **Left side (`age7`):** Games for the **7-year-old**.
  - **Right side (`age3`):** Games for the **3-year-old (girl)**.
- `manifest.json` — Lightweight PWA manifest (`display: "fullscreen"` / `"standalone"`) so adding the site to an Android phone's Home Screen launches the picker and every game with **zero browser URL bar** when casting to a TV.
- `<game-slug>.html` — Individual standalone single-HTML games in lowercase Dutch kebab-case (e.g. `boerderij-reken-avontuur.html`).
- `AGENTS.md` — These requirements and conventions.
- `README.md` — Human-readable overview of the collection.

## 3. Mandatory Requirements for Every Game

1. **Single Self-Contained HTML File**
   - Put all HTML, CSS, JavaScript, Canvas/SVG graphics, and Web Audio sound synthesis inside one `.html` file.
   - Do **not** rely on external image, sprite, or audio files.
   - Google Fonts (`Fredoka One` + `Nunito`, matching existing pages) may be linked, but always include clean system fallback fonts so games work offline.
   - Include an inline SVG data-URI favicon (`<link rel="icon" href="data:image/svg+xml,...">`) to avoid `favicon.ico` 404 console errors.
2. **Dutch Language Everywhere**
   - Always set `<html lang="nl">`.
   - All visible UI text, feedback messages, menus, and buttons must be in Dutch.
   - Keep copy short, encouraging, and age-appropriate.
3. **Phone + Chromecast-to-TV Ergonomics**
   - **16:9 TV-Friendly Stage & True Fullscreen (No URL Bar):**
     - Keep the active game stage optimized for a 16:9 landscape aspect ratio (`1280×720` logical canvas or responsive stage using `100dvh` so it expands immediately when Chrome's URL bar hides), and let DOM-layout stages expand edge-to-edge on phone landscape/portrait so controls never clip.
     - Link `<link rel="manifest" href="manifest.json">` + `<meta name="mobile-web-app-capable" content="yes">` so launching from the Android Home Screen hides the URL bar permanently across all games.
     - Call `document.documentElement.requestFullscreen()` on the primary "START" click (when not already in standalone/fullscreen app mode) and provide a `⛶ Volledig scherm` toggle button on start menus so regular Android Chrome also hides the URL bar during Chromecast play.
   - **Couch-Distance Readability:** Parents watch on the TV from several meters away. Use large, bold typography, high-contrast colors, clear numbers/symbols/pictures, and expressive visual feedback (particles, animations, highlights) so every prompt and answer is readable from the couch.
   - **Smartphone Touch Controls First:**
     - Use generous, easy-to-tap touch targets (`>= 64px` for controls; even larger for 3-year-old games).
     - Set `user-select: none`, `-webkit-user-select: none`, and `-webkit-touch-callout: none` to prevent accidental selection/long-press popups.
     - Use `touch-action: none` on gameplay canvases and active touch buttons; use `overflow-y: auto` + `touch-action: pan-y` + `env(safe-area-inset-*)` on menu/quiz overlays so menus remain scrollable on small phones in both portrait and landscape.
     - Also support mouse clicks and keyboard controls so games are easy to play/test on desktop as well.
4. **Built-In Sound Effects (Web Audio API)**
   - Synthesize cheerful, kid-friendly sound effects with the Web Audio API (resume `AudioContext` on user interaction).
   - Keep sounds gentle and pleasant — never harsh or loud.
   - For 3-year-old games, optional Dutch voice prompts via `window.speechSynthesis` (`lang = 'nl-NL'`) are encouraged, always paired with clear visual cues in case speech synthesis is unavailable or muted.
5. **Navigation Back to the Game Picker**
   - Every game **must** include a clear link back to `index.html` (e.g., `← 🏠 Alle spellen`) on its start menu, game-over/pause overlay, and victory screen.
   - Place the back link where a child won't accidentally tap it during active gameplay.

## 4. Target Audience & Age Guidelines

### Left Side: 7-Year-Old (`age7`, Dutch Groep 3 / Groep 4)
- **Learning topics:** Multiplication tables (`tafels` 1–10), addition & subtraction up to 100 (`plus- en minsommen`), clock reading (`klokkijken`), Dutch reading & spelling (`lezen & spellen`), money/counting (`geld tellen`), logic & patterns.
- **Gameplay style:** Platformers, adventures, quests, puzzle rounds, multiple-choice challenges, levels, scores/stars, diplomas.
- **Parent controls:** Start menu may include quick parent-friendly settings (e.g., selecting specific tables, difficulty, or starting level).
- **Failure handling:** Never overly punishing — use encouraging retries, hints, or rescue challenges (like the *reddings-som* in *Boerderij Reken-Avontuur*) instead of hard resets.

### Right Side: 3-Year-Old Girl (`age3`, Peuter)
- **Learning topics:** Colors (`kleuren`), shapes (`vormen`), animals & animal sounds (`dieren & geluiden`), counting 1–5 / 1–10 (`tellen`), matching/sorting (`bij elkaar zoeken`), letters/first sounds, size comparisons (`groot & klein`), music & rhythm.
- **Zero reading required:** She cannot read yet! Communicate through huge, unmistakable visuals/emojis/illustrations, animations, and optional spoken Dutch (`nl-NL`).
- **Toddler-proof controls:** Extra-large touch targets (big cards, bubbles, animals, buttons) with generous spacing so small fingers always succeed.
- **No fail states & no stress:** Never show "Game Over", countdown pressure, or harsh error buzzers. On a wrong tap, give gentle, warm guidance (e.g., soft wiggle + "Probeer nog eens!" or subtle visual hint toward the right answer).
- **Immediate joy & celebration:** Every correct tap should reward her right away with sparkles, confetti, happy sounds, or animated reactions so parents watching on TV can cheer along.
- **Instant / frictionless start:** Launch directly into play or behind one giant visual "SPEEL ▶" button. Keep any parent settings discreet (e.g., small corner gear icon).
- **Themes:** Warm, joyful, varied themes a 3-year-old girl loves (animals, unicorns, butterflies, flowers, rainbows, princesses, nature, painting/colors, music).

## 5. How to Add a New Game

1. **Create the game file** in the repo root using lowercase Dutch kebab-case, e.g. `dieren-en-geluiden.html`.
2. **Follow all requirements** in sections 3 and 4 for the intended child (`age7` or `age3`).
3. **Add a back link** to `index.html` (`← 🏠 Alle spellen`) on menu and end screens.
4. **Register the game in `index.html`** inside the `GAMES` object under `GAMES.age7` (left list) or `GAMES.age3` (right list):
   ```js
   {
       title: 'Naam van het Spel',
       file: 'naam-van-het-spel.html',
       icon: '🎨',
       description: 'Korte, duidelijke Nederlandse beschrijving.',
       tags: ['Onderwerp', 'Leeftijd/Kenmerk'],
       accent: '#ff79b0',
       shadow: '#d63384',
       soft: '#fff0f6'
   }
   ```
5. **Test live in Chrome via Chrome DevTools MCP** before finishing:
   - Serve the repo locally (e.g., `python -m http.server 8765 --bind 127.0.0.1`) and verify the game live using **Chrome DevTools MCP** (`browser` subagent — requires Google Chrome running with remote debugging enabled at `chrome://inspect/#remote-debugging`). Stop the local server when done.
   - Verify across all three target viewports:
     - Phone landscape (Chromecast-to-TV mode, e.g. `844×390`)
     - Phone portrait (e.g. `390×844`)
     - TV / desktop 16:9 (`1280×720`)
   - Verify interactive gameplay, responsive scrolling/backgrounds, back-navigation to `index.html`, and **zero** console/network errors.
6. **Update `README.md`** to list the new game if helpful.

## 6. Current Games Inventory

| Side / Age | Title | File | Learning Focus |
| :--- | :--- | :--- | :--- |
| Left — 7 jaar | Boerderij Reken-Avontuur | `boerderij-reken-avontuur.html` | Tafels van 1 t/m 10 (8-level platformspel) |
| Left — 7 jaar | De Grote Klus- & Bak-Bouwer | `klus-en-bak-bouwer.html` | 36 themawoorden & betekenissen (interactieve bouw-, afval- en bakwereld) |
| Left — 7 jaar | Woorden-Kanon | `woorden-kanon.html` | 36 themawoorden & betekenissen (arcade kanon-schietspel met combo's & upgrades) |
| Left — 7 jaar | Woord-Detective: Kraak de Kluis! | `woord-detective.html` | 36 themawoorden & betekenissen (mysterie met spoor-scanner & laser-koppelkluis) |
| Right — 3 jaar | Kleuren Mandjes | `kleuren-mandjes.html` | Kleuren herkennen & sorteren (slepen of tikken, Nederlandse stem, ballonnenfeest) |
