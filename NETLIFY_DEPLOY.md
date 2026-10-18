# Deploy Blade Overdrive to Netlify

The game is static HTML. The online accounts and global leaderboard use the hosted Supabase project configured in `index.html`.

1. Extract the ZIP.
2. In Netlify, open the site whose URL is `https://steady-puffpuff-987c0a.netlify.app`.
3. Upload the extracted folder contents as a manual deploy, or push to your connected Git repository and deploy.
4. Make sure `index.html` is at the published root.
5. Hard-refresh the public URL with `Ctrl+Shift+R`.
6. In Supabase Auth URL Configuration, set the Site URL to the public URL and allow that URL plus your local development URLs; see `SETUP.md`.
