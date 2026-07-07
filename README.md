# 📚 LitLoom — Premium Audiobook, Ebook & Manga Android App

[![Release](https://img.shields.io/badge/release-v1.0.0-gold?style=for-the-badge)](https://github.com/cybruGhost/LITLOOM/releases/tag/V1.0.0)
[![Downloads](https://img.shields.io/github/downloads/cybruGhost/LITLOOM/V1.0.0/total?style=for-the-badge&color=amber&label=downloads)](https://github.com/cybruGhost/LITLOOM/releases/tag/V1.0.0)

<p align="center">
  <img width="280" alt="LitLoom preview 1" src="https://github.com/user-attachments/assets/78d30ea5-72be-41eb-9319-c1eb045abb05" />
  <img width="280" alt="LitLoom preview 2" src="https://github.com/user-attachments/assets/b541301e-27bf-4a6d-9e4c-1701a66ddc62" />
</p>

A cinematic, dark, Spotify-level audiobook, ebook, and manga app built with **Jetpack Compose + Material 3 + Media3 ExoPlayer**.

**Weaving stories together — Audiobooks, Books & Manga in one app.**

---

## ✨ Version

**[v1.0.0](https://github.com/cybruGhost/LITLOOM/releases/tag/V1.0.0)** — Initial Release

---

## 🎨 Design
Matches the provided UI mockup exactly:
- **Deep black / charcoal** backgrounds
- **Gold / amber** accent system
- **Glassmorphism** cards with blurred cover backgrounds
- **Cinematic gradients** throughout
- Premium typography, smooth animations

---

## 🏗 Architecture
```
com.litloom/
├── data/
│   ├── api/          — Retrofit interfaces + Hilt NetworkModule
│   ├── model/        — Kotlin serializable data classes
│   └── repository/   — AudiobookRepository, BookRepository, MangaRepository, LocalPreferencesRepository
├── player/           — Media3 / ExoPlayer PlaybackService (foreground)
├── navigation/       — NavHost + Screen sealed class
├── theme/            — Dark Material3 theme, gold palette, typography
└── ui/
    ├── components/   — MiniPlayer, BottomNavBar, BookCoverCard, GlassCard…
    └── screens/
        ├── home/     — HomeScreen + HomeViewModel
        ├── discover/ — DiscoverScreen + DiscoverViewModel
        ├── library/  — LibraryScreen (grid/list toggle)
        ├── search/   — SearchScreen + SearchViewModel (debounced)
        ├── profile/  — ProfileScreen
        ├── details/  — AudiobookDetailScreen, BookDetailScreen, MangaDetailScreen + ViewModels
        ├── player/   — Full-screen luxury audio player
        ├── reader/   — Ebook reader (dark/sepia/light themes, font control)
        └── mangareader/ — Manga reader (LTR/RTL paging, zoom, webtoon scroll mode)
```

---

## ⚙️ Build Requirements
- **Android Studio Hedgehog** (2023.1.1) or newer
- **JDK 17**
- **compileSdk 35**, **minSdk 26**

---

## 🔌 API Configuration

The backend endpoint is configured in `app/src/main/java/com/litloom/data/api/ApiService.kt`. Endpoint URLs, routes, and request/response schemas are kept private and are intentionally not published in this repo.

---

## 🔊 Audio Playback (ExoPlayer)

`PlaybackService.kt` is a fully wired **Media3 MediaLibraryService** with:
- Background playback
- Notification media controls
- Audio focus handling
- "Noisy" (headphone unplug) handling

To start playback from the player screen, wire `PlayerViewModel` → `PlaybackService` via `MediaController`:

```kotlin
val controllerFuture = MediaController.Builder(context,
    SessionToken(context, ComponentName(context, PlaybackService::class.java))
).buildAsync()

controllerFuture.addListener({
    val controller = controllerFuture.get()
    val item = MediaItem.fromUri(chapter.url)
    controller.setMediaItem(item)
    controller.prepare()
    controller.play()
}, MoreExecutors.directExecutor())
```

---

## 🖼️ Manga Reader

The manga reader (`ui/screens/mangareader/`) supports:
- **Paged mode** — left-to-right or right-to-left (true manga-style) page turns
- **Webtoon / scroll mode** — continuous vertical scroll for long-strip manga
- Pinch-to-zoom and double-tap zoom
- Preloading of next chapter pages
- Reading progress synced per chapter/page

---

## 🚀 Build Steps

```bash
# 1. Open in Android Studio
File → Open → select the LitLoom folder

# 2. Let Gradle sync (first time downloads ~500MB of deps)

# 3. Set your BASE_URL in ApiService.kt

# 4. Run on device / emulator
Run → Run 'app'

# 5. Build signed release APK
Build → Generate Signed Bundle/APK → APK
```

---

## 📱 Screens

| Screen | Description |
|---|---|
| **Home** | Greeting, Continue Listening/Reading hero card, Recommended, Trending, New Releases, Narrators, Short Listens, Editor's Picks |
| **Discover** | Search bar, genre chips, featured banner, New Releases, Popular Narrators, Top Reads/Listens This Week, Browse tiles |
| **Audiobook Detail** | Blurred hero, cover art, rating, stats, genres, expandable description, Play/Download/Like, Chapters tab, Similar tab |
| **Player** | Full-screen blurred bg, progress slider, 15s rewind, play/pause, 30s skip, speed (0.75x–2x), sleep timer, queue |
| **Book Detail** | Cover hero, metadata, table of contents, Read + Download buttons |
| **Reader** | Serifed text, Dark/Sepia/Light themes, font size control, progress, chapter navigation, bookmarks |
| **Manga Detail** | Cover hero, metadata, chapter list, Read + Download buttons, Similar tab |
| **Manga Reader** | Paged (LTR/RTL) or webtoon scroll mode, zoom, chapter navigation, progress tracking |
| **Library** | Books/Audiobooks/Manga/Collections tabs, Continue Listening, Downloaded, Finished, Favorites, grid/list toggle |
| **Search** | Live debounced search, recent searches, popular searches, browse tiles, results grouped by type |
| **Profile** | Avatar, Premium badge, stats (Books Read, Hours Listened, Chapters Read, Streak), settings sections, sign out |

---

## 🎯 Mini Player
Appears globally above the bottom nav when audio is playing.
Shows cover, title, play/pause, next, progress bar.
Tapping opens the full Player screen.

---

## 📦 Key Dependencies
- Jetpack Compose BOM 2024.09
- Material 3
- Navigation Compose 2.8
- Hilt 2.52
- Retrofit 2.11 + KotlinX Serialization
- Coil 2.7
- Media3 / ExoPlayer 1.4
- Accompanist SystemUI Controller

---

*LitLoom — Built for LitLoom. Powered by your APIs.*
