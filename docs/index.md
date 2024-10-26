# Efficient Vietnamese Name Retrieval using Highly Discriminative N-Grams

## Introduction

According to Wikipedia, more than [5 millions](https://en.wikipedia.org/wiki/Overseas_Vietnamese) overseas Vietnamese. Detect Vietnamese names from big database is useful for some community activities.

Recognizing Vietnamese names poses a challenge due to their shared Latin characters and similar word forms with names from other countries. For instance, the name “Nguyen” (folding form of popular surname Nguyễn or name Nguyên) is easily identifiable as Vietnamese. However, widespread Vietnamese surnames like “Lê” when written without accents, can be confused with common French prepositions like “Le” and “Đỗ” may be mistaken for the English verb “Do”. Furthermore, some names, such as “Vân” or “Văn”, sound distinctly Vietnamese but, when written without diacritics, can be confused with the common Dutch name “Van”.

This research shows the statictics of Vietnamese name based on a big dataset and propose a set of Highly Discriminative Names to retrieve Vietnamese names with high precisiona with recall from any global name database.

## Dataset

Dataset consists of positive and negative examples of Vietnamese names gathered from various sources: 

* names extracted from the Vietnamese phonebook through web crawling,
* student names from several universities in Vietnam,
* names extracted from a European country’s phonebook via web crawling, (
* names obtained from Wikipedia,
* names sourced from LinkedIn profiles. 

All names are normalization, which involves removing accents. For example, “Trần Văn” is transformed into “tran van”. Single-character tokens are excluded to reduce the risk of errors with abbreviations.

**Table 1. Overview of name dataset used in the research** 

| Total names | 4,126,815 |
| :---------------- | ----: |
| Positive names (Vietnamese names) | 635,678 |
| Negative names (Non-Vietnamese names) | 3,491,137 |
| Total tokens | 9,423,808 |
| Average tokens per name | 2.38
| Unique unigrams | 276,890 |
| Positive unigrams | 4,099 |

The top 10 unigrams and bigrams from Vietnamese names are presented in **Table 2**, revealing that a few tokens dominate the majority of names. Approximately 4,000 unigrams constitute over 635,000 Vietnamese names, with 124 of them classified as highly discriminative (primarily appearing in Vietnamese names but rarely in names from other nations). These 124 discriminative names encompass over 84% of Vietnamese names in our dataset, with “nguyen” being the most prevalent, accounting for nearly 9%. 

**Table 2. Top 10 unigrams and bigrams from 635,678 Vietnamese names**
| Unigram |  Percentage |
| :---------------- | ----: |
| nguyen | 8.90 |
| thi | 7.90 |
| van | 3.74 | 
| tran | 2.97 |
| le | 2.83 | 
| hoang | 2.27 | 
| thanh | 2.11 | 
| ngoc | 2.06 | 
| pham | 2.01 | 
| anh | 1.82 |

## Highly Discriminative N-Grams

If we use all 4000 unigrams as the query to retrieve Vietnamese names we can have 100% recall. However the precision is very low. The experiment from out dataset showed that when using nearly 1500 unigrams the presion already drop to 50%. Easy see that among top 10 popular Vietnamese unigram, "le" and "van" certainly retrieve many fall positive cases of european name.

**Figure 1. Precision and Recall (%) when using only unigram for retrieval**
![PrecRecallUnigrams](https://github.com/user-attachments/assets/908273c1-6ae2-446d-a4b4-1befe4254aba)


From what we observe in the big corpus of Vietnamese and international names, there are sets of tokens that appear only in Vietnamese names. For example “nguyen”, “huyen”, “phuong” are rarely found in the person’s name in other nations. They are considered highly discriminative unigrams. On the other hand, some unigrams, which are non-discriminative, and are common in both Vietnamese and foreign names, could be combined to create new highly discriminative bigrams. 

For example “tran” and “van” are non-discriminative unigrams as they appear in other nationalities, however, when they are combined, this pair becomes a discriminative feature for the Viet- namese name “tran van”. This bigram “tran van” is a commonly occurring combination, typically representing the family name “Trần” along with the middle or first name “Văn” in Vietnam.




