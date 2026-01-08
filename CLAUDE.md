# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

FNDTN Performance Tracker is a Progressive Web App (PWA) for iPhone that integrates recovery monitoring, supplement tracking, workout logging, nutrition analysis, and monthly challenges. The app uses AI (Claude API + Google Vision OCR) for intelligent workout parsing and menu recommendations.

**Live URL:** https://ptonagh.github.io/fndtn-tracker/
**Repository:** https://github.com/ptonagh/fndtn-tracker
**Architecture:** Single-page React app (via CDN), no build process required

## Development Commands

### Deployment
- **Update app:** Edit `index.html` in GitHub web interface, commit changes, wait 2-3 minutes for GitHub Pages deployment
- **Update Cloudflare Worker:** Edit code at Cloudflare dashboard → Workers & Pages → anthropic-proxy → Edit Code → Save and Deploy
- **Test locally:** Open `index.html` directly in browser (file:// protocol works for basic testing)
- **View on device:** Navigate to GitHub Pages URL in Safari, pull down to refresh after updates

### Testing
- **Mobile testing:** Safari on iPhone is primary test environment (PWA features require Safari)
- **Desktop testing:** Chrome/Safari responsive mode at 390x844 (iPhone 14 Pro)
- **Debugging:** Safari Web Inspector → Console for error logs
- **API testing:** Check browser console for API responses and token usage

### Data Management
```javascript
// Clear daily data (keeps history)
localStorage.removeItem('fndtn-tracker-state');
location.reload();

// Clear everything
localStorage.clear();
location.reload();
```

## Architecture

### Technology Stack
- **Frontend:** React 18 (CDN), Vanilla JavaScript/JSX, single HTML file
- **Storage:** localStorage for all data persistence
- **Hosting:** GitHub Pages (static hosting)
- **APIs:**
  - Google Cloud Vision (OCR text extraction from images)
  - Anthropic Claude Sonnet 4 (intelligent parsing, AI recommendations)
  - Cloudflare Worker proxy at https://anthropic-proxy.peter-5aa.workers.dev/
  - Whoop API (ready to integrate, code prepared)

### File Structure
```
index.html              # Main app (all React components, styles, logic in one file)
callback.html           # Whoop OAuth redirect handler (deployed but unused)
manifest.json           # PWA configuration
icon-192.png           # App icon (192x192)
icon-512.png           # App icon (512x512)
README.md              # Repository documentation
SETUP-GUIDE.md         # GitHub Pages setup instructions
```

### Data Flow
```
iPhone App (PWA)
    ↓
localStorage (biomarkers, supplements, workouts, challenges)
    ↓
Cloudflare Worker (CORS proxy)
    ↓
External APIs (Google Vision, Claude, Whoop)
```

### localStorage Schema

**Keys:**
- `fndtn-tracker-state` - Daily state (resets at midnight): supplements, recovery data, challenge progress
- `fndtn-workouts` - Persistent workout history array
- `fndtn-monthly-challenges` - Persistent challenge history array
- `fndtn-meals` - Persistent meal analysis history array
- `google-vision-api-key` - Google Cloud Vision API key
- `anthropic-api-key` - Claude API key
- `whoop-access-token` - Whoop OAuth token (when integrated)
- `whoop-refresh-token` - Whoop refresh token (when integrated)

## Core Features

### 1. Recovery Tab
- **Personal Recovery Formula:** `(sleep * 0.40) + ((hrv / 70) * 48) - (debt * 0.12) + day_adjustment`
  - 65% accuracy validated from 5 years of Whoop data
  - Day adjustments: Sunday +4, Saturday +3, Wed-Fri -2
- **Status System:** GREEN (≥75%), AMBER (60-74%), RED (<60%)
- **AI Advisory Board:** 5 expert personas (Dr. Andy Galpin, Dr. Matthew Walker, Dr. Rhonda Patrick, Dr. Peter Attia, Dr. Leah Lagos) provide personalized recommendations based on biomarkers and genetic profile

### 2. Supplements Tab
- 26 supplements across 7 time slots (5:30 AM to 8:30 PM)
- Tap-to-check interface, progress ring shows completion
- Resets automatically at midnight
- All supplements optimized for user's genetic variants (IL-6 GG, LDL-R CT, diabetes variants, etc.)

### 3. Workout Tab
- Weekly F45 program (PERFORM, BUILD, ADAPT, POWER)
- Image upload → Google Vision OCR → Claude intelligent parsing
- Round-by-round tracking for EMOM/circuit workouts
- Recovery-based intensity warnings
- Exercise logging with reps/weights per round

### 4. Nutrition Tab
- **Meal Analysis:** Upload photo → OCR → AI analysis (macros, genetic alignment, recovery optimization)
- **Menu Recommendations:** Upload restaurant menu → AI selects optimal dish based on genetic profile and macros
- Currently debugging meal analysis output display

### 5. Monthly Challenge Tab
- January 2026: 100 pushups (5x20) + 26 pullups daily
- Tap-to-track sets, monthly statistics
- Historical tracking preserved

## Genetic Optimization

The entire app is optimized for Peter Tonagh's genetic variants. When making recommendations or modifications, consider these high-priority variants:

**Cardiovascular:**
- **LDL-R CT:** Saturated fat sensitivity → <7% sat fat, plant sterols, Red Yeast Rice
- **eNOS3 TT:** Reduced nitric oxide → L-Citrulline 6g pre-workout, astaxanthin

**Inflammation:**
- **IL-6 GG (Critical):** Elevated inflammation risk → Omega-3 2-3g, EVOO, anti-inflammatory foods, astaxanthin

**Metabolic/Diabetes:**
- **TCF7L2, WFS1, SLC30A8:** Increased diabetes risk → Berberine 1500mg daily, low glycemic diet
- **FTO AT, PPARG CC, APOA5 TT:** Fat-sensitive → Total fat 25-30%, high protein per meal

**Vitamins:**
- **CYP24A1 AA:** Requires higher vitamin D → 4000 IU daily (adjusted for AG1)
- **BCMO1 GG/CT:** Reduced beta-carotene conversion → Preformed vitamin A 3000 IU

**Performance:**
- **ACE GG:** Power/strength advantage → Exploit in training
- **APOE E3/E3:** Fasting advantage → 24-hour Thursday fasts

## AI Integration Patterns

### Pattern 1: OCR → Claude Text Parsing (Recommended)
**Used for:** Workouts, meals, menus
**Flow:** Image → Google Vision OCR → editable text field → Claude parses text → structured output
**Advantages:** User can correct OCR errors, reliable, debuggable

### Pattern 2: Claude Vision (Causes Issues)
**Attempted for:** Direct meal image analysis
**Issue:** Large payloads cause React crashes, more expensive, harder to debug
**Recommendation:** Stick with Pattern 1

### Pattern 3: Text-Only Claude Analysis
**Used for:** Advisory Board recommendations
**Flow:** Structured data (biomarkers, genetics) → Claude → personalized advice JSON
**Advantages:** Fast, reliable, cost-effective

## Code Patterns

### React Component Structure
All components are in one `App()` function in `index.html`:
```javascript
function App() {
  // 1. All state at top level (useState)
  // 2. useEffect for side effects (localStorage load, midnight reset)
  // 3. Helper functions (calculateRecovery, saveWorkout, etc.)
  // 4. Render functions for each tab (renderRecovery, renderSupplements, etc.)
  // 5. Main render with tab navigation
  return ( ... );
}
```

### Error Handling Pattern
```javascript
try {
  const response = await fetch(...);
  const data = await response.json();
  if (!response.ok) {
    throw new Error(data.error?.message || 'Request failed');
  }
  // Process successful response
} catch (error) {
  console.error('Operation failed:', error);
  alert('User-friendly error message');
} finally {
  setLoading(false);
}
```

### Workout Parsing (EMOM Logic)
- Detects time ranges like "9-12MIN EMOM" → uses maximum (12)
- Counts total stations (exercises + rest periods)
- Calculates rounds: Time ÷ Stations
- Example: 12MIN EMOM with 3 stations (2 exercises + rest) = 4 rounds

## API Configuration

### Google Cloud Vision API
- **Purpose:** Text extraction from workout/meal/menu images
- **Cost:** First 1,000 images/month FREE
- **Setup:** console.cloud.google.com → Enable Cloud Vision API → Create API key → Restrict to https://ptonagh.github.io/*
- **Storage:** API key stored in app's grey box (Workout tab)

### Anthropic Claude API
- **Purpose:** Intelligent parsing, personalized recommendations
- **Model:** claude-sonnet-4-20250514
- **Cost:** ~$0.01 per analysis (~$1/month estimated)
- **Setup:** console.anthropic.com → Generate API key (starts with sk-ant-api03-)
- **Storage:** API key stored in app's green box (Workout tab)

### Cloudflare Worker
- **URL:** https://anthropic-proxy.peter-5aa.workers.dev/
- **Purpose:** CORS proxy for Claude API, protects API credentials
- **Endpoints:**
  - `POST /` → Claude API proxy (working)
  - `POST /whoop/token` → OAuth token exchange (ready, not deployed)
  - `POST /whoop/data` → Fetch Whoop data (ready, not deployed)
- **Cost:** FREE (100,000 requests/day)
- **Files:** `cloudflare-worker.js` (current), `cloudflare-worker-with-whoop.js` (with Whoop integration ready)

### Whoop API (Ready to Integrate)
- **Credentials:**
  - Client ID: 8bd2a955-6f43-4cf1-90d0-806819258104
  - Client Secret: fbe88f7f3874c77bbcb5f3f167699397ba47e9ae3b884ce2403c11fc184f3eee
  - Redirect URI: https://ptonagh.github.io/fndtn-tracker/callback.html
- **Status:** Code prepared but causes black screen (needs debugging)
- **Files:** `index-with-whoop.txt`, `cloudflare-worker-with-whoop.js`

## Known Issues

### Issue 1: Meal Analysis Not Displaying Results
**Symptom:** Meal photo uploads, shows "analysing", but no output appears
**Root Cause:** Claude prompt says "analyse photo" but receives only OCR text
**Solution:** Update prompt to clarify "Analyse this meal based on OCR text from photo"
**Files:** `index-nutrition-debug.txt` (debugging version), `index-nutrition-fixed.html` (corrected prompt, needs testing)

### Issue 2: Whoop Integration Causes Black Screen
**Symptom:** App loads to black screen after adding Whoop code
**Root Cause:** Unknown - likely React state management or function syntax error
**Status:** Code ready (`index-with-whoop.txt`) but needs debugging
**Testing Approach:** Add features incrementally, test in isolation, use try-catch extensively

### Issue 3: EMOM Round Calculation Edge Cases
**Issue:** Rest periods not always explicitly listed in workout boards
**Solution:** Update Claude prompt to explicitly count and return total stations per block

## Mobile-First Design Considerations

- Use `env(safe-area-inset-*)` for notch/home bar spacing
- `box-sizing: border-box` everywhere to prevent overflow
- `min-width: 0` on grid items to prevent expansion
- Test on actual iPhone, not just browser resize
- PWA features (Add to Home Screen) require Safari on iOS

## Deployment Workflow

### Making Updates
1. **Code Changes:** Edit `index.html` locally or in GitHub web interface
2. **GitHub Update:** Repository → index.html → Edit → Delete all → Paste new version → Commit
3. **Wait:** 2-3 minutes for GitHub Pages deployment
4. **Refresh:** Open app in Safari, pull down to refresh (or clear cache if needed)
5. **Rollback:** Keep previous working version as backup (`index-australian-final.txt` is current stable)

### Version Control
**Working Versions:**
- `index-australian-final.txt` - Current stable deployment (Australian English)
- `index-ai-board-fixed.txt` - Before nutrition tab
- `index-emom-correct.txt` - Before AI advisory board

**Experimental Versions:**
- `index-with-whoop.txt` - Whoop integration (untested, causes black screen)
- `index-nutrition-with-vision.txt` - Claude vision API (causes crash)
- `index-nutrition-debug.txt` - Debugging version for meal analysis

### Troubleshooting Black Screen
**Common Causes:**
1. JavaScript syntax error
2. React state management error (useState in wrong location)
3. localStorage data corruption
4. Missing function definition

**Immediate Fix:** Upload `index-australian-final.txt` (last known working version)

**Debugging:**
1. Safari Developer Tools → Console
2. Look for error messages
3. Check which component is crashing
4. Add features incrementally, not all at once

## Design System

### The Foundation Gym Colour Palette
- **Movement Blue:** #2C5AA0 (buttons, borders, primary actions)
- **Nourishment Green:** #4A7C59 (success states, secondary actions)
- **Community Orange:** #FF6B35 (warnings)
- **Foundation Black:** #0A0A0A (main background)
- **Dark Grey:** #1A1A1A (cards, surfaces)
- **Medium Grey:** #3A3A3A (borders, inputs)

### Status Indicators
- **GREEN:** #4A7C59 (≥75% recovery, full training cleared)
- **AMBER:** #FF9500 (60-74% recovery, modified training)
- **RED:** #FF3B30 (<60% recovery, rest required)

## Important Notes for Development

1. **Single File Architecture:** All code is in `index.html` - no build process, no separate JS files
2. **React via CDN:** Using React 18 and Babel standalone - no npm, no bundler
3. **localStorage Limits:** ~5-10MB total - don't store images in localStorage, only in React state
4. **Australian English:** All text uses Australian spelling (optimise, analyse, centre, etc.)
5. **State Management:** ALL useState calls must be at top level of App() function, never inside render functions
6. **API Keys:** Stored in localStorage, user enters them in the app (grey/green boxes in Workout tab)
7. **Prompt Engineering:** Always specify "Return ONLY valid JSON, no other text" for Claude API calls
8. **Testing Priority:** Always test on actual iPhone Safari before considering a feature complete
9. **Cost Optimization:** Claude API is the only cost (~$1/month) - keep prompts concise but effective
10. **Genetic Context:** Always include user's genetic profile in AI prompts for personalized recommendations

## Future Enhancements Ready to Implement

1. **Whoop Auto-Sync** - Code ready, needs debugging
2. **Fixed Meal Analysis** - Prompt correction ready, needs testing
3. **CGM Integration** - High priority for diabetes risk management
4. **Historical Analytics** - Recovery trends, workout correlations
5. **Supplement Timing Alerts** - Push notifications via service worker

## Support Resources

- **Google Cloud Console:** console.cloud.google.com (Vision API)
- **Anthropic Console:** console.anthropic.com (Claude API usage/billing)
- **Cloudflare Dashboard:** dash.cloudflare.com (Worker logs/deployments)
- **Whoop Developer Portal:** developer.whoop.com
- **GitHub Repository:** https://github.com/ptonagh/fndtn-tracker

## Critical Files Reference

- `/Users/admin/Desktop/FNDTN Tracker/files/Final Summary 545 pm 04012026/FNDTN_Performance_Tracker_Complete_Summary.md` - Comprehensive project documentation (1,553 lines)
- `/Users/admin/Desktop/FNDTN Tracker/files/Final Summary 545 pm 04012026/index-australian-english-WORKING.txt` - Current working version
- `/Users/admin/Desktop/FNDTN Tracker/files/Final Summary 545 pm 04012026/Updated_Supplement_Protocol_January_2026.md` - Genetic-optimized supplement rationale
