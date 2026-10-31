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
