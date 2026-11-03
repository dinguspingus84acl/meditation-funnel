# Quiet Mind Check v5.1: page setup + full reminder sequence

Files to upload together (same folder): `index.html`, `quiet-mind-vsl.mp4` (720p web cut, 1:20, ~8 MB), `vsl-poster.jpg`. The video and poster are unchanged from v4.
Backups: the v4 page and notes (ElevenLabs Sarah voice) are saved as `index_v4.html`, `NOTES_v4.md`, `quiet-mind-vsl_v4.mp4`, `vsl-poster_v4.jpg`. the previous v4 (Kokoro voice) page, notes, video, and poster are saved as `index_v4kokoro.html`, `NOTES_v4kokoro.md`, `quiet-mind-vsl_v4kokoro.mp4`, `vsl-poster_v4kokoro.jpg`. The v3 page, notes, video, and poster are saved as `index_v3.html`, `NOTES_v3.md`, `quiet-mind-vsl_v3.mp4`, and `vsl-poster_v3.jpg`. The older v2 files are the `_v3prev` copies.

## What changed in v5.1 (real scarcity: countdown + live seats)
Backups of v5 before this change: `index_v5.html`, `NOTES_v5.md`.
- **Live countdown** (days / hrs / min / sec):
  - Counts down to the person's real `{{session_date_iso}}`. It sits near the top, under the subhead, and a compact copy appears in the sticky mobile button bar ("Live in 2d 04:12:31").
  - **At 0** it turns into "🔴 We're live, join now", with a **Join the live class** button pointing to `{{join_link}}`. The sticky bar switches to the join button and stays visible.
  - If there's no join link, it says the link is in their texts and email.
  - 75 minutes after the start it shows "This session has ended" with a link to pick another session.
  - If `{{session_date_iso}}` isn't merged, the countdown **hides**. It never counts down to the placeholder fallback date (the calendar buttons still use that fallback).
- **Live seats bar:**
  - Driven by `{{seats_left}}` / `{{seats_total}}`, e.g. "Only 23 of 100 live seats left for Wednesday, Nov 4", with a "77% taken" fill.
  - **Urgent red styling** at ≤20 seats left or ≤25% left. Above that it's neutral amber with no "Only" ("80 of 100 live seats left…").
  - At 0 left it shows "All 100 live seats … are taken. Yours is saved."
  - **The whole bar hides** if either field is empty, unmerged, non-numeric, total is 0, or left > total. There is no hardcoded fallback number anywhere.
  - The number never ticks down on the page. It's the snapshot GHL merged at page load.
  - The bar hides once class is live.
- **Room line** (under the bar): "Live seats are capped so Shalin can guide the practice and take questions. The guided practice and live Q&A only happen in the room, for the people who show up." This matches the replay FAQ ("the part a recording can't give you").
- **WAIT / action-needed note** at the top of the opt-in card:
  - "**Wait, live seats fill.** Your seat is saved. Finish this step to confirm you're coming, and your join link lands on your phone so you don't miss the start."
  - The pill still reads "Seat saved · Action needed: 1 step left".
  - Nothing says the seat will be cancelled or released. **If you ever do release unconfirmed seats, tell me and the copy can say so honestly. Until then, don't claim it.**
  - The note hides once class is live.
- **Payload:** one additive field, `seats_left_seen` (the number this person saw, or null if the bar was hidden). Use it to test whether lower numbers lift opt-ins or show rate.

## What changed in v5 (diagnostic-page upgrade; playbook in `quiet-mind-v5/julius-playbook.md`)
- **Text-first opt-in (the big one):**
  - On phones, "💬 Text me my Quiet Mind score" opens their texting app with their result pre-filled, addressed to `{{sms_number}}`. The person texts you first.
  - That opens a two-way thread, and your join link arrives in a conversation they started.
  - The typed-number form is still there: one tap away on phones ("Rather type your number?"), on desktop, and whenever `{{sms_number}}` isn't set.
- **Ask moved directly under the result.** The 3 placeholder testimonials now sit just below the opt-in card. They're still flagged.
- **Copy & paste assessment:** a box with their result, so desktop users or anyone without a texting app can paste it anywhere.
- **Honest pattern interrupt:**
  - Pill: "Seat saved · Action needed: 1 step left".
  - H1: "Wait, {{first_name}}. Before you close this tab, find out what's actually keeping your mind on at night."
  - There is no cancellation threat. The seat really is saved.
- **Justified asks:** "Why these 4?" under the grid, "Why text?" by the button, and "P.S. We hate spam as much as you do. We only text about this class…"
- **Diagnosis framing:**
  - The result is labeled "Your Quiet Mind Check result · a self-check, not a medical assessment".
  - The red/yellow list is headed "Where your nights leak rest, and where the class covers it".
- **Mechanism bridge ("Why this works")** before "What happens live". It opens with implied framing ("You already know 'just relax' doesn't work at 11 p.m."), blames the advice instead of the reader, and lands on the VSL payoff line.
- **Identity + soft loss line** in the exhausted block: "You're the one who holds everything together all day. For 75 minutes, nobody needs anything from you… Skip it, and tonight runs like last night."
- **FAQ:**
  - Why it's free (justified).
  - Validate-then-wedge apps answer.
  - New questions: "Is this religious?", "Will this work for my situation?", "What if I nod off?", "What if my kids or partner interrupt?".
  - FAQ opens are tracked in the payload.
- **Tracking:** Meta Pixel custom events with `event_id`, plus `fbp`/`fbc` in the payload for CAPI dedupe and match quality. There's a slot for Microsoft Clarity.
- **AI-tell cleanup:** the "No … No … No" tag stack and "It's a signal problem, not a discipline problem" contrast lines are rewritten.
- `STORE_KEY` is now `qmc_answers_v5`. The webhook payload keeps every v2–v4 field; the new fields are additive.
- **Video: no rebuild.** The VSL's CTA ("Tap 'Text me my Quiet Mind score'") still matches both buttons word for word.

## What changed in v4 (copy + video)
- **The VSL is a full recut: 1:20 (80.2s), down from 1:40.**
  - Hook: "Unclench your jaw. … Was it clenched?" It's the first two words, with no wind-up.
  - About 32 shots (~2.5s each), with punch-ins and kinetic word-by-word captions.
  - A music bed that ducks under the voice, whooshes, and a heartbeat under the quiz.
  - A beat of silence before the payoff: "Your mind doesn't have an off switch. Your body does."
- **Narrator voice: ElevenLabs "Sarah"** (premade: mature, reassuring, confident; American female).
  - Model `eleven_multilingual_v2`, stability 0.42, similarity 0.75, style 0.15, speed 1.1.
  - Generated line by line with surrounding-text context, plus natural pauses and breaths.
  - It is not a clone of Shalin or anyone real; the narrator talks *about* Shalin.
  - It replaced the earlier Kokoro af_heart voice, which is backed up as `_v4kokoro`.
- **Quiz rows are read as fast either/or questions** that match the buttons word for word (about 4.5s per row instead of 8–10s).
- **Page copy rewrite:**
  - New headline and sub
  - Video cover: "Unclench your jaw. Now press play."
  - Shorter button descriptors
  - "4 taps to unlock" locked state
  - One-line type descriptions
  - SMS card: "Get your score by text, plus the 2-minute prep"
  - Exhausted block: "Good. Come anyway."
- **Tech:**
  - `ROW_CUES` were re-timed to the new video.
  - `STORE_KEY` was bumped to `qmc_answers_v4`, so no stale v3 answers carry over.
  - The webhook payload is unchanged.
- **SMS:** S1, S2, S3, S4 (replay), S6, and S7 were rewritten. The prep line moved out of S2 into its own text 2 hrs before (S5b).
- The rationale and line-by-line critique are in `quiet-mind-v4/copy.md`.

## What changed in v3 vs v2 (short)

- **Angle is now "The Jaw Test".** The video opens with a 10-second felt experience, the quiz shows your **Quiet Mind type** (worst row), and the class is "that, for 10 minutes, guided live."
- **Honest status.** "Spot booked · 1 step left" replaces "seat isn't fully saved".
- **Mobile flow:**
  - The video is above the fold and **pins to the top while playing**.
  - The row being read lights up ("GRADE NOW") and scrolls into place.
  - Each choice button carries a plain-English descriptor.
- **Results by type, plus extras:**
  - A score band line and "In your session, watch for…" per red/yellow row.
  - A per-type **2-minute prep**.
  - An optional **before-number** (1–10 mind noise), which gets re-checked after the live practice.
- **SMS opt-in reframed** around what they get: type + score, prep, join link.
- **After submit:** a "make it real" checklist (calendar, pick your spot, tell your household) and a "send a free seat" share.
- **Objections handled:** camera optional, sweatpants welcome, "Exhausted? Come anyway", pitch disclosed upfront, "apps never worked for me", replay.

## Replace before launch: placeholder testimonials
The page has **3 placeholder attendee quotes**. They're in `#voices`, between the result card and the "Text me my Quiet Mind score" opt-in, which is the moment right before the ask where trust matters most.
- **They're written, not real.** Do NOT launch with them. They're not used in the video, the texts, or the emails, and they shouldn't be.
- **How they're marked:**
  - Each one sits under an HTML comment, `<!-- PLACEHOLDER TESTIMONIAL: replace before launch -->`.
  - Each `<figure>` has `data-placeholder="true"`.
  - That attribute also shows a **red dashed outline + "PLACEHOLDER" badge** on the page, and a console note, so they can't ship unnoticed.
- **To finish:** paste the real quote and name, then delete `data-placeholder="true"` from that `<figure>`. The red flag disappears.
- **What to collect:** quotes from real past attendees, with written permission to use first name + last initial + city/role.
  - Keep them about the in-class experience (what they noticed during or after the 10 minutes).
  - No sleep-cure, anxiety, or medical claims (Meta + brand risk).
  - Keep "Individual experiences vary." under them.
  - If you only get 1–2 real ones, delete the remaining placeholders rather than keeping them.

| # | Placeholder quote | Placeholder attribution |
|---|---|---|
| 1 | "I came in expecting another 'just relax' talk. Partway through the breathing I realized my shoulders had come down from my ears, and I hadn't noticed them drop." | Dana R. · project manager, Columbus, OH |
| 2 | "Camera off, on my couch in sweats. Nobody put me on the spot. I just followed along, and for the first time all week my head wasn't running the day back." | Mike T. · Indianapolis, IN |
| 3 | "I'd quit two meditation apps. Starting with the body instead of trying to clear my head was the first time it didn't feel like I was failing at it." | Priya S. · nurse, Edison, NJ |

## Merge fields (URL params of the same name also work as fallback)
| Field | Where | Example |
|---|---|---|
| `{{first_name}}` | Headline ("You're in, Maria.") | Maria |
| `{{session_day}}` | Session strip, success message | Wednesday, Nov 4 |
| `{{session_time}}` | Session strip, SMS card, checklist (ET appended) | 8:30 PM |
| `{{session_date_iso}}` | JS `SESSION_ISO` → .ics + Google Calendar | 2026-11-04T20:30:00-05:00 |
| `{{join_link}}` | JS `JOIN_LINK` → calendar location/description | https://zoom.us/j/... |
| `{{phone}}` | Phone input prefill | +13175550123 |
| `{{rebook_link}}` | FAQ "Can I just watch a replay?" → switch session | https://go.us.artofliving.org/quiet-mind-method |
| `{{contact_id}}` | Payload (or `?contact_id=`); also the `ref` in the pre-filled text | GHL contact ID |
| `{{seats_left}}` *(v5.1)* | Seats bar + sticky bar. Whole number of **real** live seats still open for this person's session. Empty/unmerged → bar hidden. | 23 |
| `{{seats_total}}` *(v5.1)* | Seats bar. The **real** live-seat cap for that session. Must be ≥ `seats_left`, or the bar hides. | 100 |
| `{{sms_number}}` *(new in v5)* | Text-first button: the number the pre-filled text goes to. It must be the same GHL number your reminders come from (ideally iMessage-capable, see below). If it's not set, the button hides and the form shows. | +13175550100 |

The yellow highlight disappears automatically once a field is filled. Easiest GHL setup: redirect after registration to
`.../confirm?first_name={{contact.first_name}}&session_day=...&session_time=...&session_date_iso=...&phone={{contact.phone}}&contact_id={{contact.id}}&join_link=...&sms_number=%2B1XXXXXXXXXX&seats_left=...&seats_total=...`.

### How to populate `{{seats_left}}` / `{{seats_total}}` in GHL (pick one)
1. **Calendar capacity (best, automatic):**
   - If registrations book into a GHL **class/group calendar** (one slot per session), set the slot's **capacity** (e.g. 100) to the real live cap.
   - `seats_total` = that capacity.
   - `seats_left` = capacity minus confirmed (non-cancelled) appointments for that slot.
   - In the post-registration workflow, read the count with a Custom Code / webhook step (GHL API: calendar events for the slot, count status ≠ cancelled), compute `capacity - count`, and put it in the redirect URL or a contact field.
   - Compute it **after** this person's booking is counted, so their own seat is already taken.
2. **Custom Values per session (simple):**
   - Create Custom Values like `qm_seats_total_nov4` and `qm_seats_left_nov4`.
   - A workflow step (Math/Update Custom Value) decreases `seats_left` by 1 on each registration for that session, and adds it back on a cancellation.
   - Merge the right session's values into the redirect URL.
3. **Webinar platform cap:** if the cap comes from Zoom/WebinarJam room size, use that size as `seats_total` and the platform's registrant count for `seats_left`, pulled via Zapier/Make at redirect time.

**Rules:**
- **Only merge real numbers.** If the count can't be computed for a session, leave the fields empty and the bar disappears on its own.
- Never type a "looks urgent" number.
- If capacity goes up (bigger room), update `seats_total` too, so "Only 23 of 100" stays true.
- **Time zone:** `{{session_date_iso}}` must include its offset (e.g. `2026-11-04T20:30:00-05:00`). The countdown runs in each visitor's own clock.
Other URL params read: `utm_*`, `sleep|replay|tension|energy=green|yellow|red`, `q`.

## Config (top of `<script>`)
- `WEBHOOK_URL`: swap for your GHL Inbound Webhook / Zapier / Make URL.
- `ROW_CUES = [29.4, 35.2, 40.2, 45.4, 50.6]` (v4 ElevenLabs video): the seconds where each row is read in the VSL; the last value is the end of row 4. They drive the "GRADE NOW" highlight. Only change them if the video is re-edited.
- `SHARE_URL`: the link the "send a free seat" button shares (the funnel).
- `SEATS_LEFT`, `SEATS_TOTAL` (v5.1): the merge tags; URL params `seats_left` / `seats_total` also work. `SEATS_LOW_PCT = 0.25`, `SEATS_LOW_ABS = 20` set when the bar turns red.
- **Video:** self-hosted `<video>` by default, which is what makes the sync work. To use Vimeo/YouTube, swap per the HTML comment; sync highlighting turns off and everything else still works.

## Webhook payload (POST JSON). Same shape as v2, plus additive fields (3 from v3, 6 from v5)
```json
{
  "scores": {"sleep":"yellow","replay":"red","tension":"red","energy":"green"},
  "score_total": 3,
  "reds": ["replay","tension"],
  "yellows": ["sleep"],
  "question": "I replay work calls at 2am",
  "phone": "+13175550123",
  "sms_consent": true,
  "contact_id": null,
  "utm": {},
  "tag": "qmc_completed",
  "submitted_at": "2026-10-31T21:14:26.610Z",
  "page": "https://.../confirm?...",
  "top_signal": "replay",
  "quiet_mind_type": "The Replayer",
  "mind_noise_before": 8,
  "channel": "form",
  "ref": "abc123",
  "faq_opened": ["catch","religion"],
  "event_id": "qmc-abc123-QuietMindTextOptIn",
  "fbp": "fb.1.1730000000000.123456789",
  "fbc": null,
  "seats_left_seen": 23
}
```
- **v5 `channel`:**
  - `"form"`: they typed their number. `phone` is set and `sms_consent: true`, same as v4.
  - `"sms_link"`: they tapped the text-first button. `phone: null` and `sms_consent: false`, because consent comes when they actually **send** the text (the button's disclosure covers it).
  - For `sms_link`, GHL learns their number from the inbound text. Match it using `ref` (it appears in the text as `QMC-<ref>`, and equals `contact_id` when one is passed).
- **`faq_opened`:** which objections they checked. Use it to pick the S4 angle, and to spot which objections the VSL should answer.
- **`event_id` / `fbp` / `fbc`:** for a Conversions API copy of the opt-in event, deduplicated against the browser pixel. See "Tracking" below.
- **Scoring:** green=2, yellow=1, red=0, out of 8.
- **Type:** the first red row, in order sleep → replay → tension → energy. If there are no reds, the first yellow. If everything is green, "The Steady".
- **Types:**
  - `sleep` → The Wide-Awake
  - `replay` → The Replayer
  - `tension` → The Braced
  - `energy` → The Running-on-Empty
  - all green → The Steady
- **GHL custom fields:** `qm_type`, `qm_top_signal`, `qm_score`, `qm_noise_before`, `qm_question`, and one field per row.
- **Tags:** `qmc_completed`, plus `qmc_red_<row>` per red and `qmc_top_<row>`.

### Prep line by top signal (`{{prep_line}}`, same as on the page)
- **sleep:** "Join from wherever you can get most comfortable. Comfort is the point, not a reward."
- **replay:** "5 minutes before class, write tomorrow's list on paper, so it's out of your head for 75 minutes."
- **tension:** "Right before class, rate your jaw and shoulder tension 1–10. Check again after the practice."
- **energy:** "Eat beforehand and keep water nearby, so you can simply settle in."
- **steady/none:** "Notice what your body does on your next stressful evening. Bring that moment to class."

---

## Tracking (v5)
- **Meta Pixel:** if the pixel is installed on this page, it fires:
  - `QuietMindCheckComplete` once, when all 4 rows are graded;
  - `QuietMindTextOptIn` on either opt-in path.
  - Each fires with `eventID = qmc-<ref>-<EventName>`.
- **Conversions API:**
  - From GHL/Zapier, send `QuietMindTextOptIn` with the same `event_id`.
  - Hash phone/email/first name/zip, and include `fbp`/`fbc` and the client IP/user agent from the original registration.
  - Aim for an Event Match Quality of 8+ on whatever event you optimize for.
  - Optimizing the campaign for this deeper event (instead of raw registrations) is worth a test once you have ~50/week.
- **FAQ click tracking:**
  - Paste your Microsoft Clarity snippet where the HTML comment says.
  - After ~300 visits, check which FAQ rows get clicked the most, and also look at `faq_opened` in the webhook data.
  - The top 1–2 objections belong in the ad, the VSL, or the hero copy.

## Full reminder sequence

`{{confirm_link}}` = this page with the contact's params.
`{{type}}` = `quiet_mind_type`.
`{{top_label}}` = the name of their worst row: falling asleep / the replay / body tension / mornings.
Keep every SMS under 160 characters where possible. STOP language goes on the first message per your carrier setup.

### SMS
**Sender (recommended, Jack's call):** use an iMessage-capable sender on the GHL number, so people see blue bubbles and links stay clickable. @iblamejulius uses Blooio (~$89/mo, direct GHL integration) instead of Sendblue (~$1k/mo). He *estimates* it lifts show-up by 20%+; that's his estimate, not a measured result.

**The "we only text about this class" promise (the page's P.S.):**
- S0–S9 below are all class logistics.
- Don't add promo or sales texts to this list.
- Sell the paid course by email, or live in the session.

| # | When | Who | Message |
|---|---|---|---|
| S0 *(new in v5)* | **Inbound text containing "QMC-"** (from the text-first button) | Texted in | Reply within a minute: "Got it, {{first_name}} ✅ You're {{type}}. Your 2-min prep: {{prep_line}} Your link comes 1 hr before {{session_time}} ET, right here. Reply STOP to opt out." Then tag `qmc_completed` + `qmc_texted_in`, parse the type/score into the custom fields (or just use the webhook data matched by `ref`), and **skip S2** for this person. |
| S1 | Instantly on registration (**replaces your current confirmation**) | All | {{first_name}}, you're in ✅ {{session_day}} {{session_time}} ET, free + live with Shalin. Quick test first: unclench your jaw. Then find your Quiet Mind type (60 sec): {{confirm_link}} |
| S2 | Instantly on quiz submit (webhook, `channel: form` only) | Form completers | {{first_name}}, you're {{type}}. Your loudest signal: {{top_label}}. Shalin covers it live {{session_day}} {{session_time}} ET. Coming? Reply YES and I'll text your link 1 hr before. |
| S2a | Auto-reply to YES | Replied YES | Love it. Your link comes 1 hr before. Camera optional, sweatpants welcome. |
| S3 | 2 hrs after registration, **only if no `qmc_completed`** (skip if class is < 3 hrs away) | Non-completers | {{first_name}}, 4 taps, 60 sec: find out what's actually keeping your mind on at night. {{confirm_link}} |
| S4 | 24 hrs before (**branch on `top_signal`**) | All | see variants below |
| S5 *(new, optional, evening sessions)* | Morning of, 9:00 AM local | All | Tonight {{session_time}} ET. If you're wiped out by then, good: come anyway. That's who this is for. Couch + camera off is fine. |
| S5b *(new in v4)* | 2 hrs before | Completers | Your 2-min prep for today's class, {{first_name}}: {{prep_line}} |
| S6 | 1 hr before | All | 1 hour, {{first_name}}. Your before-number: {{mind_noise_before}}/10. Let's see what 10 minutes does to it. Couch is fine: {{join_link}} *(no number: "In 1 hour, {{first_name}}. Comfy spot, water, phone on silent once you join: {{join_link}}")* |
| S7 | 3 min before | All | We're live. Camera off is fine, just tap: {{join_link}} |
| S8 *(new)* | +10 min, **only to registrants not yet in the room** (Zoom/WebinarJam attendance → GHL) | No-shows so far | We've started, and the guided practice is still ahead. Join now: {{join_link}} *(only send if the practice really is later than minute 10, which matches the Recognize → Understand → Experience order)* |
| S9 | +30 min after end, no-shows only | Didn't attend | Missed it, {{first_name}}? No stress. Pick the next live room (your answers are saved): {{rebook_link}} |

**S4 (24h) variants by `top_signal`:**
- **replay:** "Tomorrow {{session_time}} ET. You said your mind loops all night. Here's why \"stop thinking\" makes it louder, and what Shalin has you try instead, live."
- **sleep:** "Tomorrow {{session_time}} ET: falling asleep is your loudest signal. You'll see why trying harder backfires, then try a different starting point, live."
- **tension:** "Tomorrow {{session_time}} ET: you marked body tension. Before class, rate your jaw + shoulders 1–10. After the 10-min practice, rate again."
- **energy:** "Tomorrow {{session_time}} ET: you said mornings start drained. Tomorrow is about your evenings, and setting up the night differently."
- **no quiz / steady:** "Tomorrow {{session_time}} ET: why you can't think your way out of a racing mind, and 10 minutes to feel the difference. Free, live, camera optional."
- **Sunday 11 AM sessions:** same copy (nothing says "tonight").

**Question follow-up** (if `question` isn't empty, sent with S4): "Shalin's team got your question. Bring it live, there's Q&A at the end."

### Emails
**E1: instantly on registration**
- **Subject:** You're in, {{first_name}} (one 90-second step left)
- **Preheader:** Find your Quiet Mind type before class.
- **Body:**
  > Before you do anything else, unclench your jaw.
  >
  > Did your shoulders drop a little too? Your mind didn't decide that. Your body did. That's the whole idea behind **The Quiet Mind Method**, and you'll feel it for 10 full minutes, live with Shalin Desai.
  >
  > 📅 {{session_day}} · {{session_time}} ET · online · about 75 min · free
  > [Add to calendar]
  >
  > **One step left:** take the 90-second Quiet Mind Check. You'll get your Quiet Mind type, and Shalin's team reviews the room's answers before class.
  > **[Find my Quiet Mind type →]** ({{confirm_link}})
  >
  > P.S. Camera optional, no experience needed. At the end, Shalin shares the optional paid course (SKY Breath Meditation Part 1). You'll know what it is and costs. No pressure.

**E2: 24 hrs before (skip if registered < 24 hrs out)**
- **Subject:** Why "stop thinking" makes it louder
- **Preheader:** And what you'll try instead, {{session_day}}.
- **Body:**
  > It's 9:30. The house is finally quiet. And your mind picks that exact moment to reopen a conversation you can't change.
  >
  > So you tell it to stop. But "stop thinking" is just one more thought, and the replay gets louder.
  >
  > That's not you failing. Pushing a racing mind toward quiet is more mental effort. **The Body-to-Mind Shift starts one level down:** settle the body first, and the mind takes its cue.
  >
  > Here's what happens live with Shalin:
  > 1. **Recognize:** why discipline was never going to fix it
  > 2. **Understand:** why sequence beats effort
  > 3. **Experience:** a 10-minute guided breathing practice (the part a recording can't give you)
  > 4. **Reflect:** check your before-number, ask your question
  >
  > *"Meditation never works for me."* That's who this was built for. You don't need to clear your mind first.
  >
  > **[Add to calendar]** · {{session_day}} {{session_time}} ET
  > {{#if no qmc_completed}}Haven't taken the 90-second check yet? [Find my Quiet Mind type →]{{/if}}

**E3: 1 hr before**
- **Subject:** Starting in 1 hour (your link inside)
- **Preheader:** Comfy spot, camera optional.
- **Body:**
  > **[Join the live session →]** ({{join_link}})
  >
  > 60-second prep:
  > - Pick a comfortable spot (couch is fine)
  > - Water nearby
  > - Phone on silent once you've joined
  > {{#if mind_noise_before}}Your before-number: **{{mind_noise_before}}/10**. We'll check it again right after the practice.{{/if}}
  >
  > Exhausted? Come anyway. You don't need to bring any energy. Just be there.

**E4: no-shows, 1 hr after end**
- **Subject:** We missed you. Here's the next live room
- **Body:** "No guilt, evenings happen. The next live Quiet Mind session is [day/time]. Your answers are saved. [Pick my time →] ({{rebook_link}})"

---

## Show-rate tracking (as in v2, plus the new fields)
**v5 split to watch first:** show rate for `channel: sms_link` (texted in) vs `form` vs no quiz.
1. Tag attendees `attended_{{session_date}}` from the Zoom/WebinarJam export or integration.
2. **Show rate = attended / registered.** Split by:
   - `qmc_completed` vs not
   - replied YES vs not
   - `mind_noise_before` set vs not
   - `top_signal`
3. **Leading indicators to watch:**
   - page → quiz completion rate (target 50%+)
   - quiz → SMS opt-in
   - YES replies
4. Give it 2–3 cohorts (~100+ registrants each) before calling it against v3 (and v3 against v2).

## Before going live: things only you can confirm
- [ ] **Set `{{sms_number}}`** to the GHL number your reminders come from, and build S0 (inbound "QMC-" text → reply + tags). Test it on an iPhone and an Android: the text should arrive pre-filled.
- [ ] **Seat numbers are real (v5.1):**
  - For each session, check that `seats_total` equals the actual live cap (calendar capacity, room size, or what Art of Living sets).
  - Check that `seats_left` equals that cap minus real registrations: register a test contact and confirm the number drops by exactly 1. Cancel it and confirm it goes back up.
  - Spot-check the live page against the GHL calendar count once a day during launch.
  - If you can't make it exact, leave the fields empty (the bar hides).
- [ ] **Countdown:** confirm `{{session_date_iso}}` has the right date, time, and offset for each session. Open the page with `?session_date_iso=<a time 1 minute from now>&join_link=...` and watch it switch to "We're live, join now".
- [ ] **Keep the P.S. true:** only class texts go to this list (see SMS above).
- [ ] **"Is this religious?" answer:** confirm Art of Living is comfortable with "No beliefs required… you don't need any particular faith or background to take part."
- [ ] **Pixel/CAPI:** confirm the pixel is on this page, and map `event_id`/`fbp`/`fbc` into your CAPI sender.
- [ ] **Replace or delete the 3 placeholder testimonials** (see "Replace before launch" above).
- [ ] **Listen to the v4 narrator voice** (ElevenLabs Sarah) before launch.
  - The other candidates' samples are in `quiet-mind-v4/el/`: Bella (female), Brian (male), and Jon "Warm & Grounded Storyteller" (male, the runner-up).
  - Swapping voices costs about 1,300 characters plus a re-render.
  - Using an ElevenLabs voice in a paid ad requires a paid ElevenLabs plan (Starter or higher) for commercial rights. This account is on Starter.
- [ ] **"Shalin Desai" pronunciation:** the VO is fed the respelling "Shah-leen Deh-sigh" (in `build4/voices.py`, `el_text`), and Whisper transcribes it as "Shaleen Desai". Confirm that's how Shalin says his name; if not, adjust the respelling and re-voice that one line.
- [ ] **9:16 social cut:** the VO says "right below the video". That's true on this page, but not as a Reels/Stories ad. Use the 9:16 only where the page sits below it, or ask for a social variant ("tap the link").
- [ ] **AI b-roll:** some shots are AI-generated (bedroom, clock, jaw macro, laptop). Disclose them if your ad policy requires it.
- [ ] **Camera optional:** true on your platform? (The page, video, and texts say so.)
- [ ] **Replay policy:** if there's **no** replay, change the FAQ to say so plainly. It's the strongest honest show-up lever.
- [ ] **"Shalin's team reviews the room's answers before class":** set up a simple pre-class export (counts by type + top questions) so that line stays true.
- [ ] **S8 "the practice is still ahead":** confirm the practice starts after minute 10.
- [ ] **Funnel page:** it still shows "$397 / $7.99" next to "Book my free session", and some session dates have passed. Fix those; the conflict hurts trust before anyone reaches this page.
