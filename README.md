# Blade Overdrive: Enhanced Edition

Blade Overdrive is a browser-based tactical action arcade game built around precision aiming, blade launches, wall ricochets, and Bullet-Time. Players read the arena, plan indirect shots, manage limited resources, and pursue higher scores across three game modes.

The game combines a physics-inspired aiming challenge with fast arcade pacing. Its dark arena, neon targets, trajectory guides, resource indicators, and combo feedback are designed to keep each shot readable while rewarding deliberate play.

## Contents

- [Features](#features)
- [Game Modes](#game-modes)
- [How to Play](#how-to-play)
- [Scoring and Resources](#scoring-and-resources)
- [Accounts and Global Leaderboard](#accounts-and-global-leaderboard)
- [Technology](#technology)
- [Run Locally](#run-locally)
- [Deploy to Netlify](#deploy-to-netlify)
- [Supabase Configuration](#supabase-configuration)
- [Testing](#testing)
- [Project Structure](#project-structure)
- [Security and Limitations](#security-and-limitations)

## Features

- **Bullet-Time aiming:** Hold the pointer or touch input to slow the action and inspect the blade trajectory before launching.
- **Ricochet-based combat:** Use arena boundaries to redirect blades and reach targets from indirect angles.
- **Armoured targets:** Read shield placement and plan shots toward exposed cores.
- **Environmental hazards:** Avoid spike mines that detonate on impact and damage the hull.
- **Resource management:** Monitor blade energy, hull shield, and Overdrive charge during a run.
- **Combo and score feedback:** Build ricochet combos and use the score display to track performance.
- **Three game modes:** Choose Endless, Time Attack, or Precision.
- **Local records:** The browser stores selected run statistics and the local high score.
- **Online competition:** Signed-in players can submit runs to a shared leaderboard.
- **Responsive input:** The game interface supports mouse and touch interaction.

## Game Modes

### Endless

Survive successive waves and continue scoring for as long as the run allows.

### Time Attack

Score as much as possible within the 90-second time limit.

### Precision

Complete a run within a limit of 30 launches while protecting the hull.

## How to Play

1. Open the game and select a mode from the title screen.
2. Start a run and move the pointer or touch the play area to aim.
3. Hold the input to activate Bullet-Time and inspect the trajectory guide.
4. Choose a useful angle, including a wall bounce when a direct path is blocked.
5. Launch the blade and watch the target, score, combo, energy, and hull indicators.
6. Avoid spike mines and manage resources carefully.
7. Finish the run to view the result and run statistics.
8. Sign in if you want to submit the run to the global leaderboard.

## Scoring and Resources

The in-game interface displays the main values used to follow a run:

| Indicator | Purpose |
|---|---|
| Score | Tracks points earned during the run. |
| Hull Shield | Shows the hull's remaining protection. |
| Blade Energy | Tracks energy available for aiming and launches. |
| Overdrive Gauge | Tracks charge toward Overdrive activation. |
| Ricochet Combo | Displays the current ricochet combo. |
| Wave and mode status | Shows progression and the active game mode. |

Launching and aiming use energy. Successful target interactions can restore energy. Mines and other damaging events put the hull at risk. Exact outcomes depend on the current game state and mode.

## Accounts and Global Leaderboard

Local gameplay does not require an account. Supabase provides email and password authentication, profile records, score storage, and the shared online leaderboard.

A player must sign in to submit a run to the global leaderboard. The leaderboard supports mode and time-period filters, username search, and paginated results. It compares submitted runs asynchronously; it is not a live multiplayer match.

The leaderboard displays public usernames and submitted run statistics. Email addresses are not displayed.

## Technology

- **HTML:** Page structure and interface.
- **CSS:** Visual styling and responsive layout.
- **JavaScript:** Game state, controls, gameplay logic, collisions, scoring, and interface updates.
- **HTML5 Canvas:** Arena rendering and gameplay visuals.
- **Browser local storage:** Local high score and selected run statistics.
- **Supabase Auth:** Email and password accounts.
- **Supabase Database:** Profile and score records with row-level security.
- **Supabase Edge Function:** Authenticated score submission and server-side field validation.
- **Netlify:** Static frontend hosting.
- **Node.js:** Runs the project's smoke-test script.

## Run Locally

You need Python 3 or a local static server such as the VS Code Live Server extension.

1. Download and extract the project archive.
2. Open a terminal in the `blade-overdrive-online` directory.
3. Start a local server.

Using Python:

```bash
python -m http.server 8000
```

4. Open the following address in your browser:

```text
http://localhost:8000
```

Alternatively, open the folder in Visual Studio Code and launch it with Live Server.

Use a local HTTP server rather than opening `index.html` directly with a `file://` URL. Authentication redirects require an HTTP origin that is allowed in the Supabase project configuration.

## Deploy to Netlify

The project is a static frontend and can be hosted on Netlify.

1. Extract the project archive.
2. Deploy the contents of `blade-overdrive-online` so that `index.html` is at the published root.
3. Confirm that the deployed site uses the intended Supabase authentication redirect configuration.
4. Open the deployed site and perform a hard refresh.
5. Test local gameplay, account creation, sign-in, score submission, and leaderboard loading.

The repository includes a `netlify.toml` configuration file. See `NETLIFY_DEPLOY.md` and `SETUP.md` in the project for deployment-specific instructions.

## Supabase Configuration

The project uses Supabase for accounts and online scores. Before changing deployment domains or running a separate instance, review the authentication URL configuration.

1. Enable the Email provider and allow account sign-up if new accounts are required.
2. Set the correct site URL in Supabase Auth.
3. Add the deployed domain and any local development origins to the allowed redirect URLs.
4. Apply the schema in `supabase/schema.sql` if setting up a new database.
5. Deploy the `submit-score` Edge Function and configure its required server-side environment.
6. Never place a service-role or secret key in browser code.

If email confirmation is enabled, users must confirm their email before signing in. The default email sender may be rate-limited, so a production deployment may require custom SMTP.

Refer to `SETUP.md` for the project's existing configuration details and full setup procedure.

## Testing

A smoke-test script is provided at:

```text
tests/smoke-test.cjs
```

Run the project checks from the project directory:

```bash
npm test
```

The script provides basic automated checks. It does not replace full browser testing of controls, collision behaviour, game modes, authentication, score submission, and leaderboard display.

For a manual test pass, verify:

- The title screen and mode selection load.
- Mouse and touch aiming work.
- Holding the input activates Bullet-Time.
- Blade launches, wall ricochets, target interactions, and collisions behave as expected.
- Energy, hull, score, combo, and Overdrive indicators update.
- Each game mode follows its stated limit or condition.
- Game-over results display correctly.
- Account creation and sign-in work when Supabase is configured.
- A signed-in player can submit a completed run.
- The leaderboard loads and its filters work.

## Project Structure

```text
blade-overdrive-online/
├── index.html
├── game.html
├── package.json
├── netlify.toml
├── NETLIFY_DEPLOY.md
├── SETUP.md
├── README.md
├── tests/
│   └── smoke-test.cjs
└── supabase/
    ├── config.toml
    ├── schema.sql
    └── functions/
        ├── _shared/
        │   └── cors.ts
        └── submit-score/
            └── index.ts
```

## Security and Limitations

The frontend runs in the player's browser, so client-side game state cannot be fully trusted against a determined user who modifies local code or runtime values. The score-submission Edge Function authenticates the user and validates submitted fields, but this should not be described as complete anti-cheat protection.

Keep privileged credentials on the server. The browser frontend should use only the public Supabase key intended for client access. Review database policies and Edge Function validation before making changes to the online score system.

## Project Goal

Blade Overdrive is built around a simple challenge: plan the shot, use the arena, manage risk, and improve the run. Its core experience rewards players for understanding trajectories rather than relying only on direct shots.
