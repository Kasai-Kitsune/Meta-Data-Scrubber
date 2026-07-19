Ever want to upload or use an asset or file somewhere but are concerned that it might have identifying metadata in it that you either missed, can't be bothered to remove yourself, or didn't know about? yeah me too. So I made this tool for myself. Anyway.....

## Meta Data Scrubber!

A browser-only pair of tools. Meta Data Scrubber strips common embedded metadata from supported images and documents; PhosphorCrush is a companion for shrinking images back down when scrubbing pads the size back up. Nothing is uploaded; all processing happens in your tab.

## Files

index.html — landing page with description and launch links for both tools.

Meta_Data_Scrubber.html — the metadata cleaner. Fully self-contained.

Phosphor_Crush.html — the companion image compressor. Fully self-contained.

Open either file directly in a browser, or use the hosted version.

## Meta Data Scrubber: what it does

Images (PNG, JPG/JPEG, WebP, HEIC/HEIF): visible pixels are decoded and redrawn onto a new image. HEIC/HEIF is converted to PNG (tool limitations and compatibility reasons, should preserve transparency). Original metadata is not intentionally carried over.

Animated WebP/GIF is not supported and will be rejected.

Documents (PDF, DOCX, XLSX, PPTX, ODT, ODS, ODP):

· PDF: removes author, title, subject, keywords, creator, producer, and dates.
· Office/ODF: unzips and removes metadata XML files (docProps, customXml, meta.xml) before repacking.

Filenames: replaced with a random string (crypto.getRandomValues) from your chosen character sets (lowercase, uppercase, digits, safe symbols). Original extension is kept (or .png for HEIC).

Optional: SHA‑256 fingerprints of input and output can be shown locally (off by default).

One file at a time by design; no batch mode here.

## ⚠️ Note on file size

Removing metadata can sometimes increase file size, especially when converting HEIC to PNG. This is normal and happens because re-encoding may use different compression parameters or switch to a lossless format. The tool reports the cleaned size alongside the original, so you can decide if the trade‑off works for you. If it bothers you, that's what PhosphorCrush is for.

## PhosphorCrush: what it does

A companion re-encoder aimed at undoing the size bump scrubbing (or anything else) can leave behind. Takes PNG, JPG/JPEG, WebP, and HEIC/HEIF, and handles many files at once in your tab.

You set a target size reduction and a quality floor it won't drop below; it searches for the smallest output that still clears both. Format can stay on auto (defaults to WebP) or be forced to JPEG, PNG, or WebP. There's also an optional palette-reduction pass, with a choice of no dithering, 4×4 Bayer, or Floyd–Steinberg, for a more aggressive, deliberately lo-fi crunch. If the re-encode ends up larger than the original, the original is kept instead (unless palette reduction is on). Finished files can be downloaded one at a time or bundled into a ZIP.

## What it doesn't claim

Meta Data Scrubber is a metadata cleaner, not a forensic guarantee. It removes the metadata types it knows about; it does not promise removal of every unknown or proprietary structure. Unsupported file types are rejected outright rather than reported as “clean.” PhosphorCrush doesn't touch metadata at all, it's a size reducer only, so run the scrubber first if that's what you need.

## Supported formats

Type       Meta Data Scrubber                        PhosphorCrush
Image      PNG, JPG/JPEG, WebP, HEIC/HEIF (→ PNG)     PNG, JPG/JPEG, WebP, HEIC/HEIF
Document   PDF, DOCX, XLSX, PPTX, ODT, ODS, ODP       —

Not supported by either tool: DNG/RAW, GIF, animated WebP, video, audio, archives, and uncommon containers.
