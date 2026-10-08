# theui-svelte starter

### A SvelteKit project with theui-svelte and Tailwind CSS v4 already wired together, ready to build on.

## What this is

The official starting point for an application built with [theui-svelte](https://github.com/mbparvezme/theui-svelte), TheUI's Svelte 5 component library. Clone it instead of assembling SvelteKit, Tailwind and the library by hand.

Everything the library needs is in place: the stylesheet imports, the Tailwind plugin, runes mode and TypeScript. What is left is your application.

The components are documented at [www.theui.dev](https://www.theui.dev).

## Quick start

```bash
git clone https://github.com/mbparvezme/theui-svelte-starter.git my-app
cd my-app
npm install
npm run dev
```

The clone arrives with the starter's git history. To make it your own project, start a fresh one before your first commit:

```bash
rm -rf .git && git init
```

## Requirements

**Node.js 22.12 or newer.** `.npmrc` sets `engine-strict=true`, so npm refuses to install on an older runtime instead of letting it fail later at build time. If you deploy somewhere that picks its own Node version, pin it there too.

**Svelte 5.57.1 or newer**, which theui-svelte declares as a peer dependency. The versions below already satisfy it.

## What is included

| | |
|---|---|
| theui-svelte | ^3.1.0 |
| SvelteKit | ^3.0.0 |
| Svelte | ^5.57.1 |
| Vite | ^8.3.0 |
| Tailwind CSS | ^4.3.0 |
| TypeScript | ^6.0.3 |
| Adapter | `adapter-auto` |

## How it is wired

**`src/routes/layout.css`** is the stylesheet, and it is the whole setup:

```css
@import 'tailwindcss';
@import 'theui-svelte/style';
```

Two lines, in that order. Tailwind comes first and has to stay: as of 3.1.0 the library's stylesheet no longer imports Tailwind itself, so a stylesheet that drops that line fails the build. The library's stylesheet brings the design tokens, the dark variant and the forms plugin, and registers its own markup for Tailwind to scan — which is why there is no `@source` line to add. Versions before 3.1.0 needed `@source "../node_modules/theui-svelte";` as well; a project that still carries it is redundant, not broken.

**`src/routes/+layout.svelte`** imports that stylesheet and sets the favicon. It is also where the page-level singletons belong, below.

**`vite.config.ts`** holds the Tailwind and SvelteKit plugins, forces runes mode for your own files, and selects the adapter. There is no `svelte.config.js` — that configuration lives here instead.

**`src/lib/`** is your own code, reachable as `#lib` and `#lib/*` through the `imports` map in `package.json`.

## Your first component

Components are named exports, so you import the ones you use and nothing else:

```svelte
<script lang="ts">
  import { Button, Card, notify } from 'theui-svelte'
</script>

<Card title="It works">
  <Button color="brand" onclick={() => notify('Hello from theui-svelte', 'success')}>
    Raise a notification
  </Button>
</Card>
```

If the button is styled and the notification appears, then the components, the stylesheet and the shared state are all working.

## Page-level singletons

`Notification` and `Tooltip` each drive every instance on the page, so render them **once**, in `src/routes/+layout.svelte`. Rendering either more than once duplicates its listeners.

```svelte
<script lang="ts">
  import { Notification, Tooltip } from 'theui-svelte'
  import './layout.css'

  let { children } = $props()
</script>

<Notification position="top-end" />
<Tooltip />

{@render children()}
```

`notify()` does nothing during server rendering, so call it from an event handler, an `$effect` or `onMount`. Tooltips are opt-in per element with `data-tooltip="..."`.

## Dark mode

Dark mode is class-based: the library's `dark:` variant matches `.dark` on an ancestor, and the `DarkMode` component toggles it on `<html>` and remembers the choice in `localStorage`.

Because the class is applied after hydration, a server-rendered page paints light first. To avoid that flash, add a blocking script to the `<head>` of `src/app.html`:

```html
<script>
  try {
    const t = localStorage.getItem('theui-theme')
      ?? (matchMedia('(prefers-color-scheme: dark)').matches ? 'dark' : 'light');
    if (t === 'dark') document.documentElement.classList.add('dark');
  } catch {}
</script>
```

## Your brand colors

Components read their colors from CSS variables, so rebrand by overriding the tokens rather than editing components. In `src/routes/layout.css`, after the imports:

```css
@theme {
  --color-brand-500: oklch(0.62 0.19 260);
  --text-color-on-brand: white;
}
```

The full palette and the surface and text tokens are listed under [Colors and branding](https://www.theui.dev/docs/colors).

## Working with a coding assistant

theui-svelte ships its own rules for AI coding assistants. One command points your tools at them:

```bash
npx theui ai
```

It writes a short block into whichever instruction files your project already uses — `AGENTS.md`, `CLAUDE.md`, Copilot, Cursor, Windsurf, Gemini — each naming the path to the rules inside the package. It points rather than copies, so upgrading the library upgrades what your assistant reads, and it cannot overwrite rules you wrote yourself. See [AI coding assistants](https://www.theui.dev/docs/ai-assistants).

## Scripts

| Command | What it does |
|---|---|
| `npm run dev` | Start the dev server |
| `npm run build` | Build for production |
| `npm run preview` | Serve the production build locally |
| `npm run check` | Type-check with svelte-check |
| `npm run check:watch` | Type-check and watch |

## Deploying

The project uses `adapter-auto`, which detects Vercel, Netlify, Cloudflare and a few others at build time. For anything else — or to stop relying on detection — install the adapter for your target and set it in `vite.config.ts`:

```bash
npm i -D @sveltejs/adapter-node
```

See [SvelteKit adapters](https://svelte.dev/docs/kit/adapters).

## Documentation

- [Component documentation](https://www.theui.dev) — every component, with live examples
- [Installation guide](https://www.theui.dev/docs/installation)
- [theui-svelte on GitHub](https://github.com/mbparvezme/theui-svelte) — source, changelog and issues
- [theui-svelte on npm](https://www.npmjs.com/package/theui-svelte)

## License

MIT, the same as theui-svelte.
