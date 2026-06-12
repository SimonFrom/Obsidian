# Family Dashboard — planning phase

Read CLAUDE.md first to understand the project conventions. Do NOT start implementing yet. Your job in this turn is to interview me, then write a SPEC.md and update CLAUDE.md. Stop after that so I can review.

## What I'm building

A family dashboard app. Primary use case: a single Android tablet sitting on the kitchen counter, but it must also run on iOS, Android phones, and web (Expo + expo-router covers all four). Family members are just a list of names/avatars on the device — no auth, no per-person login. Data syncs across devices via Supabase.

Five features:

1. **Calendar** — read-only Google Calendar sync. Show upcoming events on the home widget, full month + agenda view on the sub-page. No event creation in-app.
2. **Meal Plan** — show and edit meals per day of the week.
3. **Weather** — multi-day forecast for a specific area.
4. **Chore List** — show and edit chores per family member.
5. **Spotify** — Premium account, full playback control (play/pause/skip/queue) with a now-playing widget on the home page and ability to search playlists and etc.

Home page is a **widget grid**: each feature shows live data inline (next event, today's meal, current temp, next chore, now-playing). Tapping a widget routes to its sub-page for detail/edit.

Design: soft colors, generous rounded corners, modern app feel. Reference points: Facebook, Instagram, the Claude.ai app. Commit to actual design tokens in the spec (border-radius scale, base palette, type scale) — don't leave it as "soft and modern."

## Architecture constraints (read carefully)

- **Data layer goes through Supabase** via the JS SDK. CRUD lives in `src/lib/crud/`, no React hooks in those files, exactly as CLAUDE.md says. This is a deliberate update to the "native DB per platform" rule — note it in the updated CLAUDE.md.
- **Platform-specific code paths** still apply to:
  - **Spotify playback**: `react-native-spotify-remote` on iOS/Android, Spotify Web Playback SDK on web. Both behind a single `PlayerService` interface in `src/services/spotify/`. Screen code must not know which platform is active.
  - **OAuth**: use `expo-auth-session` (handles web + native uniformly) for Google Calendar and Spotify.
- **State**: Zustand stores in `src/stores/`. One store per feature. No Context API.
- **Styling**: NativeWind + react-native-reusables. StyleSheet only when neither covers it.
- **Routing**: expo-router with a `(tabs)` group. Home is the index tab; each feature gets its own tab/route.
- **MVC discipline**: screens are thin — they read from stores and dispatch crud calls. Business logic lives in stores and services.

## Interview me

Before writing SPEC.md, use the `AskUserQuestion` tool to clarify the following. Ask in small batches, not one giant list. Don't ask things that are obvious from this brief — dig into the parts that genuinely change implementation:

- **App name** and any color/branding preferences beyond "soft, modern."
- **UI language**: Danish or English? (My other Expo project uses Danish strings + English code/comments.)
- **Meal Plan structure**: free-text per day, or structured fields (name, optional notes, optional link/image)? One meal per day or breakfast/lunch/dinner slots?
- **Chores**: one-off only, recurring only, or both? Assignment to a single member or multiple? Track completion (just done/not-done, or with timestamps)? Any rotation logic?
- **Weather**: location source — device GPS, manual city, or both? Provider preference — Open-Meteo (free, no API key, recommended) vs OpenWeatherMap?
- **Family member management**: how do I add/remove people? Avatar source (initials, emoji, photo)? Color per person?
- **Google Calendar scope**: primary calendar only, or all the user's calendars? How many days ahead to fetch?
- **Spotify on web**: the Web Playback SDK requires the page to be focused and has DRM caveats. Acceptable for v1, or web should fall back to "view-only" mode?

## Deliverables for this turn

1. **`SPEC.md`** at the repo root containing:
   - Architecture overview with the `src/` directory layout (`stores/`, `lib/crud/`, `services/`, `app/`, `components/`, `lib/types/`).
   - Supabase schema: every table with columns, types, FKs, and RLS posture (note: no auth means no RLS — use anon key + a shared "family" row scope, or document the alternative).
   - Per-feature breakdown: screens, store shape (state + actions), crud functions, and the widget's data dependency.
   - Design tokens: actual hex values for the palette, radius scale, spacing scale, type scale.
   - Home widget grid: layout rules at tablet / phone / web breakpoints.
   - **Out of scope for v1**: explicit list (e.g. event creation, recipe import, chore rotation if I say no, etc.).
   - **Verification plan**: `npm run typecheck` must pass; jest test files for each store covering reducers/actions; a manual smoke-test checklist per platform.

2. **Updated `CLAUDE.md`** reflecting:
   - Supabase as the data backing (with the "no hooks in crud/" rule preserved).
   - The platform-specific paths that DO remain (Spotify SDK, OAuth).
   - Any new conventions discovered while planning.

3. A brief summary in chat of the key decisions and any open questions you couldn't resolve from my answers.

**Do not write feature code yet.** When SPEC.md and the CLAUDE.md update are ready, stop and let me review. We'll implement in a fresh session against the approved spec.