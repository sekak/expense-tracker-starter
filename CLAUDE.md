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

Single-page React app with no routing or external state management. `transactions` array is the only shared state, lifted into `App`.

**Component tree:**
- [src/App.jsx](src/App.jsx) — holds `transactions` state and seed data; passes it down to all children
- [src/Summary.jsx](src/Summary.jsx) — receives `transactions`, computes `totalIncome`, `totalExpenses`, `balance` internally
- [src/TransactionForm.jsx](src/TransactionForm.jsx) — owns its own form state (`description`, `amount`, `type`, `category`); calls `onAdd(transaction)` prop on submit
- [src/TransactionList.jsx](src/TransactionList.jsx) — receives `transactions`, owns its own filter state (`filterType`, `filterCategory`)

**Data flow:** `App` owns the source-of-truth array. `Summary` and `TransactionList` read from it; `TransactionForm` writes to it via `onAdd`. No persistence — state resets on reload.

**`categories`** constant is duplicated in `TransactionForm` and `TransactionList` — not yet extracted to a shared file.
