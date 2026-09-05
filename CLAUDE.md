# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Starter Express API for the Claude Code course. A minimal REST API over an in-memory data store — not a production app.

## Commands

```
npm install
npm run dev      # start the API with auto-restart on file changes (http://localhost:3000)
npm start        # start the API without watch mode
npm test         # run all tests (Node's built-in test runner)
npm run lint     # run eslint
```

Run a single test file:
```
node --test tests/users.test.js
```

## Architecture

- `server.js` — creates and configures the Express `app`, mounts route modules, and starts listening. It only calls `app.listen` when run directly (`require.main === module`), so `tests/*.test.js` can `require("../server")` and drive it with supertest without opening a real port.
- `routes/` — one file per resource, each exporting an `express.Router()`. `server.js` mounts each under its own path prefix (e.g. `routes/users.js` → `/users`).
- `db/store.js` — an in-memory data helper standing in for a real database. State is a module-level array and resets on every server restart; there is no persistence layer to reason about.

Request flow: `server.js` → `routes/<resource>.js` → `db/store.js`. There is no service or controller layer in between.

## Notes

- `.env` is git-ignored; copy `.env.example` to `.env` for local config. The app currently only reads `PORT`.
