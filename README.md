# Local Media Scrubber

A privacy-focused, client-side metadata cleaner for common images, video, and audio. The interface detects the selected file type automatically, applies the matching operation, and replaces the original filename with a cryptographically generated name.

## Supported in this version

- Images: PNG, JPG/JPEG, WebP
- Video: MP4, MOV, WebM
- Audio: MP3, WAV, M4A

DNG/RAW, GIF, animated WebP, documents, archives, and uncommon containers are explicitly unsupported.

## Privacy model

- The application has no backend, accounts, analytics, or upload code.
- Image cleaning uses browser image decoding and canvas export.
- Audio/video cleaning uses FFmpeg compiled to WebAssembly.
- Selected file bytes stay in the browser's memory and FFmpeg's in-browser virtual filesystem.
- Temporary FFmpeg files are deleted after each attempt.
- Result object URLs are revoked when discarded or when the page closes.
- Only interface preferences are stored in `localStorage`.
- SHA-256 fingerprints are optional and off by default.

### Dependency-network disclosure

For a working GitHub Pages deployment without committing a large WebAssembly binary, the first audio/video operation loads pinned versions of `@ffmpeg/ffmpeg`, `@ffmpeg/util`, and `@ffmpeg/core` from jsDelivr. The selected media file is not uploaded to jsDelivr. Images do not load FFmpeg.

For a completely self-hosted/offline deployment, download those pinned package assets, place them under a `vendor/` directory, and replace the CDN import/core URLs in `app.js` with local paths.

## Cleaning modes

### Standard

- Images: decode visible pixels and export a new image in the same format.
- Audio/video: rewrite the container and copy ordinary media streams where possible.
- Global metadata and chapters are not mapped.
- Subtitles, attachments, cover art, and data streams are not selected.

### Maximum

- Images: decode visible pixels and export a new high-quality image in the same format.
- Audio/video: decode and re-encode ordinary media streams in the same general file type.
- The process can change file size or media quality.

No universal metadata tool can prove removal of every unknown proprietary structure. The app describes the operation it performed and never labels unsupported files as cleaned.

## Run locally

ES modules and WebAssembly work best through a local web server rather than by double-clicking `index.html`.

```bash
python3 -m http.server 8080
```

Then open `http://localhost:8080`.

## Deploy to GitHub Pages

1. Create a repository and add `index.html`, `styles.css`, `app.js`, `.nojekyll`, and this README at the repository root.
2. In the repository settings, open **Pages**.
3. Choose deployment from a branch and select the repository's main branch/root.
4. Open the published HTTPS site.

## Known practical limits

The app sets no artificial file-size limit, but browser memory, WebAssembly memory, codec support, and device performance impose real limits. Maximum cleaning is substantially heavier than Standard cleaning. MOV stream-copy or codec combinations can fail when the source streams are incompatible with the output container; failures are shown and no result is called clean.

## Project structure

```text
index.html   Interface and supported-format disclosure
styles.css   Responsive light/dark UI
app.js       Detection, filename generation, image cleaning, FFmpeg operations
.nojekyll    GitHub Pages compatibility marker
README.md    Deployment, privacy, and behavior documentation
```
