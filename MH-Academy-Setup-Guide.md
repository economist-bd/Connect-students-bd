# 🔥 MH Academy — Firebase Live করার সম্পূর্ণ গাইড

## ধাপ ১ — Firebase Config বসান (৫ মিনিট)

1. https://console.firebase.google.com এ যান
2. আপনার project সিলেক্ট করুন
3. ⚙️ Project Settings → "Your apps" → Web App (</> আইকন)
4. যদি Web App না থাকে → "Add app" → Web বেছে নিন
5. নিচের config কপি করুন:

```javascript
const firebaseConfig = {
  apiKey: "...",
  authDomain: "...",
  projectId: "...",
  storageBucket: "...",
  messagingSenderId: "...",
  appId: "..."
};
```

6. **index.html** ফাইল খুলুন
7. `"YOUR_API_KEY"` ইত্যাদি জায়গায় আসল মান বসান

---

## ধাপ ২ — Authentication চালু করুন

1. Firebase Console → **Authentication** → Get Started
2. **Sign-in method** ট্যাব → **Email/Password** → Enable করুন → Save

---

## ধাপ ৩ — Firestore Database তৈরি করুন

1. Firebase Console → **Firestore Database** → Create database
2. **Start in test mode** বেছে নিন (শুরুতে)
3. Location: **asia-south1** (বাংলাদেশের কাছের)
4. Done!

### Security Rules বসান:
1. Firestore → **Rules** ট্যাব
2. নিচের rules কপি করে বসান এবং **Publish** করুন:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId} {
      allow read: if request.auth != null;
      allow write: if request.auth != null && request.auth.uid == userId;
    }
    match /notes/{noteId} {
      allow read: if request.auth != null;
      allow create: if request.auth != null;
      allow update: if request.auth != null;
      allow delete: if request.auth != null && resource.data.authorId == request.auth.uid;
    }
    match /notices/{noticeId} {
      allow read: if request.auth != null;
      allow create: if request.auth != null;
    }
    match /quizResults/{resultId} {
      allow read: if request.auth != null;
      allow create: if request.auth != null && request.resource.data.userId == request.auth.uid;
    }
  }
}
```

---

## ধাপ ৪ — Firebase Hosting এ Live করুন

### Node.js ইনস্টল করুন (যদি না থাকে):
👉 https://nodejs.org থেকে LTS version ডাউনলোড করুন

### Terminal/Command Prompt খুলুন:

```bash
# Firebase CLI ইনস্টল করুন
npm install -g firebase-tools

# Firebase-এ লগইন করুন
firebase login

# আপনার project folder-এ যান
cd path/to/mh-academy

# Firebase init করুন
firebase init hosting

# প্রশ্নের উত্তর:
# → Use an existing project → আপনার project বেছে নিন
# → Public directory: . (dot চাপুন)
# → Single-page app: No
# → Overwrite index.html: No

# Deploy করুন!
firebase deploy
```

### ✅ Deploy হলে পাবেন:
```
Hosting URL: https://YOUR-PROJECT-ID.web.app
```

---

## ধাপ ৫ — প্রথম লগইন করুন

1. আপনার live URL খুলুন
2. "নিবন্ধন" ফর্মে নাম, ইমেইল, পাসওয়ার্ড দিন
3. Role: **শিক্ষক** বেছে নিন
4. নিবন্ধন করুন → সরাসরি ড্যাশবোর্ডে চলে যাবেন!

---

## ⚠️ সমস্যা হলে

| সমস্যা | সমাধান |
|--------|---------|
| "permission denied" | Firestore Rules ঠিকমতো বসেছে কিনা দেখুন |
| লগইন কাজ করছে না | Authentication → Email/Password Enable আছে কিনা দেখুন |
| Config কাজ করছে না | apiKey সঠিকভাবে কপি হয়েছে কিনা দেখুন |
| Deploy হচ্ছে না | `firebase login --reauth` দিন |

---

## 🚀 আপনার Live App URL হবে:
**https://YOUR-PROJECT-ID.web.app**

এই URL যেকোনো ডিভাইস থেকে খোলা যাবে!
