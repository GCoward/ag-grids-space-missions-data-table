# Mission Control

A Next.js + TypeScript app built as part of the AG Grid Foundations course. The project teaches the core concepts behind AG Grid by creating a space-missions data table with progressively richer behaviour.

## Overview

This app follows the course structure from `AG Grid Foundations.md` and builds a Mission Control dashboard that:

- loads real mission data in a Server Component
- passes it to an AG Grid client component
- defines typed column configuration
- supports sorting, filtering and pagination
- renders custom cells for status and cost information
- enables inline editing and row selection
- exports data to CSV
- applies a custom AG Grid theme

The stack is:

- Next.js 16
- TypeScript
- React 19
- AG Grid Community 36.1
- App Router

## What you'll learn in this project

The course is divided into 8 chapters, covering the ideas every AG Grid implementation builds on:

1. Getting started with AG Grid and Next.js
2. Building a first grid
3. Configuring column definitions and formatters
4. Sorting, filtering and pagination
5. Custom cell renderers and styling
6. Editing and row selection
7. Fetching server data and using the Grid API
8. Theming and next steps

## Project structure

```text
app/
  layout.tsx
  page.tsx
  providers.tsx
  globals.css
  lib/
    definitions.ts
    placeholder-data.ts
```

Key design decisions from the course:

- `app/page.tsx` stays a Server Component and renders the grid shell
- the AG Grid component lives in a Client Component
- `AgGridProvider` registers AG Grid modules at the app root
- row data and column definitions are typed with TypeScript
- functions like renderers and formatters are defined in client code, not server props

## Getting started

Requirements:

- Node.js 20.9 or later
- npm

Install dependencies:

```bash
npm install
```

Run the app locally:

```bash
npm run dev
```

Then open http://localhost:3000 in your browser.

## Features built in the course

- typed mission row data
- responsive AG Grid table with fixed height container
- custom column definitions with flex sizing
- value formatting for dates, currency and booleans
- quick filters and column filters
- pagination with page-size selector
- custom cell renderer for mission status or badges
- editable cells and row selection
- CSV export via the grid API
- custom theme styling

## Notes

This repository is intentionally aligned with the AG Grid Foundations tutorial and is designed as a hands-on learning project. It demonstrates a practical AG Grid implementation in Next.js rather than a general-purpose template.
