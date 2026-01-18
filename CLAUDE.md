# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Pendo Demosite - A React CRM demo application for showcasing Pendo's analytics and user engagement platform. It simulates a sales CRM with accounts, contacts, opportunities, and dashboard views.

## Commands

```bash
# Development (React dev server at http://localhost:3000)
npm run start:dev

# Production (Express server serving built app)
npm run start

# Build
npm run build

# Tests
npm run test

# Run browser automation scripts
npm run automateusage
npm run automateusage:a4p
```

**Note:** Requires Node 16.13.0 (`nvm use 16.13.0`) and Yarn 1.22.17.

## Architecture

### Frontend (React + Redux)

**Entry Points:**
- `src/index.js` - Redux store setup, RestDBAxios client creation
- `src/main.js` - Route definitions (/, /accounts, /contacts, /opportunities, /mobile, /*/*/details)
- `src/App.js` - Root component with Pendo keyboard shortcuts

**Redux Pattern:**
- `src/reducers/` - Combined reducers: Navigation, Timeline, Contact, Opportunities, Account, DetailsInformation
- `src/actions/` - Async actions using redux-thunk (REQUEST → RECEIVE pattern)
- `src/containers/` - Redux-connected components (mapStateToProps/mapDispatchToProps)
- `src/components/` - Presentational components

**API Client:**
```javascript
import { RestDBAxios } from './index';
RestDBAxios.get('/endpoint')  // Uses RestDB.io with CORS-enabled API key
```

### Backend (Express)

- `server/index.js` - Serves built React app with SSL enforcement
- `server/visitorApi/router.js` - Generates dynamic visitor metadata (names, accounts, roles, teams, regions)

### Pendo Integration

Pendo is initialized in `public/index.html`. Use globally via:
```javascript
window.pendo.track('Event Name', { property: 'value' });
window.pendo.showGuideById('guide-id');
window.pendo.flushNow();  // Force send events
```

Disable Pendo with URL parameter: `?disablePendo=true`

### Automation

Puppeteer-based scripts in `automation/`:
- `automation/page.js` - Browser launch and page helpers
- `automation/demoPaths/` - Demo scenario scripts
