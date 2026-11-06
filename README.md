# Teleprompter

Static, zero-dependency teleprompter, designed for a Samsung tablet and a Desview-style beam-splitter.

## Features
- Black screen with white text; local script autosave
- Horizontal and vertical mirroring
- Adjustable font size, line spacing, text width and automatic scrolling speed
- Reading guide and full-screen view
- Keyboard / Bluetooth HID remote controls and remote diagnostic tester
- Installable PWA with service worker for offline use after first successful online load

## Deploy to Cloudflare Pages
1. In Cloudflare **Workers & Pages**, create a Pages project connected to this GitHub repository.
2. Set production branch to **main**. Choose framework **None**, leave build command blank, and use repository root **/** as output directory.
3. Deploy and (optionally) attach **prompter.c5projects.ca** as a custom domain.
4. Open the resulting HTTPS URL on your Samsung tablet in Chrome. Use the menu's **Add to Home screen / Install app** entry, where offered.
5. Launch the app once while online to cache its files before relying on it offline.

## Desview remote
Pair the Bluetooth remote in Android settings, then open the app and enable **Remote tester**. Press each remote button to see whether the browser receives a keyboard event and what key/code is sent. If a button sends no event, it may be using a media-control protocol that needs additional mapping or a different remote mode.

Standard keyboard controls: Space/Enter = play/pause; up/down arrows = short scroll; PageUp/PageDown = longer scroll; Home = reset; M = mirror horizontally; F = full screen; Escape = exit.

## Privacy
No analytics, backend, cookies or external scripts. Scripts and preferences stay in that browser's local storage. Keep a separate copy of important scripts in Word.

## Development branch: camera recording (experimental)

The `dev` branch includes the current in-browser camera implementation and a guide-position range of 15–95% of the viewport height. These changes were already present in `main` when `dev` was created; the branch is now the designated place for further camera development.

- **Camera: On** requests video and microphone access; **Flip camera** switches between front and rear where supported.
- **Record** starts/stops `MediaRecorder` and downloads the recorded camera stream to the device. The scrolling script is visible on screen but is **not** included in the video.
- Camera access requires HTTPS (or localhost), user permission and browser support. Recording codecs and download behaviour differ between Android browsers and iOS Safari.
- The recording is held in browser memory until stopped. Test short clips first; long recordings can exhaust memory or be lost if the app closes. Confirm the downloaded file and audio before relying on it for production.
- Mirroring affects script presentation, not the raw camera recording. Do not assume every browser can use the camera while in PWA full-screen mode.
- This feature has **not** yet passed physical-device recording tests. Keep the existing external-camera plus Desview workflow as the production fallback.

For a separate preview deployment, configure Cloudflare Pages to build the `dev` branch as a preview environment. Keep `main` as the production branch.
