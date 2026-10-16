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

## Baseline: the bot as it was on 15.10

Production check (google/gemini-3.6-flash), 3 runs per pair, answers mapped to the four conclusions, scored by two blind scorers. "Sure" = the bot named the detail as different, or listed it as the same.

| Pair | What really differs (kind) | Bot's conclusion, 3 runs | What the bot said about the difference |
|---|---|---|---|
| g001 trousers | cuff height (length) | publish, publish, publish | called the cuffs the same, all 3 runs |
| g002 beanie | knit (texture); star logo position (logo); an added snood (other) | fix the AI photo, all 3 runs | knit sure-different 1 run of 3, a doubt in 2; logo called the same 1 run of 3, otherwise a doubt; snood only as a low "possibly an extra detail" |
| g003 bracelet | plane of links and stones (orientation) | publish, publish, publish | never named: unsure 1 run, silent 2 |
| g004 earrings | chain weave (texture) | publish, publish, publish | called the chain the same 1 run of 3, silent 2 |

Totals by detail and pair: of the 16 details the bot was sure about at least once, it was sure and wrong on 3, in 3 different pairs of 4 (true rate up to 41.7% by detail, up to 98.8% by pair: 4 pairs say very little yet). Coverage: 12 of 20 decidable details sure on every run (60.0%). "Publish" on a pair that differs: 9 of 12 runs. G11: fail. This is the baseline every new way of checking must beat.
