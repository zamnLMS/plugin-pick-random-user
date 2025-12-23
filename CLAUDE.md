# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

BigBlueButton plugin that allows moderators to randomly pick a user from the meeting participants. Built with React, TypeScript, and the BigBlueButton HTML Plugin SDK.

**SDK Version**: Uses `bigbluebutton-html-plugin-sdk` v0.0.89 (branch v0.0.x corresponds to BBB v3.0.x)

## Commands

```bash
npm ci                  # Install dependencies
npm run build-bundle    # Production build (outputs to dist/)
npm start               # Dev server on port 4701
npm run lint            # ESLint check
npm run lint:fix        # ESLint autofix
```

## Architecture

### Entry Point
- `src/index.tsx` - Mounts the React app, receives `uuid` and `pluginName` from BigBlueButton

### Core Components
- `src/components/pick-random-user/` - Main plugin logic
  - `component.tsx` - Plugin root: initializes SDK, manages state, handles user picking logic
  - `hooks.ts` - Custom hooks for modal control, settings, filter options, notifications
  - `queries.ts` - GraphQL queries for user data
  - `context.ts` - React context for filter options

- `src/components/modal/` - Modal UI for picked user display
  - `presenter-view/` - UI shown to presenter (pick controls, options, previously picked list)
  - `picked-user-view/` - UI shown when a user is picked

- `src/components/extensible-areas/action-button-dropdown/` - BBB action button integration

### Data Channels (defined in manifest.json)
The plugin uses BBB data channels for real-time state sync:
- `pickRandomUser` - Stores currently picked user
- `pickedUserSeenEntry` - Tracks who has seen the picked user notification
- `filterOptions` - Stores presenter's filter settings

### Localization
- `public/locales/` - JSON translation files (en, de, fr-FR, it, ja, pt-BR, ar)
- New locales must be added to `public/locales/index.json`
- Use `react-intl` for i18n

## Configuration

Plugin accepts these settings via BBB (configured in `/etc/bigbluebutton/bbb-html5.yml`):
- `pingSoundEnabled` - Play sound for picked user (default: true)
- `pingSoundUrl` - Sound file URL (default: resources/sounds/doorbell.mp3)
- `browserNotificationEnabled` - Browser notifications (default: false)
- `pickedUserTimeWindow` - Seconds to consider user as recently picked (default: 30)

## Build Output

`npm run build-bundle` produces:
- `dist/PickRandomUserPlugin.js` - UMD bundle
- `dist/manifest.json` - Plugin manifest
- `dist/locales/` - Translation files

## Resources

For more information on developing and improving BBB plugins, see the official documentation:
https://docs.bigbluebutton.org/plugins/
