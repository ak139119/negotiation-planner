# Negotiation Planner

A shared negotiation planning document for student teams, based on the course's *Negotiation Planning Document* template. Team members open the same link and edit together in real time.

**What it covers**
- **Self vs other:** positions, interests, BATNA, reservation price, targets, sources of power, weaknesses, authority and time pressure. Every entry is tagged **S** (stated), **I** (inferred, with basis and confidence H/M/L) or **U** (unknown).
- **The numbered template sections:** 1. ZOPA · 2. Concession plan · 3. Process design · 4. Walk-away triggers · 5. Relationship after the deal · 6. Intelligence gap register · 7. Opening move · 8. Contingencies · 9. Other information.
- **ZOPA visualizer:** draws both sides' reservation prices and targets and shows the zone, or the gap.
- **Owners and completeness:** each section gets an owner, a progress dot and a checklist. Unknowns move to the gap register with one click.
- **Sparring partner:** builds role-play, debrief and red-team prompts from the plan, for the team's AI workspace from Session 1.
- **Export:** a Word file in the template layout, print or save as PDF, and a JSON backup that can be restored.

It opens with a fully worked **LoopBox × Maison Vercors** example. The counterpart and all figures are fictional.

**Files**
- `index.html`: the whole app.
- `firestore.rules`: the database security rules for step 4 below.
- `README.md`: these notes.

---

## How it works without setup
Open `index.html` as it is and the app runs in **This device only** mode: plans are saved in the browser. It's good for trying it out or working alone, but there's no live sharing. Live collaboration needs a free Firebase database, set up once by you. Students need no accounts.

---

## Set up live collaboration (about 20 minutes, once)

### 1. Create a Firebase project
1. Go to [console.firebase.google.com](https://console.firebase.google.com) and sign in with a Google account.
2. Click **Create a project** (or **Add project**), name it, for example `negotiation-planner`, and continue.
3. Turn **Google Analytics off**. The app doesn't need it. Then click **Create project**.

### 2. Register the web app and copy its settings
1. On the project overview page, click the **web icon `</>`** (Add app → Web).
2. Give it a nickname, for example `planner`. Leave Firebase Hosting unticked and click **Register app**.
3. Firebase shows a `firebaseConfig` block. Keep the page open; you need four values from it: `apiKey`, `authDomain`, `projectId` and `appId`.

### 3. Turn on anonymous sign-in
1. In the left menu, go to **Build → Authentication** and click **Get started**.
2. Open the **Sign-in method** tab, choose **Anonymous**, switch it **on** and click **Save**.

Students are signed in invisibly, with no email or password. This only lets the database rules tell app users apart from random internet traffic.

### 4. Create the database and paste the rules
1. Go to **Build → Firestore Database** and click **Create database**.
2. Choose a **location** in Europe, for example `eur3 (europe-west)`. It cannot be changed later.
3. Choose **Start in production mode** and click **Create**.
4. Open the **Rules** tab, replace everything with the contents of `firestore.rules`, and click **Publish**.

The rules let anyone using the app open or edit a plan whose link they have. Nobody can list, browse or delete other teams' plans.

### 5. Put your settings into the app
Open `index.html` in a text editor. Near the top, find this block:

```js
window.FIREBASE_CONFIG = {
  apiKey: "",
  authDomain: "",
  projectId: "",
  appId: ""
};
```

Paste in your four values from step 2, keeping the quotation marks. Save the file.

These Firebase web settings are designed to be public. They identify your project but grant no access by themselves. The rules from step 4 control access.

### 6. Publish on GitHub Pages
Same steps as for Shapley Split:
1. Create a new **public** repository, for example `negotiation-planner`.
2. Upload `index.html`, `README.md` and `firestore.rules` with **Add file → Upload files → Commit changes**.
3. Go to **Settings → Pages**, set **Source** to **Deploy from a branch**, **Branch** to `main` and the folder to `/ (root)`, then click **Save**.
4. After a minute or two, your address appears: `https://YOUR-USERNAME.github.io/negotiation-planner/`

### 7. Authorise your address in Firebase
In Firebase: **Authentication → Settings → Authorized domains → Add domain**, and enter `YOUR-USERNAME.github.io`.

### 8. Test it
1. Open your GitHub Pages address. The home page should show **Live sync on**.
2. Click **Create plan** and enter your name.
3. Click **Copy team link** and open the link in a second browser, or on your phone.
4. Type in one window. The text should appear in the other within a second or two, and the header shows who else is there.

---

## Running it in class
1. **One student per team** opens the app, clicks **Create plan** (optionally ticking *Start from the LoopBox example*), and shares the **team link** in the team chat.
2. **Everyone** opens the link and picks their name. Each section shows who is working in it.
3. Assign **owners** to sections. Each owner fills their section; everyone can comment by editing.
4. Tag every *Self vs other* entry S, I or U. Send unknowns to the **gap register** and give each one a plan and a date.
5. Before the role-play, use the **Sparring partner** prompts in the team's AI workspace.
6. **Export → Download Word file** to submit, or **Print or save as PDF**.

**Good to know**
- Anyone with a team link can edit that plan, so links should stay within the team. Use **Download backup** before big changes.
- Edits save automatically about a second after typing stops. If two people edit the *same box* at once, the last one saved wins. Different boxes never overwrite each other.
- The badge at the top shows **Saved · live**, **Saving…**, or **Not saved: check connection**.
- Without a connection, the app keeps your typing and saves it when the connection returns, as long as the tab stays open.

## Free-tier limits
Firebase's free **Spark** plan allows 50,000 document reads and 20,000 writes per day. A team of five working for an hour typically uses a few thousand reads, because each saved edit is read by every open teammate. One class section fits comfortably. If you run several large sections on the same day and hit the limit, switch the project to the pay-as-you-go **Blaze** plan. At classroom scale it costs cents, and you can set a budget alert in Google Cloud.

## Privacy notes
- The database stores only what students type into plans, plus each person's chosen display name for the "who is here" indicator. There are no emails or passwords.
- Choosing a European Firestore location keeps the data in the EU.
- Like Shapley Split, the page loads fonts from Google Fonts. If your institution prefers no outside requests, ask for a version with bundled fonts.
- To remove a plan, delete it in the Firebase console under **Firestore Database → Data → plans**.
