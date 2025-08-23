# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is "Pater Imon Practice" - a Greek language learning web application that helps users learn to pronounce the Lord's Prayer in modern Greek. The app synchronizes audio playback with text highlighting and provides pronunciation guides.

## Development Commands

```bash
# Start development server
npm run dev

# Build for production
npm run build

# Preview production build
npm run preview
```

## Architecture

### Core Structure
- **Svelte + Vite**: Modern build tooling with ESM modules (`"type": "module"` in package.json)
- **Single Page Application**: Entry point at `src/main.js` mounting `App.svelte` to document.body
- **Component Hierarchy**: `App.svelte` → `Header.svelte` + `LanguagePlayer.svelte`

### Data Flow
- **Static Data**: Greek text with pronunciation and timing stored in `src/data.js`
- **Audio Synchronization**: Each word has `startTime` and `length` properties for precise audio timing
- **Real-time Highlighting**: Uses Svelte's reactivity to highlight words based on audio `currentTime`

### Key Components

**LanguagePlayer.svelte**: Core interactive component handling:
- Audio playback controls with custom play/pause buttons
- Speed adjustment (0.75x "Slow", 1.6x "Faster")
- Word-level click-to-seek functionality
- Toggle for pronunciation hints
- Real-time word highlighting during playback

**Header.svelte**: Static content with app title and description

### Styling Approach
- **Font Loading**: External fonts from Glitch CDN (GFS Didot for Greek, Lato for headers, OpenSans for UI)
- **Responsive Design**: Mobile-specific breakpoints for portrait orientation
- **CSS Grid/Flexbox**: Word layout uses CSS Grid for pronunciation alignment below Greek text

### External Dependencies
- Audio file hosted on Glitch CDN
- Custom fonts from Glitch CDN
- GoatCounter analytics integration

## Development Notes

- The app uses `import.meta.hot` for Vite HMR in `src/hmr.js`
- Audio timing data is precisely mapped to word boundaries for smooth highlighting
- Speed control directly manipulates `HTMLAudioElement.playbackRate`
- Click-to-seek sets both `currentTime` and unpauses audio for immediate playback