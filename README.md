# A statistics about Vietnamese name

Statistics of vietnamese names computed from a list of 115'000 Vietnamese full names I got from internet.

This project includes:

1. `unigrams.tsv`: the word appear in names and frequency.
2. `bigrams.tsv`: bigrams (2 words) appear in names and frequency.
3. `first_names.tsv`: first names and frequency
4. `last_names.tsv`: last names and frequency.

The stats showed that top 20 unigrams already appear in 82% vietnamese names and top 200 unigrams appear in 99% Vietnamese names. To search for recall of 90% vietnamese names from a database you just need an OR query of top 40 words in unigrams file.

| Top Unigram  | Percentage of names contains |
|-|-|
| 20 | 	82.68% |
| 40 |	90.88% |
| 100 |	97.17% |
| 200 |	99.09% |
| 300 |	99.59% |
| 400 |	99.77% |
