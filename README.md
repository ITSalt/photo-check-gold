# Photo check gold set

Real test pairs for the photo bot @contractsforpeoplebot (ITSalt/photo-check-bot): an AI-generated product photo next to a photo of the real item. Images and the notes come from the bot's owner and his staff, published for the whole development team, the getpostingboard.dev agents included. They are not customer photos: the bot deletes customer photos after each check.

Layout: `gNNN/generated.jpg` (the AI photo), `gNNN/real.jpg` (the real item), and one line per pair in `truth.jsonl`:

- `verdict`: `publish` | `fix` | `better_photo` | `other_item` — what a seller should be told;
- `differences`: what really differs, with `kind` (length, orientation, count, texture, colour, logo, shape, other);
- `same`: what matches and must never be flagged;
- `case` on a difference, and `cases_same` on the same side: which failure from the owner's test of 14.10 the entry pins;
- `decidable` on each detail: can it be decided from these two photos; `action` per pair: what the owner would do (list / retake_ai_photo / send_better_item_photo / other_item);
- `status`: `owner` = his words; `draft` = our reading, waiting for his confirmation.

Rule of use (gate G11). The unit is a pair and a detail, never a run: each pair is run 3 times, and a detail counts as wrong if any run was sure and wrong. No change to the bot's check or answer merges unless, on this set: no run says "publish" on a pair that really differs; at least 8 in 10 pairs get the owner's action on every run; every real difference is found on every run; at most 1 pair in 10 has a false difference; and every planned run was scored. The number of runs is fixed: r = 3 for every version compared, runs 1 to 3 all kept (none dropped or re-run for its content; a run that errors is retried until it has an answer), because with "wrong in any run" a different r changes the measure itself (theone). Every report line states r. Next to it stands one verdict per pair chosen in advance, the first run's, which is what a seller sees once. Reports give two numbers, never one: sure-wrong (details the bot was sure about and got wrong, with the exact 95% upper bound) and coverage (decidable details it was sure about on every run). Images are shared for work on this project. Every error a user finds becomes a new pair here.

## The plain question, and the end of the product (19.10)

Rule fixed before any call: the bot may be called better than asking a chat assistant only if, on held-out pairs, it publishes less often where the item differs than the plain question on both models and raises no more false alarms on matching pairs. First run on the four development pairs, r = 3, two blind scorers (they agreed on all 24 conclusions); the plain question: "Вот фото товара и ИИ-фото этого товара. Это одна и та же вещь? Можно публиковать ИИ-фото на маркетплейсе?"

| Runs saying publish where the item differs (of 12) | Pairs with the owner's action on every run (of 4) |
|---|---|
| Today's bot (15.10): 9 | 1 |
| Plain question, the bot's model (gemini-3.6-flash): 9 | 1 |
| Plain question, a ChatGPT-class model (gpt-6.1-sol): 0 (11 fix, 1 unclear) | 3 |
| Best arm, S5b v3: 0 | 4 |

On this evidence the bot is not better than asking a strong model. The owner closed the product on 19.10: even a gain on held-out pairs would be a rare case, not a reason for sellers to use it. The set, the kits and the arms stay public.

19.10.2026: the bot's waitlist (Telegram IDs) and check counter rows were deleted, without a backup (owner's consent; 3 and 2 rows before, 0 after).

## Held-out run (fixed in advance, 17.10)

The four pairs here are development pairs: every arm so far was built on them, and on 4 pairs and 3 runs a gap of about 4 details between two versions is run-to-run noise (Fisher two-sided p = 0.35 and 0.54, zenith-claude). Repeated runs show stability, not independent evidence: on today's bot 3 of 4 pairs gave the same wrong publish on all 3 runs (agent-798e120e-112). So the owner's next pairs are held out and run once, r = 3, on three arms chosen before they arrive: the bot of 15.10, S5b v1 and S5b v3 by zenith-claude. Each held-out pair is used once and then joins the development pairs. Nothing is chosen after the numbers are seen.

The held-out report gives, per arm, next to the two numbers and the G11 verdict (format proposed by agent-798e120e-112, extended by the studio):
1. Pairs by category: item type (clothes, jewellery, shoes, bags, other) and the kind of each real difference (shape, size, count, colour, material, position, presence), tagged from the owner's truth before any arm is run.
2. Pairs with a false publish on at least one run.
3. Runs with a false publish, in all.
4. Pairs whose three conclusions are not all the same, and of them pairs where all three differ.
5. "Send a better photo": runs and pairs answered better_photo, shown apart, so that declining to answer never improves accuracy silently at the cost of coverage.
6. On pairs where the item fully matches: runs and pairs that still ask to fix the AI photo (a false alarm costs the seller a retake).
Counts per pair stand next to every total.

## Baseline: the bot as it was on 15.10

Production check (google/gemini-3.6-flash), 3 runs per pair, answers mapped to the four conclusions, scored by two blind scorers. "Sure" = the bot named the detail as different, or listed it as the same.

| Pair | What really differs (kind) | Bot's conclusion, 3 runs | What the bot said about the difference |
|---|---|---|---|
| g001 trousers | cuff height (length) | publish, publish, publish | called the cuffs the same, all 3 runs |
| g002 beanie | knit (texture); star logo position (logo); an added snood (other) | fix the AI photo, all 3 runs | knit sure-different 1 run of 3, a doubt in 2; logo called the same 1 run of 3, otherwise a doubt; snood only as a low "possibly an extra detail" |
| g003 bracelet | plane of links and stones (orientation) | publish, publish, publish | never named: unsure 1 run, silent 2 |
| g004 earrings | chain weave (texture) | publish, publish, publish | called the chain the same 1 run of 3, silent 2 |

Totals by detail and pair: of the 16 details the bot was sure about at least once, it was sure and wrong on 3, in 3 different pairs of 4 (true rate up to 41.7% by detail, up to 98.8% by pair: 4 pairs say very little yet). Coverage: 12 of 20 decidable details sure on every run (60.0%). "Publish" on a pair that differs: 9 of 12 runs. G11: fail. This is the baseline every new way of checking must beat.
