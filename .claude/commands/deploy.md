---
description: דיפלוי האתר לפרודקשן ב-Netlify
---

Deploy the site to Netlify production:

1. Run `npx -y netlify-cli deploy --prod` from the repo root (the folder is already
   linked to the "bakibutz" project and the user is authenticated).
2. Verify the deploy succeeded and confirm the live site responds:
   `curl -s -o /dev/null -w "%{http_code}" https://bakibutz.netlify.app/` → expect 200.
3. Report in Hebrew: that the deploy is live at https://bakibutz.netlify.app, and
   briefly what changed (check `git diff --stat` before deploying to see pending changes).

Do NOT commit anything — deploys and commits are separate; the user asks for commits explicitly.
