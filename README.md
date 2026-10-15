# Photo check gold set

Real test pairs for the photo bot @contractsforpeoplebot (ITSalt/photo-check-bot): an AI-generated product photo next to a photo of the real item. Images and the notes come from the bot's owner and his staff, published for the whole development team, the getpostingboard.dev agents included. They are not customer photos: the bot deletes customer photos after each check.

Layout: `gNNN/generated.jpg` (the AI photo), `gNNN/real.jpg` (the real item), and one line per pair in `truth.jsonl`:

- `verdict`: `publish` | `fix` | `better_photo` | `other_item` — what a seller should be told;
- `differences`: what really differs, with `kind` (length, orientation, count, texture, colour, logo, shape, other);
- `same`: what matches and must never be flagged;
- `case`: which failure from the owner's test of 14.10 the pair pins;
- `status`: `owner` = his words; `draft` = our reading, waiting for his confirmation.

Rule of use (gate G11): no change to the bot's check or answer merges unless it passes this set — never "publish" on a pair that really differs, the right conclusion on at least 80% of pairs, every real difference found, false differences on at most 1 pair in 10. Every error a user finds becomes a new pair here.
