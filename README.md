# Medclean: hygiene staff app (clickable prototype)

A responsive, click-through prototype of the hospital hygiene staff app (Ward 4B, St. Vincent's University Hospital). It works on iPhone, Android, iPad and desktop. Everything lives in one file, `index.html`, with no build step and nothing to install.

## Put it on GitHub Pages (to test on your phone)

1. Create a new repository on GitHub, for example `hygiene-app`. It can be public or private (private needs a paid plan for Pages).
2. Upload every file in this folder: `index.html`, `manifest.webmanifest`, the `icon` files, `.nojekyll` and this README. Use **Add file → Upload files**, then **Commit changes**.
3. Go to **Settings → Pages**. Under **Build and deployment**, set **Source** to *Deploy from a branch*, choose the **main** branch and the **/ (root)** folder, then click **Save**.
4. Wait a minute, then open `https://<your-username>.github.io/hygiene-app/` on your phone.
5. Optional: add it to your home screen so it opens full screen like an app.
   - iPhone: Safari → Share → **Add to Home Screen**.
   - Android: Chrome → menu → **Add to Home screen**.

You can also just double-click `index.html` to open it on your computer.

## What you can test

| Flow | How to try it |
|---|---|
| Supervisor on shift | Top of Home: who is on, until when, and a **Message** button. |
| Job types | Terminal clean and blood spill: scan, checklist, report, signature, then a supervisor check. Discharge: the same, but the report is optional. Sluice room: scan, checklist, **Mark as done**. Hand gel and daily clean: checklist, then **Mark as done**, with no scan or signature. The quick jobs can still report a problem from the finish screen. Set per job in `TASKS` (`scan`, `report`, `sign`). |
| AI description | Add a photo to a report and a description is written for you. You pick who it goes to under **Send to**; nothing is ticked for you. In this prototype the text is a canned demo; a live app would send the photo to an AI model. |
| Lesson video | Training or Watch how: play, pause, 10 seconds back or forward, drag the bar, captions per step, tap a chapter to jump. Plays in real time; the photo stands in for the video. |
| Sign off page | After a full job's checklist: open **Something to report?** if needed, then tick **I completed every step** under your signature and tap **Sign off**. |
| Accept a critical job | Home → **Accept job**. The red card folds away and the job slides into Today, highlighted red. |
| Do a task | Home → **Start** on any task → prep screen for that task. |
| Log the bottle | **Scan to log the bottle** → scanning animation → green tick → checklist opens by itself. |
| Contact timer | Checklist step 5 → **Start**. It counts down fast (demo speed) and turns into a ticked step. |
| Report a problem | In **Something to report?**: the location, job, time and your name are filled in. **Take a photo** opens the camera inside the problem card (on a phone over https). If the camera is blocked it offers your photos instead. **Send to** is a tick-box dropdown for each problem. **Add another problem** puts several issues in one report. |
| After sign off | Tick animation, then back to Home, where the job shows as *Signed off. Being checked* (faded). |
| Messages | Filter pills, **+** for a new message (WhatsApp-style), chat. |
| Notifications | Bell on Home. All / Unread filters work. |
| Training | **Keep going**, **To do** and **Completed**. A course shows numbered **Lessons** (ticks for done, quiz unlocks after lesson 4). A lesson plays its video; **In this lesson** lists the chapters with times. **Next lesson** moves on. The full screen button fills the screen. |
| Results and feedback | Home banner or Profile → **Results and feedback**: what needs fixing, with **Mark as fixed** and **Message Mary**. |
| Profile | Settings, results, sign out. |

The phone's back button and swipe-back work. Each screen has its own address, for example `#TaskSteps`.

## How the file is built (for editing in a new Claude chat)

- **Screens:** each screen is a `<section class="screen" id="s-NAME">` inside `index.html`. Only one is shown at a time.
- **Navigation:** any element with `data-go="NAME"` opens the screen called `NAME`, for example `data-go="TaskSteps"`. There are no real links, so previews never ask to open an external page. The router script at the bottom of the file handles this.
- **Brand:** Medclean, blue #0071F2, font Urbanist (embedded in the file), wordmark with a drawn slanted M on the launch screen (`#splash`).
- **Photos:** seven training photos, colour-graded to the brand blues, are stored once as CSS classes `.ph-a` to `.ph-g` and reused on Training, Course, the lesson player and Watch how.
- **Look:** one card style everywhere: soft grey, no outline or shadow, 16px corners. Buttons, inputs and chips have 12px corners; only avatars, icons, badges and dots are round. Status is shown by tint only: red needs a fix, amber being checked, green done, blue in progress. Lists of people and items (Messages, Notifications, Training) are plain rows.
- **Design system:** colours, type and spacing are CSS variables at the top of the `<style>` block, for example `--primary`, `--danger`, `--surface-sunken`. The tokens come first, then the component styles (`.hh-…` classes).
- **Button states:** hover, pressed, focus and disabled are defined once, in the "Interaction states" section of the CSS (`.hh-press-solid`, `.hh-press-soft`, `.hh-press-outline`, `.hh-press-card`).
- **Text links:** always underlined, using `.hh-textlink`.
- **Animations:**
  - Accept job: CSS classes `.hh-prio` and `.hh-acc`, switched on by `.is-accepted`.
  - Task done on Home: `.hh-pend` and `.hh-jd`, switched on by `.just-done`.
  - Contact timer: the `.hh-soak-start` handler in the script.
  - Report screen, camera and Send to dropdown: the `Rep` module in the script.
  - Demo clock: `nowMin` moves on each time a job is signed off.
  - Scan auto-advance and the done screen returning to Home: `onEnter()` in the script.
- **Responsive:** layouts grow from phone to tablet with container queries on `.hh-viewport`, at breakpoints of 600px, 840px and 1024px.

### Prompt to start a new chat

> Here is my hygiene staff app prototype (index.html). It's a single-file click-through prototype. Screens are `<section class="screen" id="s-NAME">`, elements with `data-go="NAME"` navigate between them, and the design tokens are CSS variables at the top. Keep every animation, the button states and the underlined text links. I want to change: …

## Notes

- Demo data only: no real backend, so nothing is saved or sent. Photos stay on your device and clear when you go back to Home.
- The timer runs at demo speed (10 minutes in about 4 seconds).
- Course pictures are drawn icons. Swap in real photos by replacing the thumbnails in the Training section.
- Chemical, colour-coding and PPE details follow the HSE guidance used in the design. Check them against your local policy before using them with staff.

## Brand

Blue `#0071F2`. Font: Urbanist (embedded). Change the blue in one place: `--primary` at the top of `index.html`; gradients, tints and the scan photo follow it.

## Wording

Short labels have no full stops. Chemical strengths follow common UK and Irish practice: chlorine 1,000 ppm for routine and isolation cleans, 10,000 ppm granules for blood spills. Contact times follow the product label. Check all clinical details against your hospital's own policy before real use.
