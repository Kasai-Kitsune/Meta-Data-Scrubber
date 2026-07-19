Ever want to upload or use an asset or file somewhere but are concerned that it might have identifying metadata in it that you either missed, can't be bothered to remove yourself, or didn't know about? yeah me too. So I made this tool for myself. Anyway.....

## Meta Data Scrubber!

A two-page, browser-only tool for stripping common embedded metadata from supported images and documents; then replacing the original filename with a randomly generated one. Nothing is uploaded; all processing happens in your tab.

## Files

index.html — landing page with description and launch link.

Meta_Data_Scrubber.html — the tool itself. Fully self-contained.

Open Meta_Data_Scrubber.html directly in a browser, or use the hosted version.

## What it does

Images (PNG, JPG/JPEG, WebP, HEIC/HEIF): visible pixels are decoded and redrawn onto a new image. HEIC/HEIF is converted to PNG (tool limitations and compatibility reasons, should preserve transparency). Original metadata is not intentionally carried over.

Animated WebP/GIF is not supported and will be rejected.

Documents (PDF, DOCX, XLSX, PPTX, ODT, ODS, ODP):

· PDF: removes author, title, subject, keywords, creator, producer, and dates.
· Office/ODF: unzips and removes metadata XML files (docProps, customXml, meta.xml) before repacking.

Filenames: replaced with a random string (crypto.getRandomValues) from your chosen character sets (lowercase, uppercase, digits, safe symbols). Original extension is kept (or .png for HEIC).

Optional: SHA‑256 fingerprints of input and output can be shown locally (off by default).

## ⚠️ Note on file size

Removing metadata can sometimes increase file size, especially when converting HEIC to PNG. This is normal and happens because re-encoding may use different compression parameters or switch to a lossless format. The tool reports the cleaned size alongside the original, so you can decide if the trade‑off works for you.

## What it doesn't claim

This is a metadata cleaner, not a forensic guarantee. It removes the metadata types it knows about; it does not promise removal of every unknown or proprietary structure. Unsupported file types are rejected outright rather than reported as “clean.”

## Supported formats

Type       Formats
Image      PNG, JPG/JPEG, WebP, HEIC/HEIF (→ PNG)
Document   PDF, DOCX, XLSX, PPTX, ODT, ODS, ODP

Not supported: DNG/RAW, GIF, animated WebP, video, audio, archives, and uncommon containers.
