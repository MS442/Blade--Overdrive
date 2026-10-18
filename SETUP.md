# Blade Overdrive — public game + shared online leaderboard

The frontend is a static HTML5 Canvas game. Supabase hosts shared accounts and scores, so the same leaderboard is used by the Netlify site and local development copies. Players can create accounts using email/password; Google OAuth is intentionally hidden while its provider is not ready for public use.

## Supabase project already configured

- Project URL: `https://qaomvgwryemkrycfjbzk.supabase.co`
- Project ref: `qaomvgwryemkrycfjbzk`
- Region: Mumbai (`ap-south-1`)
- The public frontend uses the Supabase publishable key only. No service-role key is included in the frontend.
- The `profiles` / `scores` schema and `submit-score` Edge Function have been deployed.

## 1. Configure authentication redirects

Open [Supabase URL Configuration](https://supabase.com/dashboard/project/qaomvgwryemkrycfjbzk/auth/url-configuration).

Set **Site URL** to the current public site:

`https://steady-puffpuff-987c0a.netlify.app`

Add these entries under **Redirect URLs** and save:

- `https://steady-puffpuff-987c0a.netlify.app/**`
- `http://localhost:5500/**`
- `http://127.0.0.1:5500/**`
- `http://localhost:8000/**`
- `http://127.0.0.1:8000/**`

Email confirmation links will redirect to the origin where the account was created. Redirects must be allowed in Supabase.

## 2. Enable email/password signup

Open [Supabase Auth Providers](https://supabase.com/dashboard/project/qaomvgwryemkrycfjbzk/auth/providers). Make sure **Allow new users to sign up** is enabled and the **Email** provider is enabled. Email confirmation can remain enabled for safer public signup; players must click the confirmation email before signing in. The default Supabase email sender is rate-limited, so configure custom SMTP if you expect real public traffic.

## 3. Deploy the public game to Netlify

1. Extract this ZIP.
2. Open [Netlify](https://app.netlify.com/) and select the site currently published at `https://steady-puffpuff-987c0a.netlify.app`.
3. Upload the extracted folder contents using Netlify manual deploy, or push them to the connected repository and deploy. Ensure `index.html` is at the published root.
4. Open the site and hard-refresh with `Ctrl+Shift+R`.

## 4. Run the same game locally

Do not open the HTML as `file://` if you need email-confirmation redirects. Serve the folder over localhost instead.

**Option A — Python:** open a terminal in the extracted folder and run `python -m http.server 8000`, then visit `http://localhost:8000`.

**Option B — VS Code:** install/use Live Server and visit its local URL (usually `http://127.0.0.1:5500`). If the port differs, add that exact localhost URL to Supabase Redirect URLs.

Local players and Netlify players will share the same Supabase account system and global leaderboard. Local runs only appear globally after a player signs in and finishes a run successfully.

## 5. Test the full flow

1. Create an account using an email you can access, a password of at least 8 characters, and a username of 3–20 letters/numbers/underscores.
2. If email confirmation is enabled, confirm it from your inbox.
3. Sign in, play a run, and finish it to submit the score.
4. Open the global leaderboard and verify that the score appears.
5. Test from a second browser or device; it should show the same leaderboard.

## Security notes

- Never place the Supabase service-role/secret key in frontend files.
- The Edge Function authenticates users and validates score fields before inserting. Browser games cannot fully prevent a determined player from tampering with client-side code.
- The leaderboard displays public usernames and run statistics, not email addresses.
