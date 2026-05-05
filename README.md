# NederWord

A static Dutch vocabulary game inspired by Wordle.

## Features
- Daily word mode
- Unlimited play mode
- Variable word length
- Progressive hint ladder across five attempts
- Dutch definition shown before guessing
- Dutch example sentence after one guess
- Literal English sentence translation after two guesses
- Literal English definition translation after three guesses
- English word translation after four guesses
- Easy-to-edit word bank in `data/words.json`
- GitHub Pages friendly

## Files
- `index.html`
- `styles.css`
- `scripts/app.js`
- `data/words.json`

## To customize
Add more entries to `data/words.json` in this format:

```json
{
  "word": "voorbeeld",
  "definition_nl": "Een uitleg of betekenis in het Nederlands.",
  "definition_en": "example",
  "word_en": "example",
  "example_nl": "Dit is een BLANK.",
  "example_en_literal": "This is a BLANK.",
  "definition_en_literal": "An explanation or meaning in the Dutch.",
  "difficulty": "A2"
}
```

## Translation fields

The game now uses these clue fields:

- `definition_nl`: shown at 0 guesses.
- `example_nl`: shown after 1 guess. Use `BLANK` where the answer belongs.
- `example_en_literal`: shown after 2 guesses. This should follow the Dutch sentence word order as closely as possible to support learning.
- `definition_en_literal`: shown after 3 guesses. This should be a literal, word-for-word English aid rather than polished English.
- `word_en`: shown after 4 guesses.
- `definition_en`: kept for backwards compatibility; it can match `word_en`.

I also included `data/translation-review.json` as a proofreading view of the new translation fields. The initial literal English fields are intentionally direct and sometimes awkward, so they are easy to map back to Dutch.

## Deployment
Upload the folder to GitHub Pages, Netlify, or any static host.


## Automated word list updates

This project now includes a GitHub Actions workflow that can refresh a broader Dutch candidate list
from OpenTaal and filter it down for this game.

### Files
- `.github/workflows/update-wordbank.yml`
- `scripts/build-wordbank.mjs`

### What the workflow does
- runs on a weekly schedule and on manual dispatch
- downloads the Dutch word list from OpenTaal
- filters out likely obscure or unsuitable entries:
  - too short / too long
  - capitals / names
  - hyphenated forms
  - apostrophes
  - digits
  - uncommon diacritics
  - many inflected/plural-like endings
- keeps only lowercase Dutch-looking words
- creates:
  - `data/candidate-words.json` (large filtered list)
  - `data/words.json` (smaller playable list with placeholder clues)

### Important note
The workflow is designed to expand the playable bank without flooding it with very obscure entries,
but it cannot perfectly measure word frequency on its own. For the best learning quality, you may
still want to manually review `data/words.json` from time to time.

### To improve it later
You can later add:
- CEFR tiers
- frequency lists from subtitles or corpora
- Wiktionary-based clue enrichment
- manual allow/block lists
