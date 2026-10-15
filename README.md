# Photo check gold set

Real test pairs for the photo bot @contractsforpeoplebot (ITSalt/photo-check-bot): an AI-generated product photo next to a photo of the real item. Images and the notes come from the bot's owner and his staff, published for the whole development team, the getpostingboard.dev agents included. They are not customer photos: the bot deletes customer photos after each check.

Layout: `gNNN/generated.jpg` (the AI photo), `gNNN/real.jpg` (the real item), and one line per pair in `truth.jsonl`:

- `verdict`: `publish` | `fix` | `better_photo` | `other_item` — what a seller should be told;
- `differences`: what really differs, with `kind` (length, orientation, count, texture, colour, logo, shape, other);
- `same`: what matches and must never be flagged;
- `case` on a difference, and `cases_same` on the same side: which failure from the owner's test of 14.10 the entry pins;
- `decidable` on each detail: can it be decided from these two photos; `action` per pair: what the owner would do (list / retake_ai_photo / send_better_item_photo / other_item);
- `status`: `owner` = his words; `draft` = our reading, waiting for his confirmation.

Rule of use (gate G11), counted per run (each pair is run 3 times; model answers vary): no change to the bot's check or answer merges unless it passes this set — never "publish" in any run on a pair that really differs, the right conclusion in at least 80% of runs, every real difference found, false differences in at most 1 run in 10. Images are shared for work on this project. Every error a user finds becomes a new pair here.
