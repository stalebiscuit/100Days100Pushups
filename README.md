# 100 × 100 — deploy on GitHub Pages

A two-person pushup tracker. One file (`index.html`), hosted free on GitHub Pages, with Firebase doing the syncing between your two phones. No build tools, no server.

## What you'll end up with
A permanent address like `https://yourname.github.io/pushups/` that you both open on your phones. Everything works: daily counter, quick-add, undo/edit, the green calendar with You / Partner / Both, the Overview with the 100-grid and month-by-month, the open-time reminder banner, and live partner sync.

## Files you need
Just **one**: `index.html`. That's the whole app. (This README is optional — keep it in the repo if you like.)

---

## Part A — Firebase (the shared database) · ~5 min

1. Go to **console.firebase.google.com**, sign in with a Google account, and click **Add project**. Name it anything (e.g. `pushups`). You can skip Google Analytics.
2. In the project, open **Build → Firestore Database** and click **Create database**. Pick a location near you. Choose **Start in production mode** (we'll set the access rules in step 4).
3. Add a web app: click the **`</>`** (web) icon on the project overview, give it a nickname, and **don't** check Firebase Hosting. Firebase will show you a `firebaseConfig = { ... }` block — **copy it**, you'll paste it into `index.html` in Part C.
4. Open **Firestore Database → Rules**, replace what's there with the block below, and click **Publish**:

   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /pushup100/{doc} {
         allow read, write: if true;
       }
     }
   }
   ```

   Note on this rule: it lets anyone who has your config read and write *only* the two documents this app uses. For a private hobby tracker between two people that's normally fine, and your pushup counts aren't sensitive. If you'd rather lock it down further later, ask and I'll add a simple shared-code gate.

*(Firebase's console wording shifts over time — if a button isn't where I said, their "Get started with Firestore" quickstart has the current version.)*

---

## Part B — GitHub repository + Pages · ~5 min

1. Make a free account at **github.com** if you don't have one.
2. Click **New repository**. Name it e.g. `pushups`, set it to **Public** (Pages is free on public repos), and create it.
3. On the repo page, click **Add file → Upload files**, drag in your edited `index.html` (after Part C), and **Commit changes**. *(You can also "Create new file", name it `index.html`, and paste the contents.)*
4. Go to **Settings → Pages**. Under **Source**, choose **Deploy from a branch**, set the branch to **main** and the folder to **/ (root)**, then **Save**.
5. Wait about a minute, refresh, and Pages will show your live URL: `https://yourname.github.io/pushups/`.

---

## Part C — connect the two (the only edit you make)

1. Open `index.html` in any text editor.
2. Near the top, find the `firebaseConfig = { ... }` block with the `PASTE_...` placeholders.
3. Replace the whole block with the one you copied from Firebase in Part A, step 3.
4. Save, and make sure that edited file is the one uploaded to GitHub (Part B, step 3).

Order tip: do Part A first, paste the config (Part C), *then* upload to GitHub (Part B).

---

## Part D — use it

1. Both of you open the Pages URL on your phones.
2. First person to open it enters both names + the start date and taps **Start the 100 days** (this is shared, so it only happens once).
3. Each of you taps your name once — that phone remembers you.
4. **Add to Home Screen** for an app-style icon (in the browser's share menu).

That's it. Log your reps, watch each other's counts move in real time, and the calendar fills in green as the days get done.

---

## Editing later
To change anything, edit `index.html` and re-upload it to the repo (or edit it directly on GitHub with the pencil icon). Pages redeploys automatically in about a minute. Your logged data lives in Firebase, so editing the page never wipes your progress.
