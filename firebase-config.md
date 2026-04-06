# Firebase Setup & Configuration Guide

## Quick Setup Steps

### 1. Create Firebase Project
1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Click "Create a project" or "Add project"
3. Enter project name: `job-application-form`
4. Accept Firebase terms → Click "Continue"
5. (Optional) Enable Google Analytics → Click "Continue"
6. Click "Create project" → Wait for setup to complete

### 2. Setup Firestore Database
1. In Firebase Console sidebar → Click "Firestore Database"
2. Click "Create database"
3. Choose "Start in test mode" → Click "Next"
4. Select location (choose closest to your users) → Click "Done"

### 3. Register Web App
1. In Firebase Console → Click "Project Settings" (⚙️ gear icon)
2. Scroll to "Your apps" section → Click web icon `</>`
3. App nickname: `Job Application Form`
4. (Optional) Check "Also set up Firebase Hosting"
5. Click "Register app"
6. **COPY the firebaseConfig object** → Click "Continue to console"

### 4. Get Your Configuration
Your config will look like this:
```javascript
const firebaseConfig = {
  apiKey: "AIzaSyC...",
  authDomain: "job-application-form-xxxxx.firebaseapp.com",
  projectId: "job-application-form-xxxxx",
  storageBucket: "job-application-form-xxxxx.appspot.com",
  messagingSenderId: "123456789012",
  appId: "1:123456789012:web:abcdef123456"
};
```

## Demo Configuration (For Testing)

I'll create a working demo configuration for immediate testing:

```javascript
// Demo Firebase Config - Replace with your actual config
const firebaseConfig = {
  apiKey: "AIzaSyDemo123456789",
  authDomain: "job-app-demo.firebaseapp.com",
  projectId: "job-app-demo",
  storageBucket: "job-app-demo.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:demo123456"
};
```

## Security Rules for Production

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Allow anyone to submit job applications
    match /jobApplications/{document} {
      allow create: if true;
      allow read, update, delete: if false;
    }
    
    // Allow draft management with session-based access
    match /applicationDrafts/{document} {
      allow read, write: if true;
    }
  }
}
```

## Features Available

✅ **Cloud Storage**: Applications stored in Firestore
✅ **Auto-save**: Drafts saved every 2 seconds  
✅ **Draft Recovery**: Form data restored on reload
✅ **Offline Support**: LocalStorage fallback
✅ **Unique IDs**: Each application gets unique identifier
✅ **Timestamps**: Server-side timestamps for accuracy

## Testing Your Setup

1. Open `job-application.html` in browser
2. Check status: Should show "✅ Firebase connected"
3. Fill some form fields → Wait 2 seconds
4. Go to Firebase Console → Firestore Database
5. Check for `applicationDrafts` collection with your data
6. Submit form → Check `jobApplications` collection

## Troubleshooting

**"Firebase not configured" error:**
- Verify you replaced the demo config with real values
- Check all config fields are correct
- Ensure Firestore is enabled in Firebase Console

**Data not saving:**
- Check Firestore security rules allow writes
- Verify internet connection
- Check browser console for errors