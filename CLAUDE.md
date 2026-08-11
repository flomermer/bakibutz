# bakibutz — דף הנחיתה של ״בקיבוץ״

Hebrew RTL landing page for "בקיבוץ" — Aya & Ovadia Tal's art-workshop + home-cooked-meal
hosting business in Kibbutz Barkai. **Speak Hebrew with the user.**

## Structure
- `public/index.html` — **the entire site and the single source of truth.** Fully
  self-contained: all images and Hebrew fonts are embedded as base64 data URIs.
  Edit this file directly.
- `attaches/` — original full-res photos (source material, never deployed).
- `netlify.toml` — tells Netlify to publish only `public/`.

## Deploy
```bash
npx -y netlify-cli deploy --prod
```
Run from the repo root. The folder is already linked (`.netlify/state.json`) and the
user is logged in. Production URL: https://bakibutz.netlify.app
After deploying, report the URL and what changed.

## Design language
Dark "forest evening" theme modeled on the owner's Canva reference — deep green ground
(`#17211A`/`#1D2A21`), gold headings (`#D9B45B`), cream text (`#F0EBD8`). Display font:
Amatic SC 700 (hand-lettered, Hebrew); body: Assistant. Single committed dark theme (no
light mode), RTL throughout. Keep new UI inside these tokens (they're CSS variables at
the top of the `<style>` block).

## Adding/replacing images
Originals go in `attaches/`. For the site, compress first, then embed as base64:
```bash
sips -Z 1000 -s format jpeg -s formatOptions 62 original.jpeg --out /tmp/web.jpg
base64 -i /tmp/web.jpg   # → data:image/jpeg;base64,<paste into src>
```
(logo used `-Z 700 ... formatOptions 75`). Target ≤ ~250KB per image. Gallery/section
images carry `loading="lazy"`; keep that on new ones.

## Owner edit mode (backoffice)
Opening `public/index.html` locally with `#edit` in the URL (file:// or localhost only)
enables in-place text editing; "שמירת קובץ" downloads an updated file the user swaps in.
The mode is hard-blocked on any real domain. The file gets re-serialized by this flow,
so attribute order/whitespace may shift — grep for actual strings before Edit calls.

## Registration form
No backend: submit opens WhatsApp (Aya, +972556612048) with the details as a prefilled
message. Ovadia: +972523606893. Form fields carry `data-1p-ignore`/`autocomplete="off"`
to silence password managers — keep those when touching the form.

## Git
Never create or offer git commits. The user commits manually and will ask explicitly
when they want one.
