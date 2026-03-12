# GatheRice

A minimalist, high-performance rice-gathering idle game. 

- **Live:** [gatherice.wokspec.org](https://gatherice.wokspec.org)
- **Repo:** [WokSpec/GatheRice](https://github.com/WokSpec/GatheRice)
- **Architecture:** [→](../../GatheRice/docs/architecture.md)

---

## Overview

GatheRice is a pure-client-side incremental game built with React, TypeScript, and Vite. It demonstrates how to manage complex application state and persistence without a backend.

## Key Features

- **Progressive Automation**: Buy upgrades to automate grain collection.
- **Prestige System**: Polish grains to multiply click efficiency.
- **Local Persistence**: Automatic state saving via `localStorage`.
- **High Performance**: Optimized 10Hz game loop for smooth updates.

## Technical Details

| Aspect | Implementation |
|---|---|
| Framework | React 19 (Hooks) |
| Build Tool | Vite |
| Persistence | Web Storage API (localStorage) |
| State | `useState` + `useMemo` for derived values |
