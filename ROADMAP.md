# DubHacks 2026 Roadmap

Oct 17, 2026 · exported from the live roadmap doc: https://claude.ai/code/artifact/06761220-6439-4e7a-b6c2-09abbf984f40 (ask Falisha for access). This file is a snapshot; beebot PLAN.md and CONTRACT.md win on any conflict.

## Summary

We build BeeBot as written in the beebot repo: field video goes through Perceptron for bee tracking, analytics and search, then becomes a pollination map and a robot pollinator route. The 20-hour plan fits the deadlines if hour 0 is Sat 12:30 PM, but it has no step for the **Saturday 10:30 PM draft**, which is required to be judged at all.

- **Source of truth:** [b-ananya/beebot](https://github.com/b-ananya/beebot) (PLAN.md, CONTRACT.md, README.md, DATA\_NOTES.md, data release `data-v1`). The claude-gc design doc describes an older product and stack; it should be replaced (draft changes at the end).
- **Hour 0 = Sat Oct 17, 12:30 PM PDT** (the plan commit landed at 12:30:01 PM, and the context notes say coding may start at 12:30). Each plan hour adds one clock hour.
- **Deadlines (Devpost):** draft before Sat Oct 17, 10:30 PM; final before **Sun Oct 18, 10:00 AM PDT**. Judging Sun 11:30 AM to 2:00 PM; winners 4:00 PM.
- **Judging weights (Devpost):** functionality 30%, track applicability 30%, design 20%, creativity 20%.
- **Two decisions for humans:** Perceptron model (Isaac vs Mk1.5) and confirming who owns each piece of the split.

Checked against the [Devpost overview](https://dubhacks-2026.devpost.com/), [dates](https://dubhacks-2026.devpost.com/details/dates), [rules](https://dubhacks-2026.devpost.com/rules) and [dh26.dubhacks.co](https://dh26.dubhacks.co/). The Notion hacker guide and the Devpost prizes page could not be opened from here, so track details come from the team and the project context file.

## Schedule in clock time

Hour 0 is Sat 12:30 PM, so the 20-hour plan ends Sun 8:30 AM with 90 minutes to spare. The draft deadline (10:30 PM) falls between G2 and G3, so it gets its own step, D0, at 9:30 PM.

```text
Sat 12:30 PM  h0   Start. Phase 1: video analysis (must-have) runs to 12:30 AM
Sat  2:30 PM  h2   G1 contract frozen, shared Perceptron client ready
Sat  4:30 PM  h4   Standup
Sat  8:30 PM  h8   G2 real tracks in the app. Standup
Sat  9:30 PM  h9   D0 Devpost draft submitted (due 10:30 PM)
Sun 12:30 AM  h12  G3 Phase 1 demo works end to end. Standup. Phase 2: field planner
Sun  4:30 AM  h16  Standup
Sun  5:30 AM  h17  G4 code freeze, numbers locked. Phase 3: freeze and present
Sun  7:30 AM  h19  G5 submission-ready
Sun  8:30 AM  h20  Final Devpost submission (due 10:00 AM PDT)
```

| Gate | Plan hour | Clock (PDT) | Done when | Owner |
| --- | --- | --- | --- | --- |
| G1 contract frozen | h2 | Sat 2:30 PM | CONTRACT.md frozen; first Perceptron call parses; everyone runs the app on mock | Ananya, Raika |
| G2 real tracks in the app | h8 | Sat 8:30 PM | Real tracks for 5 clips pass validate and load in the app | Raika |
| D0 Devpost draft | h9 | Sat 9:30 PM (due 10:30 PM) | Names, Discords, tracks, repo link submitted | Falisha |
| G3 Phase 1 works end to end | h12 | Sun 12:30 AM | Tracking, analytics, search demo; `eval/report.json` v1 | Falisha, Ananya |
| G4 code freeze, numbers locked | h17 | Sun 5:30 AM | Field map and robot route done; numbers final | Ananya, Flora |
| G5 submission-ready | h19 | Sun 7:30 AM | Offline demo, backup video, rehearsed | Falisha |
| Final submitted | h20 | Sun 8:30 AM (due 10:00 AM) | Devpost final with all code pushed | Falisha |

It is now about 1:30 PM, plan hour 1. Workshops run 1:30 to 4:30 PM and dinner 6:00 to 7:00 PM, so whoever attends the Perceptron workshop should feed what they learn into A1.

## Team split: who owns what

Perception is split into four pieces, one per person, so all four work on it at the same time. Everyone calls Perceptron through one shared client and cache, so no clip and prompt is paid for twice. Raika takes the heaviest implementation and first use of Fable, Ananya second. These assignments are a proposal for the team to confirm.

### Perception, split four ways

| Piece | Owner | What it asks Perceptron | Writes (only this owner) | Feeds |
| --- | --- | --- | --- | --- |
| P1 Shared client and bee tracking | Raika | One-bee tracking, 720p vs 1080p test, 48-clip batch, v2 reruns (A1 to A4, A6) | `perception/client.py`, `perception/track.py`, `perception/merge.py`, `prompts/track/`, `tracks/*.json` | Everyone at G2, 8:30 PM |
| P2 Flowers and visits | Ananya | Flower boxes from a reference frame, flower state (A5), landing and visit events | `perception/flowers.py`, `prompts/flowers/`, `tracks/parts/*.flowers.json`, `tracks/parts/*.events.json` | Field maps, planner |
| P3 Search and Q&A | Falisha | Captions and events per clip for the index (B2), live question toggle (D4) | `perception/search.py`, `prompts/search/`, `search/index.json` | App search box |
| P4 Model test | Flora, after VEYZL | Isaac vs Mk1.5 on the same 5 clips with the P1 and P2 prompts | `perception/bakeoff.py`, `eval/bakeoff.json` | Model decision by 5:30 PM |

- **Raika ships the client first, by G1 (2:30 PM).** Until then the others write prompts and parsers against single test frames and make no batch calls.
- **One file per clip still comes out.** Raika's `merge.py` combines the P1 track with P2's flowers and events into `tracks/<clip>.json`, which is what CONTRACT.md and `validate.py` expect. The `tracks/parts/` folder is a contract addition, so Ananya announces it in team chat.
- **If one piece is late,** the others fall back on `truth/` (flowers and visits exist for the 10 farm clips) or `tracks/mock/`.

### Everything else

| Person | Perception piece | Other work | Owns |
| --- | --- | --- | --- |
| Raika | P1 | Offline cache freeze and `analyze_clip(path)` for the upload button (A7) | `perception/` core, `tracks/` |
| Ananya | P2 | Eval (B3, B4), field maps (B5, B6), number fact-check (B7) | `clips/manifest.json`, `truth/`, `eval/`, `field/`, CONTRACT.md |
| Falisha | P3 | App (D1 to D7), deck, Devpost, demo video | `app/`, `search/`, requirements.txt |
| Flora | P4 | VEYZL setup and log, then planner and sim (C1 to C7) | `plan/`, `sim/`, `rollouts/`, `docs/veyzl.md` |

**Fable.** Raika uses it first, on the client, the tracking parser and the batch. Ananya uses it second, on flower and event parsing, the eval script and the field-map builder. If Flora's OR-Tools solver (C3) stalls past 8:30 PM, Ananya pairs on it with Fable.

**Flora and VEYZL.** Flora finishes connecting VEYZL to the beebot repo first (target: the 4:30 PM standup), runs the small model test, then starts the planner. The planner feeds nobody until 12:30 AM, so a late start costs nothing downstream. The VEYZL track needs a real audit on a pull request and proof that the findings changed the build, so Flora keeps `docs/veyzl.md` (finding, change, PR link) and everyone merges through PRs, not direct pushes to `main`.

Rules that keep work from overlapping:

- **Files have one owner.** Anyone who needs a change asks the owner in team chat.
- **One branch per person** (`raika/...`, `ananya/...`, `falisha/...`, `flora/...`), merged by small PRs.
- **Need another piece's output? Read its file, don't re-ask Perceptron.**
- **Standups at 4:30 PM, 8:30 PM, 12:30 AM and 4:30 AM:** what shipped, what's blocked, credit spent.

### Perceptron credits: never pay twice

Each person pays for their own piece on their own key, so spending spreads over all four accounts and no kind of call has two payers. PLAN.md estimates about $28 of the $200 in total, from third-party prices; Raika's client logs the real cost per call.

1. **Every call goes through the shared client, which checks the cache first.** The cache key is a hash of model id, prompt version, prompt text, clip and settings. Responses are saved under `tracks/raw/<piece>/`. The model id comes from config, so switching to Isaac is a one-line change.
2. **The cache is committed to git, so teammates pull it instead of calling.** Today `.gitignore` has `raw/`, which also blocks `tracks/raw/`; change it to `/raw/`.
3. **Who pays for what:**

| Key | Pays for | PLAN.md estimate |
| --- | --- | --- |
| Raika | P1: tracking prompt tuning, 48-clip batch, v2 reruns, 1080p test | \~$15 |
| Ananya | P2: flower boxes, flower state, visit events | \~$3 plus events |
| Falisha | P3: search captions and events; live search, upload-your-own-clip and demo-day calls | \~$10 |
| Flora | P4: the Isaac vs Mk1.5 test; spare key for anyone past $35 | \~$1 plus spare |

4. **Claim before you spend.** Post in team chat what you're running (piece, clips, prompt version). Afterwards, push the cache and your cost log right away.
5. **One cost log per person** (`costs/<name>.csv`), so logs never conflict. The client refuses calls past $40 on a key.
6. **Rehearsals play from cache.** Live calls happen only on Falisha's key, on stage or in the final dry run.
7. **Keep at least $10 per key** unspent until the final submission.

Open point for Raika at A1: if Perceptron file IDs belong to one account, each key uploads the clips its piece needs once, and the manifest stores one `file_id` per key.

## Demo app: Streamlit, plus a hosted link

**Recommendation: build the app in Streamlit as PLAN.md says, run it locally on stage, and deploy the same app to Streamlit Community Cloud after G3 for a live link on Devpost.** Don't rebuild it as a Next.js app on Vercel.

- **Why Streamlit:** the whole pipeline is Python (OpenCV, pandas, OR-Tools, Perceptron SDK), requirements.txt already pins `streamlit` and `plotly`, and D can render tracks and plans straight from the JSON files. A Vercel front end means a second language, an API between the two, and a hosted Python worker for anything heavy, which is the old design doc's stack.
- **Why a hosted copy too:** judges and Devpost visitors can click it, and it is free and deploys from the public repo in minutes. Hugging Face Spaces (Streamlit SDK) is the fallback if Community Cloud is slow or out of memory.
- **How to keep it safe:** the hosted copy plays from committed cache (`tracks/`, `search/`, `field/`, `plan/`, a few demo clips and route videos), with live Perceptron calls off by default. If live search stays on, put the key in the host's secrets, never in the repo, and cap it.
- **Hosting gotchas to check early (D6):** use `opencv-python-headless` on the host, add ffmpeg through `packages.txt` if the app needs it, and keep demo media small, since the 48 working clips (104 MB) come from the release, not git.
- **On stage, use the local app** with the network off (A7 and D6), with the backup video ready. The hosted link is for judges and Devpost, not the live demo.

## Demo plan

The demo is PLAN.md's 3-minute story, rehearsed to 2:30 so there is room for one question. It runs from the local app with cached results; the backup video covers a crash.

1. **Problem (15 s):** growers can't see which flowers bees miss, and pollinator numbers are falling.
2. **Tracking (40 s):** pick a farm clip (`sm3_12_*`, the clips with flower boxes and visits). The bee gets a box and trail, flowers get boxes, visits flash.
3. **Analytics (25 s):** visits per flower, dwell time, heatmap, timeline.
4. **Search (25 s):** type "when does the bee land on a flower?" and the video jumps to that moment.
5. **Field view (35 s):** zoom out to the 9-station pollination map with deficits, then the robot's route beside the three baselines, with the deficit-covered score.
6. **Honesty and next steps (10 s):** one accuracy number from `eval/report.json`, said as "agreement with a reference tracker," plus the three caveats from PLAN.md's "Say on stage."

Prepare by Sun 7:30 AM (G5): three timed rehearsals, a Q&A sheet (C7's assumptions paragraph is the core of it), the backup screen recording, and a laptop that runs the app with Wi-Fi off. At least one teammate must be at judging, Sun 11:30 AM to 2:00 PM.

## Devpost submission checklist

Falisha owns submission and Ananya fact-checks the numbers (B7). Devpost says a team that misses the Saturday draft cannot be judged on Sunday.

**Draft, before Sat 10:30 PM (aim for 9:30 PM):**

- [ ] Project page created on [Devpost](https://dubhacks-2026.devpost.com/) with a working title and one-line pitch
- [ ] All four members added as contributors: Raika, Ananya, Falisha, Flora, each with their Discord handle
- [ ] Title track declared: exactly one (Community Kitchen recommended)
- [ ] Side tracks declared: Perceptron, VEYZL and SideShift (the 3-sponsored cap)
- [ ] Public GitHub repo linked (b-ananya/beebot is already public)

**Final, before Sun 10:00 AM PDT (aim for 8:30 AM):**

- [ ] All code pushed to the public repo; README explains how to run the app
- [ ] Description: problem, what it does, how Perceptron is used (name the localization, tracking and video-event features for that track), how it was built, challenges, what's next
- [ ] Accuracy number and caveats match `eval/report.json` (B7)
- [ ] Screenshots of tracking, analytics, search and the field route
- [ ] Demo video uploaded if any track judges by video (the hacker guide says which; the backup recording can double as it)
- [ ] Hosted Streamlit link, tested in a private window
- [ ] Data attribution: both Monash datasets, CC BY 4.0, with DOIs from DATA\_NOTES.md
- [ ] Disclosure of what existed before 12:30 PM (see disagreements)
- [ ] VEYZL evidence (`docs/veyzl.md` with PR links) if entering VEYZL
- [ ] Startup framing (customer, distribution) if entering SideShift
- [ ] No API keys in the repo or its history; `.env` is gitignored

To confirm with organizers or the hacker guide: whether a demo video is required, and its length.

## Risks

The biggest risk is Perceptron tracking quality on small bees (20 to 30 px at 720p), because everything downstream depends on it. The plan's cut list already protects the must-haves.

| Risk | Owner | Early sign | Fallback |
| --- | --- | --- | --- |
| Tracking misses small or fast bees | Raika | Overlay wrong on 2+ of 5 clips at A2 (5:30 PM) | Test 1080p; favor farm clips; report honest coverage |
| Real tracks late for G2 (8:30 PM) | Raika | A3 parser not passing validate by 7:30 PM | Keep the app on mock; ship the 5 working clips first |
| Draft deadline missed | Falisha | No Devpost page by 9:00 PM | Draft needs only names, Discords, tracks; do it at the 8:30 PM standup |
| Phase 1 shaky at G3 (12:30 AM) | Falisha | Search or analytics broken at 12:30 AM | Cut Phase 2 as the plan says; the robot becomes a slide |
| Model switch to Isaac mid-build | Raika | Decision not made by 5:30 PM | Flora's 5-clip test decides; keep Mk1.5 if it's late; the model id is one config line |
| Credits burned by duplicate calls | Raika | Two cost logs with the same clip and prompt | Cache in git, one paying key per call type, $40 cap |
| Hosted app fails (memory, ffmpeg) | Falisha | First deploy not up by 2:30 AM | Hugging Face Spaces, or Devpost uses video + screenshots |
| Pre-hackathon commit questioned | Ananya | Judges ask about the 12:08 PM scaffold | Disclose it as data preparation on Devpost |
| Live demo crash or no Wi-Fi | Falisha | Any crash in rehearsal | Offline mode (A7, D6) and the backup video |
| Overclaiming the science | Ananya | Slide numbers not in `report.json` | B7 fact-check; say the three stage caveats |

## Disagreements and how to resolve them

There are 19 disagreements: most are the old design doc against beebot, which beebot wins. Three need a human call: the model (6), the pre-12:30 commit (16) and approving the claude-gc rewrite (1).

### Between the two repos (and the project context file)

| # | Topic | claude-gc design doc / context file | beebot | Resolution |
| --- | --- | --- | --- | --- |
| 1 | Source of truth | claude-gc README: design-doc.html is the source of truth | Team says beebot is current | Rewrite claude-gc README to point at beebot (draft below); delete or archive design-doc.html |
| 2 | Product | Robo-bee route drawn over one uploaded video | Video analysis first, then a field-scale robot route planner | beebot |
| 3 | Robot code | Context file: "Do not build robot code" | Phase 2 is a robot route planner | beebot; the context file (PolliNet) is older |
| 4 | Stack | Next.js on Vercel, Modal worker, Supabase, Vercel Blob | Python, Streamlit + Plotly, local files | beebot; hosted Streamlit for a link (Demo app section) |
| 5 | Data contract | One `results.json`, coordinates 0–1; context file: a visit-table CSV | Several files in CONTRACT.md, boxes 0–1000 | beebot CONTRACT.md |
| 6 | Model | `perceptron-mk1` via the OpenAI SDK | "Mk1.5 only", `perceptron` SDK 0.4.0; team leans Isaac | Keep Mk1.5 as default. Flora runs both on the same 5 clips (P4); switch only if Isaac wins on overlay and cost, decided by 5:30 PM and written into PLAN.md |
| 7 | Planner | Greedy by score, spline smoothing, energy slider | OR-Tools orienteering vs three baselines, deficit covered per meter | beebot |
| 8 | Team split | Frontend, Perception, Planner, Infra | Lanes A, B, C, D | beebot lanes with the names in this doc |
| 9 | Repo layout | Context file: `backend/`, `frontend/`, `data/` | `tracks/`, `field/`, `plan/`, `app/` ... | beebot |

### Inside beebot

| # | Issue | Where | Resolution |
| --- | --- | --- | --- |
| 10 | `.gitignore` has `raw/`, which also ignores `tracks/raw/`, so the shared API cache can't be committed | .gitignore vs README layout | Change to `/raw/` |
| 11 | Flower state (A5) is in lane A, but the budget bills account C | PLAN.md workstreams vs budget | Ananya owns it as part of perception piece P2, on Ananya's key |
| 12 | Python 3.10+ vs 3.11 | README vs PLAN.md | Use 3.11 everywhere |
| 13 | Truth files carry `box_from`, not in the contract; example IDs differ (`honeybee_1` vs `bee_1`) | truth/\*.json vs CONTRACT.md | Add `box_from` as optional to CONTRACT.md; IDs are free-form |

### Plan against the hackathon rules

| # | Issue | Rule (source) | Resolution |
| --- | --- | --- | --- |
| 14 | Tracks: the Devpost overview shows Perceptron and VEYZL only as sponsor logos, but the hacker guide lists them as sponsored side tracks (team confirmed). Perceptron: use its models via the API out of the box, judged on localization, tracking and video event finding. VEYZL: a real audit on a PR, with findings that changed the build | Max 1 title + 3 sponsored side tracks ([Devpost](https://dubhacks-2026.devpost.com/)) | Enter Community Kitchen (title) plus Perceptron, VEYZL and SideShift, which uses all 3 sponsored slots. Add DubHacks Next only if the guide calls it non-sponsored |
| 15 | The plan has no step for the Saturday draft | Draft before Sat 10:30 PM or no judging (Devpost) | Falisha submits it at 9:30 PM, added to the schedule as gate D0 |
| 16 | The beebot scaffold (data scripts, clips manifest, truth) was committed at 12:08 PM, before the 12:30 PM start | "Started and completed during DubHacks '26" (Devpost rules); context file: no code before 12:30 counts | Disclose it on Devpost as data preparation; ask an organizer if unsure |
| 17 | Final deadline: 10:00 AM vs 10:30 AM | Devpost dates page: 10:00 AM PDT; dh26 site and context file: 10:30 AM | Use 10:00 AM; plan to submit by 8:30 AM |
| 18 | Demo video: plan has a backup video but no Devpost video | "Some side tracks will use a video submission" (Devpost) | Make the backup recording judge-ready (under 3 min) |
| 19 | Hacking length: 20 h plan | Devpost says 24 h, dh26 says 36 h | Fine either way: the plan ends at 8:30 AM, 1.5 h before the deadline |

## Draft changes to claude-gc (after the team approves)

Nothing here has been pushed. Once a human approves, Raika (repo owner) or a Claude with push access makes three changes.

1. **Update README.md** so it points at beebot and this roadmap instead of design-doc.html (done in the same commit as this file).
2. **Delete design-doc.html** (git history keeps it) or move it to `archive/design-doc-v1.html` with a line on top saying it is outdated.
3. **Add ROADMAP.md** (this file) and update it as decisions are made (model, assignments).

The suggested README.md is committed alongside this file.
