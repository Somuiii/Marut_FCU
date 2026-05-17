# DESIGN.md — Marut Web Presence & Documentation UI

## Philosophy: Industrial Utilitarian

Marut is embedded firmware for a real machine that flies. The design language should reflect that — **nothing decorative that doesn't serve a function**. Every visual decision earns its place the same way every byte of firmware does.

The guiding philosophy is **Industrial Utilitarian**: raw materials, tight tolerances, zero ornamentation. Think aerospace technical manuals, oscilloscope UIs, and avionics displays — not consumer apps.

---

## Core Principles

### 1. Density over whitespace
Embedded developers work in information-dense environments (IDEs, datasheets, terminal output). The UI should respect that context. Compact layouts, tight line-height, and efficient use of horizontal space are preferred over airy "landing page" padding.

### 2. Monochrome-first, accent with intent
The primary palette is near-black, off-white, and mid-grey. A single accent color — a sharp amber or signal green — is used exclusively for interactive states, warnings, and status indicators. Color is never decorative.

### 3. Monospace as a first-class citizen
Code, register values, addresses, build flags, file paths, and version strings should render in a fixed-width font (`JetBrains Mono`, `Iosevka`, or `Berkeley Mono`). Prose uses a utilitarian geometric sans (`DM Sans`, `IBM Plex Sans`) — nothing editorial, nothing playful.

### 4. State legibility over visual delight
Every component must communicate its state clearly at a glance: build passing/failing, target connected/disconnected, PR open/merged, documentation up to date/stale. Status comes first; animation is never used to mask loading or failure.

### 5. No dark-mode toggle — dark is the default
Firmware developers work in dark IDEs. The default theme is dark. A high-contrast light override may be provided for documentation print/export but should not be the primary experience.

---

## Color System

| Token | Value | Usage |
|---|---|---|
| `--bg-base` | `#0d0d0e` | Page background |
| `--bg-surface` | `#161618` | Card / panel background |
| `--bg-raised` | `#1e1e21` | Hover state / elevated element |
| `--border` | `#2a2a2e` | Dividers, outlines |
| `--text-primary` | `#e8e8ea` | Body copy |
| `--text-secondary` | `#7a7a82` | Labels, metadata |
| `--text-muted` | `#45454d` | Disabled, placeholder |
| `--accent` | `#f5a623` | Amber — interactive, status-active |
| `--danger` | `#e05252` | Build failure, critical warning |
| `--success` | `#4caf7d` | Build passing, target connected |
| `--code-bg` | `#111113` | Inline and block code background |

Gradients are forbidden except inside hardware target diagrams or block schematics.

---

## Typography

```
Display / headings:    IBM Plex Mono (weight 600)
Body prose:            DM Sans (weight 400 / 500)
Code / values:         JetBrains Mono (weight 400)
```

- Headings use monospace to reinforce the firmware character of the project.
- Body text uses a clean geometric sans for sustained reading comfort.
- All numeric values, register names, memory addresses, baud rates, and version strings **must** render in `JetBrains Mono`.
- Line length for prose is capped at **72 characters** — the same discipline applied to commit messages.

---

## Layout

- **12-column grid**, 1280px max-width, 24px gutter.
- Navigation is a persistent **left sidebar** (not a top bar) — consistent with tool-heavy developer environments.
- Documentation pages follow a three-pane layout: sidebar nav | content | in-page TOC.
- No hero sections, no illustration panels, no marketing copy patterns.
- Horizontal rules and thin 1px borders (`--border`) are the primary visual dividers — no shadow cards.

---

## Component Conventions

### Code blocks
- Dark background (`--code-bg`), 1px border in `--border`.
- Line numbers displayed by default.
- Copy-to-clipboard on hover only — no persistent icon clutter.
- Language tag shown in top-right as `--text-muted` label.

### Status badges
- Pill-shaped, 2px border radius, text in monospace.
- Three states only: `PASS` (green), `FAIL` (red), `PENDING` (amber).
- Never use icons as the sole indicator — always pair with text.

### Tables
- Full-width, no outer border, zebra-striped with `--bg-raised`.
- Header row in `--text-secondary`, uppercase, `0.75rem`.
- Used extensively for register maps, target comparisons, and changelog entries.

### Navigation
- Active item: left border in `--accent` (3px), text in `--text-primary`.
- Inactive: `--text-secondary`, no decoration.
- Hover: background shifts to `--bg-raised` only — no underline, no color pop.

---

## Motion

Motion is minimal and purposeful:

- **Page transitions**: none. Instant navigation — matching IDE behavior.
- **Expand/collapse** (e.g., sidebar sections): `height` transition, 120ms ease-in.
- **Toast / alert entry**: translate Y -8px → 0, opacity 0 → 1, 150ms.
- **Hover state transitions**: 80ms max. No easing drama.

No scroll animations. No parallax. No entrance effects on content.

---

## Iconography

Use **Phosphor Icons** (regular weight) or equivalents from a consistent single set. Icons supplement text labels — they never replace them in navigation or status indicators. Size at 16px inline, 20px standalone.

Avoid filled icons; use outline variants to match the lean, technical aesthetic.

---

## Documentation Pages

Documentation is the primary surface for this project. It must:

- Render Markdown faithfully without creative reinterpretation.
- Preserve code block formatting exactly.
- Support deep-linking to every heading (auto-generated anchor IDs).
- Print cleanly to PDF with a light-mode stylesheet (`@media print`).
- Include a "last updated" timestamp and Git commit SHA in the footer.

Avoid sidebars that collapse on mobile in unexpected ways — the left nav should convert to a hamburger menu only below 768px, and must not obscure content when open.

---

## What This Design Is Not

| Avoid | Reason |
|---|---|
| Gradient hero banners | This is not a SaaS product landing page |
| Illustration / mascot | Introduces personality inconsistent with a technical tool |
| Rounded cards with drop shadows | Too soft; undermines the industrial register |
| Animated counters / scroll reveals | Decorative, adds no information |
| Purple / teal gradient schemes | Clichéd developer-tool aesthetic |
| Sans-serif headings in light weight | Lacks the density and authority the project requires |

---

## Rationale

Marut is a flight controller. It operates in a domain where readability is correctness — a misread register value or an unclear build status can mean a crashed vehicle. The design must match that standard of clarity. Form follows function; aesthetics emerge from consistency and restraint, not from decoration.

The industrial utilitarian philosophy is not a constraint — it is the identity.
