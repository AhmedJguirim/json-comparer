# JSON Comparer

A self-contained, single-file JSON diff tool for comparing API responses side by side.

## Usage

Open `index.html` in any browser — no build step, no dependencies.

Paste two JSON objects into the left and right panes, then click **Compare**.

## Features

- **Deep diff** — recursively walks nested objects and arrays
- **Smart array matching** — matches array elements by identity key (`@id`, `id`, `uuid`, `key`, `name`, `code`) so reordered or partially changed arrays show accurate per-element diffs instead of treating the whole array as changed
- **Diffs only mode** — toggle to collapse identical lines and focus on what actually changed
- **Synchronized scrolling** — both panes scroll together
- **Color-coded output**
  - Orange — changed value
  - Red — present in left, missing in right
  - Green — present in right, missing in left

## Path format

Diff paths use dot notation for objects and bracket notation for arrays:

```
hydra:member[id=/api/learners/61408].firstName: "Aline" → "Alice"
hydra:member[id=/api/learners/61408].training[id=59013].category: "auto" → "manual"
```

When no identity key is found, arrays fall back to index-based paths: `items[0].name`.
