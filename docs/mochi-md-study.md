# Study: Mochi.md (mochimd.app), and what it means for an iPhone/iPad reader

Studied 31 Oct 2026 from the homepage, the support and privacy pages, the stylesheet and all 5 product screenshots.

## What it is
- "AI markdown reader and editor for **Mac**." It's Mac-only (macOS 13+), with **no iPhone or iPad version**.
- Made by one indie developer (Barnaby Carter). $9.99 launch price, then $14.99, paid once. No account, and the app has no network code.
- Positioning: a nice place to read what your AI agents write (plans, specs, CLAUDE.md, AGENTS.md, task lists, READMEs), plus Obsidian vaults.

## Feature inventory
### Library / navigation
- Open a single file, or a **whole folder**, left where it is (repos, docs folders, Obsidian vaults). It never moves, renames or imports files.
- Sidebar with 3 tabs: **Files · Outline · Search**.
- File tree, **pins**, **recents**, and an **Inbox** of files changed since you last opened them (with a "Clear" button and a count, e.g. "Inbox · 3").
- Sidebar footer shows "12 files · pantry-app". On a document it shows "129 words · 1 min read".
- **⌘K command palette** for jumping to any file or command (fuzzy search of filenames).
- Full-text search across thousands of files. Folders are scanned in the background.
- Very large files open read-only first so scrolling stays smooth.

### "What changed since you last read it" (the signature feature)
- Keeps a private snapshot of each file as last read. Snapshots skip files over 2 MB and are capped at 200 MB in total.
- When a file changes, a banner reads "Changed since you last read it · **+5 −11 lines**", with **Show Changes** and **Mark as Read** buttons.
- Show Changes highlights the added and edited blocks, new words and removed text **in the rendered page** (not a raw diff).
- Live reload while an agent is still writing, without losing your scroll position.

### Rendering
- CommonMark/GFM: tables, footnotes, nested lists, strikethrough, inline code shown as tinted "pills".
- **Task lists you can tick while reading**, and the tick is saved to the file.
- **Callouts** (Note / Tip / Heads up, Obsidian-style `> [!note]`), each with its own colour.
- **Frontmatter** shown as a "METADATA" card: key/value rows, tags shown as chips, and a "copy json" button.
- Code blocks have a language label, a "copy" button and highlighting for dozens of languages.
- **Mermaid** diagrams with a "copy source" button, styled to match the theme.
- Math, and local images. Everything works offline, and images on the web are never fetched.
- Headings get a small coloured dot after them in the "cute" themes, and a marker-style underline on the H1.

### Editing
- **⌘E** turns the page you're reading into an editable page in place. It's a "live preview" editor: Markdown syntax (`#`, `- [x]`, `**`) appears only around the cursor, rendered faintly in the margin.
- An **Aa** menu for typography, and a **Done** button to leave editing.
- Autosaves in under a second. Saves are atomic, and the file's encoding and line endings are kept. It never rewrites parts of the file you didn't touch.

### Themes
- **10 themes: 4 calm, 6 cute.** The calm ones are white, sepia/serif, charcoal and navy. The cute ones are pink, peach, lavender, matcha, lemon and plum. Every theme works in light and dark.
- The website's tokens give a sense of the palette:
  - Pink: bg `#FFF5F4`, accent `#BB2649`, tint `#F7CAC9`
  - Plum dark: bg `#1C1626`, accent `#F2A7D8`
- Fonts: SF Pro Text, plus SF Pro Rounded for the cute themes and SF Mono for code. A serif is used for the sepia theme.
- A comfortable line length (about 60–70 characters), generous spacing, and the content shown as a card.

### Personality
- **Mascot**: a small daifuku in the bottom-right corner. It changes colour with the theme, blinks, bounces when a file opens and dozes when idle. Click it to hide it.

### Mac-specific features
- Quick Look previews in Finder, window tabs, ⌘F find, print, Export as PDF, default-app setting.
- Shortcuts: ⌘O, ⇧⌘O, ⌘E, ⌘K, ⌥⌘T (themes), ⌥⌘D (toggle changes), ⌘F/⌘G, ⌘P.

### Privacy and architecture clues
- Uses **Apple's web view (WKWebView)** to render, with the renderer, fonts, Mermaid and math libraries bundled in the app. The page is locked down with no network access.
- Remembers sandboxed folder access with security-scoped bookmarks.

## Translating it to iPhone and iPad

| Mochi.md feature | iOS/iPadOS equivalent | Difficulty |
|---|---|---|
| Renderer in a web view | Same approach: a WKWebView with markdown-it, Shiki, KaTeX and Mermaid bundled | Easy–Med |
| Open folder in place | Pick the folder once with the document picker, then remember it with a security-scoped bookmark. Works with iCloud Drive and the Obsidian iCloud vault folder | Medium |
| Live reload and changes inbox | Watch iCloud folders with NSFilePresenter / NSMetadataQuery. Reliable while the app is open; files rescan on launch | Medium |
| Changes since last read | Snapshot each file, diff it by block and by word, and highlight the differences in the rendered page | Medium |
| Tickable tasks, frontmatter card, callouts | Pure renderer work | Easy |
| ⌘K palette and search | Same, and it also works with an iPad keyboard. On iPhone, a search sheet | Medium |
| Live-preview editing | CodeMirror 6 inside the web view (the same technique Obsidian uses) | Hardest |
| Quick Look | iOS has Quick Look preview extensions, plus an "Open in…" option in the share sheet | Medium |
| Themes and mascot | CSS themes. **We design our own mascot and palettes rather than copying theirs.** | Easy–Med |
| Layout | iPad: sidebar and reader split view. iPhone: stacked navigation | Medium |

## Not copied
Their name, mascot artwork, copy and exact theme set are theirs. Our app should take the *ideas* (an agent-output reader, changes since last read, calm and cute themes, a companion character) and give them its own name, character and look.

---

# Handoff: decisions made with the user (read this first)

**Goal:** a native iOS/iPadOS Markdown reader for the user's **iPhone Duo** and **iPad**. It's for reading Obsidian notes and the `.md` files AI tools produce, and should be as beautiful and convenient as Mochi.md (which is Mac-only).

## Settled decisions
- **Native SwiftUI app**, not a web app or PWA. One universal target for iPhone and iPad.
- The user **has a Mac and a paid Apple Developer account**. Build, sign and install with Xcode; TestFlight is fine for getting it onto both devices.
- **No file watching or live reload needed.** It just opens .md files well. Mochi.md's "changes since last read" is out of scope for v1.
- **New GitHub repo** for the app (working name `md-reader`; confirm the name with the user). Don't build it inside `clawdbot-dashboard`.
- **Don't copy Mochi.md's branding**: name, mascot art, website copy and exact themes. Reuse the ideas with our own look.

## Target devices
- **iPhone Duo**: Apple's first foldable (released 23 Oct 2026, running iOS 27). Folded, the outer screen is phone-sized; unfolded, the inner screen is about 50% larger than an iPhone 18 Pro Max (close to iPad size). Use a `NavigationSplitView` that collapses to stacked navigation when folded and shows sidebar + reader when unfolded. Test both size classes, and the switch between them while the app is open (folding or unfolding mid-read must keep the scroll position).
- **iPad**: sidebar + reader, keyboard shortcuts (⌘K quick open, ⌘F find), Stage Manager / multiple windows.

## v1 scope
1. **Opening files:** register the `.md`/`.markdown` document types so the app appears in the Files app's "Open with…" and the share sheet. Also a document picker for single files.
2. **Library:** recent files and pinned files. "Open Folder" picks a folder (e.g. an Obsidian vault in iCloud Drive), remembered with a security-scoped bookmark, then a file tree and full-text search across it.
3. **Reader:** a WKWebView with a bundled, offline renderer (markdown-it + plugins, Shiki or highlight.js, KaTeX, Mermaid). Nothing is fetched from the network.
   - GFM: tables, footnotes, strikethrough, task lists. Ticking a checkbox writes back to the file.
   - Obsidian: `> [!type]` callouts, `[[wikilinks]]` (navigate within the opened folder), `![[embeds]]` for images, `==highlights==`, `#tags`.
   - Frontmatter shown as a metadata card (tags as chips, with a copy-as-JSON button).
   - Code blocks with a language label and a copy button. Mermaid diagrams with a copy-source button. Math.
   - Outline of headings (sidebar or sheet), word count and reading time, remembered scroll position per file.
4. **Themes:** our own set, a few calm and a few cute pastel ones, each with light and dark versions. Plus a typography menu (font: system / rounded / serif; text size; line width). Comfortable line length of about 60–70 characters.

## Later, only if the user asks
Edit-in-place (CodeMirror 6 live preview), a companion mascot, changes since last read, a Quick Look preview extension, export to PDF.
