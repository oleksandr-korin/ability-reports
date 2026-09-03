# Outbound reporting standard — proposal

**Status:** proposal (PR from Alex, 2026-09-03). Adopt section by section. The narrative report stays; this sits next to it.

**Why.** From Week 10 the machine carries several lists (full lead base, partnerships wave 1 with waves 1b and 2 behind it, Tom's queue). The narrative can't answer *which list is working* or *what the binding constraint is*. Today: 2,029 sends, ~0.4% replies all time, 0 in the last 249 — and that number has never been a headline.

## 1. Per-campaign scoreboard

One row per campaign × wave, weekly and cumulative:

| Column | Definition |
|---|---|
| imported · first touches · sequence complete | contacts loaded · step 1 sent · final step sent or replied/opted out earlier |
| bounces | hard bounces, % of first touches |
| replies: positive / neutral / negative / wrong-person | "tell me more" or books time / OOO, "not now", forwarded / "no", opt-out / "wrong company, never heard of you" — a **targeting** signal, kept apart from negative |
| meetings | booked, and held |
| positive reply rate | positive ÷ sequence complete |

Plus the cut by `fit_tier` and `keyword_source` / `segment` where the list carries them (that is what turns wave 1 into a learning). The full lead base is a campaign too, split by its tier 1–4.

## 2. Signals

A signal fires, someone acts, the report says so.

| Signal | Red → action (owner) |
|---|---|
| Bounce per wave > 5% | pause the wave; list owner diagnoses enrichment (Alex) |
| Positive reply rate < 0.5% once ≥ 300 sequence-complete | stop scaling that list; change one thing — copy, targeting or CTA — and rerun (Vic + Alex) |
| Wrong-person > 25% of replies | targeting is wrong; re-tier before more sends (Alex) |
| Opt-out > 1% per wave | pause; review copy and cadence (Vic) |
| Reply threads open > 2 business days | response handling is the constraint; warm threads to a named human same day (Vic → Eugene) |

## 3. Wave sign-off

Every wave, until three in a row pass their gates cleanly — then a one-line notification instead.

- [ ] list file + version, row count
- [ ] tags on every row: `campaign`, `wave`, `fit_tier`, `keyword_source`/`segment`, mapped 1:1 at import
- [ ] suppression run (opt-outs domain-wide, CRM accounts, prior sends) — rows removed: __
- [ ] copy checked against canon: every claim, metric and logo sourced
- [ ] CTA and reply path named (who answers, SLA, where a booked call lands)
- [ ] domains / mailboxes assigned, daily cap per domain and mailbox, opt-out enforcement on
- [ ] gate defined: date + numbers that release the next wave
- [ ] signed: list owner __ · sender __ · date __

## 4. The constraint of the week

Four lines, every week: **binding constraint** (supply · targeting · copy · offer/CTA · deliverability · capacity · response handling) · **evidence** · **next lever, and the number that would show it worked** · **hypothesis for next week**.

Current hypothesis from the list side, offered for disagreement: the constraint is reply rate, not supply — 0.4% over 2,029, 0 in the last 249, same as the earlier Apollo sequences; supply only *looks* binding because tier 3–4 sends are cheap to add and don't reply. Wave 1 (178 hand-checked tier-1 on the vertical-pain copy) is the controlled read. If it still reads under 0.5% positive after ~150 complete, the constraint is the offer/CTA, not the list or the copy.

## 5. `metrics.json` next to each report

Same numbers the narrative quotes, machine-readable (`templates/metrics.example.json`). Consumers: the list owner closing the loop on M1/M2, TOM's CRM sync, any dashboard.

Also worth doing: one change per week (or the read is lost); meetings *held*, not just booked.
