# FNDTN Performance Tracker

Complete performance tracking app combining recovery monitoring, supplement tracking, workout logging, and monthly challenges.

## Features

- **Recovery Tab**: Daily biomarker input with Personal Recovery Formula (65% accuracy)
- **Supplements Tab**: Complete daily supplement schedule with 26 supplements across 7 time slots
- **Workout Tab**: F45 program tracking with image upload and recovery-based intensity recommendations
- **Monthly Challenge Tab**: January challenge tracking (100 pushups + 26 pullups daily)
- **Virtual Advisory Board**: 5 expert personas providing personalized recommendations

## Installation on iPhone

### Quick Setup (5 minutes)

1. **Visit the app URL**: https://YOUR-USERNAME.github.io/fndtn-tracker
2. **Open in Safari** (important: must be Safari, not Chrome)
3. **Tap the Share button** (square with arrow pointing up)
4. **Scroll down and tap "Add to Home Screen"**
5. **Tap "Add"**

The app will now appear on your home screen and work like a native app!

## Features

### Recovery Monitoring
- Personal recovery formula integrating sleep performance, HRV, sleep debt
- GREEN/AMBER/RED training clearance system
- Day-of-week adjustments for natural recovery patterns
- Virtual Advisory Board with 5 expert personas

### Supplement Tracking
- 5:30 AM - AG1
- 6:00 AM - Pre-workout stack (L-Citrulline, Creatine, L-Theanine)
- 7:00 AM - Post-workout (Protein, Creatine, Vitamin C)
- 10:00 AM - Breakfast stack (12 supplements)
- 1:00 PM - Lunch (Berberine, Vitamin C)
- 6:00 PM - Dinner (Berberine, Red Yeast Rice, Vitamin A)
- 8:30 PM - Before bed (Ashwagandha, L-Theanine, Magnesium)

### Workout Tracking
- Weekly F45 program (PERFORM, BUILD, ADAPT, POWER)
- Image upload for daily workout variations
- Exercise logging with reps/weights
- Recovery-based intensity warnings
- Training notes field

### Monthly Challenges
- January: 100 pushups (5x20) + 26 pullups daily
- Tap-to-track pushup sets
- Pullup rep entry (up to 4 sets)
- Monthly statistics (perfect days, totals)
- Historical tracking

## Technical Details

- **Storage**: All data stored locally in browser localStorage
- **Offline**: Works completely offline after first load
- **Privacy**: No data sent to servers, everything stays on your device
- **Updates**: Refresh the page to get latest version

## Data Management

### Daily Reset
- Supplement progress resets at midnight
- Recovery and challenge data persist until saved
- Historical workout and challenge data preserved

### Backup
Your data is stored in browser localStorage. To backup:
1. Open Safari Developer Tools
2. Go to Storage → Local Storage
3. Copy the data values

### Clear Data
To start fresh:
1. Open the app
2. Safari → Settings for this website → Clear Website Data

## Development

Built with:
- Vanilla JavaScript (React via CDN)
- No build process required
- Single HTML file architecture
- localStorage for data persistence

## Support

For issues or questions, contact Peter Tonagh.

## Version

v1.0 - January 2026
- Initial release with all 4 tabs
- Recovery formula with 65% validation accuracy
- Complete supplement schedule
- F45 workout program with image upload
- January pushup/pullup challenge
