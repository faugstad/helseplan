# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**Helseplan 2025** is a Norwegian personal health tracking PWA (Progressive Web App) for tracking supplements, mobility exercises, training sessions, and alcohol-free habits.

**Language**: Norwegian (UI text, comments, variable names)

## Technology Stack

- **React 18** (loaded via CDN from unpkg.com)
- **Babel Standalone** (for JSX transformation in browser)
- **Vanilla CSS** (inline styles and style tag, no CSS frameworks)
- **LocalStorage** (primary data persistence)
- **Service Worker** (offline support via sw.js)
- **JSONBin.io API** (optional cloud backup/sync)

## Development Workflow

### No Build Process

This project has **no build tools, no npm, no package.json**. All code lives in a single HTML file.

**To develop:**
1. Open `index.html` directly in a browser
2. Make changes to the file
3. Refresh the browser to see changes

**To test locally:**
```bash
# Simple HTTP server (Python 3)
python -m http.server 8000

# Or use any static file server
```

**To deploy:**
- Push to GitHub and enable GitHub Pages (deploys from main branch root)
- Or upload files to any static hosting service

### Service Worker Development

When modifying [sw.js](sw.js):
1. Update the `CACHE_NAME` version (e.g., `'helseplan-v2'`)
2. Add any new files to the `urlsToCache` array
3. Test in browser DevTools → Application → Service Workers
4. Use "Update on reload" during development to avoid caching issues

## Architecture

### Single Component Structure

The entire app is a single React component: `HealthDashboard` (starts at [index.html:312](index.html#L312))

**State management:**
- All state lives in the `HealthDashboard` component using React hooks
- No Redux, Context API, or other state management libraries
- Data structure: `{ weeks: { 'YYYY-Wxx': { ... } }, startDate: ISO string }`

**Data persistence:**
- Primary: `localStorage` key `'healthDashboardHistory'`
- Optional: Cloud backup via JSONBin.io (user can connect devices)

### Week-Based Data Structure

All health data is organized by ISO week keys (`'YYYY-Wxx'`):

```javascript
{
  weeks: {
    '2025-W01': {
      sundayAlcoholFree: boolean,
      exerciseSessions: { session1: boolean, session2: boolean },
      trainingSessions: { trening1: boolean, trening2: boolean },
      bidetRoutine: boolean,
      exerciseChecks: { bretzel: boolean, hipflexor: boolean, ... },
      dailySupplements: {
        mon: { supplements: { kefir: boolean, ... }, completed: boolean },
        tue: { ... },
        // ... for all 7 days
      }
    },
    '2025-W02': { ... }
  },
  startDate: '2025-01-01T00:00:00.000Z'
}
```

**Current week**: Computed using `getWeekNumber()` and `getWeekKey()` helper functions

### Tab Navigation

Five main tabs (bottom navigation):
1. **Oversikt** - Weekly overview with quick actions
2. **Tilskudd** - Daily supplement tracking (morning/lunch/evening)
3. **Trening** - Training sessions (2x per week goal)
4. **Alkohol** - Alcohol-free Sunday tracking
5. **Historikk** - Historical stats, graphs, achievements, and cloud backup

### Supplement System

Supplements are categorized by time of day:
- **Morning**: kefir, probiotic, d3k2, tran
- **Lunch**: collagen, kreatin
- **Evening**: magnesium, glycine, theanine

Users can add custom supplements via the supplement editor (accessed via ⚙️ button in Tilskudd tab).

Custom supplements stored in `localStorage` key `'healthplan_supplement_config'`:
```javascript
{
  kefir: 'morning',      // period assignment
  customSupp1: 'lunch',  // user-added
  // ...
}
```

And metadata in `'healthplan_supplement_names'`:
```javascript
{
  kefir: { icon: '🥛', name: 'Kefir' },
  customSupp1: { icon: '💊', name: 'Omega-3' },
  // ...
}
```

## Key Code Locations

- **Main component**: [index.html:312](index.html#L312) - `function HealthDashboard()`
- **State initialization**: [index.html:348-369](index.html#L348-L369) - `getInitialState()`
- **Week creation**: [index.html:329-344](index.html#L329-L344) - `createFreshWeek()`
- **LocalStorage persistence**: [index.html:492-498](index.html#L492-L498) - `useEffect` hook
- **Cloud backup**: [index.html:411-483](index.html#L411-L483) - `backupToCloud()` and `restoreFromCloud()`
- **Service worker**: [sw.js:1-47](sw.js)
- **PWA manifest**: [manifest.json](manifest.json)

## Common Modifications

### Adding a New Supplement to Defaults

Edit the `defaultSupplements` object ([index.html:321-325](index.html#L321-L325)) and add to a period array (morningSupps/lunchSupps/eveningSupps defined around [index.html:800](index.html#L800)).

### Adding a New Tab

1. Add tab to state: `const [activeTab, setActiveTab] = useState('oversikt')`
2. Add navigation button in bottom nav array ([index.html:2738-2744](index.html#L2738-L2744))
3. Add tab content in main section with conditional: `{activeTab === 'newtab' && <div>...</div>}`
4. Update header title logic ([index.html:1125-1131](index.html#L1125-L1131))

### Modifying Data Structure

When changing the week data structure:
1. Update `createFreshWeek()` function
2. Update `defaultDayData` if changing daily supplements structure
3. Consider migration logic in `getInitialState()` for existing users
4. Test with both new and existing localStorage data

### Styling Changes

All styles are in the `<style>` tag ([index.html:33-289](index.html#L33-L289)). Key classes:
- `.card` - Main card containers
- `.check-btn` - Checkbox-style buttons
- `.bottom-nav` - Fixed bottom navigation
- `.timer-btn` - Timer control buttons
- Mobile-optimized with safe-area-inset for notched phones

## Testing Considerations

### Manual Testing Checklist

- Test on both desktop and mobile viewports
- Test PWA installation (iOS Safari, Android Chrome)
- Test offline functionality (disable network in DevTools)
- Test cloud backup/restore (sync between "devices")
- Test week transitions (localStorage persistence)
- Test with Norwegian date formats and week numbers

### LocalStorage Testing

To reset app state:
```javascript
// In browser console
localStorage.removeItem('healthDashboardHistory');
localStorage.removeItem('helseplan_bin_id');
localStorage.removeItem('healthplan_supplement_config');
localStorage.removeItem('healthplan_supplement_names');
location.reload();
```

## Important Notes

- **No TypeScript**: This is vanilla JavaScript with JSX
- **No linting**: No ESLint, Prettier, or other code quality tools configured
- **No tests**: No test framework or test files
- **Inline everything**: All JavaScript, CSS, and React code in one HTML file
- **CDN dependencies**: React loaded from unpkg.com (requires internet for initial load)
- **API key exposed**: JSONBin.io API key is visible in code ([index.html:381](index.html#L381)) - this is intentional for client-side use
- **Norwegian language**: All UI text, variable names (some), and comments are in Norwegian
