# DSH development brief — Shanhaijing Creature Test (English web version)

> For DSH only: Build v0.1 from this brief alone. Do not add features that are not listed here. If anything is unclear or missing, stop that item, write down the question, and finish everything else first.

## How to use this brief

- Product name (working title): **Which Shanhaijing Creature Are You?**
- Version to build: **v0.1 — validation prototype**
- Deliverable: one deployable static web page, plus the return package listed in the last section
- Language of the product: English only. No Chinese UI, except each creature's Chinese name shown as a cultural label next to its English name
- The person who gave you this brief only accepts or rejects the result. Do not ask that person to make product decisions that this brief has already made

## Goal and target users

One-sentence goal: a 30-second English personality quiz that matches overseas Gen Z users to one of eight creatures from the Shanhaijing tradition, gives them a shareable result card, and tests whether they click to unlock more — with no app, no login, and no social account needed to start.

Target users:

- Gen Z and young millennials outside mainland China who already enjoy MBTI, astrology and personality quizzes
- First validation group: overseas Chinese students and young professionals who can read English and already recognise the creature names
- Second group, only after v0.1 passes: English-speaking users with no Chinese background, reached through Reddit, Discord and short video

Form factor decision, already made:

- Web page / H5 first. Not an app, not a WeChat mini-program
- One link must be enough to play, share and test payment intent
- No overseas social account exists yet, so the page must work when sent by direct link, group chat, Reddit or Discord post

## Positioning and compliance — non-negotiable

Positioning in every headline, button and card:

- **Oriental Wisdom + Self-Exploration**
- Approved framing words: personality quiz, self-discovery, mirror, strengths, blind spot, reflection, ancient stories, mythical creatures, for fun, share with friends

Banned words and claims — never in user-facing copy, page title, meta description, share text or button text:

- fortune telling, fortune teller, divination, psychic, astrology reading
- predict your fate, your destiny is, fated to, destined to
- guaranteed, guaranteed results, 100% accurate, scientifically proven
- cure, heal your, treatment, diagnosis, fixes your anxiety
- make you rich, attract wealth, bring you luck, change your luck, remove bad luck
- Chinese equivalents of any of the above if any Chinese text is added later

Also banned in v0.1:

- No birth date, birth time, birth place, gender, email or phone collection
- No medical, financial, relationship-outcome or career-outcome advice
- No claim that a result is a fact about the user. Use soft language: "This creature mirrors a side of you", "Many people with this result say…"

Required footer disclaimer, copy exactly, visible on the start screen and the result screen:

> For entertainment and self-exploration only. Inspired by ancient Chinese mythology. This quiz does not predict your future and is not medical, financial or professional advice.

Required source line, copy exactly, on the result screen:

> Creature stories are inspired by the Shanhaijing (Classic of Mountains and Seas) and later Chinese folklore retellings. Wording here is a modern retelling, not a translation of an original passage.

## Tech constraints and delivery format

Build rules:

- Single-page static site. One `index.html` file with inline CSS and inline JavaScript is preferred for v0.1, so it can be deployed and edited without a build step
- If you use Vite or another static builder instead, the final output must still be a static folder that works when opened from any plain static host, with no server code
- No backend, no login, no database, no account system, no server-side storage
- Browser storage only: `localStorage`, using exactly the keys defined in this brief
- No external fonts, trackers, image files or libraries loaded from a third-party address in v0.1. Creature visuals must be inline SVG, CSS shapes or CSS gradients made by you in the file
- Do not invent a domain, a shop link or a payment link. Use the placeholder constants below and nothing else

Config constants — put all of these at the top of the script, in one `CONFIG` object, so later rounds only edit this block:

```js
const CONFIG = {
  SITE_URL: "TBD",              // replace with the real deployed URL after deployment
  REPORT_SKU: "full-report",
  REPORT_PRICE_USD: 3.99,
  REPORT_PAYMENT_URL: "TBD",    // Stripe payment link to be supplied later
  SUB_SKU: "daily-plus",
  SUB_PRICE_USD: 8.99,          // per month
  SUB_PAYMENT_URL: "TBD",       // Stripe payment link to be supplied later
  BOX_SKU: "mystery-box",
  BOX_PRICE_USD: 19.90,
  BOX_PAYMENT_URL: "TBD",       // shop / pre-order link to be supplied later
  DEBUG_PARAM: "debug"
};
```

Performance and layout:

- Mobile first. Design for 360–430 px wide phones first, then check desktop
- No horizontal scroll at 360 px, 390 px or desktop width
- First screen usable without scrolling on a typical phone: title, one-line intro, Start button
- Page must be interactive in under 2 seconds on a normal mobile connection. Keep the whole file under 500 KB
- Tap targets at least 44 px high. Body text at least 16 px
- Works on current mobile Safari and Chrome. No feature that only works in one browser, unless a simple fallback is provided

Visual direction:

- Dark ink background, warm gold and jade accents, one accent colour per creature (defined per creature below in the copy section as its element colour in your CSS: use gold, teal, coral, sky, violet, ember orange, sage green, storm blue)
- Clean, playful, card-based. It should feel like a modern quiz game, not a temple, not a horror game, and not a textbook
- Each creature gets a simple, recognisable silhouette or symbol in SVG or CSS. Abstract is fine in v0.1. Do not use copyrighted artwork and do not hot-link images

## Flow — three screens plus two return hooks

### Screen 1 — Start

Show, in this order:

- Title: "Which Shanhaijing Creature Are You?"
- Subtitle: "An Eastern MBTI-style mirror — 5 questions, 30 seconds, 8 ancient creatures."
- Three small points: "5 questions" / "No sign-up" / "Get a shareable card"
- Primary button: "Start the quiz"
- Small link: "See all 8 creatures" — opens the collection view (Screen 5)
- Small link: "Today's creature card" — opens the daily card (Screen 4)
- Footer disclaimer, exact wording from the compliance section

Firing `test_started` happens on Start, not on page load.

### Screen 2 — Quiz

- One question per view, 5 questions in fixed order Q1 to Q5, using the exact English text in the next section
- Progress shown as "2 / 5" plus a thin progress bar
- Four options per question, shown as tappable cards labelled A, B, C, D
- Tapping an option records the answer, gives a brief selected state (about 250 ms), then moves to the next question automatically. No Next button, no Back button in v0.1
- No free-text input anywhere in the quiz
- After Q5, show a short reveal state for about 800 ms: "Matching you with your creature…" then go to the result

### Screen 3 — Result

Show, in this order:

- Line: "Your creature mirror is"
- Creature English name (large), Chinese name and pinyin (small)
- Creature visual, element line and its 3 tags as chips
- One-line hook (exact copy from the creature section)
- Strengths paragraph (exact copy)
- Blind spot paragraph (exact copy), headed "Your blind spot"
- Best match line: "Best match: [Creature] — [one-line reason from the creature copy]"
- Today's tip box, headed "One small thing to try today", using that creature's tip copy
- Primary button: "Share my creature" — opens the share actions (see result card spec)
- Secondary button: "See my full report — $3.99" — opens the paywall (see paywall spec)
- Secondary button: "Get this creature — Mystery Box $19.90" — opens the paywall for the box SKU
- Tertiary actions: "Draw today's card" and "Retake the quiz"
- Source line and footer disclaimer, exact wording from the compliance section

Free versus locked, shown clearly on this screen:

- Free: creature name, visual, tags, hook, strengths, blind spot, best match name, today's tip
- Locked preview, visibly blurred or folded behind the paywall button, with these section titles only and no invented body copy filled in: "How you work best", "How you recharge", "Your friendship pattern", "Your creature pairing guide". Under the titles write: "Full report content arrives in v0.2. In this test version, tapping unlock records your interest — no charge is made."

### Screen 4 — Daily creature card

Purpose: give a reason to come back tomorrow. No server, no login.

- Button on start and result screens: "Today's creature card"
- The day's creature is chosen deterministically from the local date string `YYYY-MM-DD`: hash the string (simple sum of character codes is fine), take the hash modulo 8, use that index in the fixed creature order given in the copy section
- Same date always gives the same creature, on the same device and on any device using the same rule
- Show: creature visual, name, one-line hook, and today's tip for that creature
- First draw of the day: button "Save to my collection". After saving, the button becomes "Saved for today — come back tomorrow"
- Store the draw in `localStorage` under the key `shanhai_daily_YYYY-MM-DD` with the creature id as the value. Also update the collection key defined below
- Show a simple streak count: number of different date keys stored, labelled "Days visited: N". Do not call it anything stronger than that

### Screen 5 — Collection (all 8 creatures)

- Grid of 8 creature cards
- Unlocked creature (user got it as a quiz result, or drew it as a daily card): full visual, name and tags, tappable to view its full free copy from the creature section
- Locked creature: silhouette or greyed visual, name hidden as "???", tags hidden, line: "Not discovered yet"
- Collection is stored in `localStorage` under the key `shanhai_collection`, as a JSON array of creature ids, for example `["taotie","kun"]`
- Header shows progress: "Discovered 3 / 8"
- A button under the grid: "Daily cards and full collection — Daily Plus $8.99/mo" — opens the paywall for the subscription SKU

## The 5 questions — exact English text

Do not reword the questions or options. Button labels and headings elsewhere in this brief are also to be used as written.

**Q1. It's Friday night. What sounds best?**

- A. A night market run — try a bit of everything, then dessert
- B. A deep dive at home — a documentary, a wiki rabbit hole, total focus
- C. Getting ready with friends, music on, then heading out together
- D. A quiet project I can finish my way, no rush, no audience

**Q2. You and a close friend disagree. What do you do first?**

- A. Slow it down and make sure nobody leaves feeling unheard
- B. Lay out the facts calmly and work out what actually happened
- C. Crack a joke, read the room, and soften the whole thing
- D. Ask for space, clear my head, then come back to it later

**Q3. Pressure is building. What is your default mode?**

- A. Comfort first — good food, a cozy corner, then I can think again
- B. Small steps, every day. I keep going until the job is done
- C. Full reset — new playlist, new look, clean desk, fresh start
- D. Escape for a bit — a walk, a view, somewhere my mind can wander

**Q4. Pick a place to recharge.**

- A. A windy hilltop before a storm — dramatic and wide open
- B. A colourful festival street, full of art, lights and people
- C. A calm garden where I can help out and take my time
- D. A hidden bookshop with a great cafe attached

**Q5. What do your friends most often say about you?**

- A. "You take charge. People naturally follow your call."
- B. "You're the kind one. Everyone trusts you with the real stuff."
- C. "You're a free spirit. Nobody can quite pin you down."
- D. "You always reinvent yourself. Every season, a new era."

## Scoring — deterministic, no randomness

Rules DSH must implement exactly:

- Each answer gives **+2 points to its primary creature** and **+1 point to its secondary creature**, using the table below
- The result is the creature with the highest total after Q5
- Tie-break, in this order: (1) the tied creature with more primary picks wins; (2) if still tied, the tied creature picked as primary or secondary in Q5 wins; (3) if still tied, use this fixed priority order and pick the earliest: Jingwei, Phoenix, Kun, Baize, Qilin, Yinglong, Nine-Tailed Fox, Taotie
- No random element anywhere in scoring. The same five answers must always give the same creature, on any device
- Fixed creature order for the daily card index and for tie-break lists: 0 Nine-Tailed Fox, 1 Baize, 2 Taotie, 3 Jingwei, 4 Kun, 5 Phoenix, 6 Qilin, 7 Yinglong

Scoring table:

| Question | A primary / secondary | B primary / secondary | C primary / secondary | D primary / secondary |
| --- | --- | --- | --- | --- |
| Q1 | Taotie / Phoenix | Baize / Kun | Nine-Tailed Fox / Yinglong | Jingwei / Qilin |
| Q2 | Qilin / Jingwei | Baize / Yinglong | Nine-Tailed Fox / Qilin | Kun / Phoenix |
| Q3 | Taotie / Qilin | Jingwei / Yinglong | Phoenix / Nine-Tailed Fox | Kun / Baize |
| Q4 | Yinglong / Kun | Phoenix / Nine-Tailed Fox | Qilin / Jingwei | Baize / Taotie |
| Q5 | Yinglong / Baize | Qilin / Phoenix | Kun / Nine-Tailed Fox | Phoenix / Taotie |

Worked examples DSH must self-test:

- Answers A, A, A, D, D give Taotie as the result
- Answers B, B, D, D, A give Baize as the result
- Answers A, A, B, B, C give Jingwei as the result. If your build gives a different result for any of these three examples, report the totals you calculated. Do not change the table to make it fit; report the difference instead

## The 8 creatures — full English copy, ready to paste

Use this copy exactly in the free result. Do not rewrite it, shorten it or add new claims. Creature ids, in fixed order: `nine-tailed-fox`, `baize`, `taotie`, `jingwei`, `kun`, `phoenix`, `qilin`, `yinglong`.

### 1. Nine-Tailed Fox

- Name: Nine-Tailed Fox / 九尾狐 / Jiǔwěihú
- Element: Charm and intuition
- Tags: Magnetic / Quick-thinking / Playful
- One-line hook: "You don't enter a room. You change its weather."
- Strengths: You read people fast and you know how to make them feel at ease. In a group, you are often the one who turns an awkward moment into a story everyone retells later. Your curiosity and humour are real social skills, not just charm.
- Blind spot: Because you can adapt to almost anyone, you can lose track of what you actually wanted. Sometimes you say yes with a smile and regret it on the way home. Picking one clear no is as powerful as your easiest yes.
- Best match: Kun — "Kun gives you space to be yourself; you give Kun a reason to come back down to earth."
- Today's tip: Send the message you have been rehearsing in your head. Keep it to two sentences.
- Source note: Inspired by Shanhaijing and later folklore retellings of the nine-tailed fox.

### 2. Baize

- Name: Baize / 白泽 / Báizé
- Element: Knowledge and clarity
- Tags: Wise / Observant / Calm under noise
- One-line hook: "You are the friend everyone calls when things stop making sense."
- Strengths: You notice patterns other people walk straight past. You remember the detail from three weeks ago that suddenly explains everything today. When a problem gets messy, your first move is to understand it properly — and that usually saves everyone time.
- Blind spot: Understanding can become a way to delay deciding. You may keep researching long after you already know enough. A good-enough answer shared today often helps more than a perfect one kept to yourself.
- Best match: Yinglong — "Yinglong turns your insight into action; you help Yinglong choose the right battle."
- Today's tip: Share one useful thing you know in a group chat or class thread. Don't polish it first.
- Source note: Inspired by later Chinese folklore about Baize, a creature said to know the stories of many beings, retold alongside the Shanhaijing tradition.

### 3. Taotie

- Name: Taotie / 饕餮 / Tāotiè
- Element: Appetite and enjoyment
- Tags: Food-first / Big-hearted / All-in
- One-line hook: "If it's worth doing, you think it's worth doing properly — with snacks."
- Strengths: You bring appetite for life, not just for food. You are generous, funny about your own cravings, and often the person who turns an ordinary plan into a feast, a trip or a proper celebration. When you love something, you go all in, and people love being around that energy.
- Blind spot: All-in can quietly become too much — too many plans, too many tabs open, too much on your plate in every sense. Enjoying one thing fully beats collecting ten things at half attention.
- Best match: Phoenix — "Phoenix helps you start fresh when you overdo it; you remind Phoenix that joy is a reason, not a reward."
- Today's tip: Pick one meal or break today and enjoy it with your phone in another room.
- Source note: Inspired by the taotie motif in ancient Chinese bronze art and later folklore retellings.

### 4. Jingwei

- Name: Jingwei / 精卫 / Jīngwèi
- Element: Persistence and purpose
- Tags: Determined / Loyal / Small steps, every day
- One-line hook: "Everyone else sees a huge sea. You see the next small stone."
- Strengths: You do not quit quietly — you return, you retry, and you carry one more small piece than yesterday. People underestimate how far that takes you. You are at your strongest when a goal matters to you personally, not when someone else is watching.
- Blind spot: Persistence can turn into refusing to change the plan. Carrying stones forever is noble in a story and exhausting in real life. Checking whether the goal still matters is not giving up; it is aiming.
- Best match: Qilin — "Qilin softens your edges on hard days; you show Qilin how powerful steady effort can be."
- Today's tip: Spend 15 minutes on the task you keep postponing. Stop when the timer ends — done is the point.
- Source note: Inspired by the Shanhaijing story of Jingwei, the bird retold as filling the sea stone by stone.

### 5. Kun

- Name: Kun / 鲲 / Kūn
- Element: Freedom and imagination
- Tags: Dreamer / Independent / Bigger-picture
- One-line hook: "You were never hard to understand. You just need a bigger sky."
- Strengths: You think in wide shots. While others argue about the next step, you are already imagining where the whole road could go. You protect your freedom, follow your curiosity, and often come back from time alone with the most original idea in the room.
- Blind spot: Big-picture thinking can leave the small print undone — the reply, the deadline, the plan for getting there. Your ideas deserve a landing strip, not just open sky.
- Best match: Nine-Tailed Fox — "The Fox helps your ideas land with people; you remind the Fox what genuine freedom feels like."
- Today's tip: Write your big idea at the top of a page, then write only the first tiny step under it.
- Source note: Inspired by early Chinese stories of Kun, the vast fish retold as transforming into a great bird.

### 6. Phoenix

- Name: Phoenix (Fenghuang) / 凤凰 / Fènghuáng
- Element: Renewal and expression
- Tags: Expressive / Resilient / Self-reinventing
- One-line hook: "New season, new era — and somehow, it always suits you."
- Strengths: You know how to begin again without pretending the last chapter never happened. You have taste, presence and a strong sense of occasion, and you often help a whole group feel that a fresh start is possible. Change is not a threat to you; it is raw material.
- Blind spot: A fresh start can become an escape hatch. New playlist, new look, same unfinished thing waiting underneath. Finishing one old chapter makes the next one mean more.
- Best match: Taotie — "Taotie grounds your big restarts in real pleasure; you show Taotie that starting over can be beautiful."
- Today's tip: Finish one small leftover task before you start anything new today.
- Source note: Inspired by Chinese folklore about the fenghuang, a bird retold as a sign of harmony and renewal.

### 7. Qilin

- Name: Qilin / 麒麟 / Qílín
- Element: Kindness and trust
- Tags: Gentle / Trustworthy / Steady presence
- One-line hook: "You don't make the most noise in the room. You make the room feel safe."
- Strengths: People tell you the real version, not the polished one. You listen properly, you remember what matters to people, and you would rather solve a problem quietly than win an argument loudly. Groups hold together better when you are in them.
- Blind spot: Being the safe person can mean your own needs come last, every time. You may smooth things over when a clear boundary would serve everyone better. Kind and firm can live in the same sentence.
- Best match: Jingwei — "Jingwei lends you momentum on days you hesitate; you give Jingwei a calmer way to keep going."
- Today's tip: Say one honest preference out loud today, even about something small like where to eat.
- Source note: Inspired by Chinese folklore about the qilin, retold as a gentle creature linked with harmony.

### 8. Yinglong

- Name: Yinglong / 应龙 / Yìnglóng
- Element: Leadership and momentum
- Tags: Decisive / Protective / Natural organiser
- One-line hook: "When nobody knows what to do next, everyone looks at you."
- Strengths: You decide, you organise and you carry responsibility without making a performance of it. In a crisis you get clearer, not louder, and people follow because your plans usually work. You are protective of your people and generous with your strength.
- Blind spot: Taking charge can become taking over. Doing it yourself is faster today and weaker next month, when nobody else has practised. Delegating one real task is also leadership.
- Best match: Baize — "Baize sharpens your plan before you move; you turn Baize's insight into something real."
- Today's tip: Hand one task you would normally do yourself to someone else — and let them do it their way.
- Source note: Inspired by Shanhaijing and later folklore retellings of Yinglong, a winged dragon linked with rain and great deeds.

## Result card spec — the shareable image

Purpose: this card is the main sharing unit for TikTok, Instagram and group chats. It must look good as a screenshot even if the download fails.

- Generated in the browser with Canvas, no server and no external image
- Size: 1080 × 1350 px (4:5 portrait)
- Card contents, top to bottom: product title "Which Shanhaijing Creature Are You?", creature visual (large), creature English name, Chinese name and pinyin, element line, 3 tags, one-line hook, strengths shortened to the first two sentences only, best match name, footer line "For entertainment and self-exploration only." and the site address taken from `CONFIG.SITE_URL` — if `SITE_URL` is still "TBD", print "Link coming soon" instead of inventing an address
- Share actions on the result screen, in this order:
  - "Download card (PNG)" — downloads the canvas as `my-creature-[creature-id].png` and fires `share_clicked` with channel `download`
  - "Copy link" — copies the current page address and fires `share_clicked` with channel `copy_link`. If `CONFIG.SITE_URL` is set and is not "TBD", copy that instead of the local file address
  - "Share…" — uses the Web Share API if the browser has it, and fires `share_clicked` with channel `native_share`. If it is not available, hide this button; do not show a broken button
- Share text, copy exactly: "I got [Creature Name] on Which Shanhaijing Creature Are You? — an Eastern personality mirror with 8 ancient creatures. Try it:"
- After any share action, show a short confirmation line such as "Card saved. Send it to a friend and compare creatures." Do not claim the card was posted anywhere

## Paywall spec — three SKUs, all mock in v0.1

No real payment in v0.1. No Stripe key, no checkout, no charge, no card input anywhere. Every price below was agreed before this brief; do not change a price, add a discount or invent a higher tier.

| SKU id | Name shown to user | Price shown | What the button promises |
| --- | --- | ---: | --- |
| `full-report` | Full Creature Report | $3.99 one-time | The four locked report sections on the result screen |
| `daily-plus` | Daily Plus | $8.99/mo | A daily creature card for every creature, plus the full collection unlocked |
| `mystery-box` | Creature Mystery Box | $19.90 one-time | A physical mystery box of the user's result creature, pre-order interest only in v0.1 |

Paywall behaviour:

- Any paywall button first fires `paywall_viewed` with the SKU, then opens a modal for that SKU
- Modal contents: SKU name, price exactly as in the table, one line describing what it includes (from the table), and this exact line: "Demo checkout — no payment is taken in this test version. The payment link will be added here."
- Modal primary button: "Notify me / Continue" — fires `paywall_clicked` with the SKU, then changes to a confirmation state: "Recorded. Thanks for testing — nothing was charged."
- If `CONFIG` payment URL for that SKU is later replaced with a real link, the primary button label becomes "Continue to payment" and opens that link in a new tab. Build this as a simple check on whether the value is "TBD"; do not build any other payment logic
- Modal must be closable with an obvious close button and by tapping outside it. Closing fires no event
- Never use countdown timers, fake stock counts ("only 3 left"), strike-through fake original prices or pressure wording

## Events and measurement — local only in v0.1

No analytics service, no external tracking script, no cookies in v0.1. Events exist so the validation numbers can be judged.

Implementation:

- Keep a global array `window.dataLayer` (create it if it does not exist)
- Every event is pushed as an object: `{ event: "event_name", ts: "<ISO time>", ...fields }`, and also logged with `console.log("[event]", ...)`

- Also keep running counts in `localStorage` under the key `shanhai_events_count`, as a JSON object counting each event name, so a tester can read totals without a console
- Debug view: when the page address contains `?debug=1`, show a small panel at the bottom listing the event counts and a button "Copy event log" that copies the `dataLayer` JSON to the clipboard. The panel must not appear without that parameter

Events to fire, exactly these names and fields:

| Event | When | Extra fields |
| --- | --- | --- |
| `test_started` | Start button tapped | none |
| `question_answered` | Each option tapped | `question` (1–5), `option` ("A"–"D") |
| `test_completed` | Result calculated | `creature_id` |
| `result_viewed` | Result screen shown | `creature_id` |
| `share_clicked` | Any share action tapped | `channel` (`download`, `copy_link` or `native_share`), `creature_id` |
| `paywall_viewed` | Paywall modal opened | `sku` |
| `paywall_clicked` | Modal primary button tapped | `sku` |
| `daily_card_drawn` | Daily card saved for the first time that day | `creature_id`, `date` |
| `collection_viewed` | Collection screen opened | `unlocked` (count) |
| `restart_clicked` | Retake button tapped | none |

Validation thresholds — these judge the product after launch; DSH only has to make them measurable:

- Test group for the first round: 500 visits sent the link
- Completion: more than 60% of `test_started` reach `test_completed`. Below that, the quiz is too long or too confusing
- Sharing: more than 15% of completed tests fire at least one `share_clicked`
- Paywall interest: more than 5% of result views lead to a `paywall_clicked` for any SKU
- Real paid conversion (only measurable once a real payment link replaces a "TBD" value in a later version): more than 1% of completed tests paying $3.99 counts as a pass for the report SKU. In v0.1, report clicks, not revenue — never present a mock click as a sale
- Return: at least some testers come back on a later date and fire `daily_card_drawn` again. Count distinct date keys, not raw visits

## Acceptance checklist — DSH copies this back, filled in

DSH: copy everything between the lines into your reply, fill in every line, and attach the evidence named. The person accepting the work will re-check these items independently; a tick here is your self-test, not the acceptance itself.

```text
SHANHAIJING TEST v0.1 — SELF-TEST RETURN
Deployed address or file delivered: [full address, or "local file only"]
Build form: [single index.html / static folder]
File size: [KB]

FLOW
[ ] Start screen shows title, subtitle, 3 points, Start, collection link, daily link, exact disclaimer
[ ] Quiz shows Q1–Q5 in order, exact wording, 4 options each, progress x / 5, tap-to-advance, no Back, no text input
[ ] Reveal state shows, then result screen shows all free fields with exact creature copy
[ ] Locked report preview shows 4 section titles + the v0.1 note, and no invented body copy

SCORING
[ ] A A A D D -> Taotie
[ ] B B D D A -> Baize
[ ] A A B B C -> Jingwei
[ ] All 8 creatures reachable — list one answer set you tested for each of the other 5:
    Nine-Tailed Fox: [ ]  Kun: [ ]  Phoenix: [ ]  Qilin: [ ]  Yinglong: [ ]

CREATURES AND CARD
[ ] All 8 creatures have visual, element, 3 tags, hook, strengths, blind spot, best match, tip, source note
[ ] Share card downloads as PNG at 1080 x 1350 with all required card contents
[ ] Copy link works; Share button only appears when Web Share API exists; share text matches the brief exactly

DAILY AND COLLECTION
[ ] Same date always gives the same daily creature; a different date gives the rule-based creature
[ ] Daily draw saves under shanhai_daily_YYYY-MM-DD and updates shanhai_collection
[ ] Collection shows Discovered N / 8, locked creatures show ??? with no name or tags

PAYWALL (MOCK)
[ ] 3 SKUs show exact prices: report $3.99, Daily Plus $8.99/mo, mystery box $19.90
[ ] Modal shows the exact demo line; primary button records the click and confirms nothing was charged
[ ] No payment form, no card input, no countdown, no fake stock, no fake discount anywhere

EVENTS
[ ] All 10 events fire with the exact names and fields in the brief — sample dataLayer JSON attached: [yes/no]
[ ] ?debug=1 shows the event panel; without it, no panel appears

COMPLIANCE AND LAYOUT
[ ] Banned-word check done: searched all user-facing copy for every banned word in the brief — result: [0 found / list what was found]
[ ] Exact disclaimer appears on start and result screens; exact source line appears on result screen
[ ] No birth data, email, phone or other personal data is asked for anywhere
[ ] 360 px and 390 px: no horizontal scroll, no text cut off — phone screenshots attached: [yes/no]
[ ] Desktop width: no horizontal scroll, layout still centred and readable — screenshot attached: [yes/no]
[ ] No console errors on load, quiz, result, card download and paywall — checked: [yes/no]

KNOWN GAPS / QUESTIONS FOR NEXT ROUND
- [write any item from this brief you could not finish, and any question, here. Write "none" if there are none.]
```

## Iteration rules — for DSH and for later rounds

- This brief is v0.1. Later changes arrive as a short change list naming a version (v0.2, v0.3), never as a full rebuild request. Build so that a change list is cheap to apply
- All prices, payment links, the site address and the debug parameter live in the single `CONFIG` block. Nothing priced or linked may be hard-coded anywhere else
- All quiz questions, options and scoring live in one clearly marked data block in the script, separate from display code. All creature copy lives in one data block, keyed by creature id
- Do not add, in any round, without a written instruction naming it: login or accounts, a backend or database, real payment code, a social media account, extra creatures, extra questions, a second language, push notifications, or an app / mini-program version
- Wording changes follow the compliance section first. If a requested change in a later round conflicts with the banned-word list or the disclaimer, flag the conflict in your return instead of silently building it
- Every return includes: what changed since the last version, the updated deployed address or file, and the filled acceptance checklist above, re-run in full — not only for the parts that changed
