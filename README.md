# Meta Data Scrubber

A two-page, browser-only tool for stripping common embedded metadata from images, video, and audio, then replacing the original filename with a randomly generated one. Nothing is uploaded anywhere; all processing happens in the browser tab.

## Files

- `index.html` — landing page with a short description and a launch link.
- `Meta_Data_Scrubber.html` — the actual tool. Fully self-contained (styles and script inline).

To use it, open `Meta_Data_Scrubber.html` directly in a browser, or simply use my version in GitHub pages.

## What it does

**Images (PNG, JPG/JPEG, WebP):** the visible pixels are decoded and redrawn onto a new canvas, which is then re-exported as a fresh file of the same format. Nothing from the original file's metadata is intentionally carried over. Animated WebP isn't supported and will report as such rather than being silently mishandled.

**Audio/video (MP3, WAV, M4A, MP4, MOV, WebM):** handled locally via FFmpeg compiled to WebAssembly (loaded from jsDelivr on first use, not the file itself).

- *Standard mode:* rewrites the container and copies the normal media streams losslessly where the format allows, omitting global metadata, chapters, subtitles, attachments, cover art, and extra data streams.
- *Maximum mode:* decodes and re-encodes those streams instead of copying them, which is slower and can change size or quality slightly.

**Filenames:** replaced with a random string built from `crypto.getRandomValues`, using whichever character sets are enabled (lowercase, uppercase, digits, a safe symbol set), at a length you choose. The original extension is kept.

**Optional:** SHA-256 fingerprints of the input and output can be calculated and shown locally, off by default.

## What it doesn't claim

This is a metadata *cleaner*, not a forensic guarantee. It removes the metadata types it knows about and describes; it does not promise removal of every unknown or proprietary structure a file might contain, and it says so rather than reporting files as unconditionally "clean." Unsupported file types are rejected outright instead of being passed through and marked safe.

## Supported formats

| Type  | Formats |
|-------|---------|
| Image | PNG, JPG/JPEG, WebP |
| Video | MP4, MOV, WebM |
| Audio | MP3, WAV, M4A |

Not supported: DNG/RAW, GIF, animated WebP, documents, archives, and less common containers.