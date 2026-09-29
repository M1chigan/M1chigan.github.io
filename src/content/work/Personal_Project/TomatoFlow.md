---
title: TomatoFlow — Hybrid Productivity & Structured Note-Taking Environment
publishDate: 2026-09-28
img: /assets/tomatoflow-architecture.png
img_alt: TomatoFlow Software Architecture Diagram
description: |
  Technical specifications and software architecture of TomatoFlow: a lightweight desktop application (Tauri / Rust) coupled with a reactive, framework-free frontend engine (Vanilla JavaScript). It features real-time Mermaid.js integration and custom DOM mutations for optimal performance.
tags:
  - Tauri
  - Rust
  - Vanilla JS
  - DOM Manipulation
  - Mermaid.js
---

## Technical Overview & Stack Justification

TomatoFlow eliminates the memory overhead typical of Electron containers by leveraging native OS webviews and direct DOM manipulation, ensuring response times under 16 ms (constant 60 fps).

- **Backend & Shell (Tauri / Rust):** Provides a compact native binary (< 15 MB) with minimal memory consumption (~35 MB RAM idle), secure system I/O access, and IPC isolation.
- **Frontend Runtime (Vanilla JS ES6+):** Deliberately avoids frameworks like React or Vue to bypass Virtual DOM reconciliation costs during intensive typing operations.
- **Vector Rendering (Mermaid.js):** Enables dynamic compilation of syntax graphs into SVG via a non-blocking asynchronous pipeline.
- **Persistence (Web Storage API):** Utilizes a synchronous caching mechanism structured in a deterministic namespace per session.
- **Styling Engine (CSS3 Grid & Flexbox):** Relies on hardware geometrical calculations and CSS variables for theming, with zero third-party CSS dependencies.

## Software Architecture & Core Components

The architecture is divided into specialized subsystems:

- **Session Engine:** Manages timers, Pomodoro cycles, and state clocks.
- **Editor Subsystem:** Handles dynamic DOM initialization (Line Bootstrapper), real-time numbering (Synchronized Virtual Gutter), and DOM tree mutations via Keyboard Interceptors (Tab, Enter, Backspace).
- **Diagram Runtime:** Integrates the Mermaid headless engine for syntax parsing, an SVG Sandbox Container (`contenteditable="false"`), and a Re-entrant Error Boundary for compilation fallbacks.
- **Storage Layer:** Manages serialization, sanitization, and atomic key handling.

## Implementation Challenges & Technical Deep Dive

### The Hybrid Editor: Taming `contenteditable`

Using a `contenteditable="true"` area offers nesting flexibility (text, checkboxes, SVG blocks) but poses major DOM consistency challenges.

1.  **Dynamic Line Bootstrapping:** To solve the issue of empty `contenteditable` elements lacking structural nodes, a predictive calculation algorithm compares the element's height with the strict line height. The editor automatically injects `<div><br></div>` tags scaled to the window, allowing users to click anywhere vertically without forcing manual paragraph creation.
2.  **Synchronized Gutter (Line Numbering):** The `updateLineNumbers()` function simultaneously compares the number of physical blocks, textual line breaks, and minimal available height. A contextual safety lock (`isNotepadEmpty`) verifies the absence of rich tags before any purge or automatic reset to prevent layout thrashing.
3.  **Surgical Keyboard Mutation Interception:** Critical interactions bypass native browser behavior via the `keydown` listener and the Selection/Range API. For example, `Tab` injects four non-breaking spaces at the cursor offset, while `Ctrl + B` atomically instantiates a hybrid task component. Contextual `Enter` and `Backspace` extract subsequent text via `Range.extractContents()`, ensuring smooth checkbox replication or conversion to a standard paragraph.

### Diagram Engine: Sandboxed Mermaid.js Integration

Integrating a heavy syntax compiler into a continuously editable text stream requires strict isolation.

- **Structural Isolation:** Each Mermaid block is encapsulated in a rigid container marked `contenteditable="false"` to prevent the native cursor from corrupting or accidentally deleting the internal SVG markup.
- **Non-Deterministic Unique Identifiers:** To avoid SVG node collisions in the global DOM, each render assigns a pseudo-random hash to the target container.
- **Retractable Double Panel:** The diagram source code is kept in a retractable `.mermaid-editor-panel`, synchronizing its raw value with its textual content to ensure perfect fidelity during saves.

### Data Persistence & Session Determinism

Session management relies on strict local storage partitioning. Each work session resolves its memory space via a normalized deterministic key calculation. Backups are atomic: the entire DOM of the `#notepad` container (including checkbox states, injected SVGs, and inline styles) is persisted as a serialized raw string. During session switching, a tolerant hydration process reinjects the saved HTML and immediately triggers a reconciliation cycle (`renderAllMermaidBlocks()`) to reattach listeners and verify vector integrity.

## Performance Benchmarking

Compared to a classic Electron + React architecture, TomatoFlow (Tauri + Vanilla) achieves significant performance gains:

- **Installation Binary Weight:** < 12 MB (vs. ~85 MB to 130 MB).
- **RAM Footprint (1 active session):** ~38 MB (vs. 180 MB - 350 MB).
- **Cold Boot Time:** < 250 ms (vs. ~1 800 ms).
- **DOM Intermediary:** Direct Range DOM mutation (vs. Full Virtual DOM Diffing).

## Technical Roadmap

Future development focuses on:

- **AST Compilation:** Decoupling HTML serialization to an Abstract Syntax Tree (Markdown AST) for better interoperability.
- **Unified Export:** Headless compilation to PDF and pure Markdown with automatic extraction of Mermaid schemas as vectorized PNGs.
- **SQLite Storage Engine (Rust Backend):** Migrating from `localStorage` to a local database file managed by the Tauri runtime to support large notes without browser storage quota limitations.
