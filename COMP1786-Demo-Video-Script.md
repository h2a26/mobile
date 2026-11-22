# COMP1786 demonstration video: script and running order

Hein Htet Aung (001557664).
- **Length:** 15 minutes.
- **Apps:** Event Manager (native Android, features a–f) and Event Explorer (Flutter, features g–h).
- **Screen:** both apps run on the Android emulator. No code is opened.

> **Length:** the brief asks for "approximately 10 minutes". This script runs for 15. To shorten it, use the ✂️ cuts listed at the end, which bring it to about 10–11 minutes without losing any feature.

> **How each feature is explained:** each one follows the same pattern, which is what the 90–100% band rewards:
> 1. what the brief asks for;
> 2. show it working;
> 3. one or two sentences on how it is built;
> 4. why it was built that way.
>
> The brief also asks you to link the features to your code. These spoken explanations cover that without opening any files. In the Q&A, be ready to open the code.

**How to read this script:**
- **Do** is what to do on the emulator.
- **Say** is what to say. The wording is a guide, so speak naturally.

| Time | Segment | Feature |
|---|---|---|
| 0:00 | Introduction | — |
| 0:45 | Create an event: validation, duplicates, review, design | (a) |
| 3:30 | Sessions; events linked to their sessions | (b), (c) |
| 6:00 | Search | (d) |
| 7:15 | Admin sign-in with Google, upload, network check, automatic sync | (e) |
| 10:15 | Extra admin features | (f) |
| 11:45 | Attendee app: live list, search, details, offline | (g) |
| 13:15 | Favourites, change alerts, QR scanning | (h) |
| 14:35 | Summary | — |

---

## Before recording

### 1. Emulator and apps (on the day)
1. **Start the emulator:** the Pixel 10 Pro emulator in Android Studio (Device Manager → ▶).
2. **App versions:** both apps on the emulator already match the current code. If you want fresh installs anyway, run these from the `Mobile` folder. They keep the apps' data and the admin sign-in.
   ```bash
   cd EventManagerApp && JAVA_HOME="/Applications/Android Studio.app/Contents/jbr/Contents/Home" ./gradlew installDebug
   ```
   ```bash
   cd UserApp && flutter build apk --debug && ~/Library/Android/sdk/platform-tools/adb install -r build/app/outputs/flutter-apk/app-debug.apk
   ```
3. **Location for "Use my location":** in the emulator's Extended controls (⋯) → **Location**, search "Queen Anne Court, Greenwich", then press **Set location**.
4. **Phone settings:**
   - light mode;
   - normal text size;
   - auto-rotate on (or use the emulator's rotate buttons);
   - **Do Not Disturb** on;
   - airplane mode off.
5. **Recents:** close every other app.
6. **Restart Event Explorer:** close it from Recents and open it again, then check the list is live with no offline notice. After a long idle spell it can keep showing its saved copy with an offline notice until it's restarted.

### 2. Fresh demo data: the day before recording
The old sample events now have past dates, so they would show "Date has passed" warnings. Refresh the data, then upload it.

**Ready for recording (checked 22 Nov 2026, 11:00):**
- the data is refreshed and the cloud holds 5 events and 12 sessions;
- favourites are exactly the Summit and Freshers;
- **Upload changes automatically is off**;
- the emulator's location is set to Queen Anne Court, and Do Not Disturb is on;
- Recents is cleared and old QR images are removed;
- **the admin is signed out:** do this last (Cloud tab → **Sign out**), so the Cloud tab shows **Sign in with Google** when you reach segment (e).

After 3 Dec the summit is in the past; if that happens, repeat these steps.

Event Explorer labels events added in the last 24 hours **New**. If you record the same day you load the data, every sample shows New, which is fine: the script doesn't rely on those labels.
1. **Event Manager → Settings → Delete all events**, then confirm.
2. **Settings → Load sample events.** The dates are relative to today.
3. **Cloud tab:**
   - check it says **Signed in as …**, and sign in with Google if not;
   - make sure **Upload changes automatically** is **off**. You switch it on during segment (e). If it's on earlier, the event you create in (a) and the deletion in (c) upload by themselves before (e).
   - tap **Upload all to cloud**. If it asks "Remove items that are not on this phone?", choose remove.
4. **Event Explorer:**
   - check the events appear;
   - tap the ♡ on **Greenwich Mobile Dev Summit** and on **Freshers Tech Social**;
   - open the **Favourites** tab once, then go back to **Events**.

   The alerts in segment (h) need this step: the app has to see the "before" versions first.
5. **Leftover data:** make sure there is no "Greenwich Mobile Showcase" event yet. You create it on camera.
6. **Sign out last:** Event Manager → **Cloud → Sign out**. The tab now shows **Sign in with Google**, and you sign in on camera in segment (e).
7. **Google account on the emulator:** the admin Google account must already be added to the emulator (**Settings → Passwords & accounts**). Then signing in on camera takes one tap in the account picker, and no password is ever typed on screen.

### 3. Recording (on the day)
- **What to record:** only the emulator window, which is all the video shows.
- **Start recording:** macOS **⌘⇧5** → **Record selected portion**, drawn around the emulator → **Options**: choose your microphone and turn on **Show mouse clicks**.
- **Privacy:** the account picker and the Cloud tab show your Google email. That is fine for your tutors, but be aware it is on screen. Never type a password while recording.
- **Rehearse:** do one full run with a stopwatch first. Speak while you tap rather than in silence. If you make a mistake, pause and carry on; a short pause is better than restarting.

---

## 0:00 Introduction (45 s)

**Do:** the emulator's home screen, showing both app icons.

**Say:**
> Hello, I'm Hein Htet Aung, student ID 001557664, and this is my COMP1786 coursework demonstration. I built two apps that share one cloud database.
>
> - **Event Manager** is the organisers' app. It is a native Android app written in Kotlin with Jetpack Compose, and it stores events and sessions in an SQLite database on the phone. It covers features a to f.
> - **Event Explorer** is the attendees' app. It is a hybrid app built with Flutter, and it covers features g and h.
>
> Both apps use the same Cloud Firestore database, so what an organiser uploads appears live for attendees. For each feature, I'll show it working and explain how it's built and why.

---

## 0:45 Feature (a): event management and design (2 min 45 s)

**Do 1:** open **Event Manager**.
**Say:**
> This is the admin app's event list, read from SQLite. Each card shows the type, status, date, venue, organiser, session count and upload state. Everything in features a to d works on the phone without an account; only uploading needs an admin sign-in, which I'll show in feature (e).

**Do 2:** tap **New event**, then **Review details** with nothing filled in.
**Say:**
> Feature (a) adds three optional fields and stronger validation. If I continue with an empty form, every required field gets its own message, for example "Please enter the event name", so the user knows exactly what to fix.

**Do 3:** fill in an existing event to trigger the duplicate check:
- name **Greenwich Mobile Dev Summit**;
- date: type the digits **04122026**, which becomes 04/12/2026, the date on its card;
- location **Old Royal Naval College**;
- organiser **University of Greenwich**;
- type **Conference**.

Then tap **Review details**. It shows "An event with this name already exists on …".

**Say:**
> Two events can't have the same name and date. When I press Review details, the app asks the database for an event with this name on this date, ignoring letter case and spacing, and refuses the save. When an event is being edited, it's left out of the check, so it never clashes with itself.

**Do 4:** change the details:
1. Change the name to **Greenwich Mobile Showcase**.
2. In the date field, type the digits **31022027**. It shows "This date does not exist".
3. Tap the calendar icon and pick a date **two weeks from today**.
4. Change the location to **Queen Anne Court, London SE10 9LS** and the organiser to **Computing Society**.

**Say:**
> The date is DD/MM/YYYY. I can type just the digits and the slashes are added, but only real dates are accepted, so the 31st of February is rejected. Or I pick it from the calendar. Type and status are chosen from a list, not typed, and the status must fit the date: a past event can't be Upcoming.

**Do 5:** fill in the optional fields:
1. Description: **Student app demos, talks and networking.**
2. Contact email: type **showcase@**. An error appears.
3. Correct the email to **showcase@example.org**.
4. Contact phone: type **12ab**. An error appears.
5. Correct the phone to **+44 20 7946 0999**.

**Say:**
> These are the three new optional fields: description, contact email and contact phone. Left empty they give no error, but if I fill them in they must be valid, and each message shows an example of the right format. All these rules are written once in plain Kotlin and covered by unit tests, so adding and editing use exactly the same rules.

**Do 6:**
1. Tap **Review details**.
2. Scroll the review page.
3. Tap **Edit details**. The values are kept.
4. Tap **Review details** again, then **Confirm and save**. The message "Event saved" appears.

**Say:**
> When everything is valid, the review page shows every value exactly as it will be saved; that's the confirmation step in the brief. Edit details takes me back with nothing lost, even if the phone is rotated. I'll confirm. A double tap here can't save the event twice. The event is saved, and its cloud icon shows it isn't uploaded yet.

**Do 7:**
1. Go to **Settings → Appearance → Dark**, then go back to the list.
2. Rotate to landscape. The side rail and grid appear.
3. Rotate back to portrait, and set the theme back to System default.

**Say:**
> For the design, I used Material 3 throughout. Its pickers, dropdowns and radio buttons let users choose rather than type, which prevents errors. There's a light and a dark theme, chosen in Settings, and all text is in string resources. In landscape, the bottom bar becomes a navigation rail and the list becomes a grid, so wider screens are used well.

---

## 3:30 Features (b) and (c): sessions, linked to their event (2 min 30 s)

**Do 1:** on the Showcase card, tap **+ Session**. Then tap **Save session** with the form empty.
**Say:**
> Feature (b) is sessions. The session ID is assigned automatically when the session is saved: it's the database row's own ID, so it's always unique. Name and start time are required, so saving now explains what's missing.

**Do 2:** fill in the first session:
- name **Opening keynote**;
- start **10:00** and end **10:45**, both picked on the clock;
- type **Keynote**;
- speaker **Dr Amelia Hart**;
- room **QA075**.

Tap **Save session**. The message "Session saved" appears.

**Say:**
> Times are picked on a clock, so they can't be mistyped, and the end must be after the start. The six session types from the brief are in this list. Speaker, room and description are optional.

**Do 3:** tap **+ Session** again and fill in **Compose in practice**, start **10:00**, room **QA075**.
1. Tap **Save session**. The "Room double-booked?" dialog appears.
2. Tap **Change**.
3. Set start **11:00**, end **12:00**, type **Workshop**, speaker **Ravi Patel**.
4. Tap **Save session**.

**Say:**
> If I book the same room at the same time, the app warns me before saving. It's an extra check that prevents a real scheduling mistake.

**Do 4:** open the Showcase event. Its sessions are listed in time order.
1. Tap **Compose in practice**. The sheet shows its ID (S-00xx), time and length.
2. Tap **Edit**, change the end to **12:30**, then tap **Update session**.
3. Open it again, tap **Delete**, then confirm.

**Say:**
> Opening the event shows feature (c): the event with all its sessions in time order, each with its generated ID. I can edit a session, and the ID stays the same, and I can delete one after a confirmation.

**Do 5:** go back to the list; the Showcase card says **1 session**.
1. On **Freshers Tech Social**, tap **⋮ → Delete**. The dialog says "…and its 1 session will be permanently deleted".
2. Tap **Delete**.

**Say:**
> The list shows each event's session count. Sessions are stored in a second table in the same SQLite database as my Logbook 3 events. I upgraded that database to a new version rather than replacing it, so existing events were kept. Each session points to its event with a foreign key set to cascade on delete, so when an event is deleted, the database removes its sessions too, and a session can never be left without its event. The dialog says exactly how many will go.

---

## 6:00 Feature (d): search (1 min 15 s)

**Do:**
1. Open the **Search** tab.
2. Type **mob**. The results are highlighted. Then clear the search.
3. Tap **Organiser → Computing Society**.
4. Tap **Date** and pick a range from today to about a month ahead. The two filters combine.
5. Tap a result. Its full details open.
6. Open **Greenwich Mobile Dev Summit** and type **security** in its session search.

**Say:**
> Feature (d), search. Typing the first letters, "mob", finds every event with a word starting with those letters in its name or description, highlighted as I type. The advanced filters search by organiser and by date, a single day or a range, and they combine. The organiser list is built from the saved events, so it can't be mistyped. Tapping a result opens its full details, and inside an event I can search its sessions too: "security" finds the security-rules talk. All of this is one database query. The search text is passed in as a parameter, never pasted into the SQL, so it can't be used for SQL injection.

---

## 7:15 Feature (e): admin sign-in, upload to the cloud and keep it in sync (3 min)

**Do 1:** open the **Cloud** tab. It shows **Online**, and under **Admin account**: "Organisers sign in with their Google account. Only accounts on the admin list can upload." **Upload all to cloud** is greyed out.
1. Tap **Sign in with Google**. The account picker opens.
2. Choose your admin Google account. The tab now says **Signed in as …**, and Upload all is enabled.

**Say:**
> Feature (e), the cloud. I chose Cloud Firestore rather than writing my own web service because it gives live updates, an offline copy on the phone and server-side security rules, without a server to maintain.
>
> Uploading is the one admin-only action, so the Cloud tab asks me to sign in, and Upload all stays disabled until I do. Sign in with Google goes through Android's Credential Manager and Firebase Authentication, so the app never sees or stores a password, and the organiser keeps Google's two-step verification. Signing in proves who I am. What I'm allowed to do is checked separately: the app looks up my account in the admins list in the database, and an account that isn't on it is signed straight out with an explanation. The server's security rules check the same list, so even a modified app can't upload without admin rights, and 21 automated tests check those rules.

**Do 2:** tap **Upload all to cloud**. The result is "Upload complete … Removed from cloud: 2". Then turn on **Upload changes automatically**.
**Say:**
> Upload all sends every event and session in one go. "Removed from cloud: 2" is the Freshers event I deleted earlier and its session, so deletions are synchronised too. Now I'll switch on automatic upload, so from here on, changes upload by themselves.

**Do 3:** turn on **airplane mode** (pull down Quick Settings). The Cloud tab shows **Offline**: "No internet connection. Uploads will work again when the connection is back." **Upload all to cloud** is greyed out.
**Say:**
> Before uploading, the app checks that the connection actually reaches the internet, not just that Wi-Fi is on. In airplane mode it says it's offline and disables the upload, instead of letting it fail.

**Do 4:** still offline, open **Events → Greenwich Mobile Dev Summit → Edit**.
1. Change the location to **Painted Hall, Old Royal Naval College, London SE10 9NN**.
2. Tap **Review details**, then **Confirm and update**.
3. The card now says **Changed since the last upload**. On the Cloud tab it says **Waiting for the internet to upload changes**.

**Say:**
> For the full mark, changes on the phone must be synchronised with the cloud. I'll change this event's venue while offline. Every row remembers when it was last uploaded, so the app knows this event has changed. The card shows "Changed since the last upload", and a background upload is waiting for the internet.

**Do 5:** turn **airplane mode off** and wait 5–20 seconds. The Cloud tab shows **Uploading in the background…**, then the card says **Uploaded to the cloud**.
**Say:**
> As soon as the connection is back, the change uploads by itself. I didn't press anything. This background job uses Android's WorkManager, which waits for a network and retries on failure. I chose it over a plain thread because it keeps working even after the app is closed. Each event is uploaded in the same all-or-nothing batch as its sessions, so attendees never see an event without them.

---

## 10:15 Feature (f): additional admin features (1 min 30 s)

**Do 1:** open the **Greenwich Mobile Showcase → Edit → Use my location**. Allow the permission if asked. The message "Venue filled from your location" appears. Then tap **Review details → Confirm and update**.
**Say:**
> Feature (f): my extra features, each solving a real organiser problem. "Use my location" fills in the venue address from GPS and saves the exact map position, so attendees get precise directions. Location permission is only requested when the button is tapped, never in advance.

**Do 2:**
1. In the event details, tap **QR code** to show the event code, then close it.
2. Tap the **Opening keynote** session, then **QR code**.
3. While the session QR code is showing, open **Recents** and tap **Screenshot**. This saves the image for segment (h).

**Say:**
> Every event and session has a QR code for posters and door signs, generated with the ZXing library. The code holds a link to the live record, not a copy of the details, so a printed sign stays correct even after a last-minute change. I'll take a screenshot of this one to scan later in the attendee app.

**Do 3:** tap **⋮ → Export agenda (PDF)**. The share sheet opens. If the emulator has a PDF viewer, open the PDF with it; otherwise the share sheet is enough. Go back and point out **Print door signs (PDF)** and **Add to calendar** in the menu.
**Say:**
> The menu exports the event's agenda as a PDF, drawn on the phone with Android's own PDF tools, and prints a door sign for each session. Both can be shared to any app. Add to calendar, email and call open the right apps. And the background upload from feature (e) is the first extra the brief suggests: uploading on a separate worker thread.

---

## 11:45 Feature (g): the hybrid attendee app (1 min 30 s)

**Do 1:** go to Recents, or the home screen, and open **Event Explorer**. Point out:
- **Greenwich Mobile Showcase** under **Coming up**;
- **Greenwich Mobile Dev Summit**, which already shows its new venue, **Painted Hall, …**.

**Say:**
> Now the attendee app, Event Explorer, built with Flutter: feature (g). It connects to the same Firestore database as the admin app and listens to it, so changes arrive without refreshing. Here's the Showcase I created a few minutes ago, and the summit already shows the new venue I set while the admin phone was offline. Events are grouped into Coming up and Past events.

**Do 2:** search for events:
1. Type **work**. The matches are highlighted. Clear the search.
2. Tap **Date: any → Next 30 days**. The list narrows.
3. Open the date menu again to show **Pick a date…** and **Pick a date range…**.
4. Tap **Clear filters**.

**Say:**
> I can search by name, which ignores case and accents and highlights the matches, and by date: today, the next 7 or 30 days, a single date or a range.

**Do 3:** open the **Showcase**. Point out the date, the location with **Directions**, the organiser with email and call, and the **Sessions** timeline.
**Say:**
> The details page shows the venue with directions, buttons to email or call the organiser, and the session timeline, which has its own search for sessions, speakers or rooms.

**Do 4:** check offline mode and landscape:
1. Turn **airplane mode** on and go back to the list. The offline notice appears, and events still open.
2. Turn **airplane mode** off.
3. Rotate to landscape: the side rail and columns appear. Rotate back.

**Say:**
> Firestore keeps a copy on the phone, so the app still works with no signal at a venue, and this notice says so. Landscape uses a side rail and columns. I chose Flutter over .NET MAUI because its Firebase support includes the live listener and the offline copy, and the same code can build an iPhone version.

---

## 13:15 Feature (h): favourites and extra attendee features (1 min 20 s)

**Do 1:** tap the ♡ on **Greenwich Mobile Showcase**. The message "Saved to favourites" appears. Tap the ♥ again, which shows "Removed from favourites" with **Undo**, then tap **Undo**.
**Say:**
> Feature (h), favourites. Tapping the heart saves an event on the phone for quick access. If I remove one by mistake, I can undo it.

**Do 2:** open the **Favourites** tab, which shows a badge. You'll see:
- "1 favourite has changed since you last looked" and **New location: Painted Hall, …**;
- "No longer available: Freshers Tech Social", with **Remove**.

Tap **Remove**.

**Say:**
> Favourites are stored on the phone together with a copy of each event as the attendee last saw it. That means they work offline, and the app can compare that copy with the live event. Two things happened since I saved these earlier. The venue change I made offline in the admin app has arrived: the card says exactly what changed, and the tab shows a badge. And the Freshers event the organiser deleted is pointed out, so it can be removed.

**Do 3:** open **Scan**; the camera view opens. Tap **Scan from photo** and choose the screenshot from segment (f). The **Opening keynote** session details open, marked **Scanned**.
**Say:**
> Finally, the Scan tab. On a real phone the camera reads the QR code on a door sign; here I'll scan the screenshot of the admin app's session code. It opens that session's live details. The app only opens its own event links with valid IDs, so a tampered QR code can't send attendees to another website.

---

## 14:35 Summary (25 s)

**Do:** return to the Events tab of Event Explorer.

**Say:**
> To sum up: all eight features, a to h, work, and an organiser's change, even one made offline, reaches attendees by itself. Both apps are covered by automated tests, 153 in total across the admin app, the Flutter app and the security rules, all passing. Thank you for watching.

---

## ✂️ Cuts to bring it to about 10 minutes

| Cut | Saves |
|---|---|
| Feature (a): skip the dark theme and landscape (Do 7); just say "the app also has a dark theme and a landscape layout" | 35 s |
| Feature (a): skip the scroll on the review page (Do 6 step 2) | 10 s |
| Feature (b): skip editing the session (Do 4 step 2); keep the delete | 20 s |
| Feature (d): skip the session search (step 6) | 15 s |
| Feature (e): skip showing the Cloud tab offline (Do 3); the offline edit still shows the network check | 20 s |
| Feature (f): show only the menu, not the share sheet | 20 s |
| Feature (g): skip offline mode and landscape (Do 4), but keep the Flutter sentence | 35 s |
| Feature (h): skip the Undo (Do 1, second tap) | 10 s |
| **Each segment:** keep speech brisk (about 150 words per minute) | about 2 min |

## If something goes wrong on camera
| Problem | What to do |
|---|---|
| The automatic upload doesn't start after reconnecting | Wait 20 seconds. Otherwise tap **Upload all to cloud** and say the background job retries on its own schedule. |
| "Use my location" finds no address | The app says "Map position saved…". Type the venue and carry on; the map position is still saved. |
| The photo picker doesn't show the screenshot | Choose **Recent** or **Screenshots** in the picker. You can also drag a QR image from the Mac onto the emulator; it lands in Downloads. |
| The account picker asks for a password, or offers to add an account | Pause the recording. Add the Google account to the emulator first (Settings → Passwords & accounts), then start again. Never type a password on camera. |
| Sign-in says "… is not an admin yet" | A different account was chosen. Tap **Sign in with Google** again and choose the admin account. This also shows the admin check working. |

## After recording
1. **Upload:** put the video on Panopto with the title **COMP1786-Hein-Htet-Aung-001557664**, exactly as written in the report's video row.
2. **Sharing:** set it so your tutors can view it.
3. **Report link:** paste the share link into `VIDEO_LINK` in `Docs/tools/content_cw.py`, then rebuild the report:
   ```bash
   Docs/tools/.venv/bin/python Docs/tools/build_report.py
   ```
