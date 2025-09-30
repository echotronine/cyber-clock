# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a cyberpunk-themed clock application built with SvelteKit 2.x, Svelte 5, and Tailwind CSS 4. The app displays the current time with a futuristic neon aesthetic featuring glitch effects, animated scanlines, and progress bars for hours, minutes, and seconds.

## Technology Stack

- **Framework**: SvelteKit 2.x with Svelte 5 (using new runes API: `$props()`, `$:` reactive statements)
- **Styling**: Tailwind CSS 4 with Typography plugin
- **Language**: TypeScript with strict mode enabled
- **Package Manager**: pnpm (note: `onlyBuiltDependencies: ["esbuild"]` in package.json)
- **Deployment**: Vercel (via @sveltejs/adapter-vercel)

## Development Commands

```bash
# Install dependencies
pnpm install

# Start development server
pnpm run dev

# Build for production
pnpm run build

# Preview production build
pnpm run preview

# Type checking
pnpm run check

# Type checking with watch mode
pnpm run check:watch
```

## Project Structure

```
src/
├── routes/
│   ├── +layout.svelte    # Root layout with global CSS import and favicon
│   └── +page.svelte      # Main clock component with all visual effects
├── lib/
│   ├── assets/           # Static assets like favicon
│   └── index.ts          # Library exports
└── app.d.ts             # TypeScript declarations
```

## Architecture Notes

### Main Clock Component (`src/routes/+page.svelte`)

The entire clock application is a single-page component with several key features:

- **Time Management**: Uses Svelte's `onMount` and `onDestroy` lifecycle hooks to manage a 1-second interval timer
- **Reactive State**: Leverages Svelte 5's reactive statements (`$:`) for computed values like formatted time strings and progress percentages
- **Visual Effects**:
  - Grid background with pulsing animation
  - Scanline animation that moves vertically
  - SVG-based noise overlay
  - Random glitch effect (5% chance per second)
  - Custom CSS with multiple neon glow effects (cyan, pink, purple)
  - Corner decorations and progress bars

### Styling Approach

- Tailwind CSS classes are used extensively for layout and basic styling
- Custom CSS in component `<style>` tags handles animations and special effects
- Color scheme: primarily cyan (#00ffff), pink (#ff00ff), and purple for cyberpunk aesthetic
- Responsive: includes mobile breakpoint at 768px

### Configuration

- **SvelteKit**: Uses Vite preprocessor and Vercel adapter
- **Vite**: Configured with Tailwind CSS and SvelteKit plugins
- **TypeScript**: Strict mode enabled with bundler module resolution
- Path aliases: `$lib` resolves to `src/lib` (SvelteKit convention)

## Localization

The app currently uses Chinese locale for date formatting (`zh-CN`) in `+page.svelte:32`. The day of week uses English abbreviations (SUN, MON, etc.).