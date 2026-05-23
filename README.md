# La.Movie – CloudStream Extension

CloudStream plugin for **la.movie** — pelíuclas, series y anime en español latino.

---

## ⚠️ Before This Works: Fix the CSS Selectors

This extension was generated with the correct **structure and logic** for a CloudStream provider, but the CSS selectors that scrape la.movie's HTML **must be verified** by inspecting the real site.

Open `LaMovieProvider/src/main/kotlin/com/lagradost/LaMovieProvider.kt` and search for every comment marked:

```
// TODO: VERIFY
```

For each one, open `la.movie` in Chrome/Firefox, press **F12** to open DevTools, and find the matching HTML element to copy the correct CSS selector.

### What you'll need to verify

| Part | What to check |
|---|---|
| `getMainPage` | Selector for movie/series cards on listing pages |
| `toSearchResult` | Selectors for title, poster image, and link inside a card |
| `search()` | The correct search URL (e.g. `/?s=query` or `/buscar?q=query`) |
| `load()` | Selectors for title, poster, plot, year, genres on a detail page |
| `load()` (series) | Selectors for episode list and season groupings |
| `loadLinks()` | How the video embed/iframe URL is stored in the page |

---

## Project Setup

### Requirements
- Android Studio (Hedgehog or newer)
- JDK 17
- A GitHub account

### Steps

1. **Fork the [CloudStream plugin template](https://github.com/recloudstream/TestPlugins)**  
   This gives you the Gradle setup and build tooling.

2. **Copy these files into your fork:**
   - `LaMovieProvider/` folder (the whole thing)
   - Update `settings.gradle.kts` to include `:LaMovieProvider`

3. **Fix the CSS selectors** (see above)

4. **Build locally** to verify it compiles:
   ```bash
   ./gradlew :LaMovieProvider:make
   ```

5. **Push to GitHub** — the included GitHub Actions workflow auto-builds and publishes to a `builds` branch.

6. **Add to CloudStream:**  
   Settings → Extensions → Add Repository → paste your `repo.json` URL:
   ```
   https://raw.githubusercontent.com/TU_USUARIO/LaMovieProvider/builds/repo.json
   ```
   Replace `TU_USUARIO` with your GitHub username.

---

## How the Extension Works

```
User opens CloudStream
    └── getMainPage()   ← shows rows of movies/series
    └── search()        ← handles search queries

User taps a title
    └── load()          ← fetches metadata + episode list

User taps Play
    └── loadLinks()     ← finds the video embed URL
        └── loadExtractor() ← CloudStream's built-in extractor
                             handles ~50 known video hosts
                             (Streamtape, Doodstream, Filemoon, etc.)
```

---

## Troubleshooting

**"No links found"** — `loadLinks()` selector is wrong. Inspect the player page.

**Empty home screen** — `getMainPage()` selector is wrong. Check the card container HTML.

**Wrong titles/posters** — `toSearchResult()` selectors need updating.

**Search returns nothing** — The search URL format is wrong. Check the network tab in DevTools when searching on la.movie.
