# Outbound reporting standard — proposal

**Status:** proposal (PR from Alex, 2026-09-03). Adopt section by section; nothing here replaces the narrative report, it sits next to it.

**Why.** From Week 10 the machine carries more than one list: the full lead base, the partnerships campaign (wave 1 = 178 hand-checked tier-1 contacts, waves 1b and 2 behind it), and whatever Tom's queue holds. The weekly narrative is good at telling the story of the week; it cannot answer "which list is working" or "what is the binding constraint" without a small amount of structure. Two facts from the last nine weeks make the case:

- 2,029 sends all time, 8 threads ever in reply status (~0.4%), 0 replies in the last 249 sends. The older Apollo sequences ran at the same ~0.4%. That is below the "pitch is off" line (0.5% positive) in the partnerships brief, and it has never been a headline number.
- The base is 1,400 with 29 tier 1 and 218 tier 2; growth is entirely tier 4, and the "full lead base" goes out on a daily cadence. Whether tiers 3–4 are diluting the reply rate is unknowable from the current report.

Five additions, in order of value.

---

## 1. Per-campaign scoreboard

One row per campaign × wave, this week and cumulative. A campaign is a list with its own copy and its own tags; a wave is a release of that list.

| Column | Definition |
|---|---|
| campaign / wave | as tagged at import (`campaign`, `wave`) |
| imported | contacts loaded into the sequence, cumulative |
| first touches | contacts whose step 1 was sent |
| sequence complete | contacts who received the final step or replied/opted out earlier |
| bounces | hard bounces, count and % of first touches |
| replies: positive | "tell me more", asks a question about the offer, books or proposes time |
| replies: neutral | out of office, "not now / check back", forwarded to someone else |
| replies: negative | "no", unsubscribe, opt-out |
| replies: wrong-person | "I don't do this / never heard of you / wrong company" — a **targeting** signal, kept separate from negative |
| meetings | calls booked, and held |
| positive reply rate | positive ÷ sequence complete (not ÷ first touches — the denominator must have had a chance to reply) |

Plus, for lists that carry them, the same cut by `fit_tier` and by `keyword_source` / `segment`. Those two columns are what turn wave 1 into a learning: M1 (positive reply rate) and M2 (which cohort replies) are defined in `partnership_agencies-operators/brief.md` in the sales-agent repo.

The full lead base is a campaign too. Its rows split by Vic's tier (1–4) so we can finally see whether tier 3–4 sends earn replies or only burn domain reputation.

## 2. Signals — thresholds that name an action and an owner

A signal is not a KPI. It fires, someone does something, it is noted in the report.

| Signal | Green | Amber | Red → action (owner) |
|---|---|---|---|
| Bounce rate, per wave | < 2% | 2–5% | > 5%: pause the wave, list owner diagnoses enrichment (Alex) |
| Positive reply rate, once ≥ 300 sequence-complete | ≥ 2% | 0.5–2% | < 0.5%: stop scaling that list, change one thing (copy **or** targeting **or** CTA), rerun (Vic + Alex) |
| Wrong-person share of replies | < 10% | 10–25% | > 25%: targeting is wrong, list owner re-tiers before more sends (Alex) |
| Opt-out rate, per wave | < 0.5% | 0.5–1% | > 1%: pause the wave, review copy and cadence (Vic) |
| Send days in the week | 5 | 3–4 | ≤ 2: name why — supply, infra, or process (Vic) |
| Share of sends going to tier 3–4 of the base | < 20% | 20–40% | > 40%: supply problem, not a sending problem (Alex) |
| Open reply threads older than 2 business days | 0 | 1–2 | ≥ 3: response handling is the constraint (Vic; warm threads → Eugene) |

Thresholds are starting points from 2026 cold-email benchmarks and our own brief. Change them in a PR, not silently.

## 3. Sign-off before a wave enters the machine

For now, every wave. Once three consecutive waves pass their gates cleanly, this drops to notification-only (a line in the report), so it does not slow a working machine.

Checklist, both parties tick it in the wave's thread before import:

- [ ] **List file + version** named (path and git commit, or sheet tab and date). Row count stated.
- [ ] **Tags present** on every row: `campaign`, `wave`, `fit_tier`, `keyword_source` / `segment`. Import maps them 1:1.
- [ ] **Suppression run**: against opt-outs (domain-wide), current CRM accounts/deals, and every prior send. Count of rows removed stated.
- [ ] **Copy reviewed against canon**: every claim, metric, client name and logo traceable to `source-of-truth`. No unverified numbers. (The Jul–Aug audit found "70–95% time savings", "60→6 hours" and two logos in active copy without a source.)
- [ ] **CTA confirmed** and the reply path named: who answers, within what SLA, and where a booked call lands.
- [ ] **Domains / mailboxes assigned** to this wave, with the daily cap per domain and per mailbox. New mailboxes only if warm.
- [ ] **Opt-out enforcement** on, domain-wide, for this sequence.
- [ ] **Gate defined**: the date and the numbers that release the next wave (bounce, wrong-person share, positive replies).
- [ ] Signed: list owner ______ · sender ______ · date ______

## 4. The constraint of the week

One short section, every week, in this shape:

> **Binding constraint:** one of *supply · targeting · copy · offer/CTA · deliverability · capacity · response handling*.
> **Evidence:** the two or three numbers that say so.
> **Next lever:** the single change being made, and the number that would show it worked.
> **Hypothesis for the following week:** what the constraint becomes once this one is relieved.

Current hypothesis from the list side, offered for disagreement: the binding constraint is **not supply**. It is reply rate — 0.4% over 2,029 sends, 0 in the last 249, identical to the earlier Apollo sequences — and supply only looks like the constraint because tier 3–4 sends are cheap to add and are the ones that don't reply. Wave 1 is the controlled read: 178 hand-checked tier-1 contacts on the new vertical-pain copy. If that still reads under 0.5% positive after ~150 sequence-complete, the constraint is the **offer/CTA** (no agency landing page, no publishable price, call-with-Eugene as the ask), not the list and not the copy.

## 5. Machine-readable metrics next to each report

Each week's folder gets a `metrics.json` (schema in `templates/metrics.example.json`) with the same numbers the narrative quotes. Consumers: the list owner (to close the loop on M1/M2 without re-typing numbers), TOM's CRM sync, and any future dashboard. The narrative stays the human product; the JSON is the contract.

---

## Smaller things worth adding when convenient

- **Reply-handling SLA**: every reply answered within 2 business days; warm or partner-adjacent threads handed to a named human the same day (the Luminous thread sat 15 days).
- **One change per week**: copy, targeting, cadence or CTA — not two at once, or the read is lost.
- **Domain rotation log**: domains warming / active / retired, with dates. Eight domains have been dropped; the pattern is worth seeing.
- **Cost per positive reply**, once there are enough to divide by.
- **Meetings held, not just booked**, and where they went (the Fusion call from July has no recorded outcome).
