# Personality Variety Leaderboard

A static preview of a leaderboard that compares how much **Big Five personality**
varies across the texts a language model writes, against how much it varies across
human-written texts of the same kind.

**Live page:** https://lin3141.github.io/personality-variety-leaderboard/

This repository holds only the built page. The pipeline that produces the data lives in a
separate private repository.

## What the numbers mean

For one model, one text type and one word floor:

```
SD ratio (trait) = SD of the model's scores on that trait
                   ---------------------------------------
                   SD of human RAID texts of the same type

Variety          = mean of the five SD ratios
Overall          = mean of the six text types' Variety  (equal weight per type)
```

Human writing is 100% by definition. Models are ranked most varied first, so a model can
rank above the human baseline.

Scores come from a DLATK language-based assessment of the Big Five (openness,
conscientiousness, extraversion, agreeableness, neuroticism).

## Data snapshot

- 5 models, 6 text types, 3 word floors, default decoding settings only
- ~100 generated texts per model x type x floor
- Human baseline: 9,820 scored RAID human documents (943-1,781 per text type)

## Known limitations — read before drawing conclusions

This is a working preview, not a result. Four issues are known and unresolved:

1. **Length.** The word floor applies only to the model side; the human denominator is
   always at the human documents' natural length. At the 500+ and 1000+ floors the models
   write 700-1,600 words while RAID's Reddit, abstracts and Wikipedia documents top out at
   374, 312 and 346 words — there is no human text of comparable length in those cells.
   Scoring noise falls with document length, so the percentages are not comparable across
   floors.
2. **The `reviews` denominator is inflated ~17.5%.** 102 of 943 human review documents are
   under 100 words, which is below the models' own minimum, and their scores are much
   noisier. Every `reviews` percentage here is correspondingly too low.
3. **What the human baseline is.** RAID has no author field. Only Reddit posts and IMDb
   reviews are individually authored; Wikipedia articles, BBC news, arXiv abstracts and the
   `books` plot summaries are collaborative or institutional, so in those four types the
   denominator is genre variance, not between-person variance.
4. **Precision.** Each cell has ~100 texts. Bootstrap 95% CIs on Overall at the 500+ floor
   overlap for the top two models, and the two DeepSeek models are genuinely tied rather
   than tied by rounding. Read the ranks as rough tiers.

"Default settings" also differ by provider, and decoding temperature affects output
diversity directly.

## Viewing locally

The page reads its data from a `<script>` tag, not `fetch`, so opening `index.html`
from disk works without a server.
