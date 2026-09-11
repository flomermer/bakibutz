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
Netlify builds from GitHub: **any push to `main` publishes the site automatically**
(no build command, publish dir `public/`). The flow is:
```bash
git pull --rebase   # ALWAYS, before touching public/index.html
# ...edits...
git add -A && git commit -m "..." && git push
```
Production URL: https://bakibutz.netlify.app — live ~15–30s after the push (measured
builds: 10–15s, plus webhook and queue). Verify with
`curl -s -o /dev/null -w "%{http_code}" https://bakibutz.netlify.app/` (expect 200),
then report the URL and what changed.

Repo: https://github.com/flomermer/bakibutz. Two people work on this — Tomer and
Danielle — each from their own machine with their own Claude. The repo is **public on
purpose**: on Netlify's free plan a private repo only builds from commits by paid team
members, so Danielle's pushes were rejected with "Unrecognized Git contributor". Don't
flip it back to private without also moving deploys off Netlify's git integration.

`npx -y netlify-cli deploy --prod` still works as a manual fallback, but only on Tomer's
machine (his is the only Netlify account), and it publishes the local file without going
through git, so repo and production can drift apart. Prefer the push.

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
A push to `main` *is* a deploy to the live site, so **ask the user for approval before
committing and pushing** — then do it, since that's the only way a change reaches
production. Never commit unasked-for or unrelated work.

The whole site is one ~3.7MB file full of base64 images, so a merge conflict inside it is
practically unfixable by hand. Discipline:
- `git pull --rebase` at the start of every editing session, no exceptions.
- Commit and push each finished change right away; don't sit on local edits.
- If a conflict does hit `public/index.html`, don't hand-merge — take one side whole
  (`git checkout --ours/--theirs public/index.html`) and redo the other change on top.
