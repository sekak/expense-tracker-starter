# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
npm run dev      # Start dev server at http://localhost:5173
npm run build    # Production build
npm run lint     # Run ESLint
npm run preview  # Preview production build
```

> **Node version:** Vite 7 requires Node 20+. On Node 18, downgrade with `npm install vite@5 @vitejs/plugin-react@4`.

## Architecture

Single-page React app. All logic and UI live in [src/App.jsx](src/App.jsx) — no routing, no external state management, no component split.

**App state:**
- `transactions` — array of `{ id, description, amount, type, category, date }`. `amount` is stored as a **string** (intentional bug: causes `reduce` to concatenate instead of sum).
- Form fields: `description`, `amount`, `type`, `category` (controlled inputs for adding transactions).
- Filter fields: `filterType`, `filterCategory` (filter the transaction table).

**Data flow:** summary totals (income / expenses / balance) and filtered transactions are derived inline during render — no memoization, no derived state. No persistence; state resets on reload.

**Intentional issues (part of a course):** `amount` as string breaks arithmetic totals; "Freelance Work" is typed `expense` but categorized `salary`.
