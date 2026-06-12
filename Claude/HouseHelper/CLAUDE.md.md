## Commands
- npx expo start: dev server
- npm test: run Jest suite
- npm run typecheck: tsc --noEmit

## Stack & conventions
- Expo SDK 54, React 19, expo-router 6
- Use Typescript and check for platform where needed.
- State: Zustand stores in src/stores/. NEVER import Context API.
- DB: Use Supabase and native storage where it makes sense. CRUD lives in src/lib/crud/ — these files MUST NOT call React hooks.
- Styling: NativeWind and React Native Reuseables only. Only use regular stylesheets when no other option is available.

## Patterns
- Make sure to follow a strict pattern with MVC structure.
- Don't overcomplicate and prefer verbose syntax to improve readability.
- Screen files should contain as little logic as possible.

## Workflow
- Always run typecheck after a series of edits.
- Prefer running a single test file over the full suite.