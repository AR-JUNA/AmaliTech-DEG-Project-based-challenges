# SecureVault — File Explorer Dashboard

A high-performance, accessible file explorer UI built for SecureVault Inc., a company serving law firms and financial institutions. Designed for navigating deeply nested secure document structures with speed and precision.

**Live Demo:** [https://your-username.github.io/securevault](https://your-username.github.io/securevault) ← _replace after deployment_

**Design File:** [Figma – SecureVault Design System](https://www.figma.com/file/...) ← _replace with your Figma link_

---

## Setup Instructions

This project is a single HTML file with zero build dependencies — no npm, no Webpack, no Node required.

### Run Locally

```bash
git clone https://github.com/AR-JUNA/AmaliTech-DEG-Project-based-challenges.git
cd securevault

# Option 1: Python (any machine)
python3 -m http.server 8080
# Open http://localhost:8080

# Option 2: VS Code Live Server extension
# Right-click index.html → "Open with Live Server"
```

### Deploy to GitHub Pages

```bash
# From your repo settings → Pages → Source: main branch / root
# Your app will be live at: https://your-username.github.io/securevault
```

---

## Features

### Core (Required)

| Story | Feature | Status |
|-------|---------|--------|
| 1 | Recursive folder tree — unlimited depth | ✅ |
| 2 | Expand/collapse folders on click | ✅ |
| 3 | File selection with distinct visual state | ✅ |
| 4 | Properties panel (Name, Type, Size, Modified) | ✅ |
| 5 | Keyboard navigation (↑↓ → ← Enter) | ✅ |
| 6 | Search with auto-expand of parent folders | ✅ (Bonus) |

### Wildcard Feature — Raw File Preview Panel

**What it is:** When a file is selected in the Properties panel, a "Raw Preview" section appears showing a simulated hex/text preview of the file contents based on its type.

**Why it adds business value:** Law firms and banks deal with high-stakes documents where opening the wrong version has real consequences. A raw preview lets power users confirm file type signatures and content hints before downloading or opening the file — reducing errors and supporting audit trails. This is the kind of detail-first UX that enterprise security clients expect, and it differentiates SecureVault from generic file storage tools.

---

## Design System

### Brand Direction

"Cyber-secure, precise, and fast" — the UI uses a dark terminal aesthetic with a blue accent system, monospace type for metadata, and precision-focused density. Every visual choice reinforces that the data inside is controlled and trustworthy.

### Color Palette

| Token | Hex | Usage |
|-------|-----|-------|
| `--bg-base` | `#0b0e14` | Page background |
| `--bg-surface` | `#111520` | Sidebar, panels |
| `--bg-elevated` | `#171c2a` | Cards, inputs |
| `--accent` | `#3b82f6` | Selection, focus, links |
| `--text-primary` | `#e2e8f0` | Main text |
| `--text-secondary` | `#8b9ec7` | Labels, captions |
| `--success` | `#22d3a5` | Status indicators |

### Typography

- **UI / Body:** Inter, Segoe UI — clean, readable at small sizes
- **Metadata / Code:** JetBrains Mono, Fira Code — conveys precision and technical authority
- Scale: 11px (status), 12px (labels), 13px (tree nodes), 14px (body), 15px (logo)

### Spacing Grid

Base unit: 4px. Padding: 6 / 10 / 14 / 16 / 20px. Component gaps: 6 / 8 / 12px.

---

## Recursive Strategy

The file tree data is a **tree structure** — each node is either a `file` (leaf) or a `folder` (node with a `children` array). The render function `renderNodes()` calls itself on each node's `children` array, passing `depth + 1` to calculate the CSS indent offset.

```
renderNodes(nodes, container, depth)
  for each node in nodes:
    create row with indent = depth × 16px
    if node.type === 'folder' && expanded:
      renderNodes(node.children, childContainer, depth + 1)  ← recursion
```

This naturally handles any depth (2 levels or 20) without special cases. State is stored flat using a `Set<id>` for expanded folders and a single `string` for the selected file — no tree-copying or deep cloning needed.

For search, a second recursive pass (`computeMatches`) walks the tree, marks all nodes whose names match, and auto-adds ancestor folder IDs to the `expanded` Set so matching items are always visible.

---

## Tech Stack

- **Vanilla JS (ES6+)** — no frameworks, no build tools
- **Zero external dependencies** — fully self-contained HTML file
- **Custom CSS design system** — all components built from scratch, no UI libraries used

---

## Keyboard Shortcuts

| Key | Action |
|-----|--------|
| `↑` / `↓` | Move focus between visible tree items |
| `→` | Expand focused folder |
| `←` | Collapse focused folder |
| `Enter` | Select focused file / toggle folder |

A keyboard shortcut hint overlay fades in automatically when keyboard navigation is detected.
