[**AG Grid Foundations**Next.js · TypeScript · AG Grid 36](#overview)

0 of 8 chapters complete

About 30 minutes in total. Your progress is saved in this browser only.

Free course · \~30 minutes

# Learn AG Grid with Next.js

Build a space-missions data table in a Next.js App Router project, one feature at a time. By the end you'll understand the handful of concepts that every AG Grid feature builds on.

**8** chapters**8** quizzesStack: **Next.js 16, TypeScript, AG Grid Community 36.1**

## What you'll build

A Mission Control page that loads a real dataset in a Server Component and hands it to an AG Grid client component with formatted columns, filters, pagination, a custom cell renderer, inline editing, row selection, CSV export and a custom theme.

Each chapter ends with a live preview of the grid at that stage, running the same configuration in your browser, followed by a short quiz.

## Chapters

> **Before you start:**
> 
> - You'll need Node.js 20.9 or later and a package manager (this course uses npm).
> - The course assumes you're comfortable with React and TypeScript. No prior AG Grid knowledge is needed.

[Start Chapter 1](#chapter-1)

Chapter 1

# Getting Started

AG Grid is a data grid library: a component for showing, sorting, filtering and editing rows of data. Its core is written in plain TypeScript and it ships thin wrappers for React, Angular and Vue, so the concepts you learn here carry over to other frameworks.

In this chapter, here are the topics we'll cover

- The difference between AG Grid Community and Enterprise.
- How to create a Next.js project and install the AG Grid packages.
- The folder structure we'll use for the rest of the course.

## Community and Enterprise

AG Grid comes in two editions:

- **`ag-grid-community`**: free and MIT-licensed. It includes sorting, filtering, pagination, editing, row selection, custom cells, theming and CSV export. Everything in this course uses Community.
- **`ag-grid-enterprise`**: a paid licence that adds features such as row grouping, pivoting, Excel export, integrated charts and the Server-Side Row Model.

## Creating a new project

Create a Next.js app with the recommended defaults (TypeScript, App Router, Tailwind CSS and ESLint):

Then install the React wrapper. It pulls in `ag-grid-community` as a dependency:

Open `http://localhost:3000` and you should see the Next.js starter page.

> **If you use pnpm:**
> 
> - pnpm doesn't hoist transitive dependencies, and you'll import from `ag-grid-community` directly. Install both: `pnpm add ag-grid-react ag-grid-community`.

## Exploring the project

We'll follow the same conventions as the Next.js dashboard course:

- **`/app/lib`**: types, placeholder data and formatting helpers.
- **`/app/ui`**: UI components, including the grid itself.
- **`/app/page.tsx`**: the home route. It stays a Server Component and renders the grid.
- **`/app/providers.tsx`**: a small client component that registers AG Grid's modules.

Replace the contents of `/app/page.tsx` with an empty page for now. We'll fill it in the next chapter.

## It's time to take a quiz!

Test your knowledge and see what you've just learned.

Which of these features needs an AG Grid Enterprise licence?

Row grouping, pivoting, Excel export and integrated charts are Enterprise features. Sorting, pagination and CSV export are all in the free Community edition.

Chapter 2

# Your First Grid

Every AG Grid starts with the same three ingredients: **row data** (an array of objects), **column definitions** (which fields to show) and a **container with a height**.

In this chapter, here are the topics we'll cover

- Registering AG Grid's modules with `AgGridProvider`.
- Typing your row data and column definitions.
- Why the grid must live in a Client Component.
- The most common first-grid bug.

## Typing the data

We'll work with a dataset of space missions. Start by describing one row:

Then add some placeholder rows. These are made up; we'll swap them for a real dataset in Chapter 7.

## Registering modules

AG Grid is split into modules so you only ship the features you use. The `AllCommunityModule` bundle turns on every Community feature, which is the easiest place to start. Provide it once, near the root of your app:

Wrap your app in it from the root layout. Keep everything else `create-next-app` generated; the only change is wrapping `children`:

> **Older tutorials:**
> 
> - You'll often see `ModuleRegistry.registerModules([AllCommunityModule])` instead. It still works and registers modules globally; `AgGridProvider` is the React-friendly way to do the same thing.
> - If you see `import 'ag-grid-community/styles/ag-grid.css'`, that's the legacy CSS theming. Since v33 the Quartz theme is applied automatically with no CSS imports. We'll customise it in Chapter 8.

## Rendering the grid

Create the grid component. Note the `'use client'` directive at the top:

Then render it from the page, which stays a Server Component:

A few things are worth understanding here:

- **Why `'use client'`?** The grid uses state, effects and DOM events, so it has to run in the browser. The page fetches or imports data on the server and passes it down as a prop.
- **Why `ColDef<Mission>`?** The generic makes `field` type-safe: `{ field: 'misson' }` is a compile error, and editors autocomplete the field names.
- **Why `useState` for columns?** It keeps the array reference stable. A new array on every render makes the grid reprocess its columns.
- **Free behaviour:** the grid infers each column's data type from the values. Booleans render as checkboxes, and you can already sort by clicking a header.

## It's time to take a quiz!

Test your knowledge and see what you've just learned.

Your page builds without errors, but the grid doesn't appear on screen. What's the most likely cause?

The grid sizes itself to fill its parent. A parent with no height gives the grid zero height, so nothing shows. No CSS import is needed since v33, and only the grid component itself needs `'use client'`.

Chapter 3

# Column Definitions

Column definitions are where most of your AG Grid configuration lives. In this chapter you'll tidy up the headers, make the columns fill the width, and format the raw values.

In this chapter, here are the topics we'll cover

- Renaming headers and sizing columns with `flex`.
- Sharing settings across columns with `defaultColDef`.
- Formatting values with `valueFormatter`.
- Creating derived columns with `valueGetter`.

## Adding formatting helpers

## Updating the columns

### Getters decide the value, formatters decide the look

This is the single most useful idea in AG Grid:

- **`field`** reads a property from the row object.
- **`valueGetter`** computes a value from the whole row. The result is the real cell value, so it's what sorting and filtering use.
- **`valueFormatter`** turns a value into display text. It only affects what you see; sorting and filtering still use the underlying value.

That's why the Cost column above still sorts numerically even though it displays strings like `$62,000,000`.

> **If you're a Next.js developer:**
> 
> - Column definitions often contain functions (formatters, getters, renderers). Functions can't be passed from a Server Component to a Client Component, so define `colDefs` inside the client component, not in `page.tsx`.

## It's time to take a quiz!

Test your knowledge and see what you've just learned.

The Cost column uses a `valueFormatter` to display `$7,500,000`. When you sort the column, what is it sorted by?

Formatters only change the display. Sorting and filtering work on the value from `field` or `valueGetter`.

Chapter 4

# Sorting, Filtering & Pagination

With your columns configured, you can turn on the features users expect from a data table with a few props.

In this chapter, here are the topics we'll cover

- How sorting works by default, and how to set an initial sort.
- Column filters and floating filters.
- A quick filter that searches every column.
- Pagination with a page-size picker.

## Sorting

Sorting is already on for every column. Click a header to cycle ascending, descending and none; hold Shift while clicking to sort by several columns. To start sorted, add `sort` to a column, and to turn sorting off for one, set `sortable: false`.

## Filtering

Filters are off by default. Setting `filter: true` lets the grid pick a filter from the column's data type: text, number or date. You can also name one explicitly. `floatingFilter` adds an always-visible filter input under each header.

## Quick filter and pagination

The quick filter matches text across all columns, which is ideal for a search box. Pagination is a single boolean plus optional page sizes:

> **Good to know:**
> 
> - Keep `paginationPageSize` as one of the values in `paginationPageSizeSelector`, otherwise the grid warns in the console.
> - Everything in this chapter runs in the browser on rows you've already loaded (the Client-Side Row Model). For very large datasets, AG Grid can fetch pages on demand instead; see Chapter 8.

## It's time to take a quiz!

Test your knowledge and see what you've just learned.

Without any extra configuration, which feature is already enabled on every column?

Sorting is on by default. Filters, floating filters and pagination all need to be switched on.

Chapter 5

# Custom Cells

Formatters give you text. When a cell needs markup, such as a badge, an icon or a button, you use a cell renderer, which in React is just a component.

In this chapter, here are the topics we'll cover

- Writing a cell renderer as a React component.
- Styling cells conditionally with `cellClassRules`.
- Choosing between formatters, class rules and renderers.

## A cell renderer component

The grid passes your component a props object with the cell's `value`, the whole row as `data`, the grid `api` and more. Type it with `CustomCellRendererProps<TData, TValue>`:

Pass the component itself (not JSX) to `cellRenderer`. While you're there, use `cellClassRules` to highlight expensive missions:

Add the styles to your global stylesheet:

## Which tool should I use?

- **`valueFormatter`**: plain text such as currency, dates or units. Cheapest.
- **`cellClassRules`**: same text, different styling based on the value. Classes are added and removed as values change.
- **`cellRenderer`**: anything that needs elements or interactivity. Each cell is a React component, so use it where it adds something.

> **Passing extra props:**
> 
> - Use `cellRendererParams` on the column to pass extra props to a renderer, for example `cellRendererParams: { showIcon: true }`.

## It's time to take a quiz!

Test your knowledge and see what you've just learned.

You want a column to show distances like `1,240 km`. What's the best fit?

It's plain text, so a formatter is the cheapest option and keeps sorting numeric. A getter returning a string would make the column sort alphabetically.

Chapter 6

# Editing & Selection

Grids are often used to change data, not only to read it. In this chapter you'll make cells editable, listen for edits, and let users select rows.

In this chapter, here are the topics we'll cover

- Making columns editable and choosing an editor.
- Responding to edits with `onCellValueChanged`.
- Row selection with the `rowSelection` options object.

## Editable cells

Set `editable: true` on a column. Users double-click a cell (or press Enter) to edit, and the grid picks an editor from the data type: a text input, number input or date picker. For a fixed list of options, use the built-in select editor:

## Listening for changes

When an edit is committed, the grid writes the new value onto the row object and fires `cellValueChanged`. This is where you'd save to an API or a Server Action:

> **Important:**
> 
> - The grid updates the row object in place. React doesn't know anything changed, so nothing re-renders. If other parts of your UI depend on the data, update your own state (or the server) from this event.

## Row selection

Pass an options object to `rowSelection`. In `multiRow` mode the grid adds a checkbox column and a select-all checkbox in the header:

> **Older tutorials:**
> 
> - `rowSelection="multiple"` with `checkboxSelection: true` on a column is the pre-v32.2 API. Use the options object instead.

## It's time to take a quiz!

Test your knowledge and see what you've just learned.

A user edits a mission name. What happens to your React state?

The grid mutates the row object and fires an event. It's up to you to sync that change to state or to the server.

Chapter 7

# Server Data & the Grid API

Time to replace the placeholder rows with a real dataset, and to control the grid from your own buttons.

In this chapter, here are the topics we'll cover

- Fetching row data in a Server Component.
- Showing a loading state with `loading.tsx`.
- Giving rows stable IDs with `getRowId`.
- Calling the Grid API through a ref.

## Fetching in a Server Component

AG Grid publishes a sample space-missions dataset with the same shape as our `Mission` type. Fetch it on the server and pass it to the grid:

Add a loading UI. Next.js shows it automatically while the page's data is being fetched:

> **Fetching on the client instead?**
> 
> - If you load data in a client component, start with `rowData` as `null` and pass `loading={rowData === null}`. The grid shows its own loading overlay until the data arrives.

## Stable row IDs

When `rowData` changes (for example after `router.refresh()`), the grid needs to know which new row matches which old one. Without IDs it rebuilds every row and loses the selection. With `getRowId` it updates only the rows that changed:

## The Grid API

Props describe what the grid should look like. The Grid API is for one-off actions: exporting, clearing filters, scrolling to a row. Get it through a ref:

Event callbacks also receive the API as `e.api`, which is what you used for `getSelectedRows()` in Chapter 6.

> **Rule of thumb:**
> 
> - If it's state (row data, search text, theme), pass it as a prop. If it's an action (export, reset, scroll), call the API.

## It's time to take a quiz!

Test your knowledge and see what you've just learned.

You move `colDefs` into `page.tsx` (a Server Component) and pass them to `MissionsGrid` as a prop. What happens?

Props crossing the server/client boundary must be serialisable. Plain data like `rowData` is fine; functions are not. Keep column definitions in the client component.

Chapter 8

# Theming & Next Steps

The grid has used the default Quartz theme so far. In this final chapter you'll make it match your app, then look at where to go after this course.

In this chapter, here are the topics we'll cover

- Customising a theme with the Theming API.
- Adding a dark variant.
- The finished component.
- What to learn next.

## The Theming API

Themes are JavaScript objects. Start from a built-in theme (`themeQuartz`, `themeAlpine`, `themeBalham` or `themeMaterial`), adjust parameters with `withParams`, and add parts such as colour schemes with `withPart`:

Pass the theme object to the `theme` prop. Switching themes is just changing the prop:

> **Tip:**
> 
> - The AG Grid Theme Builder at [ag-grid.com/theme-builder](https://www.ag-grid.com/theme-builder/) lets you design a theme visually and copy out the `withParams` code.

## The finished component

Here's everything from the course in one file:

## Where to go next

- **Smaller bundles:** swap `AllCommunityModule` for just the modules you use, such as `TextFilterModule`, `NumberFilterModule` and `CsvExportModule`.
- **Better errors in development:** since v36, call `enableDevValidations()` from `ag-grid-community` in development builds to get full warning messages instead of error codes.
- **Saving layouts:** `api.getColumnState()` and the `initialState` prop let users keep their column order, widths, sorts and filters.
- **Custom editors and filters:** like renderers, these are React components.
- **Large datasets:** the Infinite Row Model (Community) loads rows in blocks as the user scrolls; the Server-Side Row Model (Enterprise) adds server-side grouping and sorting.
- **Enterprise:** row grouping, pivoting, Excel export and integrated charts.

## It's time to take a quiz!

Test your knowledge and see what you've just learned.

In AG Grid 36, how do you change the grid's accent colour?

Since v33 themes are objects built with the Theming API. The CSS-file and class-name approach is legacy.

**Sources**

- [AG Grid React Quick Start (v36.1)](https://www.ag-grid.com/react-data-grid/getting-started/)
- [AG Grid React tutorial and space-missions dataset](https://www.ag-grid.com/react-data-grid/deep-dive/)
- [Upgrading to AG Grid 36](https://www.ag-grid.com/react-data-grid/upgrading-to-ag-grid-36/)
- [AG Grid modules](https://www.ag-grid.com/react-data-grid/modules/)
- [Next.js Learn: Dashboard App](https://nextjs.org/learn/dashboard-app/getting-started) (course format)

Code verified to type-check against ag-grid-react 36.1.0, React 19 and TypeScript 5.8. Live previews load AG Grid Community 36.1.0 from jsDelivr and use made-up sample data.