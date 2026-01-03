# GitHub Pages Setup Guide

Follow these steps to get your FNDTN Performance Tracker live on the web.

## Prerequisites

- A GitHub account (free at github.com)
- These files ready to upload

## Step 1: Create GitHub Account (if needed)

1. Go to https://github.com
2. Click "Sign up"
3. Follow the registration process
4. Verify your email

## Step 2: Create New Repository

1. Click the "+" icon in top right corner
2. Select "New repository"
3. Repository name: `fndtn-tracker` (or any name you prefer)
4. Description: "FNDTN Performance Tracker - Recovery, Supplements, Workouts, Challenges"
5. Select **Public** (required for free GitHub Pages)
6. ✅ Check "Add a README file"
7. Click "Create repository"

## Step 3: Upload Files

### Option A: Web Interface (Easiest)

1. In your new repository, click "Add file" → "Upload files"
2. Drag and drop these files:
   - `index.html`
   - `manifest.json`
   - `icon-generator.html`
3. Scroll down, add commit message: "Initial commit"
4. Click "Commit changes"

### Option B: GitHub Desktop (If you prefer)

1. Download GitHub Desktop: https://desktop.github.com
2. Clone your repository
3. Copy the files into the cloned folder
4. Commit and push

## Step 4: Generate App Icons

1. Open `icon-generator.html` in your browser (just double-click it)
2. Right-click the first image → "Save Image As..." → name it `icon-192.png`
3. Right-click the second image → "Save Image As..." → name it `icon-512.png`
4. Upload both PNG files to your GitHub repository

## Step 5: Enable GitHub Pages

1. In your repository, click "Settings" (top menu)
2. Scroll down left sidebar, click "Pages"
3. Under "Source", select "Deploy from a branch"
4. Under "Branch", select `main` (or `master`)
5. Select `/ (root)` folder
6. Click "Save"

**Wait 2-3 minutes for deployment**

## Step 6: Get Your App URL

1. Stay on the Pages settings page
2. You'll see a message: "Your site is published at https://YOUR-USERNAME.github.io/fndtn-tracker/"
3. Click that URL to test
4. **Bookmark this URL** - this is your app!

## Step 7: Install on iPhone

Now for the magic moment:

1. **Open Safari on your iPhone** (not Chrome!)
2. **Navigate to your GitHub Pages URL**
3. **Tap the Share button** (square with arrow pointing up at bottom of screen)
4. **Scroll down and tap "Add to Home Screen"**
5. You'll see the FNDTN icon and name
6. **Tap "Add"**

🎉 **Done!** The app is now on your home screen.

## Step 8: Test Everything

Open the app from your home screen and test:
- ✅ Opens full screen (no Safari UI)
- ✅ Recovery tab: Enter biomarkers
- ✅ Supplements tab: Check off a few items
- ✅ Workout tab: Upload an image
- ✅ Challenge tab: Track some pushups
- ✅ Close app and reopen: Data persists

## Updating Your App

When you want to update:

1. Edit files on GitHub (or upload new versions)
2. Commit changes
3. Wait 1-2 minutes
4. **Refresh the app** in Safari (pull down to refresh)
5. It will reload with new version

## Troubleshooting

### "Site not found" error
- Wait 3-5 minutes after enabling Pages
- Check Settings → Pages shows "Your site is published"

### Icons not showing
- Make sure icon-192.png and icon-512.png are in root folder
- File names must be exact (lowercase, with dashes)

### App doesn't work offline
- First load must be online
- After first load, it works offline

### Data disappeared
- Don't use Safari Private Browsing mode
- Don't clear Safari data
- Each browser/device has separate data

### Can't add to home screen
- Must use Safari (not Chrome, Firefox, etc)
- Must be on the actual GitHub Pages URL
- Try refreshing the page first

## Advanced: Custom Domain (Optional)

Want your own URL like `tracker.yourname.com`?

1. Buy domain from Namecheap, Google Domains, etc.
2. In GitHub Settings → Pages
3. Enter your custom domain
4. Update DNS settings at your domain registrar
5. Wait for DNS propagation (up to 24 hours)

## Privacy & Data

- **All data stored locally** on your iPhone
- **Nothing sent to servers** (except loading the app initially)
- **GitHub can't see your data** (it's all in your browser)
- **Works offline** after first load

## Need Help?

Common questions:
- **Where's my data stored?** In Safari's localStorage on your device
- **Can I use on multiple devices?** Yes, but data doesn't sync between devices
- **Can others see my data?** No, it's only on your device
- **What if I delete the app icon?** Data persists - just re-add to home screen

## Success Checklist

Before you finish, verify:
- [ ] Repository created and files uploaded
- [ ] Icons generated and uploaded
- [ ] GitHub Pages enabled
- [ ] Site loads in Safari
- [ ] Added to iPhone home screen
- [ ] App opens full screen
- [ ] Data persists when closing and reopening

---

**Your app URL will be:**
`https://YOUR-GITHUB-USERNAME.github.io/fndtn-tracker/`

**Replace YOUR-GITHUB-USERNAME with your actual GitHub username!**

Example: If your GitHub username is `petertonagh`, your URL would be:
`https://petertonagh.github.io/fndtn-tracker/`
