# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A simple full-stack Pomodoro timer application demonstrating AI agent workflows. The app has a deliberate bug in the state transition logic that causes the timer to get stuck in "break" mode.

## Architecture

**Backend** (`backend/`)
- Express server running on port 3000
- `server.js`: HTTP API with endpoints GET `/state` and POST `/next`
- `timer.js`: State machine managing focus/break transitions (contains the bug)

**Frontend** (`frontend/`)
- Vanilla HTML/CSS/JS (no build step)
- `index.html` must be opened directly in browser
- Communicates with backend via fetch/XHR

**Tests** (`tests/`)
- Jest test suite validating timer state transitions
- Tests expose the bug: timer never transitions back to "focus" from "break"

## Common Commands

```bash
# Install dependencies
npm install

# Start the backend server
node backend/server.js
# or
npm start

# Run tests
npm test

# Run a single test file
npm test -- tests/timer.test.js

# Run tests matching a pattern
npm test -- --testNamePattern="should alternate"
```

## Development Workflow

1. Start the server: `node backend/server.js`
2. Open `frontend/index.html` in a browser (file:// or local server)
3. Click "Next" button to trigger state transitions
4. Run `npm test` to verify fixes

## Key Implementation Details

- The bug is in `backend/timer.js:nextSession()` - the else branch sets state to "break" instead of "focus"
- State variables: `state` ("focus" | "break") and `sessions` (count of completed cycles)
- Tests expect: focus → break → focus transitions with session increment after full cycles
