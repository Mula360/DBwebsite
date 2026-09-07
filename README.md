# Deccan Birders — static site

A single self-contained `index.html`. No build step, no dependencies, no framework.

## Deploy

```bash
git init
git add .
git commit -m "Deccan Birders site"
git remote add origin git@github.com:<you>/deccan-birders.git
git push -u origin main
```

Then in Vercel: **Add New → Project → import the repo**. Leave every build setting empty:

| Setting          | Value              |
| ---------------- | ------------------ |
| Framework Preset | Other              |
| Build Command    | *(leave blank)*    |
| Output Directory | *(leave blank)*    |
| Install Command  | *(leave blank)*    |

Vercel serves `index.html` at the root. Custom domain goes under Project → Settings → Domains.

Or straight from this folder without Git:

```bash
npx vercel --prod
```

## What's inside

`index.html` has the fonts, logo, styles and all page logic inlined. Navigation between Home, About, Aims, Committee, Activities, Sightings, Gallery, Archives, Membership and Contact happens client-side — the URL does not change, so `cleanUrls` in `vercel.json` is only there for future files.

## Before it goes live

1. **Bird photographs** are currently hotlinked from `deccanbirders.org/wp-content/uploads/`. They render as long as that host stays up. To serve them yourself, drop the files into `public/` (or an `img/` folder next to `index.html`) and point the `<img src>` and the `ASSET_ID` map at the local paths.
2. **eBird data** — the Sightings page (recent, notable, hotspot species lists, nearest-species lookup) and the "On this day" panel run on sample records in the API's response shape. Replace the `SIGHTINGS`, `HOTSPOT_SPECIES`, `SPECIES_INDEX` and `ON_THIS_DAY` constants with `fetch` calls to `https://api.ebird.org/v2/...`, sending your key as the `X-eBirdApiToken` header. Get a free key at ebird.org/api/keygen. Call it from a serverless function rather than the browser so the key is not public.
3. **Field trips** come from a `EVENTS` constant. Point it at the Google Calendar events feed.
4. **YouTube** — video cards and the channel button are inert placeholders. Add the channel URL and video IDs; thumbnails then come from `https://img.youtube.com/vi/<videoId>/hqdefault.jpg`.
5. **Forms** — the enquiry, volunteer and photo-submission forms are markup only. Wire them to a form handler (Vercel serverless function, Formspree, or the WordPress plugin you settle on) so submissions reach `photos@deccanbirders.org`.
6. **Placeholders to fill** — committee photographs, the enquiry phone number, and the WhatsApp group invite link.

## If you go the WordPress route instead

This file is the visual reference, not the theme. Hand it to a developer along with the palette (`#2B72B8` blue, `#0F7A3E` green, `#F2B705` yellow, `#16241D` ink, `#F5F8F6` paper) and the type pairing (Sora headings, Source Sans 3 body).
