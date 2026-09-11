---
description: דיפלוי האתר לפרודקשן — commit + push, ו-Netlify מפרסם אוטומטית
---

Publish the site. Netlify auto-deploys every push to `main`, so deploying = pushing.

1. `git status` + `git diff --stat` — show the user in Hebrew exactly what is about to go
   live. If there's nothing to push, say so and stop.
2. `git pull --rebase` first. If it conflicts inside `public/index.html`, STOP and tell
   the user — do not hand-merge that file (see CLAUDE.md § Git).
3. Ask the user to approve the push (it publishes to the live site). Wait for a yes.
4. `git add -A && git commit -m "<short summary>" && git push`
5. Wait ~40s, then verify:
   `curl -s -o /dev/null -w "%{http_code}" https://bakibutz.netlify.app/` → expect 200.
   If it isn't 200 yet, retry once after another 30s before reporting a problem.
6. Report in Hebrew: live at https://bakibutz.netlify.app + what changed.

Fallback (Tomer's machine only, if the GitHub↔Netlify link is broken):
`npx -y netlify-cli deploy --prod`. It bypasses git, so commit and push afterwards anyway.
