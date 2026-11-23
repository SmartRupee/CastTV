# Nilesh Cast UI

A free, dark-themed Android web-casting app UI built with **Jetpack Compose** and **Material 3**.
Browse the web, detect videos on a page, pick a TV, and control playback from a full remote screen.

> **Status: UI prototype.** All TVs, detected streams, the ad-block counter and the web page are
> mock data. The app does not yet scan for real devices or send video to a TV.

## Features

**Main browser screen**
- Top bar with a connection pill ("Living Room TV • CAST" or "Tap to Cast"), an ad-block counter and an overflow menu
- Home hub with Quick Access cards (Local Media, IPTV M3U, Bookmarks), a Recently Visited list and one labelled native-ad card
- Docked mini-controller (thumbnail, title, 15s rewind, play/pause) that appears only while casting
- Bottom omnibox with back/forward, URL or search field, a pulsing "Detected Media" badge showing the stream count, and a cast button. Controls sit at the bottom for one-handed use

**Device picker (bottom sheet)**
- Devices grouped into *Streaming Devices* (Chromecast, Roku) and *Smart TVs & DLNA* (LG WebOS, Samsung Tizen, DLNA)
- Protocol tags and hints, for example "Requires TV PIN approval" (WebOS) and "Transcoding on phone" (Roku)
- TV volume slider, "Route Audio to Phone" switch and a Disconnect button

**Detected media (bottom sheet)**
- Recommended main video (1080p MP4, size, duration) separated from other streams and previews (480p, 15s ads)
- Cast Now, Add to Queue and Copy Link (copies to the clipboard)
- A 320×50 banner ad placeholder anchored below all buttons

**Expanded remote (full screen)**
- Poster, timeline scrubber (disabled automatically when the device has `supportsSeek = false`), 15s rewind, 30s forward, play/pause and TV volume
- Tabs: **Subtitle Sync** (-0.5s / +0.5s and a real `.srt` file picker), **D-Pad / Gesture Trackpad** (swipe and tap), and **Queue**

## Design

| Token | Value |
|---|---|
| Background | `#121212` |
| Elevated cards | `#1E1E24` |
| Active / detected accent | `#00E5FF` (vibrant cyan) |

Material 3 dark theme only.

## Project structure

```
app/src/main/java/com/nilesh/cast/
├── MainActivity.kt                  Entry point (edge-to-edge)
└── ui/
    ├── theme/Theme.kt               Colours and Material 3 dark theme
    ├── CastUiState.kt               UI state, models and the CastActions interface
    ├── CastViewModel.kt             State, logic and mock data
    ├── CastComponents.kt            Shared composables (badges, poster, sliders)
    ├── MainBrowserScreen.kt         Browser, home hub, mini-controller, omnibox
    ├── DevicePickerBottomSheet.kt
    ├── DetectedMediaBottomSheet.kt
    └── ExpandedRemoteSheet.kt
```

**Architecture:** unidirectional data flow. `CastViewModel` exposes a single `StateFlow<CastUiState>`
and implements `CastActions`. Every screen is stateless and takes `(state, actions)`.

## Build

**Requirements:** Android 8.0+ (API 26), Kotlin 2.0.20, AGP 8.5.2, Compose BOM 2024.09.03, JDK 17.

### Option A: Android Studio
1. Open the project folder in Android Studio (Koala or newer).
2. Let Gradle sync, then press **Run** on a device or emulator.

### Option B: GitHub Actions (no PC needed)
1. Create a GitHub repository.
2. Add `build-compose.yml` as `.github/workflows/build.yml`.
3. Open the **Actions** tab and wait for the green tick (about 5 to 8 minutes).
4. Download `Nilesh-Cast-UI.apk` from the repository's **Releases** page.
5. Install it on your phone (allow "Install unknown apps" if asked).

## Try it

1. Open the app and tap a site under **Recently Visited**.
2. Tap the pulsing cyan badge in the bottom bar to see the detected streams.
3. Tap **Cast Now**, choose a TV, and the mini-controller appears.
4. Tap the mini-controller to open the full remote.
5. Connect to **Sony Bravia (DLNA)** to see the seek bar disabled.

## Known limitations

- Device discovery, casting, volume and D-pad commands are simulated
- The browser page is a preview, not a real `WebView`
- Local Media, IPTV M3U and Bookmarks show a message only
- Subtitle delay is stored but not applied to any player
- Ad cards are placeholders; no ad SDK is integrated
- DRM-protected video (Netflix, Hotstar and similar) cannot be captured or cast this way

## Roadmap

- [ ] Replace the page preview with a real `WebView` and detect `.mp4` / `.m3u8` requests
- [ ] Real discovery for Google Cast, Roku, LG WebOS, Samsung Tizen and DLNA
- [ ] Local file server for casting files from the phone
- [ ] M3U playlist import and bookmarks storage
- [ ] Apply subtitle delay during playback
- [ ] Previews and UI tests

## License

Add your preferred licence here (for example MIT).
